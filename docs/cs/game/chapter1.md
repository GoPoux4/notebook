# Introductions

## Taxonomies 分类

- By genre: action, adventure, role-playing, simulation, strategy, sports, puzzle, idle, etc.
- By platform: PC, console, mobile, VR, etc.
- By purpose: entertainment, education, training, marketing, etc.
- By controller: touchscreens, motion capture, gamepads, keyboards, etc.

## Pygame

### Modules

<div align="center"><img src="/assets/img/CS/game/chapter1/modules.png" width="80%"></div>

## Game Loop

```python title="intro1.py"
pygame.init()

size = width, height = 1024, 768
speed = [3,2]
black = (0, 0, 0)

screen = pygame.display.set_mode(size)
pygame.display.set_caption('Logo Bouncer')
logo = pygame.image.load(logo_file)
logo_width, logo_height = logo.get_size()
logo_x = logo_y = 0

running = True
clock = pygame.time.Clock()
while running:
    clock.tick(30)
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
            
    logo_x += speed[0]
    logo_y += speed[1]
    
    if logo_x < 0 or logo_x + logo_width > width:
        speed[0] = -speed[0]
    if logo_y < 0 or logo_y + logo_height > height:
        speed[1] = -speed[1]
    
    screen.fill(black)
    screen.blit(logo, (logo_x, logo_y))
    pygame.display.flip()
```

The structure of `intro1.py`:

- some initialization stuff
- a game loop: Game ends when loop is exited

game loop:

- check for updates (from user input, network, etc.)
- update game state
- draw the next frame

## Frame

One picture is rendered (calculated and shown to user) each time around the loop.

FPS: frames per second

## Event Queue

```python
# Example event processing
def get_inputs():
    done = left = right = False
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            done = True
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_ESCAPE:
                done = True
            elif event.key == pygame.K_RIGHT:
                right = True
            elif event.key == pygame.K_LEFT:
                left = True
    return (left, right, done)
```

一次 game loop 中，可能有多个事件，如键盘按键、鼠标点击等，需要逐个处理。

## MVC: model-view-controller

<div align="center"><img src="/assets/img/CS/game/chapter1/mvc.png" width="50%"></div>

### Model

Manages the data and operations on the data.

Provides a well-defined interface to the data.

### View

Provides a way to visualize the data: Draw the frame, make the music, etc.

### Controller

Maps input actions to model or view actions.

## Object-oriented model/view 面向对象

Each object knows its own state, so is part of the model.

Each object knows how to draw itself, so is part of the view. have a `draw` method.

## Graphic Primitives 图形基元

An engine provides some level of graphic algorithms to be embedded in your code. Pygame is an engine for 2D games.

Each pixel has a color. Usually formed by mixing 8-bit values for red, green, blue and alpha.

In Python and Pygame, the color is a tuple or a hex string.

```python
Red = (255, 0, 0)
Red = '0xFF0000'
```

### Alpha Channel

An extra 8-bits specifies the amount of transparency.

- 0 = transparent. 完全透明
- 255 = opaque. 完全不透明

## The Surface

In Pygame, a surface is a place to draw.

Special surface: the display surface -- will be visible to the user in the game window.

```python
display_surface = pygame.display.set_mode((800, 600))
```

Other surfaces can be created and drawn on the display surface.

```python
background = pygame.Surface(display_surface.get_size())
display_surface.blit(background, (0, 0))
```

### Surface Operations

- `get_at`, `set_at`: get or set the color of a pixel

    ```python
    color = background.get_at((100, 100))
    background.set_at((100, 100), color)
    ```
- `fill`: fill the surface with a color

    ```python
    background.fill((255, 255, 255))
    ```
- And a line can be drawn between two points with `pygame.draw.line`.

    ```python
    pygame.draw.line(surface, color, (x0, y0), (x1, y1))
    ```
## Draw module

- `pygame.draw.arc`: draw an elliptical or circular arc from `start_angle` to `stop_angle` (in radians)

    ```python
    pygame.draw.arc(surface, color, rect, start_angle, stop_angle, width)
    ```

    Position and dimensions are specified by the rect parameter, a bounding box. 位置和尺寸由 rect 参数指定，在一个边界框内。

- `pygame.draw.lines`: draw a series of connected lines

    ```python
    pygame.draw.lines(surface, color, closed, pointlist, width)
    ```

    If `closed` is True, will also connect the last point back to the first.

- `pygame.draw.polygon`: draw a polygon

    ```python
    pygame.draw.polygon(surface, color, pointlist)
    ```

    polygon acts similarly, connecting points from a list, but then it fills in the enclosed space. 

    与 lines 设置 closed 参数为 True 类似，polygon 会自动连接最后一个点和第一个点。若未设置 width 参数，则会填充多边形。

- `pygame.draw.circle`: draw a circle

    ```python
    pygame.draw.circle(surface, color, center, radius)
    ```

    与 polygon 类似，circle 会填充圆形。

## Rect

Rects are defined by four values.

```python
rect = pygame.Rect(x, y, width, height)
```

### Rect Methods

- `copy`: return a new rect with the same values
- `move`: return a new rect moved by a given offset
- `move_ip`: move the rect by a given offset
- `inflate`: return a new rect with the size changed by a given amount
- `clip`: return a new rect that is the intersection of two rects
- `union`: return a new rect that is the union of two rects
- `contains`: check if a rect is completely inside another
- `collidepoint`: check if a point is inside the rect
- `colliderect`: check if two rects overlap
- `collidelist`: check if a rect overlaps with any in a list

### Draw

- `pygame.draw.rect`: draw a rectangle

    ```python
    pygame.draw.rect(surface, color, rect, width)
    ```

    If width is 0, the rectangle will be filled.

- `pygame.draw.ellipse`: draw an ellipse. 矩形的内切椭圆

    ```python
    pygame.draw.ellipse(surface, color, rect, width)
    ```

    If width is 0, the ellipse will be filled.

## Images and Blit

### Image Load / Save

- `pygame.image.load`: load an image from a file

    ```python
    surface = pygame.image.load(filename)
    ```
- `pygame.image.save`: save an image to a file

    ```python
    pygame.image.save(surface, filename)
    ```
- `pygame.surface.convert`: convert a surface to a new format

    ```python
    new_surface = surface.convert()
    ```

    convert will change the format of a surface to match the display surface.

### BLIT

BLIT "pastes" one image into another.

The Surface object has a blit method for pasting another image (or portion) into the image

```python
surface.blit(source, dest, area=None, special_flags=0)
```

- source: the surface to be pasted
- dest: the position to paste the source
- area: can be a rect to paste only a portion of the source, only the area that intersects with the rect in the source will be pasted
- special_flags: can be used to control the pasting

### Colorkey

Colorkey is a special color that is ignored when blitting.

```python
surface.set_colorkey(color)
```

## Surface Transforms

`pygame.transform` module has methods to manipulate a surface. rotate / scale / chop / greyscale / ...