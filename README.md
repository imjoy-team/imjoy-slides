# ImJoy Slides

Interactive presentation powered by [ImJoy](https://imjoy.io) and [reveal.js](https://revealjs.com/). 

You can now run ImJoy plugins directly in your slides!

[ImJoy Slides Demo: See it in action!](https://slides.imjoy.io/?theme=white&slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/imjoy-interactive-annotation.md)
## Getting started

Here are the steps for making interactive presentation with ImJoy Slides:

Go to https://slides.imjoy.io?edit=1 and write your slides in [markdown format](https://www.markdownguide.org/basic-syntax/) with our builtin slide editor. For example:
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

  With the builtin editor, you can edit directly and click "Save" (or press the "Shift+Enter" key combination). The slides will be rendered directly beside the editor, you can either keep the editor side-by-side with the preview or close it to see it in fullscreen. To open the editor again, you can click ["Edit Slides"](https://github.com/imjoy-team/imjoy-slides/blob/master/assets/screenshot-imjoy-slide-editor.png) in the ImJoy icon located in the upper-right corner.

  At any time, you can click the `Export` button to export your slides as a markdown file, we recommend you do that before you close the browser tab to avoid losing your slides. 

  If you like, you can also use your own text editor locally, but you won't be able to preview it as easy as the builtin editor.


## Basic features

### Change the background of a specific slide

You can also specify the background for a specific slide by adding the following line to the beginning of your slide:
```markdown
----
<!-- .slide: data-background="red" -->
# A slide in red

```

### Reveal fragments incrementally
Fragments are used to highlight or incrementally reveal individual elements on a slide. If you want to reveal a line gradually, you can add a comment after the line: `<!-- .element: class="fragment" data-fragment-index="1" -->` (change the index number according the order you want it to be revealed).

For example, with the following markdown, you will first see "Item 2", then "Item 1":
```markdown
- Item 1 <!-- .element: class="fragment" data-fragment-index="2" -->
- Item 2 <!-- .element: class="fragment fade-up" data-fragment-index="1" -->
```

You can custom the fragment style by setting different values for the fragment `class`, more info can be found [here](https://revealjs.com/fragments/).
### Insert math equations

You can add math equations in LaTeX (based on [MathJax](https://www.mathjax.org/)), see an example here:
```markdown

`$$ J(\theta_0,\theta_1) = \sum_{i=0} $$`

```

### Embed Google drawings

[Google drawings](https://docs.google.com/drawings/) is interactive tool for making online drawings, you can create a google drawing file and embed it directly to your slides. After creating the drawing, you can then go to "File->Publish to to web", click "Embed" and copy and paste the code to your slides markdown.


### Share your slide with others
To share your slides with others, you can upload your markdown file (as a file with `.md` extension) to [Gist](https://gist.github.com/) or any Github repo. For example: https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md. Then construct a presentation URL with your markdown file URL: `https://slides.imjoy.io/?slides=URL_TO_YOUR_SLIDES`. For example: `https://slides.imjoy.io/?slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md`. You can try it [here](https://slides.imjoy.io/?slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md).

Note: you can pass `&edit=1` if you want to display the slide editor when the page is loaded. For example `https://slides.imjoy.io/?slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md&edit=1`.

#### Specify the theme of your slides
You can add `&theme=white` after into the URL to specify a slide theme.

For example `https://slides.imjoy.io/?slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md&theme=white`.

A list of available theme can be found [here](https://revealjs.com/themes/).


#### Specify the transition of your slides
You can add `&transition=convex` after into the URL to specify a slide transition.

For example `https://slides.imjoy.io/?slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md&transition=convex`.

A list of available transition styles can be found [here](https://revealjs.com/transitions/).

#### Show slide numbers
You can add `&number=1` after into the URL if you want to display the number of each slide.

For example `https://slides.imjoy.io/?slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md&number=1`.

#### Link to a specific slide

You can add for example `#/<slide number>` (e.g. `#/3`) to the end of the URL if want to jump directly to a specific slide.

For example `https://slides.imjoy.io/?slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/basic.md#/3`.


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
```markdown
## My Slide

<div id="window-x" style="display: inline-block;width: 46%; height: calc(100vh - 200px);"></div>
```
2. When you call `api.createWindow`, pass a `window_id` key with the value of the container div id you defined above. For example:
```markdown
## My Slide
<button class="button" onclick="api.createWindow({src: 'https://kaibu.org', window_id: 'window-x'})">Run</button>

<div id="window-x" style="display: inline-block; width: 100%; height: calc(100vh - 200px);"></div>
```

### Add a script block
Even though we can use the `onclick` expression to execute JS code, it's not convenient if we want to define longer functions. To support that, you can define a JS code block in markdown by specifying its language as `javascript execute`, for example:

````markdown
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
````markdown
```javascript execute
Reveal.addEventListener('my-awesome-slide-loaded', async function(){
    await api.createWindow({src: 'https://kaibu.org', window_id: 'awesome-window'})
})
```
````

[Here](https://github.com/imjoy-team/imjoy-slides/blob/master/slides/imjoy-interactive-annotation.md) you can find a more complete example.

For more details about the implementation, see https://revealjs.com/markdown/.
