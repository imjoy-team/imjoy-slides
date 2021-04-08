# ImJoy Slides

Interactive presentation powered by [ImJoy](https://imjoy.io) and [reveal.js](https://revealjs.com/). 

You can now run ImJoy plugins directly in your slides!

## Getting started

Here are the steps for making interactive presentation with ImJoy Slides:

 1. Prepare your slides in markdown format, for example, the following markdown will be rendered as two slides:
 ```markdown
## Slide 1
A paragraph with some text and a [link](http://imjoy.io).

A run button: <button class="button" onclick="api.showDialog({src: 'https://kaibu.org'})">Run</button>

---
## Slide 2

* bullet point 1
* bullet point 2

---
## Slide 3

 ```

 2. Save your markdown file to [Gist](https://gist.github.com/) or any Github repo. For example: https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md

 3. Construct a presentation URL with your markdown file URL: `https://imjoy-team.github.io/imjoy-slides/?slides=URL_TO_YOUR_SLIDES`. For example: `https://imjoy-team.github.io/imjoy-slides/?slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md`. You can try it [here](https://imjoy-team.github.io/imjoy-slides/?slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md).

## Advanced features

### Add a run button
The easiest way to run a ImJoy plugin is to insert a Run button, to do that, you can insert the following code to your markdown file:
```markdown
## My Awesome Slide

Try it here: <button class="button" onclick="api.createWindow({src: 'https://kaibu.org'})">Run</button>
```
In button attributes, you can set `onclick` to a JS expression, and you can use any ImJoy api functions defined [here](https://imjoy.io/docs/#/api).

To add a draggable and resizable window, you can call `api.createWindow()` as in the above example. 

### Embed a window in a slide
It's also possible to embed a window directly to the slide. To do that, you need to:
1. Insert a div tag in the place where you want to insert the window in your markdown file. The div tag will be used as a window container. You will also need to set a unique `id`, if necessary also set the style of the container element. For example:
```
## My Slide

<div id="window-x" style="display: inline-block;width: 46%; height: calc(100vh - 200px);"></div>
```
2. When you call `api.createWindow`, pass a `window_id` key with the value of the container div id you defined above. For example:
```
## My Slide
<button class="button" onclick="api.createWindow({src: 'https://kaibu.org', window_id: 'window-x'})">Run</button>

<div id="window-x" style="display: inline-block; width: 100%; height: calc(100vh - 200px);"></div>
```

### Add a script block
Even though we can use the `onclick` expression to execute JS code, it's not convenient if we want to define longer functions. To support that, you can define a JS code block in markdown by specifying its language as `javascript execute`, for example:

````
```javascript execute
function myCustomFunction(){

}

alert('hello world!');
```
````

This code block will be executed while loading the slides. A common design pattern is to define some utility functions in this script code block, then call these functions in button `onclick` callback.
### Run a plugin when the slide appears
Instead of always using a button to trigger a function, you can also run a specific function automatically when a certain slide appears. To do that, you need to following these steps:
1. At the beginning of the slide, add a HTML comment to define an event id:
```markdown
----
<!-- .slide: data-state="my-awesome-slide-loaded" -->
# Slide 5

<div id="awesome-window" style="width: 100%; height: 100%;"></div>

```
2. In a script block, add register an event callback function as follows:
````
```javascript execute
Reveal.addEventListener('my-awesome-slide-loaded', async function(){
    await api.createWindow({src: 'https://kaibu.org', window_id: 'awesome-window'})
})
```
````

[Here](https://github.com/imjoy-team/imjoy-slides/blob/master/slides/imjoy-interactive-annotation.md) you can find a more complete example.

For more details about the implementation, see https://revealjs.com/markdown/.
