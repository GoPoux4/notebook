# ZJUCTF2024 Writeup

## Misc

### Minec(re)tf

最喜欢的 MC 题。

给了一个存档和一个 testmod，还给了游戏版本 1.20.4 和 fabric 版本，下载下来打开存档看看。

#### Start

进入存档后出生点在一个 box 里面，游戏模式是 spectator。

<div align="center"><img src="/assets/img/ctf/writeups/zjuctf2024/start.png" width="70%"/></div>

尝试更改游戏模式，立刻会被改回 spectator；尝试走出 box 会被立刻传送回来。告示牌提示 “these constrains must be activated EVERY TICK”，也就是检查玩家游戏模式/位置的指令每 tick 都会执行。

众所周知，tick 是可被命令控制的，于是直接 `/tick freeze`，此时就随便改游戏模式走出 box 了。

但显然 tick 一直被冻结是没法继续进行的，需要找到限制我们行为的命令方块（？）的位置。这里应该可以用 NBTExplorer 找命令方块，但不知为何一找 command_block 就卡死（可能命令方块太多了？毕竟有一整个区块的命令方块）。

网上到处搜搜，搜到了 [CommandSpy](https://modrinth.com/mod/commandspy) 这个 mod，可以显示正在执行的命令的发送者信息，但这个 mod 最高到 1.20，没法放进 1.20.4。

这个好办，下个 1.20 然后降级存档再打开，此时控制台上就会显示正在执行的命令方块的位置了。

<div align="center"><img src="/assets/img/ctf/writeups/zjuctf2024/commandspy.png" width="50%"/></div>

可以看到只有 5 个命令方块在循环执行，全都在 run 某个 function，这些 function 看着不像原版的，应该是数据包引入的。于是打开存档目录下 `datapacks` 文件夹，找到上面的 function 内容。

先看看 `lobby/tptolobby.mcfunction`：

```mcfunction
tp @a[tag=!inlobby] 50 53 50
spawnpoint @a[tag=!inlobby] 50 53 50
tag @a[tag=!inlobby] add inlobby
```

好像没什么用，玩家一直都有 `inlobby` 标签。再看看 `obfuscation/775533.mcfunction`：

```mcfunction
gamemode spectator @a

say The map is under construction. I can only view the map
give @a repeating_command_block{display:{Name:'["",{"text":"Script Engine","italic":false,"color":"aqua","bold":true}]',Lore:['["",{"text":"Flag Piece[3/9]:cjRmNydz","italic":false},{"text":"","italic":false}]','["",{"text":"\\"","color":"#b2aca2"},{"text":"the script engine of the flag.","color":"#b2aca2"},{"text":"","italic":false},{"text":"\\"","color":"#b2aca2"}]','["",{"text":"You ask me why a flag need a script engine? How could the flag fly without a script engine to run the phisics simulation?","italic":false,"color":"#e8e6e3"},{"text":"","italic":false},{"text":"","italic":false},{"text":"","italic":false}]']}}
clear @a
```

看来这个就是强制玩家模式为 spectator 的命令方块，同时给了一个 flag piece `Flag Piece[3/9]:cjRmNydz`。进入游戏把对应位置的命令方块拆掉就能改模式了。

浏览一下 `obfuscation` 目录，每个 function 都会给 flag piece，但相同 flag piece 会给无数个，果然是 obfuscation。把 9 个 piece 都搜一下，发现 `Flag Piece[9/9]` 只有一个，想必就是第 9 个 piece 了，得到 `Flag Piece[9/9]:bWl6RUR9`。

再搜一下最后一个运行中的命令方块，这个命令方块执行的命令是：

```mcfunction
execute as @a[scores={spawner_mined=1000000..},tag=!recived_reward] run function obfuscation:585664
```

检测玩家 spawner_mined 分数大于 1000000，运行 `obfuscation:585664`。这个 function 里面是：

```mcfunction
# tp @a 50 62 50

say You've reached 1000000 Score, here's your reward
give @s white_dye{display:{Name:'["",{"text":"Pure Dye","italic":false,"color":"aqua","bold":true}]',Lore:['["",{"text":"Flag Piece[7/9]:SUF5X2lU","italic":false}]','["",{"text":"\\"You need some dye to draw on the flag\\"","color":"#b2aca2"}]','["",{"text":"Scoreboard and tags, the old school cmb.","italic":false,"color":"#e8e6e3"}]']}}
tag @s add recived_reward
scoreboard players remove @s spawner_mined 1000000
```

看来是挖掉 1000000 个刷怪笼会给 flag piece，得到 `Flag Piece[7/9]:SUF5X2lU`。

!!! note "非预期？"
    
    运行 `obfuscation:585664` 的这个命令方块藏在了那一个区块命令方块里面，但是在这个区块最底下有个凸出来的命令方块，里面运行的命令是一样的，但是没有保持开启，实际上还是内部的命令方块在检查。我先找到底下这个，直接拿到 flag 了。

再回到游戏，还是没法出去，说明此时不是命令方块在执行命令了。

这里卡了很久，网上搜了很久，才知道 datapack 可以每 tick 执行一次函数。进入数据包的 `minecraft/tags/functions` 目录，找到 `tick.json`：

```json
{"values":["main:loop"]}
```

数据包每 tick 执行 `main:loop`，找到 `main` 目录下的 `loop.mcfunction`：

```mcfunction
...
tag @a[x=48,y=61,z=48,dx=4,dy=2,dz=4] add in_box
execute as @a[tag=!in_box] run function obfuscation:317754
tag @a remove in_box
```

检查玩家是否在一个 4x2x4 的 box 里面，不在的话执行 `obfuscation:317754`。这个 function 里面是：

```mcfunction
tp @s 50 62 50

say The map is under construction. I can only stay in the view box
give @s paper{display:{Name:'["",{"text":"Code","italic":false,"color":"aqua","bold":true}]',Lore:['["",{"text":"Flag Piece[2/9]:e01pTkVj","italic":false},{"text":"","italic":false},{"text":"","italic":false}]','["",{"text":"\\"","color":"#b2aca2"},{"text":"the code of the flag.","italic":false,"color":"#b2aca2"},{"text":"","italic":false},{"text":"","italic":false},{"text":"\\"","color":"#b2aca2"}]','["",{"text":"You ask me why a flag need any code?","italic":false,"color":"#e8e6e3"}]','["",{"text":"How could the flag fly without some code to specify the behavior of the phisics simulation?","italic":false,"color":"#e8e6e3"},{"text":"","italic":false},{"text":"","italic":false},{"text":"","italic":false},{"text":"","italic":false}]']}}
clear @s
```

得到 `Flag Piece[2/9]:e01pTkVj`。把 `loop.mcfunction` 里那三行注释掉，重新加载一下数据包就能走出 box 了。

box 底下是个 RPG 地图，通关了也没什么用。

在 datapacks 里面再找找，`main/recipes` 里面有个 `flag_look_at_the_code_of_the_recipe.json`。很长，只看第一行：

> Note that this recipe would note be recognized by vanilla minecraft, read the code your self! (or you can give yourself the items by /function obfuscation:855371)

进入游戏，执行 `/function obfuscation:855371`，背包里就有各个 piece 的提示了：

<div align="center"><img src="/assets/img/ctf/writeups/zjuctf2024/recipes.png" width="40%"/></div>

第一个 piece 直接给了，得到 `Flag Piece[1/9]:WkpVQ1RG`。接下来还要找第 4、5、6、8 个 piece。

#### Flag Piece 4

> There are different **model** of wool in this world. Only the unused one(a special **resources**) can be used to craft a flag.Note: this piece is not an item!

有一个 wool 的模型藏了 flag piece。打开存档目录下的 `resources.zip`，找 wool 的模型，在 `assets/minecraft/models/block` 里面找到 `lightblue_wool.json` 有点可疑。

credit 说 `Made with Blockbench`，那就用 [blockbench](https://web.blockbench.net/) 打开这个模型文件，得到：

<div align="center"><img src="/assets/img/ctf/writeups/zjuctf2024/blockbench.png" width="50%"/></div>

得到 `Flag Piece[4/9]:X20wXmVf`。

#### Flag Piece 5

> It seems that the mod only change the appearance of mobs. However, a certain mob didn't seems right by other people without the mod. People used be able to harvest this special shinny stick form her bones. However, due to the heavy demand of flags, nowadays she hides it behind some **encryption** so that you can't get them.

意思是 flag 藏在 testmod 中。反编译 `testmod-Alpha.1.0.0.jar`，找到 `TestMobEntity.class`，有个 `decrypt` 方法：

```java
private void decrypt() {
    byte[] tmp = (byte[])mask.clone();
    byte[] tmp1 = this.lore[0].getBytes(StandardCharsets.US_ASCII);
    for (int i = 0; i < mask.length; i++)
        tmp[i] = (byte)(tmp[i] ^ tmp1[i]); 
    this.lore[0] = new String(tmp);
}
```

自己实现一下这个解密过程，得到 `Flag Piece[5/9]:RnVuX2xm`。

#### Flag Piece 6

> There's 9 **regions** that does not belong to this world, however both worlds are rooted from the same **seed**, The string is the only different between our version and the other. Many taking some **schemetics** and compare the two would be a good idea. Note: this piece is not an item!

意思是存档中有个地方塞了几个其他世界的区域，要找不同。

在存档目录下找到 `region` 文件夹，看到几个可疑的区域文件：

<div align="center"><img src="/assets/img/ctf/writeups/zjuctf2024/regions.png" width="30%"/></div>

算一下坐标，tp 过去看看，发现这边只有一个区块（算坐标参考 [mcwiki](https://minecraft.wiki/w/Region_file_format)），坐标大概在 `394410 ~ -14256522`。

`/seed` 查看种子，再用相同的种子新建一个默认世界，传送到对应区块（这里作者搞错坐标了，要用 hint 给的坐标）。

那么如何比较区块差异呢？经常玩生电的朋友肯定知道，litematica 就是干这个的。

装个 litematica，将整个区块保存为原理图。回到原存档，放置原理图，和地图中的区块对齐，然后打开投影的原理图验证选中所有不同的方块，可以看到：

<div align="center"><img src="/assets/img/ctf/writeups/zjuctf2024/litematica.png" width="40%"/></div>

得到 `Flag Piece[6/9]:X3kwdV9w`。

#### Flag Piece 8

> I think there will be some gold decorations in the **house of the village gold smith**, that is if their house do spawn. Only if there's some block that let you see their houses...

“their house do spawn” 暗示需要找到方式生成这个结构。数据包下 `main/structures` 中有个 `gold_smith.nbt`，应该是结构文件。用 structure block 生成，结构名称填 `main:gold_smith`，即可加载结构：

<div align="center"><img src="/assets/img/ctf/writeups/zjuctf2024/gold_smith.png" width="70%"/></div>

结构内的箱子里找到 deocration：

<div align="center"><img src="/assets/img/ctf/writeups/zjuctf2024/decoration.png" width="70%"/></div>

得到 `Flag Piece[8/9]:X2N1NXRP`。

#### Obtain the Flag

将所有 flag piece 组合起来，得到 `WkpVQ1RGe01pTkVjcjRmNydzX20wXmVfRnVuX2xmX3kwdV9wSUF5X2lUX2N1NXRPbWl6RUR9`，base64 解码得到 flag `ZJUCTF{MiNEcr4f7's_m0^e_Fun_lf_y0u_pIAy_iT_cu5tOmizED}`。