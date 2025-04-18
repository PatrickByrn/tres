# Your first scene

This guide will help you to create your first Tres scene. 🍩

<ClientOnly>
<div style="aspect-ratio: 16/9; height: auto; margin: 2rem 0; border-radius: 8px; overflow:hidden;">
  <DonutExample />
</div>
</ClientOnly>

## Setting up the experience Canvas

Before we can create a Scene, we need somewhere to display it. Using plain [ThreeJS](https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene) we would need to create a `canvas` HTML element to mount the `WebglRenderer` and initialize the `scene`

With **TresJS** you only need to import the default component `<TresCanvas />` and add it to the template of your Vue component.

```vue
<script lang="ts" setup>
import { TresCanvas } from '@tresjs/core'
</script>
<template>
  <TresCanvas window-size>
    <!-- Your scene goes here -->
  </TresCanvas>
</template>
```

::: warning
It's important that all components related to the scene live between the `<TresCanvas />` component. Otherwise, they will be not rendered.
:::

The `TresCanvas` component is going to do some setup work behind the scene:

- It creates a [**WebGLRenderer**](https://threejs.org/docs/index.html?q=webglrend#api/en/renderers/WebGLRenderer) that automatically updates every frame.
- It sets the render loop to be called on every frame based on the browser refresh rate.

## Canvas size

By default, `TresCanvas` component will take the **parent's width and height**, if you are experiencing a blank page make sure the parent element has a proper size.

```vue
<script lang="ts" setup>
import { TresCanvas } from '@tresjs/core'
</script>
<template>
  <TresCanvas>
    <!-- Your scene goes here -->
  </TresCanvas>
</template>
<style>
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
}
#app {
  height: 100%;
  width: 100%;
}
</style>
```

If your scene is not gonna be part of a UI, you can also force the canvas to take the width and height of the full window by using the `window-size` prop like this:

```vue
<script lang="ts" setup>
import { TresCanvas } from '@tresjs/core'
</script>
<template>
  <TresCanvas window-size>
    <!-- Your scene goes here -->
  </TresCanvas>
</template>
```

## Creating a scene

We need 4 core elements to create a 3D experience:

- A [**Scene**](https://threejs.org/docs/index.html?q=scene#api/en/scenes/Scene) to hold the camera and the object(s) together.
- A [**Renderer**](https://threejs.org/docs/index.html?q=renderer#api/en/renderers/WebGLRenderer) to render the scene into the DOM.
- A [**Camera**](https://threejs.org/docs/index.html?q=camera#api/en/cameras/Camera)
- An [**Object**](https://threejs.org/docs/index.html?q=object#api/en/core/Object3D)

With **TresJS** you only need to add the `<TresCanvas />` component to the template of your Vue component and it will automatically create a `Renderer` (`canvas` DOM Element) and `Scene` for you.

```vue
<template>
  <TresCanvas window-size>
    <!-- Your scene goes here -->
  </TresCanvas>
</template>
```

Then you can add a [**PerspectiveCamera**](https://threejs.org/docs/index.html?q=perspectivecamera#api/en/cameras/PerspectiveCamera) using the `<TresPerspectiveCamera />` component.

```vue
<template>
  <TresCanvas window-size>
    <TresPerspectiveCamera />
  </TresCanvas>
</template>
```

::: warning
A common issue is that the camera default position is the origin of the scene (0,0,0), TresJS will automatically set the position of your camera to `[3,3,3]` if the prop `position`. If no camera is defined in you scene, a perspective camera is added automatically.`
:::

## Adding a 🍩

That scene looks a little empty, let's add a basic object. If we were using plain **ThreeJS** we would need to create a [**Mesh**](https://threejs.org/docs/index.html?q=mesh#api/en/objects/Mesh) object and attach to it a [**Material**](https://threejs.org/docs/index.html?q=material#api/en/materials/Material) and a [**Geometry**](https://threejs.org/docs/index.html?q=geometry#api/en/core/BufferGeometry) like this:

```ts
const geometry = new THREE.TorusGeometry(1, 0.5, 16, 32)
const material = new THREE.MeshBasicMaterial({ color: 'orange' })
const donut = new THREE.Mesh(geometry, material)
scene.add(donut)
```

A **Mesh** is a basic scene object in three.js, and it's used to hold the geometry and the material needed to represent a shape in 3D space.

Now let's see how we can easily achieve the same with **TresJS**. To do that we are going to use `<TresMesh />` component, and between the default slots, we are going to pass a `<TresTorusGeometry />` and a `<TresMeshBasicMaterial />`.

```vue
<template>
  <TresCanvas window-size>
    <TresPerspectiveCamera />
    <TresMesh>
      <TresTorusGeometry :args="[1, 0.5, 16, 32]" />
      <TresMeshBasicMaterial color="orange" />
    </TresMesh>
  </TresCanvas>
</template>
```

::: info
Notice that we don't need to import anything, that's because **TresJS** automatically generate a **Vue Component based on the Three Object you want to use in CamelCase with a Tres prefix**. For example, if you want to use an `AmbientLight` you would use `<TresAmbientLight />` component.
:::

<StackBlitzEmbed projectId="tresjs-first-scene" />

```vue
<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
</script>

<template>
  <TresCanvas clear-color="#82DBC5" window-size>
    <TresPerspectiveCamera :position="[3, 3, 3]" :look-at="[0, 0, 0]" />
    <TresMesh>
      <TresTorusGeometry :args="[1, 0.5, 16, 32]" />
      <TresMeshBasicMaterial color="orange" />
    </TresMesh>
    <TresAmbientLight :intensity="1" />
  </TresCanvas>
</template>
```

From here onwards you can start adding more objects to your scene and start playing with the properties of the components to see how they affect the scene.
