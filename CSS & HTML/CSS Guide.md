# CSS Guide - Codeland

## Code Style

Allways uses SASS syntax.

### 1. General principles

- Don't try to prematurely optimize your code; keep it readable and understandable.
- All code in any code-base should look like a single person typed it, even when many people are contributing to it.
- Strictly enforce the agreed-upon style.
If in doubt when deciding upon a style use existing, common patterns.

### 2. Whitespace

>> Only one style should exist across the entire source of your code-base. Always be consistent in your use of whitespace. Use whitespace to improve readability.

- Always use 2 spaces for indentation.

### 3. Comments

Well commented code is extremely important. Take time to describe components, how they work, their limitations, and the way they are constructed. Don't leave others in the team guessing as to the purpose of uncommon or non-obvious code.

Comment style should be simple and consistent within a single code base.

- Place comments on a new line above their subject.
- Keep line-length to a sensible maximum, e.g., 80 columns.
- Make liberal use of comments to break CSS code into discrete sections.
- Use "sentence case" comments and consistent text indentation.
- Tip: configure your editor to provide you with shortcuts to output agreed-upon comment patterns.

Example:
```css
/* ==========================================================================
   Section comment block
   ========================================================================== */

/* Sub-section comment block
   ========================================================================== */

/**
 * Short description using Doxygen-style comment format
 *
 * The first sentence of the long description starts here and continues on this
 * line for a while finally concluding here at the end of this paragraph.
 *
 * The long description is ideal for more detailed explanations and
 * documentation. It can include example HTML, URLs, or any other information
 * that is deemed necessary or useful.
 *
 * @tag This is a tag named 'tag'
 *
 * TODO: This is a todo statement that describes an atomic task to be completed
 *   at a later date. It wraps after 80 characters and following lines are
 *   indented by 2 spaces.
 */

/* Basic comment */
```
### 4. Format

The chosen code format must ensure that code is: easy to read; easy to clearly comment; minimizes the chance of accidentally introducing errors; and results in useful diffs and blames.

- Use one discrete selector per line in multi-selector rulesets.
- Include one declaration per line in a declaration block.
- Use one level of indentation for each declaration.
- Use lowercase and shorthand hex values, e.g., #aaa.
- Use double quotes, e.g., content: "".
- Quote attribute values in selectors, e.g., input[type="checkbox"].
- Where allowed, avoid specifying units for zero-values, e.g., margin: 0.

Separate each ruleset by a blank line.
```css
.selector-1,
.selector-2,
.selector-3[type="text"] {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
    display: block;
    font-family: helvetica, arial, sans-serif;
    color: #333;
    background: #fff;
    background: linear-gradient(#fff, rgba(0, 0, 0, 0.8));
}

.selector-a,
.selector-b {
    padding: 10px;
}
```

#### Declaration order

Alphabetical order

```sass
.class
  background: $some-color
  text-align: center
```

#### Exceptions and slight deviations

Large blocks of single declarations can use a slightly different, single-line format. In this case, a space should be included after the opening brace and before the closing brace.
```css
.selector-1 { width: 10%; }
.selector-2 { width: 20%; }
.selector-3 { width: 30%; }
```

Long, comma-separated property values - such as collections of gradients or shadows - can be arranged across multiple lines in an effort to improve readability and produce more useful diffs. There are various formats that could be used; one example is shown below.

```css
.selector {
    background-image:
        linear-gradient(#fff, #ccc),
        linear-gradient(#f3c, #4ec);
    box-shadow:
        1px 1px 1px #000,
        2px 2px 1px 1px #ccc inset;
}
```

## 6. Standards folder structure

```
assets/
  |--stylesheets/
  |----application.sass
  |----base/
  |--------_resets.sass
  |--------_typographs.sass
  |--------_colors.sass
  |--------_sizes.sass
  |--------_utilities.sass
  |----atoms/
  |--------_inputs.sass
  |--------_links.sass
  |--------_labels.sass
  |--------_tables.sass
  |----molecules/
  |--------_form.sass
  |--------_menu.sass
  |--------_list.sass
  |--------_search-form.sass
  |--------_modal.sass
  |----organisms/
  |--------header.sass
  |--------footer.sass
  |--------aside.sass
```
We're using Atomic Design by Brad Frost, for more information please read her blog post about it [Atomic Design](http://bradfrostweb.com/blog/post/atomic-web-design/)

### Abouse some files in our folder structure:

- `application.sass`
  This file contains only imports from other folders to be compiled by rails.

  eg:

  ```sass
    import 'organisms/**'
  ```
For imports we're using [sass-globbing](https://github.com/chriseppstein/sass-globbing)

## Tools

- [Bourbon](http://bourbon.io)
- [Neat](http://neat.bourbon.io)
- [Bitters](http://bitters.bourbon.io)
