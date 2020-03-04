> Source [github](https://github.com/PacktPublishing/Learn-D3.js/tree/master/Chapter02)

# Technical Fundamentals

This chapter covers fundamental standard web technologies used by D3: SVG, JavaScript (ES 2015), HTML Canvas and standard data formats such as JSON and CSV. It is intended as a general reference to these topics.

Most data visualizations created with D3.js generate SVG graphics. Good knowledge of SVG is important to make the most of D3, but you only really need to know the basics.

## SVG

SVG stands for Scalable Vector Graphics. It’s an XML-based image format that describes graphics using geometrical attributes. Unlike HTML5 Canvas, which is another standard for vector graphics, SVG primitives are made of individual XML elements described using tags and attributes. It is also object-based and provides a DOM, which allows CSS styling, dynamic shape creation and manipulation, and coordinate transforms using JavaScript or CSS.

Scalable Vector Graphics (SVG)
SVG stands for Scalable Vector Graphics. It’s an XML-based image format that describes graphics using geometrical attributes. Unlike HTML5 Canvas, which is another standard for vector graphics, SVG primitives are made of individual XML elements described using tags and attributes. It is also object-based and provides a DOM, which allows CSS styling, dynamic shape creation and manipulation, and coordinate transforms using JavaScript or CSS.

To control SVG elements with D3 you should understand basic SVG syntax and rules, how a document is structured, how each element is rendered, the effects caused by attributes and styles, as well as nesting and transformation rules.

All the code used in this section is available in the SVG/ folder, from the GitHub repository for this chapter. You can see the results simply loading the pages in your browser.

 
### Viewport

SVG graphics context (viewport)
When SVG is embedded in HTML it creates a viewport.

Ex (note the light grey style):

```html
<style>
    svg {
        border: solid 1px lightgray;
        background-color: hsla(240,100%,50%,0.2)
    }
</style>
<body>
<h2>SVG viewport</h2>
    <svg width="600" height="300"></svg>
</body>
```

You can also create an SVG element using the DOM API, or D3, which is much simpler. The result is identical:

```html
<body>
    <script>
        d3.select("body").append("svg").attr("width", 400).attr("height", 300);
    </script>
</body>
```

### Shapes

Shapes are positioned in the viewport using x and y coordinates. They described by XML tags like ```<rect>```, ```<circle>```, ```<ellipse>```, ```<path>```, ```<polygon>``` and others. You create SVG graphics by placing these tags inside the ```<svg>``` element. Each supports attributes that configure their position in the viewport, and specific properties for each shape, such as radii, vertices or dimensions.

A circle can be drawn in SVG using the ```<circle>``` element and at least the r attribute (radius). If you don't provide any other attributes, you will only see the lower-right quarter of the circle, since the default coordinates for its center will be (0,0).

To illustrate an important js and raw html equivalency we will show code to draw 4 circles in the exact same way:

HTML:
```html
<svg width="400" height="300">
    <circle r="25"></circle>
    <circle cx="250" cy="200" r="50"></circle>
    <circle cx="50"  cy="50"  r="20"></circle>
    <circle cx="400" cy="300" r="50"></circle>
</svg>
```
JS:
```js
const svg = d3.select("body")
                .append("svg")
                .attr("width", 400)
                .attr("height", 300);

svg.append("circle").attr("r", 25);
svg.append("circle").attr("cx", 250).attr("cy", 200).attr("r", 50);
svg.append("circle").attr("cx", 50).attr("cy", 50).attr("r", 20);
svg.append("circle").attr("cx", 400).attr("cy", 300).attr("r", 50);
```


## Essential JavaScript data structures

## HTML Canvas

## Data Formats