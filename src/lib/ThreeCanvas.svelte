<script lang="ts">
	let canvas: HTMLCanvasElement;

	import { onMount } from 'svelte';

	let resizeListener;
	let clazz;
	let stage;

	export { clazz as class };

	onMount(async () => {
		const lazyStage = () => import('./three/Stage');

		lazyStage().then(async ({ Stage }) => {
			stage = await new Stage(canvas);
			stage.start();

			resizeListener = () => stage.updateSize();

			window.addEventListener('resize', resizeListener);
		});
	});

	export function getStage() {
		return stage;
	}
</script>

<canvas class={clazz || ''} bind:this={canvas} />
