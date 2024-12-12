# Sprites and Randomness

## Fonts and Text Renders

### Font Module

Pygame renders text to a surface with the Font module.

Font module has methods to locate and load a font as a Font object.

- `pygame.font.get_fonts`: get a list of available fonts

- `pygame.font.SysFont`: get a Font object from the system fonts

    ```python
    font = pygame.font.SysFont(name, size, bold=False, italic=False)
    ```

    - name: the name of the font. `None` for the default font.
    - size: the size of the font
    - bold: True if the font should be bold
    - italic: True if the font should be italic

- `pygame.font.Font`: get a Font object from a file

    ```python
    font = pygame.font.Font(filename, size)
    ```

    - filename: the path to the font file
    - size: the size of the font

### Text Rendering

- `Font.render`: render text to a surface

    ```python
    surface = font.render(text, antialias, color, background=None)
    ```

    - text: the text to render
    - antialias: True for antialiased text. 边缘平滑
    - color: the color of the text
    - background: the color of the background
  
- `Font.size`: get the size of the text

    ```python
    width, height = font.size(text)
    ```

- `Font.get_height`: get the height of the font. 字体高度

    ```python
    height = font.get_height()
    ```

- `Font.get_linesize`: get the height of a line of text. 行间距

    ```python
    linesize = font.get_linesize()
    ```

- `Font.metrics`: get the metrics of the font. 字体度量

    ```python
    metrics = font.metrics(text)
    ```

    Returns a list with one tuple per character in the text string.

    Each tuple has information on exact sizes and placement of that character.

## Sprites

Pygame's `Sprite` class is intended to assist to make objects of the on-screen elements.

`Group` classes (there are several) are useful to manage sprites.

Need to subclass `Sprite` to create a sprite.

```python
class Mario(pygame.sprite.Sprite):

    def __init__(self, pos, sprites_image):
        pygame.sprite.Sprite.__init__(self)
        
        self.image = pygame.Surface((29,47))        
        image_surf = pygame.image.load(sprites_image).convert()
        self.image.blit(image_surf, (0,0), (4, 13, 29, 47))
        self.image.set_colorkey((255,255,255))
        self.rect = self.image.get_rect(topleft=pos)     
        
    def update(self, click):
        if click:
            self.rect.center = click
        
    def draw(self, surface):
        surface.blit(self.image, self.rect)
```

### Sprite Groups

`Group` classes are used to manage sprites.

- `pygame.sprite.Group`: a simple group

    ```python
    group = pygame.sprite.Group()
    ```

- `Group.add`: add a sprite to the group

    ```python
    group.add(sprite)
    ```

- `Group.remove`: remove a sprite from the group

    ```python
    group.remove(sprite)
    ```

- `Group.draw`: draw all sprites in the group

    ```python
    group.draw(surface)
    ```

- `Group.update`: update all sprites in the group

    ```python
    group.update(*args)
    ```

- `Sprite.kill`: remove the sprite from all groups

    ```python
    sprite.kill()
    ```

- `Group.clear`: draw from the background into the destination at every place a sprite was last drawn

    ```python
    group.clear(surface, background)
    ```

    just erase where the sprites were, then update them, then redraw them.

    不用每次循环都设置 `background`，只需在游戏开始时设置一次即可。

### Other Classes

- `RenderUpdates`: a group that keeps track of the dirty rectangles

    ```python
    group = pygame.sprite.RenderUpdates()
    ```

- `OrderedUpdates`: a group that keeps track of the order of the sprites as they are added

    ```python
    group = pygame.sprite.OrderedUpdates()
    ```

- `LayeredUpdates`: a Group that has layers, sort of a group within the Group

    ```python
    group = pygame.sprite.LayeredUpdates()
    ```

- `GroupSingle`: a group that only holds one sprite

    ```python
    group = pygame.sprite.GroupSingle()
    ```

    If a new sprite is added, the old one is ejected.

### Collisions

A collision occurs when two sprites occupy the same space.

