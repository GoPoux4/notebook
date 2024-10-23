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

### 锅里捞面

给了一个四小时的 mp4，画面是 AAA logo，音频是噪声。

> 据说来自 CCBC 的传奇电报员可以一口气发四个小时电报……

那就是摩斯电码了，把音频丢进 audacity，看到波形，发现有规律的长短音。写个脚本解码：

```python
import wave
import numpy as np
import itertools

def compress(L, v):
    values = [int(i > v) for i in L]
    return [(k, len(list(g))) for k, g in itertools.groupby(range(len(values)), values.__getitem__)]

def decompress(L):
    return [L[i][0] for i in range(len(L)) for _ in range(L[i][1])]

audio = wave.open('output.wav', 'rb')
params = audio.getparams()
print(params)
nchannels, _, samplerate, nframes = params[:4]

samples = audio.readframes(nframes)
audio.close()

samples = np.abs(np.frombuffer(samples, dtype=np.int16))
print(samples)

for i in range(15, len(samples) - 15):
    if samples[i] == 0:
        samples[i] = np.average(samples[i - 15:i + 15])

# print("Start compressing...")
compressed = compress(samples, 10000)
compressed = [(k, v) for k, v in compressed if v > 20]
compressed = compress(decompress(compressed), 0)

# Decode morse code
from morseutils.translator import MorseCodeTranslator as mct

L = compressed
morse = []

for i in range(len(L)):
    if L[i][0] == 0 and L[i][1] > 10000:
        morse.append(' ')
    elif L[i][0] == 1:
        if L[i][1] < 10000:
            morse.append('.')
        else:
            morse.append('-')

morse_code = "".join(morse)[1:-1]
print(morse_code)
print(mct.decode(morse_code))
```

修改自打 CCBC 时队友写的脚本。得到 `AYICIKQXHR320E7CHW4Y84ZGM954UG061H9QV9X2360TJJ37H9ABL42ABJH5BB`。

然后是视频，通过盯帧可以发现有些帧会有白色或者黑色的竖线，看着像条形码，写个脚本提取一下有竖线的帧，可以看到这些竖线从左到右扫描了三次（脚本代码找不到了，就不放了）。

写个脚本组合一下竖线，可以看到条形码，但是扫不出来。

文件名提示 `代号128`，说明是 [code128](https://en.wikipedia.org/wiki/Code_128) 条形码，直接手动解码：

```python
import cv2
import numpy as np
import os
import itertools

def compress(L, v):
    values = [int(i < v) for i in L]
    return [(k, len(list(g))) for k, g in itertools.groupby(range(len(values)), values.__getitem__)]

def slice(img):
    """
    只提取一行，避免 AAA logo 影响
    """
    return img[75:75+10, 0:-1]

# 打开视频文件
cap = cv2.VideoCapture('代号128.mp4')

L = np.zeros(256) # 记录
con = [] # 组合

while True:
    ret, frame = cap.read()
    if not ret:
        break

    gray_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    slice0 = gray_frame[75, :]

    for i in range(256):
        if slice0[i] < 50 or slice0[i] > 200:
            if L[i] != 0:
                con = np.concatenate((con, L[29:227]))
                L = np.zeros(256)
            L[i] = slice0[i]

con = np.concatenate((con, L[29:227]))
con = compress(con, 128)

cap.release()

# code128 table
code128_t = { "212222": " ", "222122": "!", "222221": '"', "121223": "#", "121322": "$", "131222": "%", "122213": "&", "122312": "'", "132212": "(", "221213": ")", "221312": "*", "231212": "+", "112232": ",", "122132": "-", "122231": ".", "113222": "/", "123122": "0", "123221": "1", "223211": "2", "221132": "3", "221231": "4", "213212": "5", "223112": "6", "312131": "7", "311222": "8", "321122": "9", "321221": ":", "312212": ";", "322112": "<", "322211": "=", "212123": ">", "212321": "?", "232121": "@", "111323": "A", "131123": "B", "131321": "C", "112313": "D", "132113": "E", "132311": "F", "211313": "G", "231113": "H", "231311": "I", "112133": "J", "112331": "K", "132131": "L", "113123": "M", "113321": "N", "133121": "O", "313121": "P", "211331": "Q", "231131": "R", "213113": "S", "213311": "T", "213131": "U", "311123": "V", "311321": "W", "331121": "X", "312113": "Y", "312311": "Z", "332111": "[", "314111": "\\", "221411": "]", "431111": "^", "111224": "_", "111422": "`", "121124": "a", "121421": "b", "141122": "c", "141221": "d", "112214": "e", "112412": "f", "122114": "g", "122411": "h", "142112": "i", "142211": "j", "241211": "k", "221114": "l", "413111": "m", "241112": "n", "134111": "o", "111242": "p", "121142": "q", "121241": "r", "114212": "s", "124112": "t", "124211": "u", "411212": "v", "421112": "w", "421211": "x", "212141": "y", "214121": "z", "412121": "{", "111143": "|", "111341": "}", "131141": "~"}

res = ""

for i in range(0, len(con), 6):
    c = con[i : i + 6]
    code = ""
    for j in range(6):
        code += str(c[j][1])
        print(code, code128_t[code])
        res += code128_t[code]

print(res)
```

得到 `BASE36ENCODETABLE:ZJUCTF24ABDEGHIKLMNOPQRSVWXY01356789`。

意思是用自定义的 base36 编码表解码上面那串密文，写个脚本解码：

```python
import base36

custom_alphabet = "ZJUCTF24ABDEGHIKLMNOPQRSVWXY01356789"
code = "AYICIKQXHR320E7CHW4Y84ZGM954UG061H9QV9X2360TJJ37H9ABL42ABJH5BB"

def decode_custom_base36(code, alphabet):
    # 创建字符到值的映射字典
    char_to_value = {char: index for index, char in enumerate(alphabet)}
    
    # 初始化解码后的数值
    decoded_value = 0
    
    # 遍历编码字符串
    for char in code:
        decoded_value = decoded_value * 36 + char_to_value[char]
    
    return decoded_value

def base36_to_string(value):
    result = []
    while value > 0:
        result.append(chr(value % 256))
        value //= 256
    return ''.join(result[::-1])

# 使用自定义字符集进行解码
decoded_value = decode_custom_base36(code, custom_alphabet)
decoded_string = base36_to_string(decoded_value)
print(decoded_string)
```

得到 flag `ZJUCTF{A_13G3nd4Ry_4h0uR-T3l36r4Phls7XD}`