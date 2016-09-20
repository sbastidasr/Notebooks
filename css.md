# HTML

Declaration of HTML5 ```<!DOCTYPE html>```
Referring to an external style sheet, should be done in the head tag:
``` <link rel="stylesheet" type="text/css" href="mystyle.css">```


# CSS

## BasicStuff
!important overrides any previous styles

## CSS Units
**em**	Relative to the font-size of the element (2em means 2 times the size of the current font)
**ch**	Relative to width of the "0" (zero)
**rem**	Relative to font-size of the root element
**vw**	Relative to 1% of the width of the viewport
**vh**	Relative to 1% of the height of the viewport
**vmin**	Relative to 1% of viewport's* smaller dimension
**vmax**	Relative to 1% of viewport's* larger dimension



##Display

**Inline** Occupy the width of the content. Size cannot be set. Like <b> tags or <span>
**block** Occupies full width of the screen (can be overriden with width property).
**Inline Block* like span but can take size, margin, padding but displays inline. (Add font size 0 to remove space between blocks).
**Flex** TODO



## Position Property

The position property specifies the type of positioning method used for an element. 

**Static** (Default) It is positioned according to the normal flow of the page. No *top, bottom, left, right* properties.

**Relative** "relative to itself". It works as static, but if you DO give it some other positioning attribute with ***trbl*** like, say, ```top: 10px;``` it will shift it's position 10 pixels DOWN from where it would NORMALLY be. Other content will not be adjusted to fit into any gap left by the element. The gap stays where it should have been.

**Fixed** A fixed position element is positioned relative to the viewport. It is taken out of the normal flow -> it doesn't leave a gap in the page where it would normally have been located. Example of this is a fixed navbar. Is set to the window with ***top, right, bottom, left*** properties

**Absolute** An element with ```position: absolute;``` is like fixed, but relative to the nearest relative positioned ancestor (instead of positioned relative to the viewport). They don't leave a gap and will not affect other elements.



## what is this? 

Border Order:** top right bottom left

Padding:inside (cannot take negative values)
Margin:outside
  margin:0 auto;  centers stuff

vertical-align: baseline|length|sub|super|top|text-top|middle|bottom|text-bottom|initial|inherit;



## Sizing
- width
- height
- max-width: will make the browser resize in a better way.

**box-sizing** ```box-sizing:border-box;``` If you add this property the padding, margin and border of that element no longer increase its width. The whole element takes the size of the width.
```css
div{
  width:100px;
  boder: 0 2px;
  padding: 0 10px;
}
```

On the normal way, this element would be 122px, its content 100px. On Border-box it would be 100px, and its content 78px.



##Float 

Positions an element at the top right or left. The elements remain on the page, with the width they need, not full page. And, everything else in the parent wraps around them.


**Clear:** Cleared elements will make a wall after floats. So a <div style=“clear:both;”></div> will make elements below not wrap after the floats. 
(```clear:right``` , left or both specifies which sides of an element where other floating elements are not allowed)

**Clearfix Hack:** 
If an element is taller than the element containing it, and it is floated, it will overflow outside of its container. Like a cleared image will overflow its div. Then we can add ```overflow: auto;``` to the container element to fix this problem:

--------




**z-index: -1;** Controls the position of which elements overlap others.
--

##Display Property
**Block** elements start on a new line and stretches out to the left and right as far as it can. Examples are: div, p and form, and new in HTML5 are header, footer, section, and more.

**Inline** elements do not start on a new line and only take up as much width as necessary like span, a, image. Can be used to make a grid of boxes that fills the browser width and wraps nicely (when the browser is resized). Dont have a height.

**Inline Block** elements are like inline elements but they will take the width and height they need. Can be used to make grid layouts. If something ocuppies more space, it goes to the next line.

**None** will render the page as though the element does not exist. 

```visibility: hidden;``` will hide the element, but the element will still take up the space it would if it was fully visible. The element will be hidden, but still affect the layout:


--

##Overflow
**visible**	The content is not clipped, and it may be rendered outside the content box
**hidden**	The content is clipped - and no scrolling mechanism is provided.
**scroll**	The content is clipped and a scrolling mechanism is provided.
**auto**	Should cause a scrolling mechanism to be provided for overflowing boxes.

------

##Text Properties

###font	
Sets all the font properties in one declaration	```font: 15px arial, sans-serif;```
```font: font-style font-variant font-weight font-size/line-height font-family|caption|icon|menu|message-box|small-caption|status-bar|initial|inherit;```

