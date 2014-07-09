#CSS Guidelines when you start a project
## Filetype
All projects MUST use `filename.sass` files as standard.

## Folder Structure:
```
assets/
  stylesheets/
    layouts/
      _header.sass
      _footer.sass
    standards/
      classes.sass
      colors.sass
      fonts.sass
      mixins.sass
      sizes.sass
      tags.sass
    projectname/
    application.sass
```

### About application.sass
These file will contain only the sass imports, see the example below:
```sass
@import standards/colors
@import standards/fonts
@import layouts/header
@import layouts/footer
```

### About colors.sass
Then you have to declare the color intensity:
```sass
$intensity-color-name: #000000
```
The intensities are: `light`, `medium` and `dark`. You can also combine it to each other to produce more colors e.g.: `light-medium`, `medium-dark`.
Then you have to declare a variable related to some object to use it in your project.
```sass
$my-element: $intensity-color-name
```

### About fonts.sass
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

Once this is created you can call it in a external `.sass` file:
```sass
body
  font-family: $museo_sans_100
```

### About tags.sass
It should include all the override and general style like: body, a, div, p, h1, h2, h3, etc. If you are going to create a particular style for, lets say, forms, you should create a `forms.sass` file instead.

### About mixins.sass
These are the basics mixins used in a project:

#### border-radius
```sass
=border-radius($radius)
  -webkit-border-radius: $radius
  -moz-border-radius: $radius
  -ms-border-radius: $radius
  border-radius: $radius
```

The usage
```sass
.classname
  +border-radius(3px)
```

#### transition
```sass
=transition($transition-property, $transition-time, $method)
  -webkit-transition: $transition-property $transition-time $method
  -moz-transition: $transition-property $transition-time $method
  -ms-transition: $transition-property $transition-time $method
  -o-transition: $transition-property $transition-time $method
  transition: $transition-property $transition-time $method
```

The usage
```sass
.classname
    +transition(all, 0.2s, ease-in-out)
```
