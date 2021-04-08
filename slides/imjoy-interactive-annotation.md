# ImJoy

### Supercharging interactivity and scalability of data science!

Wei Ouyang

Emma Lundberg Group @ KTH Royal Institute of Technology
-----
# Challenges

* **Interactivity**: Respond to GUI on laptop/mobile
* **Scalability**: Remote storage and compute resources
* **Privacy**: Edge computing

-----
# Progressive Web App

* Rich and interactive interface in the browser
* Access remote storage and compute power
* Computation in the browser
* Offline support

-----
### ImJoy https://imjoy.io
Data science tools in the browser

<img src="https://raw.githubusercontent.com/imjoy-team/ImJoy/master/docs/assets/imjoy-overview.jpg" style="height: 450px;"></img>


-----
# Key concept
 * Sandboxed plugins connected via Remote Procedure calls
 * Workflow composition via asynchronous programming
 * Open Integration with existing software


-----
### Calling Python from JS with RPC

<button class="button" onclick="runDemo1()">Run</button>

<div id="window-1" style="display: inline-block;width: 46%; height: calc(100vh - 200px);"></div>

<div id="window-2" style="display: inline-block;width: 46%; height: calc(100vh - 200px);"></div>

-----

### Open Integration with Web Apps

Create a window via URL and call functions directly

```js
// load the web app via its URL
viewer = await api.createWindow({src: "https://kaibu.org/#/app"})
// call api functions directly via RPC
// add an image layer
await viewer.view_image("https://images.proteinatlas.org/61448/1319_C10_2_blue_red_green.jpg")
// add an annotation layer
await viewer.add_shapes([], {name:"annotation"})
```
<button class="button" onclick="runDemo2()">Run</button>


-----
### Interactive Annotation in Colab

<iframe width="100%" height="500px" src="https://www.youtube.com/embed/A0DNcN7L5t0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


-----
### 3D Visualization with ITK/VTK + Zarr
In collabration with Matt McCormick @ Kitware

<button class="button" onclick="runDemo3()">Run</button>
<div id="window-4" style="display: inline-block;width: 100%; height: calc(100vh - 250px);"></div>


-----

### Visualization with Vizarr

Made by Trevor Manz et. al.
<iframe width="100%" height="500px" src="https://hms-dbmi.github.io/vizarr/?source=https://s3.embassy.ebi.ac.uk/idr/zarr/v0.1/4495402.zarr"  frameborder="0" allowfullscreen></iframe>

-----
### Other features and Work in Progress
 * [Jupypter Notebook Integration](https://github.com/imjoy-team/imjoy-jupyter-extension)
 * [ImageJ.JS](https://ij.imjoy.io) <button onclick="api.showDialog({src:'https://ij.imjoy.io'})">Run</button>
 * [File Manager](https://imjoy-team.github.io/elFinder/) <button onclick="api.showDialog({src:'https://imjoy-team.github.io/elFinder/'})">Run</button>
 * [Collaborative Cloud Annotation](https://github.com/imjoy-team/imjoy-cloud-annotation)
 * Remote rendering with [napari](https://napari.org/) and [BigDataViewer](https://imagej.net/BigDataViewer)

-----
### Acknowledgements
 * ImJoy Team
 * Emma Lundberg Group
 * KTH Royal Institute of Technology
 * Science for Life Laboratory


Follow us on twitter @ImJoyTeam


-----

# Thank You!


<!-- startup script  -->
```javascript execute
const PythonPluginCode = `
<config lang="json">
{
  "name": "PythonPlugin",
  "type": "native-python",
  "version": "0.1.0",
  "description": "[TODO: describe this plugin with one sentence.]",
  "tags": [],
  "ui": "",
  "cover": "",
  "inputs": null,
  "outputs": null,
  "flags": [],
  "icon": "extension",
  "api_version": "0.1.8",
  "env": "",
  "permissions": [],
  "requirements": [],
  "dependencies": []
}
</config>

<script lang="python">
from imjoy import api


class ImJoyPlugin():
    def setup(self):
        api.showMessage('Python plugin initialized')

    def add(self, a, b):
        return a + b

api.export(ImJoyPlugin())
</script>
`

const JSPluginCode = `
<config lang="json">
{
  "name": "JSPlugin",
  "type": "window",
  "tags": [],
  "ui": "",
  "version": "0.1.0",
  "cover": "",
  "description": "[TODO: describe this plugin with one sentence.]",
  "icon": "extension",
  "inputs": null,
  "outputs": null,
  "api_version": "0.1.8",
  "env": "",
  "permissions": [],
  "requirements": [],
  "dependencies": [],
  "defaults": {"w": 20, "h": 10}
}
</config>

<script lang="javascript">
window.callPython = async function(){
    const pythonPlugin = await api.getPlugin('PythonPlugin')
    const result = await pythonPlugin.add(10, 99)
    document.getElementById("result").innerHTML = "10 + 99 =" + result
}

class ImJoyPlugin {
  async setup() {
    api.log('initialized')
  }

  async run(ctx) {
  }
}
api.export(new ImJoyPlugin())
</script>

<window lang="html">
  <div>
    <button class="button" onclick="callPython()"> Calculate in Python</button>
    <h3 id="result"></h3>
  </div>
</window>

<style lang="css">

</style>
`

const ZarrPythonCode = `
<config lang="json">
{
  "name": "ZarrPythonPlugin",
  "type": "native-python",
  "version": "0.1.0",
  "description": "[TODO: describe this plugin with one sentence.]",
  "tags": [],
  "ui": "",
  "cover": "",
  "inputs": null,
  "outputs": null,
  "flags": [],
  "icon": "extension",
  "api_version": "0.1.8",
  "env": "",
  "permissions": [],
  "requirements": ["zarr", "fsspec"],
  "dependencies": []
}
</config>

<script lang="python">
import zarr
from imjoy_rpc import api
from imjoy_rpc import register_default_codecs
from fsspec.implementations.http import HTTPFileSystem
register_default_codecs()

fs = HTTPFileSystem()
http_map = fs.get_mapper("https://openimaging.github.io/demos/multi-scale-chunked-compressed/build/data/medium.zarr")
z_group = zarr.open(http_map, mode='r')

class ImJoyPlugin:
    async def setup(self):
        pass

    async def run(self, ctx):
        viewer = await api.createWindow(
            src="https://kitware.github.io/itk-vtk-viewer/app/",
            name="ITK/VTK Viewer"
        )
        await viewer.setImage(z_group)

api.export(ImJoyPlugin())
</script>
`

async function runDemo1(){
    await api.createWindow({src: 'https://if.imjoy.io', config: {fold: [1]}, data: {code: PythonPluginCode}, window_id: "window-1"})

    await api.createWindow({src: 'https://if.imjoy.io', config: {fold: [1, 29]}, data: {code: JSPluginCode}, window_id: "window-2"})
}

async function runDemo2(){
 const viewer = await api.showDialog({src: "https://kaibu.org/#/app", name: "Kaibu"})
        await viewer.view_image("https://images.proteinatlas.org/61448/1319_C10_2_blue_red_green.jpg")
        await viewer.add_shapes([], {name:"annotation"})
}

async function runDemo3(){
    await api.createWindow({src: 'https://if.imjoy.io', fullscreen: true, config: {fold: [2]}, data: {code: ZarrPythonCode}, window_id: "window-4"})
}
```
