# Flappy Bird

## Overview

Flappy Bird is a simple game where the player controls a bird, attempting to fly between columns of pipes without hitting them.
Players can control the bird by tapping to make it flap its wings and fly upwards.

## Algorithms

### Collision Detection

Collision detection is used to determine if the bird has hit a pipe or the ground.

The implementation uses two methods to detect collisions between two entities:

1. Axis-aligned bounding box (AABB) collision detection

    Get the bounding box (rectangle) of the two entities and check if they overlap.

    ```python
    self.rect.colliderect(other.rect)
    ```

2. Pixel-perfect collision detection

    Check if the two entities have overlapping pixels which are not transparent.

    We can get the hit mask of an image by checking the alpha channel:

    ```python
    @memoize
    def get_hit_mask(image: pygame.Surface) -> HitMaskType:
        """returns a hit mask using an image's alpha."""
        return list(
            (
                list(
                    (
                        bool(image.get_at((x, y))[3])
                        for y in range(image.get_height())
                    )
                )
                for x in range(image.get_width())
            )
        )
    ```

    And we can check for pixel-perfect collision using the hit masks:

    ```python
    def pixel_collision(
        rect1: pygame.Rect,
        rect2: pygame.Rect,
        hitmask1: HitMaskType,
        hitmask2: HitMaskType,
    ):
        """Checks if two objects collide and not just their rects"""
        rect = rect1.clip(rect2)

        if rect.width == 0 or rect.height == 0:
            return False

        x1, y1 = rect.x - rect1.x, rect.y - rect1.y
        x2, y2 = rect.x - rect2.x, rect.y - rect2.y

        for x in range(rect.width):
            for y in range(rect.height):
                if hitmask1[x1 + x][y1 + y] and hitmask2[x2 + x][y2 + y]:
                    return True
        return False
    ```

    Then when the entity is initialized, we can get the hit mask of the entity:

    ```python
    self.hit_mask = get_hit_mask(image) if image else None
    ```

    And when we want to check for a collision, we can use the hit mask:

    ```python
    pixel_collision(self.rect, other.rect, self.hit_mask, other.hit_mask)
    ```

### Physics Simulation

The physics simulation is used to calculate the bird's position and velocity, making its falling, flapping and rotation close to real-world physics.

The bird has three states:

1. Simple Harmonic Motion (SHM)

    The bird oscillates up and down with no rotation when the game has not started.

    The constants for SHM are defined as:

    ```python
    self.vel_y = 1  # player's velocity along Y axis
    self.max_vel_y = 4  # max vel along Y, max descend speed
    self.min_vel_y = -4  # min vel along Y, max ascend speed
    self.acc_y = 0.5  # players downward acceleration

    self.rot = 0  # player's current rotation
    self.vel_rot = 0  # player's rotation speed
    self.rot_min = 0  # player's min rotation angle
    self.rot_max = 0  # player's max rotation angle
    ```

    In each tick, the bird's position is updated by:

    ```python
    def tick_shm(self) -> None:
        if self.vel_y >= self.max_vel_y or self.vel_y <= self.min_vel_y:
            # reverse the acceleration when player reaches max/min velocity
            self.acc_y *= -1
        self.vel_y += self.acc_y
        self.y += self.vel_y
    ```

2. Normal Flight

    After the game starts, the bird falls due to gravity until the player taps to make it flap its wings.

    The constants for normal flight are defined as:

    ```python
    self.vel_y = -9  # player's velocity along Y axis
    self.max_vel_y = 10  # max vel along Y, max descend speed
    self.min_vel_y = -8  # min vel along Y, max ascend speed
    self.acc_y = 1  # players downward acceleration

    self.rot = 80  # player's current rotation
    self.vel_rot = -3  # player's rotation speed
    self.rot_min = -90  # player's min rotation angle
    self.rot_max = 20  # player's max rotation angle

    self.flap_acc = -9  # players speed on flapping
    self.flapped = False  # True when player flaps
    ```

    When no input is given, the bird falls due to gravity and rotates downwards:

    ```python
    def tick_normal(self) -> None:
        if self.vel_y < self.max_vel_y and not self.flapped:
            # when player is not flapping, update velocity (falling)
            self.vel_y += self.acc_y
        if self.flapped:
            # flap only once
            self.flapped = False

        # change player's y position and rotation
        self.y = clamp(self.y + self.vel_y, self.min_y, self.max_y)
        self.rotate()
    ```

    When the player taps, the bird flaps its wings and its velocity in the y-axis is updated:

    ```python
    def flap(self) -> None:
        if self.y > self.min_y:
            self.vel_y = self.flap_acc
            self.flapped = True
            self.rot = 80
            self.config.sounds.wing.play()
    ```

3. Crash

    When the bird hits a pipe or the ground, it falls down.

    The constants for crashing are defined as:

    ```python
    self.acc_y = 2
    self.vel_y = 7
    self.max_vel_y = 15
    self.vel_rot = -8
    ```

    When hitting a pipe, the bird will also rotate downwards:

    ```python
    def tick_crash(self) -> None:
        if self.min_y <= self.y <= self.max_y:
            self.y = clamp(self.y + self.vel_y, self.min_y, self.max_y)
            # rotate only when it's a pipe crash and bird is still falling
            if self.crash_entity != "floor":
                self.rotate()

        # player velocity change
        if self.vel_y < self.max_vel_y:
            self.vel_y += self.acc_y
    ```