**letter-spacing**	Increases or decreases the space between characters in a text	1
**line-break**	Specifies how/if to break lines	
**line-height** Sets the line height
**@font-face**	A rule that allows websites to download and use fonts other than the "web-safe" fonts
**font-family**	Specifies the font family for text
**font-style:** normal|italic|oblique
**font-weight:** normal|bold|bolder|lighter|number
**text-align:** left|right|center|justify
**text-transform:** Controls capitalization of text
**font-variant:** normal|small-caps
**text-decoration:** none|underline|overline|line-through

---

#Selectors

##Combinators
There are four different combinators in CSS3:
**descendant selector** (space) => all paragraphs inside a div: ``` div p {}```
**child selector** (>) =>all paragraphs that are immmediate children of div (meaning exactly inside the <div></div>tags) ```div > p {}```
**adjacent sibling selector** (+) The one paragraphs that comes after a div and has the same parent (the ones plaged immediately afterwards) ```div + p {}```
**general sibling selector** (~) all elements that are siblings of a specified element. 
```div ~ p {}```

*	Selects all elements
   *div, p*Selects all <div> elements and all <p> elements


##Pseudo classes: 
used to define a special state of an element. ```selector:pseudo-class {property:value; }```
**a:link**  unvisited link
**a:visited** visited link
**a:hover** mouse over link
**a:active** selected link

input:checked	Selects every checked <input> element
input:disabled	Selects every disabled <input> element.
p:empty	Selects every <p> element that has no children (including text nodes)
input:enabled	Selects every enabled <input> element
p::first-line	Selects the first line of every <p> element
p:nth-child(2)	Selects every <p> element that is the second child of its parent, starting at 1
**:focus** when an input is focused

**first Child:**Match the first child of all p. 
 ``` p:first-child {color: blue;} ```

First i in all Ps:
``` p i:first-child {color: blue;} ```

**::before** Inserts a gif before the h1. ``` h1::before {content: url(smiley.gif); } ```
**::after** pseudo-element can be used to insert some content after the content of an element.
**::selection** pseudo-element matches the portion of an element that is selected by a user.
**::first-letter** pseudo-element is used to add a special style to the first letter of a text.
--

> first children
+ after
  ~ preceded by 


*= attribute contains a value somewhere
^= attribute begins with value 
$= attribute ends with value

~=  attribute is within space separated list
|= attribute is within dash (-) separated list

##CSS attribute selector
The [attribute] selector is used to select elements with a specified attribute.

