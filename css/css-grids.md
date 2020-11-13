# CSS Grid

## Flexbox vs Grid

Biggest difference between the two is that flexbox is only useful for one dimensional positioning, either row or column
while css grid can be two dimensional, spanning both column and row.

Use flexbox for layouts that will be one dimensional such as navbars, menu items, etc. Use css grid for out elements
and grid like layouts

## Grid Columns

```css
.grid {
    display: grid;
    /* set width of each column of n columns */
    grid-template-columns: 200px 200px 200px 200px;

    /* auto will fill the remaining space within the container */
    grid-template-columns: 200px auto 200px;

    /* equivalent to auto auto auto */
    grid-template-columns: repeat(3, auto);

    /* most commonly used unit for grid columns, works similar to auto but is weighted */
    grid-template-columns: 1fr 2fr 1fr;

    /* sets the gap between grid columns */
    grid-gap: 1rem;
}
```

## Grid Rows

```css
.grid {
    display: grid;

    /* sets the width of kth row ascending: this example sets row 1 to 1fr row 2 to 2fr and row 3 to 3fr
    the remaining rows are implicitly set*/
    grid-template-rows: 1fr 2fr 3fr;

    /* sets the rest of the rows that weren't already set previously to the specified amount */
    grid-auto-rows: 3fr;
}
```

## Span items across multiple rows and/or columns

```css
.grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
}
.item {
    text-align: center;
    padding: 3rem;
    border: #ccc 1px solid;
    background: #f4f4f4;
    font-weight: bold;
    font-size: 2rem;
    margin: 0.3rem;
}

.item:first-child {
    /* the starting position of the specific item in the grid columns*/
    grid-column-start: 1;

    /* the end position of the specific item in the grid columns, exclusive so to span 2 columns must set to start + 2*/
    grid-column-end: 3;

    /* the starting row */
    grid-row-start: 1;

    /* the ending row */
    grid-row-end: 3;

    /* shortcut of above*/
    grid-column: 1 / span2;
    grid-column: 1 / span2;
}
```
