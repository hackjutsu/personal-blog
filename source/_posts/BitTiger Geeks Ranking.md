# BitTiger Geeks Ranking

title: BitTiger Geeks Ranking
date: 2016-05-24 18:00:01
tags:
- front end
- full stack
categories:
- MEAN

----------

<img src="http://i.imgur.com/uU3V2MT.png" style="max-height: 350px;"/>

**BitTiger Geeks Ranking** (太阁极客榜) is a real-time ranking board for BitTiger's Github members. Its results are based on members' Github activities in the past seven days with daily updates at **06:30 PDT**.

> **Note:** This post is my summary for those front end knowledge applied in this project.

- [Demo Link](https://bittiger-ranking.firebaseapp.com/)
- [GitHub Link](https://github.com/hackjustu/Github-Ranking-FrontEnd)

<!--more-->


## HTML
**HTML Forms**
HTML Forms are used to collect user inputs.
```
<form>
... form elements ...
</form>
```
The `<input>` element is the most important form element. Form elements are different types of input elements, checkboxes, radio buttons, submit buttons, and more.

> Check out this link for more information about [HTML Forms](http://www.w3schools.com/html/html_forms.asp).

## CSS
CSS (Cascading Style Sheets) is a stylesheet language that describes the presentation of an HTML document.
### Cascading Order
1. Inline style (inside an HTML element)
2. External and internal style sheets (in the head section)
3. Browser default **strong text**

### Margins and Padding
The CSS `margin` properties are used to generate space around elements. The margin properties set the size of the white space **OUTSIDE** the border.

The CSS `padding` properties are used to generate space around content. The padding properties set the size of the white space **BETWEEN** the element content and the element border.

### Height/Width
The `height` and `width` properties do **NOT** include padding, borders, or margins; they set the height/width of the area inside the padding, border, and margin of the element!

Besides height/width, there are also max-height/min-height/max-width/min-width.

>Check out this [link](http://www.w3schools.com/css/css_dimension.asp) for mare information.

### Display
The `display` property specifies if/how an element is displayed.

Every HTML element has a default display value depending on what type of element it is. The default display value for most elements is block or inline.

A `block` element always starts on a new line and takes up the full width available (stretches out to the left and right as far as it can).

An `inline` element does not start on a new line and only takes up as much width as necessary.

An `inline-block` element is a combination between `block` and `inline`.

> Check out this [StackOverflow post](http://stackoverflow.com/questions/8969381/what-is-the-difference-between-display-inline-and-display-inline-block) for more about their difference.

### Position
The position property specifies the type of positioning method used for an element.
There are four different position values:

- static
- relative
- fixed
- absolute

> Check out this [post](http://www.w3schools.com/css/css_positioning.asp) to see their difference.

### Float
The `float` property specifies whether or not an element should float.
The `clear` property is used to control the behavior of floating elements.

> Check out this [post](http://www.w3schools.com/css/css_float.asp) to see their difference.

### Text
####  Text Color
The `color` property is used to set the color of the text.
```
#p1 {color: #ff0000;}   /* red */
#p2 {color: rgb(0, 255, 0);}   /* green */
#p3 {color: rgba(0, 0, 255, 0.3);}   /* blue with opacity */
```
> Check out this [post](http://www.w3schools.com/cssref/css_colors_legal.asp) for more about the CSS color options.

#### Text Alignment
The `text-align` property is used to set the horizontal alignment of a text.
- left
- right
- center
- justify

#### Text Decoration
The `text-decoration` property is used to set or remove decorations from text. The value `text-decoration: none;` is often used to remove underlines from links.
- none
- overline
- line-through
- underline

####  Text Transformation
The `text-transform` property is used to specify uppercase and lowercase letters in a text.
```
p.uppercase {
    text-transform: uppercase;
}

p.lowercase {
    text-transform: lowercase;
}

p.capitalize {
    text-transform: capitalize;
}
```

#### More
`text-indent`
`letter-spacing`
`line-height`
`direction`
`word-spacing`

> Check out this [post](http://www.w3schools.com/css/css_text.asp) for more details.

### Combinators
There are four different combinators in CSS3:

- descendant selector (space)
- child selector (>)
- adjacent sibling selector (+)
- general sibling selector (~)

> Check out this [post](http://www.w3schools.com/css/css_combinators.asp) to see their difference.

### Pseudo-class
A pseudo-class is used to define a special state of an element. For example, it can be used to:

- Style an element when a user mouses over it
- Style visited and unvisited links differently
- Style an element when it gets focus

> Check out this [post](http://www.w3schools.com/css/css_pseudo_classes.asp) to see their difference.

### Pseudo-element

A CSS pseudo-element is used to style specified parts of an element. For example, it can be used to:

- Style the first letter, or line, of an element
- Insert content before, or after, the content of an element

> Check out this [post](http://www.w3schools.com/css/css_pseudo_elements.asp) to see their difference.

## CSS3
CSS3 is the latest standard for CSS. It is completely backwards-compatible with earlier versions of CSS.
> Check out this [post](http://code.tutsplus.com/tutorials/10-css3-properties-you-need-to-be-familiar-with--net-16417) for the some popular CSS3 properties.

### Rounded Corners
With the CSS3 `border-radius` property, we can give any element "rounded corners".
```
border-radius: 25px;
border-radius: 50%;
```

### Box Sizing
The CSS3 `box-sizing` property allows us to include the padding and border in an element's total width and height.

**Without the CSS3 box-sizing Property**
width + padding + border = actual width of an element
height + padding + border = actual height of an element

**With the CSS3 box-sizing Property**
Padding and border are included in an element's total width and height.

```
.div1 {
    width: 300px;
    height: 100px;
    border: 1px solid blue;
    box-sizing: border-box;
}
```
Since the result of using the `box-sizing: border-box;` is so much better, many developers want all elements on their pages to work this way.


## Bootstrap
### Grid Basics
The Bootstrap grid system has four classes:

- xs (for phones)
- sm (for tablets)
- md (for desktops)
- lg (for larger desktops)

The classes above can be combined to create more dynamic and flexible layouts.

The following is a basic structure of a Bootstrap grid;
```
<div class="row">
  <div class="col-*-*"></div>
</div>
<div class="row">
  <div class="col-*-*"></div>
  <div class="col-*-*"></div>
  <div class="col-*-*"></div>
</div>
<div class="row">
  ...
</div>
```

> Check out this [post](http://www.w3schools.com/bootstrap/bootstrap_grid_basic.asp) for more information about Bootstrap grid system.

## AngularJS
> Check out these two Udemy courses to go over AnguarJS.
> - [Learn and Understand AngularJS](https://www.udemy.com/learn-angularjs/learn/v4/content)
> - [Learn AngularJS Step By Step](https://www.udemy.com/maruti-angularjs/learn/v4/content)

We can get more details about AngularJS in its [official website](https://docs.angularjs.org/guide).
### Angular Directives
We have used the following AnguarJS Directives in our project.

`ng-app`
`ng-controller`

`ng-repeat`
`ng-if`
`ng-show/hide`
`ng-click`
`ng-model`
`ng-include`
`ng-class`
`ng-cloak`
`ng-src`
`ng-href`

### Service
Singleton + Dependency Injection

#### AngularJS Services

`$scope`
`$filter`
`$timeout`

#### Customb Services
I haven't tried it yet.

## npm and bower
Dependency management tools, like maven or gradle for Java.  I personally consider it inconvenient to use two tools to manage a project. Probably npm might replace bower's role in the front end side.

- [npm](https://www.npmjs.com/)
- [bower](http://bower.io/)

## Grunt
**[Grunt](http://gruntjs.com/)** is a automation tasks runner tool for front end projects. It is something like **Ant** for Java.

There are three steps to configure the `Gruntfile.js`.
- Configure the Grunt tasks
- Load the Grunt tasks
- Register the Grunt tasks

Here is a list of Grunt tasks used in our project.
```javascript
    grunt.loadNpmTasks('grunt-contrib-clean');
    grunt.loadNpmTasks('grunt-contrib-copy');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-csslint');
    grunt.loadNpmTasks('grunt-contrib-cssmin');
    grunt.loadNpmTasks('grunt-usemin');
```

> The best way to go over **Grunt** is to go over this [Udemy course](https://www.udemy.com/learn-grunt-automate-your-front-end-workflow/) again.

## 前端项目工程化
[前段工程--基础篇](https://github.com/fouber/blog/issues/10)

- 第一阶段：库/框架选型
	- AngularJS、React、Bootstrap...
- 第二阶段：简单构建优化
	- Grunt or Gulp
- 第三阶段：JS/CSS模块化开发
	- 分而治之
- 第四阶段：更多的工程问题
	- 项目大体量、团队大规模、高性能
	- 组件化开发 （header footer jumbotron ranking-table）
	- “智能”静态资源管理

## Resource
- [w3schools](http://www.w3schools.com/)
- [Udemy](https://www.udemy.com/courses/)
- [AngularJS](https://angularjs.org/)
- [Grunt](http://gruntjs.com/)
- [fouber's GitHub blog](https://github.com/fouber/blog)