**a[target='#']{}**    selects all links to #
**[attribute~="value"]{}** selector is used to select elements with an attribute value containing a specified word.[title~="flower"] { } will target flower, summer flower but not my-flower
**[attribute|="value"]** selector is used to select elements with the specified attribute starting with the specified value. [class|="top"] { background: yellow;} classes that begin with top
**[attribute^="value”]** attribute begins with specified value
**[attribute$="value"]** selector is used to select elements whose attribute value ends with a specified value.
**[attribute*="value"]** selector is used to select elements whose attribute value contains a specified value.


*= attribute contains a value somewhere
^= attribute begins with value 
$= attribute ends with value

~=  attribute is within space separated list
|= attribute is within dash (-) separated list

Examples
```
input[type="text"] { }
input[type="button"] {}
```
---

## CSS3 Rounded Corners

### border-radius
Four values: to top-left, top-right, bottom-right, bottom-left.
Three values: top-left, top-right and bottom-left, bottom-right
Two values:top-left amd bottom-right corner, and top-right and bottom-left corner
One value: all four corners are rounded equally

###border-image  &&&&
allows you to specify an image to be used instead of the normal border around an element.
1. The image to use as the border
2. Where to slice the image
3. Define whether the middle sections should be repeated or stretched
-----

##Backgrounds 

###Backgound property
Includes background-color, background-image, background-position, background-size, background-repeat, background-origin, background-clip, and background-attachment.
```
body { 
    background: #00ff00 url("smiley.gif") no-repeat fixed center; 
}
```

**background-position** left top, left center, right bottom, center cente, xpos ypos, % %.

###background-size
property allows you to specify the size of background images. 
1. Can be set to pixels, percentages and so.
2. **Contain** keyword scales the background image to be as large as possible (but both its width and its height must fit inside the content area).There may be some areas of the background which are not covered by the background image.
3. **Cover**  scales the background image so that the content area is completely covered by the background image (both its width and height are equal to or exceed the content area). 
   Note: It can also take multiple values for multiple images.

###background-repeat:
repeat|repeat-x|repeat-y|no-repeat|initial|inherit;

###background-image
Allows multiple backgrounds
    1. background-image: url(img_flwr.gif), url(paper.gif);
       background-position: right bottom, left top;
       background-repeat: no-repeat, repeat;
    2. background: url(img_flwr.gif) right bottom no-repeat, url(paper.gif) left top repeat;

###background-origin 
specifies where the background image starts. It takes three different values:
* border-box - the background image starts from the upper left corner of the border (takes the border and the padding)
  * padding-box - (default) the background image starts from the upper left corner ofthe padding edge(takes the padding space)
* content-box - the background image starts from the upper left corner of the content. (Inside the pedding)

###background-clip 
Takes the same values as background-origin.
background-clip: border-box|padding-box|content-box|initial|inherit;

###background-attachment
Sets whether a background image is fixed or scrolls with the rest of the page
**scroll**	The background scrolls along with the element. This is default
**fixed**	The background is fixed with regard to the viewport
**local**	The background scrolls along with the element's contents

---

##Borders
border-width, border-style, and border-color.
```border: 5px solid red;```
**border-style:** none|hidden|dotted|dashed|solid|double|groove|ridge|inset|outset|initial|inherit;

##Border Image
**border-image** is a shorthand for border-image-source, border-image-slice, border-image-width, border-image-outset and border-image-repeat properties.

**border-image-source:** url(border.png);
**border-image-slice** Specifies how to slice the image: all| horizontal vertical|top right bottom left
       left         right
top     *      *      *
        *      *      *
bottom  *      *      *

--
##Outline
An outline is a line that is drawn around elements (outside the borders) to make the element "stand out".
outline: outline-color outline-style outline-width|initial|inherit;
outline-style: none|hidden|dotted|dashed|solid|double|groove|ridge|inset|outset|initial|inherit;



-----

##CSS3 Colors
CSS supports color names, hexadecimal, RGB and:
RGBA color: RGB plus alpha.
HSL/HSLA colors: HSL stands for Hue, Saturation and Lightness. / Alpha

---
##Basic UI Properties 

###Content
The content property is used with the :before and :after pseudo-elements, to insert generated conten
```
li:before {
    content: "•"; /* Insert content that looks like bullets */
  }
```

-----

#Counters
variable values can be incremented by CSS rules
**counter-reset** Creates or resets a counter
**counter-increment** Increments a counter value
**counter() or counters()** function - Places the value of a counter to an element

This example ads a secuion counter. to each h2.
```
body { counter-reset: section; }
h2::before {
    counter-increment: section;
    content: "Section " counter(section) ": ";
}
```
--
##list Style
**list-style** shorthand property sets all the list properties in one declaration.
**list-style-type** circle, list, square
**list-style-position** inside|outside
**list-style-image** 

##Tables
**border** can be applied to tables (only outside borders), headers, and cells individually;
``` table, th, td { border: 1px solid black;} ```
**border-collapse:** collapse; makes all borders into one. or ```separate``` separates them.
**border-spacing:** property sets the distance between the borders of adjacent cells (only for the "separated borders" model).
**caption-side:** top|bottom|  specifies where the <caption> is placed

##table-layout: 
**auto** The column width is set by the widest unbreakable content in the cells;
**fixed** The horizontal layout only depends on the table's width and the width of the columns, not the contents of the cells




#Cool new stuff
&&Columns
&&Flexboxes
&&Functions like attr(href) that inserts the value of href.

&&Media queries
```
@media screen and (min-width:600px) {  }
@media screen and (max-width:599px) { }
```

&&fix borders. they are everywhere
&&Css animations: define it, assign it 

**New Stuff**

Code

Adding flexDirection to a component's style determines the primary axis of its layout. Should the children be organized horizontally (row) or vertically (column)? The default is column.

Adding justifyContent to a component's style determines the distribution of children along the primary axis. Should children be distributed at the start, the center, the end, or spaced evenly? Available options are flex-start, center, flex-end, space-around, and space-between.

Adding alignItems to a component's style determines the alignment of children along the secondary axis (if the primary axis is row, then the secondary is column, and vice versa). Should children be aligned at the start, the center, the end, or stretched to fill(stretch only works if no width/height is given)? Available options are flex-start, center, flex-end, and stretch.

\*lommismo q arriba pero en el eje secundario

flex 

-When flex is a positive number, it makes the component flexible and it will be sized proportional to its flex value. So a component with flex set to 2 will take twice the space as a component with flex set to 1.

-When flex is 0, the component is sized according to width and height and it is inflexible.

-When flex is -1, the component is normally sized according width and height. However, if there's not enough space, the component will shrink to its minWidth and minHeight

 box sizing is set to border-box, any explicit width settings are guarantees that the element will take up no more than that width.

