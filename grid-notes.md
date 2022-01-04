# Grid-Loyout Notes

## Grid fundamentals

Setup your rows and frames using grid-template. If you want to use fr units for ROW, set a height.

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: repeat(2, 300px);
  grid-gap: 20px;
}

.container2 {
  display: grid;
  grid-template: repeat(4, 100px) / repeat(3, 1fr 2fr);
  /* grid-template: ROW / COLUMN */
  grid-gap: 20px;
}
```

## Grid implicit and explicit tracks

If you plan to dynamically add in rows, you can use grid-auto-row, and set a pattern for the added rows. These are not explicitly defined rows, they are implicitly defined. As you continue to add rows, they will follow the implicit row definition and ignore the explicit. This works the same for columns.

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 200px 400px;
  grid-template-rows: 100px 100px;
  grid-auto-rows: 50px 500px;
}
```

## Fractional sizes, auto sizes

Sizing with percentages will break because gaps are not factored in. The fr unit takes the free space and divies it up amongs your columns. If you have 1fr 1fr 1fr for your 3 column widths, you will have 3 equal columns spanning the entire width of the grid. They would each be 1/3 of the whole minus gaps. These are susceptible to breaking/being overridden by large images or long strings without white space.

Look at the last line of code below. There are 6 frames total. The first row will be 1/6 the total height, the second row 2/6 or 1/3 the total height and the third row 3/6 or 1/2 the total height. 3 rows, each with a different fraction of the total.

```css
.container {
  display: grid;
  grid-gap: 20px;
  height: 600px;
  border: 10px solid var(--yellow);
  grid-template-columns: 2fr repeat(5, 1fr);
  grid-template-rows: 1fr 2fr 3fr;
}
```

## Placing Items in your grid

In order to change the natural placement of an item in your grid, use grid-column. You can define where it starts, ends or how far it spans. span 2 / 3 means it will be 2 columns wide and ends at the 3rd divider. 1 / -1 means it will span from beginning to end.
If there are not enough columns to accomodate, it will be pushed to a new row.

```css
.item4 {
  grid-column: 3 / span 2;
  grid-row: 1 / 5;
}
```

```css
.item5 {
  grid-column: span 2;
  grid-row: span 2;
}
```

## Auto-fill, auto-fit

Use these with the repeat function. Auto-fill and auto-fit will allocate a number of rows based on your screen width and column size. This makes the number of columns responsive.

The difference between fill and fit appears when there are not enough items to fill the screen. After each item has a column, "fill" will create empty columns using the col width. While "fit" will stop creating columns. With "fill", you can move your few items into any of the "empty" columns, but with "fit" they are confined to the columns created to hold them.

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: repeat(auto-fill, 150px);
}
```

## Minmax and fit-content

You can apply minmax instead of a specific number for these column widths. If you'd like your columns to stretch accross the screen, use minmax(150px, 1fr) for the width and auto-fit. They will be at least 150px wide and at most they will divide the whole screen by fractions.

This may help you avoid media queries. When the columns don't have room for their min, they will wrap.

```css
grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
```

fit-content is available if you only want to "clamp" your column to a max value.

```css
grid-template-columns: fit-content(100px) 150px 150px 150px;
```

## Areas

Areas are best used to reactively change your entire layout. You designate the space on your page for your header, content, sidebar, footer. As you resize, you can restructure these areas in an easy to conceptualize style.

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 1fr 10fr 1fr;
  grid-template-rows: 150px 150px 100px;
  grid-template-areas:
    "sidebar-1  content     sidebar-2"
    "sidebar-1  content     sidebar-2"
    "footer     footer      footer";
}
.footer {
  grid-area: footer;
}
.item1 {
  grid-area: sidebar-1;
}
.item2 {
  grid-area: content;
}
.item3 {
  grid-area: sidebar-2;
}

@media (max-width: 700px) {
  .container {
    grid-template-rows: auto 2fr 2fr 1fr;
    grid-template-areas:
      "content      content     content"
      "sidebar-1    sidebar-1   sidebar-1"
      "sidebar-2    sidebar-2   sidebar-2"
      "footer       footer      footer";
  }
}
```

You can set your conent to just span one column or row in an area or even multiple areas.

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-areas:
    "A A A A B B B B"
    "A A A A B B B B"
    "A A A A B B B B"
    "A A A A B B B B";
}
.item1 {
  grid-column: A-start / B-end;
}
.item2 {
  grid-row: A-start / A-end;
}
```

## Naming track lines

Tacks, columns and rows, have seperator lines between them. These seperator lines are what you reference when you want your content to extend through the first four columns.

```css
.item1 {
  grid-column: 1 / 5;
}
```

For clarity, these lines can be named.

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns:
    [site-left] 1fr
    [content-start] 500px [content-end]
    1fr [site-right];

  grid-template-rows: [content-top] repeat(10, auto) [content-bottom];
}
.item3 {
  background: slateblue;
  grid-column: content-start;
  grid-row: content-top / content-bottom;
}
```

These lines can also be given multiple names.

```css
.container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns:
    [site-left sidebar-start] 1fr
    [sidebar-end content-start] 500px [content-end site-right];
}
```

## Image Gallery Process

### Goal

A website that displays images of varying sizes together with few to no gaps AND can be resized responsively.

### Strategies

- Basic HTML for popup display format
  - .overlay>.overlay-inner>button.close+img
- Script to add HTML for each image in overlay
  - .item>img+.item\_\_overlay>button(view)
  - map random numbers to the v and h classes
    - the v and h class control row and column span
  - add event listeners to each image's view button

## Flexbox VS Grid

Grid can do everything FB can.
FB can animate better.
Grid is more consistent across browsers.
