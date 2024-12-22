<script lang="ts">
  import { Canvas, T } from "@threlte/core";
  import { View } from "@threlte/extras";
  import Scene from "./Scene.svelte";
  import {
    BoxGeometry,
    Color,
    MeshStandardMaterial,
    SRGBColorSpace,
  } from "three";

  let scene01Container = $state<HTMLDivElement>();
  let scene02Container = $state<HTMLDivElement>();
  let wrapper = $state<HTMLDivElement>();

  const getMeshMaterial = () =>
    new MeshStandardMaterial({
      color: new Color().setHSL(Math.random(), 1, 0.75, SRGBColorSpace),
      roughness: 0.5,
      metalness: 0,
      flatShading: true,
    });
</script>

<div class="canvas">
  <Canvas>
    {#if scene01Container}
      <View dom={scene01Container}>
        <Scene />
      </View>
    {/if}
    {#if scene02Container}
      <View dom={scene02Container}>
        <Scene>
          <T.Mesh>
            <T is={new BoxGeometry(1, 1, 1)} />
            <T is={getMeshMaterial()} />
          </T.Mesh>
        </Scene>
      </View>
    {/if}
  </Canvas>
</div>
<div class="wrapper" bind:this={wrapper}>
  <div bind:this={scene01Container} class="view"></div>
  <div bind:this={scene02Container} class="view"></div>
</div>

<style>
	:global(html, body) {
		margin: 0;
		padding: 0;
	}

	:global(body *) {
		box-sizing: border-box;
	}

  .canvas {
    position: absolute;
    inset: 0;
  }

  .wrapper {
    display: flex;
    position: relative;
    width: 100%;
    height: 100vh;
	 gap: 1rem;
	 padding: 1rem;
  }

  .view {
    width: 100%;
    height: 100%;
  }

  .view:first-child {
    border: 1px solid red;
  }
  .view:last-child {
    border: 1px solid blue;
  }
</style>
