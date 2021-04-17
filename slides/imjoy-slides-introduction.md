# ImJoy Slides

Powering interactive presentation with ImJoy plugins

You can run ImJoy plugins, for example: <button class="button" onclick="api.showDialog({src: 'https://hms-dbmi.github.io/vizarr/?source=https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/4495402.zarr'})">Run</button>

-----
<!-- .slide: data-state="embed-demo" -->
## Embed ImJoy windows

ImJoy windows can be directly embedded to your slide:

<div id="kaibu-window" style="display: inline-block;width: 100%; height: calc(100vh - 300px);"></div>


-----
## Getting started

You can make your own interactive slides by writing a markdown file!

 * [Make your own slides...](https://slides.imjoy.io#getting-started)

 * [Demo](https://slides.imjoy.io/?theme=white&slides=https://github.com/imjoy-team/imjoy-slides/blob/master/slides/imjoy-interactive-annotation.md)



<!-- startup script  -->
```javascript execute
Reveal.addEventListener('embed-demo', async function(){
  // load the web app via its URL
  viewer = await api.createWindow({src: "https://kaibu.org/#/app", window_id: "kaibu-window"})
  // call api functions directly via RPC
  // add an image layer
  await viewer.view_image("https://images.proteinatlas.org/61448/1319_C10_2_blue_red_green.jpg")
  // add an annotation layer
  await viewer.add_shapes([], {name:"annotation"})
})
```
