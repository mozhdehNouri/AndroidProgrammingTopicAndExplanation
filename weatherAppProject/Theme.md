Material and Material 2 create base on standard thats define for android

and in material 3 known as material You you can have customization theme
and ui by your needs.

Now in some case, we can see we don't use material in compose project and
create theme and components manually instead of using the ready component
which provided by the material.

with this approch we can have a custom design system.

Jetpack Compose does indeed have support for Material Design through
the`androidx.compose.material`package, which provides components that
implement the Material Design guidelines. This includes Material 3
support, also known as Material You, which offers more personalization and
dynamic color theming capabilities compared to Material 2.

However, it’s also important to note that Jetpack Compose is designed to
be unopinionated about the UI patterns you use. This means that while it
provides out-of-the-box support for Material Design, it does not force you
to use it. You have the flexibility to use the provided Material
components, extend them, or build entirely custom designs if that’s what
your applications require.

In Jetpack Compose, themes and designs are typically handled by
the`Theme.kt`file, where you define your colors, typography, shapes, and
overall theme settings. Even if you’re using Material components, you can
still customize them extensively.

If some Google source code or samples are not using Material components or
theming, there could be a few reasons for this:

1. **Demonstration Purposes:**The code might be intended to demonstrate
   how to build a UI from scratch without any Material Design components
   or how to customize things beyond what Material provides.
2. **Custom Design Language:**The app might be intended to use a custom
   design language that does not conform to Material Design guidelines.
3. **Evolution of UI Toolkit:**As Jetpack Compose is still relatively new
   and evolving, some samples might be exploring new design paradigms and
   practices that extend beyond current Material Design.
4. **Preference:**The developers may simply prefer to use custom
   components for their specific use cases. Material Design is a design
   language, not a strict requirement, and with Jetpack Compose, the
   emphasis is on giving developers the power to create the UI as they see
   fit.

If you’re looking at the source code for an app or a sample that doesn’t
use the Material components, consider what the purpose of that code may
be. It may be intended to demonstrate Compose’s capabilities for custom UI
development, or it might be a specific choice by the developers to create
a unique user experience that differentiates itself from the Material
Design language.



----

Impelement Material 3 color role :

https://m3.material.io/styles/color/roles

- **Surface**– A role used for backgrounds and large, low-emphasis areas
  of the screen.
- **Primary, Secondary, Tertiary**– Accent color roles used to emphasize
  or de-emphasize foreground elements.
- **Container**– Roles used as a fill color for foreground elements like
  buttons. They should not be used for text or icons.
- **On**– Roles starting with this term indicate a color for text or icons
  *on top*of its paired parent color. For example,**on primary**is used
  for text and icons against the**primary**fill color.
- **Variant**– Roles ending with this term offer a lower emphasis
  alternative to its non-variant pair. For example,**outline variant**is a
  less emphasized version of the**outline**color.

## Primary

Use primary roles for the most prominent components across the UI, such as
the FAB, high-emphasis buttons, and active states.

- **Primary**– High-emphasis fills, texts, and icons against surface

- **On primary**– Text and icons against primary

- **Primary container**– Standout fill color against surface, for key
  components like FAB

- **On primary container**– Text and icons against primary container

## Secondary

Use secondary roles for less prominent components in the UI such as filter
chips.

There are four secondary roles:

- **Secondary**– Less prominent fills, text, and icons against surface

- **On secondary**– Text and icons against secondary

- **Secondary container**– Less prominent fill color against surface, for
  recessive components like tonal buttons

- **On secondary container**– Text and icons against secondary container

## Tertiary

Use tertiary roles for contrasting accents that balance primary and
secondary colors or bring heightened attention to an element such as an
input field.

There are four tertiary roles:

- **Tertiary**– Complementary fills, text, and icons against surface

- **On tertiary**– Text and icons against tertiary

- **Tertiary container**– Complementary container color against surface,
  for components like input fields

- **On tertiary container**– Text and icons against tertiary container

## Surface

Use surface roles for more neutral backgrounds, and container colors for
components like cards, sheets, and dialogs.

There are three surface roles:

- **Surface**– Default color for backgrounds

- **On surface**– Text and icons against any surface color

- **On surface variant**– Lower-emphasis color for text and icons against
  any surface color