- `pygame.sprite.collide_rect`: check if the rects of two sprites overlap, return a boolean

    ```python
    collide = pygame.sprite.collide_rect(sprite1, sprite2)
    ```

    sprite1 and sprite2 must have a `rect` attribute.

- `pygame.sprite.collide_rect_ratio`: multiplies the size of the rects by a floating point value before looking for overlap

    ```python
    collide = pygame.sprite.collide_rect_ratio(ratio)(sprite1, sprite2)
    ```

- `pygame.sprite.collide_circle`: check if the circles of two sprites overlap, return a boolean

    ```python
    collide = pygame.sprite.collide_circle(sprite1, sprite2)
    ```

    sprite1 and sprite2 must have a `rect` attribute and an optional `radius` attribute.

- `pygame.sprite.collide_circle_ratio`: as `collide_rect_ratio`, but for circles

- `pygame.sprite.collide_mask`: check if the masks of two sprites overlap, return a boolean

    ```python
    collide = pygame.sprite.collide_mask(sprite1, sprite2)
    ```

    sprite1 and sprite2 must have a `rect` attribute and an optional `mask` attribute.

- `pygame.sprite.spritecollide`: returns a list of sprites in the group that collide

    ```python
    collide = pygame.sprite.spritecollide(sprite, group, dokill, collided=None)
    ```

    - dokill: if True, the sprites that collide will be removed from the group
    - collided: a callback function to check for collisions

- `pygame.sprite.groupcollide`: tests for collisions between two groups

    ```python
    collide = pygame.sprite.groupcollide(group1, group2, dokill1, dokill2, collided=None)
    ```

    Returns a dictionary with every sprite in group1 as a key and a list of every sprite in group2 that collides with it as the value.

    - dokill1: if True, the sprites that collide in group1 will be removed
    - dokill2: if True, the sprites that collide in group2 will be removed
    - collided: a callback function to check for collisions

## Animation

Show a quick sequence of images to the user, 30-72 frames per second or so.

- Slower loop: use `pygame.time.Clock` to control the frame rate

    ```python
    clock = pygame.time.Clock()
    ```

    ```python
    while running:
        clock.tick(30) # 30 frames per second
    ```

- Measure: The `Clock` object can also be used to measure the time since the last call

    ```python
    time_passed = clock.tick() # milliseconds since the last call
    ```

## Randomness

### Pseudo-randomness

- Linear Congruential Generator (LCG)

    \[
    X_{n+1} = (aX_n + c) \mod m
    \]

    used in Pseudo-Random Number Generator (PRNG).

    LCG Cycles: the number of times you can call the LCG before it repeats

- PRNG: A good PRNG will have carefully chosen constants to ensure the longest possible cycle.

    other sources of "randomness" are usually sampled and mixed in: mouse movements, keyboard presses, etc.

### Random Distributions

The distribution of a PRNG or code using a PRNG refers to the chance of attaining each value

#### Asymmetric Distributions

How can I get a higher chance of values at the upper ranges?

- Dropping the lowest roll: Roll twice (or more), pick the higher roll

    ```python
    def drop_lowest_roll(number, sides):
        roll1 = roll_dice(number, sides)
        roll2 = roll_dice(number, sides)
        return max(roll1, roll2)
    ```

- Drop the lowest dice: roll an extra die and drop the lowest value

    ```python
    def drop_lowest_die(number, sides):
        rolls = []
        for i in range(number + 1):
            rolls.append(roll_dice(1, sides))
        rolls.sort()
        return sum(rolls[1:])
    ```

- Reroll the lowest: roll two dice and then reroll whichever is lowest

    ```python
    def reroll_lowest_die(number, sides):
        rolls = []
        for i in range(number):
            rolls.append(roll_dice(1, sides))
        rolls.sort()
        rolls[0] = roll_dice(1, sides)
        return sum(rolls)
    ```

- Critical Hits: In a small percentage (5%?) of cases, extra value is added (perhaps by another die roll)

    ```python
    def roll_with_critical(number, sides, crit):
        roll = roll_dice(number, sides)
        if random.random() < crit:
            roll += roll_dice(1, sides)
        return roll
    ```

- Arbitrary Distributions: 