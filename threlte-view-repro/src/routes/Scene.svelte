<script lang="ts">
  import { T, useDOM, useThrelte } from "@threlte/core";
  import { interactivity, OrbitControls } from "@threlte/extras";
  import { Color } from "three";
  import { onMount, type Snippet } from "svelte";

  type Props = {
    children?: Snippet;
  };

  const { children }: Props = $props();
  const { scene, camera, dom, size } = useThrelte();

  onMount(() => {
    interactivity({
      target: dom,
      filter: (items, context) => {
        const filteredItems = items.filter((i) => {
          return true;
        });
        return filteredItems;
      },
      compute: (event, state) => {
        state.pointer.update((pointer) => {
          const x = event.clientX - size.current.left;
          const y = event.clientY - size.current.top;
          console.log(
            `Mouse X: ${x}, size.current.left: ${size.current.left}, dom.getBoundingClientRect().left: ${dom.getBoundingClientRect().left}`
          );
          pointer.set(
            (x / size.current.width) * 2 - 1,
            -(y / size.current.height) * 2 + 1
          );
          return pointer;
        });
        state.raycaster.setFromCamera(state.pointer.current, camera.current);
      },
    });
  });

  scene.background = new Color(0xffffff);
</script>

<T.PerspectiveCamera
  makeDefault
  position={[0, 0, 2]}
  fov={50}
  near={1}
  far={10}
>
  <OrbitControls minDistance={2} maxDistance={5} enablePan={false} />
</T.PerspectiveCamera>

<T.HemisphereLight args={[0xaaaaaa, 0x444444, 3]} />
<T.DirectionalLight args={[0xffffff, 1.5]} position={[1, 1, 1]} />

{@render children?.()}
