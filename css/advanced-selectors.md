# Advanced Selectors

## Direct Child

```css
div > p {
    color: red;
}
```

Selector above will set all p tags that are direct children of a div to color: red

## Sibling

```css
div + p {
    color: blue;
}
```

Selector above will set all p tags that are directly after a div tag to color: red

## By Attribute

```css
a[target='_blank'] {
    color: teal;
}
```

Selectors above will select all a tags that has a target attribute equal to \_blank and set their color to teal

## Nth-child pseudo selectors

```css
/* Select first child of li */
li:first-child {
    background: blue;
}

/* Select last child of li */
li:last-child {
    background: red;
}

/* Select 3rd child of li */
li:nth-child(3) {
    background: purple;
}

/* Every 3 */
li:nth-child(3n + 0) {
    background: orange;
}

/* Every odd */
li:nth-child(odd) {
    background: yellow;
}

/* Every even */
li:nth-child(even) {
    background: white;
}
```

## Before and After

### Before Example

```css
/* Insert an '*' with the specificed styling right after elements with the is-required class */
.is-required:after {
    content: '*';
    color: red;
    padding-left: 2px;
}
```

The after pseudoselector allows you to insert things directly after a specific element. The content value
is what determines what gets 'injected' into the html. While I say 'injected' it doesn't actually show in the html
at all, can even check with inspect element.

&nbsp;

### After Example

```css
/* Insert an overlay over an image without affecting the text or other children */
header:before {
    content: '';
    background: url('PATH/TO/IMAGE') no-repeat center center/cover;
    opacity: 0.4;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
}
```

Above is an example of a good use case for the before pseudo selector. The example above is setting a background image on
a component and is adding a backdrop on it in order to make the text that will be displayed readable. Without using the before
selector the opacity will also affect the other children and not just the image. **The content property must always be set for
before and after pseudo selectors even if empty**. To make sure the background image is behind the other children you must set
the z-index to be lower than them. The default z-index is 0 so we can easily just set it to -1.
