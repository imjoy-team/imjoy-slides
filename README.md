# ImJoy Slides

Interactive presentation powered by [ImJoy](https://imjoy.io) and [reveal.js](https://revealjs.com/). 

You can now run ImJoy plugins directly in your slides!

[ImJoy Slides Demo: See it in action!](https://slides.imjoy.io/?theme=white&slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/imjoy-interactive-annotation.md)
## Getting started

Here are the steps for making interactive presentation with ImJoy Slides:

 1. Go to https://slides.imjoy.io?edit=1 and write your slides in [markdown format](https://www.markdownguide.org/basic-syntax/) with our builtin slide editor. For example:
    ```markdown
    ## Slide 1
    A paragraph with some text and a [link](http://imjoy.io).

    A run button: <button class="button" onclick="api.showDialog({src: 'https://kaibu.org'})">Run</button>

    -----
    ## Slide 2

    * bullet point 1
    * bullet point 2

    -----
    ## Slide 3

    ```
  As shown above, you can use `-----` to separate horizontal slides. Optionally, if you want to fit more content in one slides, you can use `---` to separate vertical sections.

  With the builtin editor, you can edit directly and click "Save" (or press the "Shift+Enter" key combination). The slides will be rendered directly behind the editor, you can either move the editor or close it to see the preview. To open the editor again, you can click ["Edit Slides"](https://github.com/imjoy-team/imjoy-slides/blob/master/assets/screenshot-imjoy-slide-editor.png) in the ImJoy icon located in the upper-right corner.

  At any time, you can click the `Export` button to export your slides as a markdown file, we recommend you do that before you close the browser tab to avoid losing your slides. 

  If you like, you can also use your own text editor locally, but you won't be able to preview it as easy as the builtin editor.


 2. To share your slides with others, you can upload your markdown file (as a file with `.md` extension) to [Gist](https://gist.github.com/) or any Github repo. For example: https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md. Then construct a presentation URL with your markdown file URL: `https://slides.imjoy.io/?slides=URL_TO_YOUR_SLIDES`. For example: `https://slides.imjoy.io/?slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md`. You can try it [here](https://slides.imjoy.io/?slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md).

 Tips: 1) you can pass `&theme=white` to specify a slide theme, [here](https://revealjs.com/themes/) you can find a list of available themes; 2) you can pass `&edit=1` if you want to display the slide editor when the page is loaded 

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
