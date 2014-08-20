#CSS Guidelines when you start a project

## Filetype
All projects ***MUST*** use `.sass` syntax.

## Folder Structure:
```
assets/
  stylesheets/
    layouts/
      _header.sass
      _footer.sass
    base/
      base-main.sass
      _fonts.sass
      _mixins.sass
      _variables.sass
      _colors.sass
      _sizes.sass
      _reset.sass
    utility
      base-utility.sass
      _classes.sass
      _tags.sass
    modules/
      base-modules.sass
      _dropdown.sass
      _popup.sass
    projectname/
    application.sass
```

The `modules` folder can be some other folders inside if it's necessary.
```
modules/
  dropdown/
  modals/
```

### About application.sass

These file will contain only the sass imports, see the example below:
```sass
@import base/main
@import modules/main
@import utility/main
```

### About _fonts.sass
#### Importing a font-family from Google Fonts

The name of the mixin must be related to font family name and weight. It must be all lowercase:
```sass
@import url(http://fonts.googleapis.com/css?family=Raleway:100,300)

=raleway-100
  font-family: 'Raleway', Helvetica, Arial, Verdana, sans-serif
  font-weight: 100

=raleway-300
  font-family: 'Raleway', Helvetica, Arial, Verdana, sans-serif
  font-weight: 300
```

Then you have to call it in a external `.sass` file:
```sass
.classname
  +raleway-300
```

#### Importing a font-family from assets/fonts
Fontfile should be place inside fonts folder in assets:
```
assets/
  fonts/
    fontname.ttf
  images/
  javascripts/
  stylesheets/
```

Then you call it inside `fonts.sass` and create a variable to use it in external `.sass` files:
```sass
@font-face
  font-family: 'Museo Sans 100'
  src: font-url("Museo_Sans_100.ttf") unquote("format('truetype')")
  font-weight: normal
  font-style: normal

$museo-sans-100: 'Museo Sans 100'
```
Then you have to create elements variables:
```sass
$base-text: $museo-sans-100
```
Once this is created you can call it in a external `.sass` file:
```sass
body
  font-family: $base-text
```
### About utilities

If you need some style in your element like float: left and you know it's will be useful later you can make an classe utility with an u- prefix, eg:

```sass
.u-text-bold
  font-weight: bold

.u-profile-name
  text-tranform: captilize
  font-size: 1.6em
```

### About mixin
For mixins we're using [Bourbon](http://bourbon.io), this tool provide a lot of mixins.

If you need to create your own mixin you can follow this:

- Separate the name of your mixin with an hifen.
- Be consistent.
- Do something should be reusable.

### About _colors.sass

Then you have to declare the color intensity:
```sass
$intensity-color-name: #000000
```
The intensities are: `light`, `medium` and `dark`. You can also combine it to each other to produce more colors e.g.: `light-medium`, `medium-dark`.
Then you have to declare a variable related to some object to use it in your project.
```sass
$my-element: $intensity-color-name
```

You can use SASS core functions to define the intensity of your colors:

`darken($color, $amount)`[Docs](http://sass-lang.com/documentation/Sass/Script/Functions.html#darken-instance_method)  
`lighten($color, $amount)`[Docs](http://sass-lang.com/documentation/Sass/Script/Functions.html#lighten-instance_method)

### Naming conventions

All strings in classes are delimited with a hyphen (-), like so:

```sass
.page-head

.sub-content
```

#### BEM-like Naming

For larger, more interrelated pieces of UI that require a number of classes, we use a BEM-like naming convention.

BEM, meaning Block, Element, Modifier, is a front-end methodology coined by developers working at Yandex. Whilst BEM is a complete methodology, here we are only concerned with its naming convention. Further, the naming convention here only is BEM-like; the principles are exactly the same, but the actual syntax differs slightly.

BEM splits components’ classes into three groups:

- Block: The sole root of the component.
- Element: A component part of the Block.
- Modifier: A variant or extension of the Block.
- To take an analogy (note, not an example):

To take an analogy (note, not an example):

```sass
.person {}
.person__head {}
.person--tall {}
```
Elements are delimited with two (2) underscores (__), and Modifiers are delimited by two (2) hyphens (--).

Here we can see that .person {} is the Block; it is the sole root of a discrete entity. .person__head {} is an Element; it is a smaller part of the .person {} Block. Finally, .person--tall {} is a Modifier; it is a specific variant of the .person {} Block.

#### Class state

When you need some class to represents a state of an element you need to use the `is-` prefix, like so:

```sass
  .is-hidden
    display: none
```

### JS hooks.

Never use the same class to style and javascript hooks, when you need to hook some element or something like that you can use `js-` prefix in your class name.

#HTML Guidelines when you start a project

Coming soon!
