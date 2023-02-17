# CSS Positioning - Position Absolute and Relative

When we want to design complex layouts, we'll need to change the typical document flow and override the default browser styles.

We have to control how elements behave and are positioned on the page.

For example, we want to stack elements next to each other or on top of one another in a specific way or make a header stick to the top of the page and not move when we scroll up and down on the page.

To do the above, and much more, we'll use CSS's `position` property.

This property takes five values: `static`, `relative`, `absolute`, `fixed`, and `sticky`.

In this article, we'll focus on the `relative`, and `absolute` values.

We'll see an overview of how they work, their differences from one another, and how they are best used. 

Let's get started!


### How to view the position of elements using Chrome Developer Tools

A useful tool in front-end development workflow is Chrome's Developer Tools.

In many ways, we have the ability to look at the HTML/CSS/JavaScript code of any website to understand how different styles work.

To see what position an element has on a web page on a Mac machine, press `Command` and click at the same time while on the desired element. On a Windows machine, right-click on the element you want to select. A menu will appear and from there select `Inspect`.

The Chrome Developer Tools will open.

Select the `Computed` tab and from there either scroll down to the `position` element or in the `filter` search box, type in `position`.

![sn6.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1630940107636/0un-vUKLR.png)

### What is default position of HTML elements in CSS?

By default, the `position` property of all HTML elements in CSS is set to `static`. This means that if we don't specify any other `position` value or if the `position` property is not declared explicitly, it'll be `static`.

Visually, all elements follow the order of the HTML code, and in that way, the typical document flow is created.

Elements appear one after other - directly below one another, according to the order of the HTML code.

Block elements like `<div>` are stacked one after the other like this:

```
<body>
        <div class="parent">
                <div class="child one ">One</div>
                <div class="child two">Two</div>
                <div class="child three">Three</div>
                <div class="child four">Four</div>
        </div>
</body>
```
```
body {
    margin:100px auto;
}
.parent {
    width:400px;
    border:1px solid red;
    margin:auto;
    text-align:center;
}
.child {
     width:80px;
     height:80px;
     margin:20px;
}
.one {
    background-color: #ff95c5; 
}
.two {
    background-color: #00a19d;
}
.three {
    background-color: #57cc99;
}
.four {
    background-color: slateblue;
}
```

![bandicam 2021-09-06 19-09-16-612.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1630935881264/A06XRsxRaH.png)


 


The `position` property is not declared in the above code and therefore it reverts to the default `position: static`. It follows the order of the HTML code.
Whatever comes first in the HTML is shown first, and each element follows the next. This is how the document flow is created.  

In our code here, the div with the text "One" is written first so it is shown first on the page. Directly below that, the box with the text "Two" is shown, as it comes next in the HTML, and so on.

Because of this default positioning, it doesn't leave any room for flexibility or to move elements around.

**What if you wanted to move the first square a bit towards the left of the page – how would you do that?**

There are offset properties to do so, like `top`, `bottom`, `left`, and `right`.

But if you try to apply them while the square has this default static position applied to it, these properties will do nothing and the square will not move. These properties have no effect on `position: static`.

### What is position relative in CSS?
`position: relative` works the same way as `position: static`, but it lets us change an element's position.

But just writing this CSS rule alone will not change anything. 

To modify the position, we'll need to apply the `top`, `bottom`, `right`, and `left` properties mentioned earlier and in that way, we can specify where and how much we want to move the element.

The `top`, `bottom`, `right`, and `left` offsets push the tag *away* from where it's specified, working in reverse. For example, `top` moves the element towards the bottom of the element's parent container. `bottom` pushes the element towards the top of the element's parent container, and so on.

Now, we can move the first square to the left by updating the CSS like this:

```
.one {
    background-color: #ff95c5; 
    position: relative;
    right: 50px;
}
```

![sn1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1630937816625/BnuyDP77m.png)


Here, the square has moved `50px` from the left of where it was supposed to be by default.

`position: relative;` changes the position of the element *relative* to the parent element and relative to itself and where it would usually be in the regular document flow of the page. This means that it's relative to its original position within the parent element.

Using these offsets and `position: relative`, we can also change the order in which elements appear on the page.

The second square can appear on top of the first one:
```
.one {
    background-color: #ff95c5; 
    position: relative;
    top: 100px;
}
.two {
    background-color: #00a19d;
    position: relative;
    bottom: 100px;
}
```

![sn2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1630938431737/1Lqud5DkF.png)

We can see, the order is now reversed, while the HTML code remains exactly the same. 

To summarise, elements that are relatively positioned can move around, while still remaining in the regular document flow.
They also do not affect the layout of the surrounding elements.

### What is position absolute in CSS?
If we update the CSS rule for the first square to the following:

```
.one {
    background-color: #ff95c5; 
    position: absolute;
}
```
We'll get this result:

![sn3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1630938874321/8kKVSp23Y.png)

This is unexpected behavior. The second square has completely disappeared.

If we also add some offset properties like this:
```
.one {
    background-color: #ff95c5; 
    position: absolute;
    top: 50px;
    left:0;
}
```

![sn4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1630939151005/9CjpnPDp2.png)

Now the square has completely abandoned its parent.

Absolute-positioned elements are completely taken out of the regular flow of the webpage.

They are not positioned based on their usual place in the document flow but based on the position of their ancestor.

In the example above, the absolutely positioned square is inside a statically positioned parent.

This means it will be positioned relative to the whole page itself, which means relative to the `<html>` element – the root of the page.

The coordinates, `top: 50px;` and `left: 0;`, are therefore based on the whole page.

If we want the coordinates to be applied to its parent element, we need to relatively position the parent element by updating `.parent` while keeping `.one` the same:
```
.parent {
  width:400px;
  border:1px solid red;
  margin:auto;
  text-align:center;
  position: relative;
}
.one {
    background-color: #ff95c5; 
    position: absolute;
    top: 50px;
    left:0;
}
```
The result will be:

![sn5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1630939661349/lq2LUZCRD.png)

Absolute positioning takes elements out of the regular document flow while also affecting the layout of the other elements on the page.

### Conclusion

`position: relative` places an element relative to its current position without changing the layout around it, whereas `position: absolute` places an element relative to its parent's position and changing the layout around it.

> Thanks for the reading and happy learning!