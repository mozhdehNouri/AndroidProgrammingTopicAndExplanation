Material and Material 2 create base on standard that's define for android.
In material 3 known as material You you can have customization theme
and ui by your needs.
Jetpack Compose does indeed have support for Material Design through
the`androidx.compose.material`package, which provides components that
implement the Material Design guidelines. This includes Material 3
support, also known as Material You, which offers more personalization and
dynamic color theming capabilities compared to Material 2. However, it’s also important to note that Jetpack Compose is designed to be unopinionated about the UI patterns you use. This means that while it provides out-of-the-box support for Material Design, it does not force you to use it. You have the flexibility to use the provided Material components, extend them, or build entirely custom designs if that’s what your applications require.
In Jetpack Compose, themes and designs are typically handled by the`Theme.kt`file, where you define your colors, typography, shapes, and overall theme settings. Even if you’re using Material components, you can still customize them extensively.

**Color rule in material 3:**

**Primary**
Use primary roles for the most prominent components across the UI, such as
the FAB, high-emphasis buttons, and active states.

- **Primary**– High-emphasis fills, texts, and icons against surface
- **On primary**– Text and icons against primary
- **Primary container**– Standout fill color against surface, for key
  components like FAB
- **On primary container**– Text and icons against primary container

**Secondary**
Use secondary roles for less prominent components in the UI such as filter
chips.

- **Secondary**– Less prominent fills, text, and icons against surface
- **On secondary**– Text and icons against secondary
- **Secondary container**– Less prominent fill color against surface, for
  recessive components like tonal buttons
- **On secondary container**– Text and icons against secondary container

**Tertiary**
Use tertiary roles for contrasting accents that balance primary and
secondary colors or bring heightened attention to an element such as an
input field.

- **Tertiary**– Complementary fills, text, and icons against surface
- **On tertiary**– Text and icons against tertiary
- **Tertiary container**– Complementary container color against surface,
  for components like input fields
- **On tertiary container**– Text and icons against tertiary container

**Surface**
Use surface roles for more neutral backgrounds, and container colors for
components like cards, sheets, and dialogs.

- **Surface**– Default color for backgrounds
- **On surface**– Text and icons against any surface color
- **On surface variant**– Lower-emphasis color for text and icons against
  any surface color

**Custom System design:**
Now in some case, we can see we don't use material in compose project and
create theme and components manually instead of using the ready component
which provided by the material.
