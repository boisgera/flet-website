---
slug: gradients-button-textfield-styles
title: Beautiful gradients, button styles and TextField rounded corners in a new Flet release
author: Feodor Fitsner
author_title: Flet founder and developer
author_url: https://github.com/FeodorFitsner
author_image_url: https://avatars0.githubusercontent.com/u/5041459?s=400&v=4
tags: [release]
---

We've just released [Flet 0.1.46](https://pypi.org/project/flet/0.1.46/) adding new exciting features:

* Gradient backgrounds in Container
* Extensive styling for buttons, TextField and Dropdown controls
* ...and more

## Gradient backgrounds

### Linear gradient

<img src="/img/blog/gradients/linear-gradient.png" className="screenshot-30" />

```python
import math
import flet
from flet import Alignment, Container, LinearGradient, Page, alignment

def main(page: Page):

    page.add(
        Container(
            alignment=alignment.center,
            gradient=LinearGradient(
                begin=alignment.top_left,
                end=Alignment(0.8, 1),
                colors=[
                    "0xff1f005c",
                    "0xff5b0060",
                    "0xff870160",
                    "0xffac255e",
                    "0xffca485c",
                    "0xffe16b5c",
                    "0xfff39060",
                    "0xffffb56b",
                ],
                tile_mode="mirror",
                rotation=math.pi / 3,
            ),
            width=150,
            height=150,
            border_radius=5,
        )
    )

flet.app(target=main)
```

