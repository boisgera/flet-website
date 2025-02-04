---
title: Animations
sidebar_label: Animations
---

## Implicit animations

With implicit animations, you can animate a control property by setting a target value; whenever that target value changes, the control animates the property from the old value to the new one. Animation produces interpolated values between the old and the new value over the given *duration*. By default, the animation is *linearly* increasing the animation value, however, a *curve* can be applied to the animation which changes the value according to the provided curve. For example, `easeOutCubic` curve increases the animation value quickly at the beginning of the animation and then slows down until the target value is reached:

<video controls>
  <source src="https://flutter.github.io/assets-for-api-docs/assets/animation/curve_ease_out_cubic.mp4"/>
</video>

Each `Control` provides a number of `animate_{something}` properties, described below, to enable implicit animation of its appearance:

* `animate_opacity`
* `animate_rotation`
* `animate_scale`
* `animate_offset`
* `animate_position`
* `animate` (Container)

`animate_*` properties could have one of the following values:

* Instance of `animation.Animation` class - allows configuring the duration (in milliseconds) and the curve of the animation, for example `animate_rotation=animation.Animation(duration=300, curve="bounceOut")`. See [Curves](https://api.flutter.dev/flutter/animation/Curves-class.html) in Flutter docs for possible values. Default is `linear`.
* `int` value - enables animation with specified duration in milliseconds and `linear` curve.
* `bool` value - enables anumation with the duration of 1000 milliseconds and `linear` curve.

### Opacity animation

Setting control's `animate_opacity` to either `True`, number or an instance of `animation.Animation` class (see above) enables implicit animation of [`Control.opacity`](/docs/controls#opacity) property.

<img src="/img/docs/getting-started/animations/animate-opacity.gif" className="screenshot-20" />

```python
import flet
from flet import Container, ElevatedButton, Page

def main(page: Page):

    c = Container(
        width=150,
        height=150,
        bgcolor="blue",
        border_radius=10,
        animate_opacity=300,
    )

    def animate_opacity(e):
        c.opacity = 0 if c.opacity == 1 else 1
        c.update()

    page.add(
        c,
        ElevatedButton(
            "Animate opacity",
            on_click=animate_opacity,
        ),
    )

flet.app(target=main)
```

### Rotation animation

Setting control's `animate_rotation` to either `True`, number or an instance of `animation.Animation` class (see above) enables implicit animation of [`Control.rotate`](/docs/controls#rotate) property.

<img src="/img/docs/getting-started/animations/animate-rotation.gif" className="screenshot-20" />

```python
from math import pi
import flet
from flet import Container, ElevatedButton, Page, alignment, animation, transform

def main(page: Page):

    c = Container(
        width=100,
        height=70,
        bgcolor="blue",
        border_radius=5,
        rotate=transform.Rotate(0, alignment=alignment.center),
        animate_rotation=animation.Animation(duration=300, curve="bounceOut"),
    )

    def animate(e):
        c.rotate.angle += pi / 2
        page.update()

    page.vertical_alignment = "center"
    page.horizontal_alignment = "center"
    page.spacing = 30
    page.add(
        c,
        ElevatedButton("Animate!", on_click=animate),
    )

flet.app(target=main)
```

### Scale animation

Setting control's `animate_scale` to either `True`, number or an instance of `animation.Animation` class (see above) enables implicit animation of [`Control.scale`](/docs/controls#scale) property.

<img src="/img/docs/getting-started/animations/animate-scale.gif" className="screenshot-20" />

```python
import flet
from flet import Container, ElevatedButton, Page, animation
from flet.transform import Scale

def main(page: Page):

    c = Container(
        width=100,
        height=100,
        bgcolor="blue",
        border_radius=5,
        scale=Scale(scale=1),
        animate_scale=animation.Animation(600, "bounceOut"),
    )

    def animate(e):
        # c1.rotate = 1
        c.scale = 2
        page.update()

    page.vertical_alignment = "center"
    page.horizontal_alignment = "center"
    page.spacing = 30
    page.add(
        c,
        ElevatedButton("Animate!", on_click=animate),
    )

flet.app(target=main)
```

### Offset animation

Setting control's `animate_offset` to either `True`, number or an instance of `animation.Animation` class (see above) enables implicit animation of `Control.offset` property.

`offset` property is an instance of `transform.Offset` class which specifies horizontal `x` and vertical `y` offset of a control scaled to control's size. For example, an offset `transform.Offset(-0.25, 0)` will result in a horizontal translation of one quarter the width of the control.

Offset animation is used for various sliding effects:

<img src="/img/docs/getting-started/animations/animate-offset.gif" className="screenshot-20" />

```python
import flet
from flet import Container, ElevatedButton, Page, animation, transform

def main(page: Page):

    c = Container(
        width=150,
        height=150,
        bgcolor="blue",
        border_radius=10,
        offset=transform.Offset(-2, 0),
        animate_offset=animation.Animation(1000),
    )

    def animate(e):
        c.offset = transform.Offset(0, 0)
        c.update()

    page.add(
        c,
        ElevatedButton("Reveal!", on_click=animate),
    )

flet.app(target=main)
```

### Position animation

Setting control's `animate_position` to either `True`, number or an instance of `animation.Animation` class (see above) enables implicit animation of [Control's `left`, `top`, `right` and `bottom` properties](/docs/controls#left).

Please note Control position works inside `Stack` control only.

<img src="/img/docs/getting-started/animations/animate-position.gif" className="screenshot-30" />

```python
import flet
from flet import Container, ElevatedButton, Page, Stack

def main(page: Page):

    c1 = Container(width=50, height=50, bgcolor="red", animate_position=1000)

    c2 = Container(
        width=50, height=50, bgcolor="green", top=60, left=0, animate_position=500
    )

    c3 = Container(
        width=50, height=50, bgcolor="blue", top=120, left=0, animate_position=1000
    )

    def animate_container(e):
        c1.top = 20
        c1.left = 200
        c2.top = 100
        c2.left = 40
        c3.top = 180
        c3.left = 100
        page.update()

    page.add(
        Stack([c1, c2, c3], height=250),
        ElevatedButton("Animate!", on_click=animate_container),
    )

flet.app(target=main)
```

### Animated container

Setting [`Container.animate`](/docs/controls/container#animate) to either `True`, number or an instance of `animation.Animation` class (see above) enables implicit animation of container properties such as size, background color, border style, gradient.

<img src="/img/docs/getting-started/animations/animate-container.gif" className="screenshot-20" />

```python
import flet
from flet import Container, ElevatedButton, Page, animation

def main(page: Page):

    c = Container(
        width=150,
        height=150,
        bgcolor="red",
        animate=animation.Animation(1000, "bounceOut"),
    )

    def animate_container(e):
        c.width = 100 if c.width == 150 else 150
        c.height = 50 if c.height == 150 else 150
        c.bgcolor = "blue" if c.bgcolor == "red" else "red"
        c.update()

    page.add(c, ElevatedButton("Animate container", on_click=animate_container))

flet.app(target=main)
```

### Animated content switcher

[`AnimatedSwitcher`](/docs/controls/animatedswitcher) allows animated transition between a new control and the control previously set on the AnimatedSwitcher as a `content`.

<img src="/img/docs/getting-started/animations/animated-switcher-images.gif" className="screenshot-20" />

```python
import time

import flet
from flet import AnimatedSwitcher, ElevatedButton, Image, Page

def main(page: Page):

    i = Image(src="https://picsum.photos/150/150", width=150, height=150)

    def animate(e):
        sw.content = Image(
            src=f"https://picsum.photos/150/150?{time.time()}", width=150, height=150
        )
        page.update()

    sw = AnimatedSwitcher(
        i,
        transition="scale",
        duration=500,
        reverse_duration=500,
        switch_in_curve="bounceOut",
        switch_out_curve="bounceIn",
    )

    page.add(
        sw,
        ElevatedButton("Animate!", on_click=animate),
    )

flet.app(target=main)
```