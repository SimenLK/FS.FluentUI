# FS.FluentUI

### Microsoft's FluentUI V9 (React) made for F#

[![NuGet version (FS.FluentUI)](https://img.shields.io/nuget/v/FS.FluentUI.svg?style=flat-square)](https://www.nuget.org/packages/FS.FluentUI/)

### Some components/ functions are incomplete or still considered `Preview Components` and/or `unstable` by Microsoft. Please feel free to send PR's in order to get this library as close to 100% as possible!

## FS.FluentUI.V8toV9
I've created FS.FluentUI.V8toV9 in order to use Stack, StackItem, and createBrandVariants (from V8 theme). See example of a Stack component usage below.

#### IMPORTANT: Place a `Fui.fluentProvider` at the root of your app and pass the theme in as a prop. You can use pre-determined themes, or create your own `BrandVariants` or `Tokens` to use for your theme. See `src/TestGrounds/src/Main.fs` for specific examples. `BrandVariants` can be generated and copied over from Microsoft's [Theme Designer](https://react.fluentui.dev/?path=/docs/themedesigner--page). Simply pick your "Key color value", choose your theme, click `Export`, and convert the TS `BrandVariants` into something more F#.

```fsharp
open Feliz
open Browser.Dom
open FS.FluentUI
open FS.FluentUI.V8toV9

[<ReactComponent>]
let ToggleButtons () =
    let isChecked1, setIsChecked1 = React.useState true
    let isChecked2, setIsChecked2 = React.useState false
    Fui.stack [
        stack.horizontal true
        stack.tokens [ stack.tokens.childrenGap 16 ]
        stack.children [
            Fui.toggleButton [
                toggleButton.icon (Fui.icon.checkbox1Filled [])
                toggleButton.checked' isChecked1
                toggleButton.onClick (fun _ -> setIsChecked1 (isChecked1 |> not))
                toggleButton.text "Checked state"
            ]
            Fui.toggleButton [
                toggleButton.appearance.primary
                toggleButton.checked' isChecked2
                toggleButton.onClick (fun _ -> setIsChecked2 (isChecked2 |> not))
                toggleButton.text "Unchecked state"
            ]
            Fui.toggleButton [
                toggleButton.shape.circular
                toggleButton.disabledFocusable true
                toggleButton.text "Disabled focusable"
            ]
        ]
    ]

let root = ReactDOM.createRoot (document.getElementById "feliz-app")
root.render (
    Fui.fluentProvider [
        fluentProvider.theme.webDarkTheme
        fluentProvider.children [
            ToggleButtons ()
        ]
    ]
)
```

## Components / Hooks
If you don't see a component/ hook in this list.... just wait longer!

#### Please see ```src/FS.FluentUI.TestGrounds/src``` for examples of every component

| Complete Components| Complete Hooks / Functions | Has TODO's or are unstable          |  Upcoming         |
| ------------- | -------------                   | -------------                        |    -------------  |
| Accordion     | useArrowNavigationGroup         |  Toast (Complete with some TODO's)  |    TimePicker     |
| Avatar        |                useId            |  Table  (Use DataGrid for now)      |     ColorPicker    |
| AvatarGroup   |           useFocusableGroup     |  useToastController  (Complete with some TODO's) |   SwatchColorPicker  |
| Badge         |   partitionAvatarGroupItems     |       Drawer (Preview)             |     Rating           |
| CounterBadge  |           useOverflowMenu       |   useHeadlessFlatTree_unstable       |   BasicList         |
| PresenceBadge |      useIsOverflowItemVisible   |     Alert (Preview)                  |   PeoplePicker       |
| Button        |   createTableColumn             |      useTableFeatures                |   TeachingCallout |
| CompoundButton|  useStaticVirtualizerMeasure    |      InfoLabel (Preview)             |      Coachmark     |
| MenuButton    | useModalAttributes              |  VirtualizerScrollView (Preview)      |     MessageBar       |
| SplitButton   |       useObservedElement        |  InteractionTag (Preview)         |                          |
| ToggleButton  |           useFocusFinders       |        TagGroup (Preview)             |                         |
| Card          |         useOverflowCount        |        Tag (Preview)             |                         |
| CardFooter    |        makeResetStyles          |         Breadcrumb (Preview)        |                        |
| CardHeader    |      makeStyles                 |         Searchbox (Preview)         |                       |
| CardPreview   |      bundleIcon                 |VirtualizerScrollViewDynamic (Preview)|                      |
| Checkbox      |                                 |    Virtualizer (Preview)          |                     |
| Combobox      |                                 |                                    |                        |
| DataGrid      |                                  |                                    |                          |
| Dialog        |                                 |                                    |
| Divider       |                                 |                                     |
| Divider       |                                 |
| Dropdown      |
| Tablist       |
| Field|
| FluentProvider|
| Image|
| Input|
| Label|
| Link|
| Menu |
| Overflow|
| Persona|
| Popover|
| Portal |
| ProgressBar|
| Radio |
| RadioGroup|
| Select|
| Skeleton|
| Slider|
| SpinButton|
| Switch|
| Text |
| Textarea|
| Toolbar|
| Tree|
| Tooltip|
| DatePicker (w/ one small TODO) |

I have done my best so far to keep usage of these similar to Typescript usage in the docs. See docs for more usage examples:
[Microsoft Docs](https://react.fluentui.dev/?path=/docs/concepts-introduction--page)

## Styling

Fluent UI React v9 provides design `tokens` for consistent theming.
Microsoft doesn't suggest using exact colors in Fluent UI code.

❌ `style.color.darkBlue`

✅ `style.color Theme.tokens.colorBrandForeground1`

Although `shorthands` is used extensively throughout the Microsoft [documentation](https://react.fluentui.dev/?path=/docs/concepts-introduction--page), it is not necessary here when using `makeStyles` or `makeResetStyles`; I'm expanding any shorthand styling properties under-the-hood.

`makeStyles` is used to define style permutations in components and is used for style overrides. It returns a React hook that should be called inside a component:

When using single numeric values in `makeStyles` or `makeResetStyles`, specify the value's `length`.

❌ `style.minWidth 200`

✅ `style.minWidth (length.px 200)`

```fsharp
open Feliz
open FS.FluentUI

let tokens = Theme.tokens

type Styles = {
    accordion: string
    useArrowNavigationGroup: string
    tooltip: string
    icon: string
    compoundButton: string
}

let useStyles: unit -> Styles = Fui.makeStyles [
    "accordion", [
        style.color.red
        style.backgroundColor.darkGray
    ]
    "useArrowNavigationGroup", [
        style.display.flex
        style.columnGap 15
    ]
    "tooltip", [
        style.backgroundColor tokens.colorBrandBackground
        style.color tokens.colorNeutralForegroundInverted
    ]
    "icon", [
        style.color.green
    ]
    "compoundButton", [
        style.width (length.px 150)
    ]
]

[ReactComponent]
let ToolTip () =
    let styles = useStyles()

    Fui.tooltip [
        tooltip.className styles.tooltip
    ...
```
The `makeResetStyles` API works similarly to `makeStyles` and is used to generate styles as a single monolithic class to avoid the "CSS rules explosion" problem.

```fsharp
open Feliz
open FS.FluentUI
open FS.FluentUI.V8toV9

let dialogClass = Fui.makeResetStyles [
    style.position.fixedRelativeToWindow
    style.top (length.px 200)
    style.backgroundColor tokens.colorBrandBackground2
    style.margin (length.auto)
    style.borderStyle.none
    style.padding (10, 50)
    style.boxShadow (5, 5, tokens.shadow16)
    style.width (length.px 450)
    style.height (length.px 200)
    style.zIndex 100
]

[<ReactComponent>]
let Dialog () =
    let isOpen, setIsOpen = React.useState false
    let dialogClass = dialogClass()

    Fui.stack [
        stack.className dialogClass
    ...
```

## BundleIcon
You can use the `bundleIcon` method to bundle a filled and unfilled version of an icon which will combine them into one icon.

❌ `let AddIcon = Fui.bundleIcon(Fui.icon.addCircleFilled [], Fui.icon.addCircleRegular [])`

✅ `let AddIcon = Fui.bundleIcon(bundleIcon.addCircleFilled, bundleIcon.addCircleRegular)`

In the following example, the icons are toggled depending on if the button `isChecked`. Some components will toggle between the two icons on-hover, on-click, or not at all. Please see Microsoft's documentation for more examples.

```fsharp
[<ReactComponent>]
let ToggleButton () =
    let isChecked, setIsChecked = React.useState true

    let CheckedIcon = Fui.bundleIcon(bundleIcon.checkbox1Filled, bundleIcon.checkbox1Regular)

    Fui.toggleButton [
        toggleButton.icon (CheckedIcon [ icon.style [ style.color.green ] ])
        toggleButton.checked' isChecked
        toggleButton.onClick (fun _ -> setIsChecked (isChecked |> not))
        toggleButton.text "Button Text"
    ]
```

## Contributing

### Before sending PR's, please test what you've added in the "TestGrounds".
```bash
cd src/FS.FluentUI.TestGrounds
npm install
npm start
```

## Installation

Install into your Client.fsproj project using [Femto](https://github.com/Zaid-Ajaj/Femto) (recommended)
```bash
cd ./{path to Client}

# when using femto as a global CLI tool
femto install FS.FluentUI --version {newest version}

# when using femto as a local CLI tool
dotnet femto install FS.FluentUI --version {newest version}
```
This will install the nuget package and afterwards automatically installs the required npm packages used by this binding.

> Femto will detect whether you are using paket and will install the package using paket into the dependency group of the project

You can install the library manually if you want by first installing the nuget package
```bash
cd ./{path to your project}
dotnet add package FS.FluentUI --version {newest version}
```
