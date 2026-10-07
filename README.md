# radbros-3d

six radbros as rigged, animated 3d characters. glTF, textures baked in, free to use for anything.

| [![#4764](radbro4764/portrait_radbro4764.webp)](radbro4764) | [![#652](radbro652/portrait_radbro652.webp)](radbro652) | [![#3704](radbro3704/portrait_radbro3704.webp)](radbro3704) | [![#3710](radbro3710/portrait_radbro3710.webp)](radbro3710) |
|:-:|:-:|:-:|:-:|
| [**#4764**](radbro4764) · katana | [**#652**](radbro652) · nobody sweatshirt | [**#3704**](radbro3704) · launcher | [**#3710**](radbro3710) · nobody tee |

| [![#723](radbro723/portrait_radbro723.webp)](radbro723) | [![#2564](radbro2564/portrait_radbro2564.webp)](radbro2564) |
|:-:|:-:|
| [**#723**](radbro723) · cowboy hat | [**#2564**](radbro2564) · ghost |

## download

full-quality files are on the [release page](https://github.com/dexedrne/radbros-3d/releases/tag/v1).

| file | what's in it |
|---|---|
| [radbros-3d-all.zip](https://github.com/dexedrne/radbros-3d/releases/download/v1/radbros-3d-all.zip) | all six, every clip, plus the web versions and a long readme (clip timings, root motion, rig notes). start here. |
| [radbro652-3d-model-v3.zip](https://github.com/dexedrne/radbros-3d/releases/download/v1/radbro652-3d-model-v3.zip) | #652 on its own |
| [radbro4764-3d-model-v3.zip](https://github.com/dexedrne/radbros-3d/releases/download/v1/radbro4764-3d-model-v3.zip) | #4764 on its own |
| [radbro3704-3d-model.zip](https://github.com/dexedrne/radbros-3d/releases/download/v1/radbro3704-3d-model.zip) | #3704 on its own (23 clips, plus the separate launcher prop) |
| [radbro3710-3d-model.zip](https://github.com/dexedrne/radbros-3d/releases/download/v1/radbro3710-3d-model.zip) | #3710 on its own (23 clips) |
| [radbro2564-3d-model.zip](https://github.com/dexedrne/radbros-3d/releases/download/v1/radbro2564-3d-model.zip) | #2564 on its own (7 clips; the all-in-one zip has his full 21) |
| [radbro723-3d-model.zip](https://github.com/dexedrne/radbros-3d/releases/download/v1/radbro723-3d-model.zip) | #723 on its own (12 clips; the all-in-one zip has 14) |

each character comes as a static mesh, a rigged mesh in rest pose, and a rigged mesh with all its clips.

this repo holds the small web versions, one folder per character:

- `radbroNNN_web.glb`: mesh, skeleton and 4 clips (walk, run, sprint, jump). draco-compressed, unlit, 1024 webp texture. 0.6-0.9 MB.
- `radbroNNN_clips_web.glb`: skeleton and the rest of the clips, no mesh (19 for #3704 and #3710, 14 for #2564, 10 for the others). load it next to the web file.
- `preview_radbroNNN.png`: turnaround. studio-lit on top, true flat colours below.
- `portrait_radbroNNN.webp`: 384px head and shoulders, transparent background.

## specs

- 1.70 m tall (#723 is 1.80 with the hat). metres, y-up, facing +z, origin on the ground between the feet
- ~60k triangles, one mesh and one 2048 texture each. #3704 has 61,355 triangles; #3710 has 61,327. #4764's katana is its own small mesh on the hips, so you can hide it
- 24-bone mixamo-style rig with the same bone names on all six. no finger bones
- flat art style: the colour map also drives emissive, so they read like the 2d art under any light. an unlit material gives exact colours
- hands open and empty. #3704's launcher is a separate prop file in the zips. fishing, hanging and sitting are the motion only

## clips

| clip | plays | on |
|---|---|---|
| Idle | loop | all |
| Casual_Walk | loop | all |
| Run_02 | loop | all |
| Lean_Forward_Sprint | loop, moves forward | all |
| Regular_Jump | once | all |
| Big_Wave_Hello | once | all |
| Victory_Cheer | loop | all |
| Rope_Hang_Idle | loop, right hand up | all |
| Grab_Bar_and_Swing_Forward | once, moves forward | all |
| Falling_Down | once | all |
| Fishing_Cast | once | all |
| Waltz | once, turns ~180° | all |
| Free_Fall | loop | all |
| Big_Land | once | all |
| Wall_Run_Up, Wall_Run, Wall_Run_Mirror | once | #3704, #3710 |
| Ledge_Grab, Vault, Slide, Wall_Climb, Ledge_Climb, Land_Roll | once | #3704, #3710 |
| Run_and_Jump (front flip), Leap_of_Faith, Fall_1, Roll_Dodge | once / loop | #2564 |
| Stand_to_Sit_Transition_M, Chair_Sit_Idle_M, Sit_Lie_Bed | once / loop | #2564, full files only |

#3704 and #3710 have 23 clips each, #652, #4764 and #723 have 14, and #2564 has 21 in the full files (18 in the web pair).

root motion sits on the Hips bone. if your controller moves the character itself, hold the hips' x/z on the clips that travel.

## use it

### three.js

```js
import * as THREE from 'three'
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js'
import { DRACOLoader } from 'three/addons/loaders/DRACOLoader.js'

const base = 'https://cdn.jsdelivr.net/gh/dexedrne/radbros-3d@v1/radbro4764/'
const draco = new DRACOLoader().setDecoderPath('https://cdn.jsdelivr.net/npm/three@0.186.1/examples/jsm/libs/draco/gltf/')
const loader = new GLTFLoader().setDRACOLoader(draco)

const [bro, more] = await Promise.all([
  loader.loadAsync(base + 'radbro4764_web.glb'),
  loader.loadAsync(base + 'radbro4764_clips_web.glb'),
])
scene.add(bro.scene)

const clips = [...bro.animations, ...more.animations]
const mixer = new THREE.AnimationMixer(bro.scene)
mixer.clipAction(THREE.AnimationClip.findByName(clips, 'Idle')).play()

// every frame: mixer.update(clock.getDelta())
```

clips from `_clips_web.glb` bind to the character by bone name, so they play on the web file as-is. keep each character's clips with that character; the rigs differ.

### model-viewer

```html
<script type="module" src="https://cdn.jsdelivr.net/npm/@google/model-viewer@4.3.1/dist/model-viewer.min.js"></script>

<model-viewer
  src="https://cdn.jsdelivr.net/gh/dexedrne/radbros-3d@v1/radbro723/radbro723_web.glb"
  animation-name="Casual_Walk" autoplay camera-controls>
</model-viewer>
```

### blender

File > Import > glTF 2.0. the full-quality files from the release come in with the rig and every clip as an action.

## license

[Viral Public License](LICENSE). use them, remix them, re-rig them, sell them, put them in your game. anything made from them keeps the same license, and no one can add restrictions on top.

radbros #4764, #652, #3704, #3710, #723 and #2564 are my own radbros, made 3d with the Radbro Webring dev's ok. original characters by [Radbro Webring](https://radbro.xyz). 3d models by dexedrne.

made something with them? tag [@dexedrne](https://x.com/dexedrne).