Check [`Container.gradient`](/docs/controls/container#lineargradient) docs for more information about `LinearGradient` properties.

### Radial gradient

<img src="/img/blog/gradients/radial-gradient.png" className="screenshot-30" />

```python
import flet
from flet import Alignment, Container, Page, RadialGradient, alignment

def main(page: Page):

    page.add(
        Container(
            alignment=alignment.center,
            gradient=RadialGradient(
                center=Alignment(0.7, -0.6),
                radius=0.2,
                colors=[
                    "0xFFFFFF00",  # yellow sun
                    "0xFF0099FF",  # blue sky
                ],
                stops=[0.4, 1.0],
            ),
            width=150,
            height=150,
            border_radius=5,
        )
    )

flet.app(target=main)
```

Check [`Container.gradient`](/docs/controls/container#radialgradient) docs for more information about `RadialGradient` properties.

### Sweep gradient

<img src="/img/blog/gradients/sweep-gradient.png" className="screenshot-30" />

```python
import math
import flet
from flet import Container, Page, SweepGradient, alignment

def main(page: Page):

    page.add(
        Container(
            alignment=alignment.center,
            gradient=SweepGradient(
                center=alignment.center,
                start_angle=0.0,
                end_angle=math.pi * 2,
                colors=[
                    "0xFF4285F4",
                    "0xFF34A853",
                    "0xFFFBBC05",
                    "0xFFEA4335",
                    "0xFF4285F4",
                ],
                stops=[0.0, 0.25, 0.5, 0.75, 1.0],
            ),
            width=150,
            height=150,
            border_radius=5,
        )
    )

flet.app(target=main)
```

Check [`Container.gradient`](/docs/controls/container#sweepgradient) docs for more information about `SweepGradient` properties.

## Buttons styling

This Flet release introduces `style` property to all button controls which is an instance of `ButtonStyle` class.
`ButtonStyle` allows controling all visual aspects of a button, such as shape, foreground, background and shadow colors, content padding, border width and radius!

Moreover, each individual style attribute could be configured for a different "Material states" of a button, such as "hovered", "focused", "disabled" and others. For example, you can configure a different shape, background color for a hovered state and configure fallback values for all other states.

Check this "extreme" styling example:

<img src="/img/blog/gradients/styled-button.gif" className="screenshot-30" />

```python
import flet
from flet import ButtonStyle, ElevatedButton, Page, colors
from flet.border import BorderSide
from flet.buttons import RoundedRectangleBorder

def main(page: Page):

    page.add(
        ElevatedButton(
            "Styled button 1",
            style=ButtonStyle(
                color={
                    "hovered": colors.WHITE,
                    "focused": colors.BLUE,
                    "": colors.BLACK,
                },
                bgcolor={"focused": colors.PINK_200, "": colors.YELLOW},
                padding={"hovered": 20},
                overlay_color=colors.TRANSPARENT,
                elevation={"pressed": 0, "": 1},
                animation_duration=500,
                side={
                    "": BorderSide(1, colors.BLUE),
                    "hovered": BorderSide(2, colors.BLUE),
                },
                shape={
                    "hovered": RoundedRectangleBorder(radius=20),
                    "": RoundedRectangleBorder(radius=2),
                },
            ),
        )
    )

flet.app(target=main)
```

Empty string (`""`) state is a fallback style.

Button shape could also be changed with `ButtonStyle.shape` property:

<img src="/img/blog/gradients/button-shapes.png" className="screenshot-30" />

```python
import flet
from flet import ButtonStyle, FilledButton, Page
from flet.buttons import (
    BeveledRectangleBorder,
    CircleBorder,
    CountinuosRectangleBorder,
    RoundedRectangleBorder,
    StadiumBorder,
)

def main(page: Page):
    page.padding = 30
    page.spacing = 30
    page.add(
        FilledButton(
            "Stadium",
            style=ButtonStyle(
                shape=StadiumBorder(),
            ),
        ),
        FilledButton(
            "Rounded rectangle",
            style=ButtonStyle(
                shape=RoundedRectangleBorder(radius=10),
            ),
        ),
        FilledButton(
            "Continuous rectangle",
            style=ButtonStyle(
                shape=CountinuosRectangleBorder(radius=30),
            ),
        ),
        FilledButton(
            "Beveled rectangle",
            style=ButtonStyle(
                shape=BeveledRectangleBorder(radius=10),
            ),
        ),
        FilledButton(
            "Circle",
            style=ButtonStyle(shape=CircleBorder(), padding=30),
        ),
    )

flet.app(target=main)
```

Check [`ElevatedButton.style`](/docs/controls/elevatedbutton#style) property docs for a complete description of `ButtonStyle` class and its properties.

## TextField and Dropdown styling

It is now possible to configure text size, border style and corners radius for normal and focused states of `TextField` and `Dropdown` controls. `TextField` also allows configuring colors for a cursor and selection.

Additionally, the maximum length of entered into `TextField` can now be limited with `max_length` property.

We also introduced `capitalization` property for automatic capitalization of characters as you type them into `TextField`. You can choose from 4 capitalization strategies: `none` (default), `characters`, `words` and `sentences`.

An example of styled `TextField` with `max_length` and `capitalization`:

<img src="/img/blog/gradients/styled-textfield.gif" className="screenshot-50" />

```python
import flet
from flet import Page, TextField, colors

def main(page: Page):
    page.padding = 50
    page.add(
        TextField(
            text_size=30,
            cursor_color=colors.RED,
            selection_color=colors.YELLOW,
            color=colors.PINK,
            bgcolor=colors.BLACK26,
            filled=True,
            focused_color=colors.GREEN,
            focused_bgcolor=colors.CYAN_200,
            border_radius=30,
            border_color=colors.GREEN_800,
            focused_border_color=colors.GREEN_ACCENT_400,
            max_length=20,
            capitalization="characters",
        )
    )

flet.app(target=main)
```

An example of styled `Dropdown` control:

<img src="/img/blog/gradients/styled-dropdown.gif" className="screenshot-50" />

```python
import flet
from flet import Dropdown, Page, colors, dropdown

def main(page: Page):
    page.padding = 50
    page.add(
        Dropdown(
            options=[
                dropdown.Option("a", "Item A"),
                dropdown.Option("b", "Item B"),
                dropdown.Option("c", "Item C"),
            ],
            border_radius=30,
            filled=True,
            border_color=colors.TRANSPARENT,
            bgcolor=colors.BLACK12,
            focused_bgcolor=colors.BLUE_100,
        )
    )

flet.app(target=main)
```

## Other changes

`IconButton` got `selected` state which plays nice with a new `style`.

This is an example of a toggle icon button:

<img src="/img/blog/gradients/toggle-icon-button.gif" className="screenshot-20" />

```python
import flet
from flet import ButtonStyle, IconButton, Page, colors, icons

def main(page: Page):

    def toggle_icon_button(e):
        e.control.selected = not e.control.selected
        e.control.update()

    page.add(
        IconButton(
            icon=icons.BATTERY_1_BAR,
            selected_icon=icons.BATTERY_FULL,
            on_click=toggle_icon_button,
            selected=False,
            style=ButtonStyle(color={"selected": colors.GREEN, "": colors.RED}),
        )
    )

flet.app(target=main)
```

[Give Flet a try](/docs/guides/python/getting-started) and [let us know](https://discord.gg/dzWXP8SHG8) what you think!