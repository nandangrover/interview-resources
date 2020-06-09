# CSS Questions and explanation:

### 1. Explain the CSS “box model” and the layout components that it consists of.
<p align="center">
  <img  alt="CSS box model" height="500px" width="500px" src="https://www.w3.org/TR/CSS2/images/boxdim.png">
</p>

The CSS box model is a rectangular layout paradigm for HTML elements that consists of the following:

- Content - The content of the box, where text and images appear
- Padding - A transparent area surrounding the content (i.e., the amount of space between the border and the content)
- Border - A border surrounding the padding (if any) and content
- Margin - A transparent area surrounding the border (i.e., the amount of space between the border and any neighboring elements)

Each of these properties can be specified independently for each side of the element (i.e., top, right, bottom, left) or fewer values can be specified to apply to multiple sides. For example:
```css
/* top   right  bottom left  */
padding: 25px  50px   75px   100px;

/* same padding on all 4 sides: */
padding: 25px;

/* top/bottom padding 25px; right/left padding 50px */
padding: 25px 50px;

/* top padding 25px; right/left padding 50px; bottom padding 75px */
padding: 25px 50px 75px;

/* all margins set to 25px */
body { margin: 25px }

/* top & bottom = 20px, right & left = 30px */
body { margin: 20px 30px }

/* top=10px, right=20px, bottom=30px, left=20px */
body { margin: 10px 20px 30px }
```
