<script context="module">
	export async function load({ page }) {
		const path = page.path;

		return {
			props: {
				path
			}
		};
	}
</script>

<script lang="ts">
	import '../lib/tailwind.svelte';
	import ThreeCanvas from '../lib/ThreeCanvas.svelte';
	import Link from '../lib/components/Link.svelte';
	import { onMount } from 'svelte';
	import Player from '$lib/components/Player.svelte';
	import Footer from '$lib/components/Footer.svelte';

	let musicPlayer: Player;

	let threeCanvas: ThreeCanvas;

	function hoverOnLink(link) {
		musicPlayer.playSound('select.mp3', 0.25);
		const linkBackground = linkBackgrounds[link.url];
		threeCanvas?.getStage()?.getElementManager().setShownType(linkBackground);
	}

	export let path;
	const links = [];

	const linkBackgrounds = {};

	onMount(() => {
		console.log('hey 👀');
		console.log({
			linkedIn: 'https://www.linkedin.com/in/lucasgoyeche/',
			gitHub: 'https://github.com/pragmare/'
		});
		window['linkedIn'] = 'https://www.linkedin.com/in/lucasgoyeche/';
		window['gitHub'] = 'https://github.com/pragmare/';
	});
</script>

<main class="relative min-h-screen overflow-y-hidden flex flex-col">
	<nav class="container mx-auto w-full px-2 flex mt-4 py-4 justify-between items-center">
		<a class="text-lg font-bold text-white" href="/">Home</a>
		<ul class="flex items-center">
			{#each links as link}
				<li on:mouseenter={() => hoverOnLink(link)} class="mx-4">
					<Link to={link.url} class="px-1 py-4" active={link.url === path}>
						{link.label}
					</Link>
				</li>
			{/each}
		</ul>
	</nav>
	<div
		class="container mx-auto w-full flex flex-row flex-wrap md:flex-nowrap flex-grow md:pb-24 relative"
	>
		<div class="w-full md:w-1/2 relative z-50 pl-2">
			<slot />
		</div>
		<div
			class="w-full md:w-1/2 h-[560px] animate-fade-in md:fixed md:right-0 relative md:h-full overflow-x-hidden"
		>
			<div class="h-full md:h-2/3 relative flex md:flex-col flex-row">
				<ThreeCanvas
					bind:this={threeCanvas}
					class="w-[300%] md:w-full h-full md:left-0 left-1/2 transform -translate-x-1/3 md:transform-none absolute flex-grow"
				/>
				<Link
					to="https://github.com/pragmare"
					class="relative text-sm md:text-lg text-center mt-auto md:mb-0 mb-auto md:pt-0 pt-24"
					>visit this project on github</Link
				>
			</div>
		</div>
	</div>
	<Player bind:this={musicPlayer} />
</main>
<Footer />
