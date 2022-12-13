<script>
	import { setContext, createEventDispatcher, onMount, onDestroy } from "svelte";
	import { mapbox, key } from './mapbox.js';
	const dispatch = createEventDispatcher();
	export let map;
	export let id = "map";
	export let location = {
		lng: 15,
		lat: 45,
		zoom: 1
	};
	
	export let style = 'mapbox://styles/william-davis/clbee633y002n15qwtgxjmitk';
	export let options = {};
	export let minzoom = 0;
	export let maxzoom = 14;
	export let tabbable = false;
	export let zoom = null;
	export let center = null;
	export let pitch = null;
	export let bearing = null;
	export let interactive = true;
	export let attribution = true;
	let container;
	let _options = {};
	let loaded = false;
	setContext("map", {
		getMap: () => map,
	});
	function sleep (ms = 1000) {
  		return new Promise((resolve) => setTimeout(resolve, ms));
	}
	
	// Interpret location
	if (location.bounds) {
		_options.bounds = location.bounds;
	} else if (location.lng && location.lat) {
		_options.center = [+location.lng, +location.lat];
		if (location.zoom) {
			_options.zoom = +location.zoom;
		}
		if (location.pitch) {
			_options.pitch = +location.pitch;
		}
		if (location.bearing) {
			_options.bearing = +location.bearing;
		}
	}
	// Disable attribution if attribution = false
	if (!attribution) {
		_options.attributionControl = false;
	}
	_options = {..._options, ...options}; // Combine core options + custom user options
	onMount(() => {
		map = new mapbox.Map({
			container,
			style,
			minZoom: minzoom,
			maxZoom: maxzoom,
			interactive,
			..._options,
		});
		
		// Get initial zoom level
		map.on("load", (e) => {
			zoom = map.getZoom();
			center = map.getCenter();
			pitch = map.getPitch();
			bearing = map.getBearing();
			loaded = true;
			
			// Prevent map from being tabbable
			if (!tabbable && document.querySelector(`#${id} canvas`)) {
				document.querySelector(`#${id} canvas`).tabIndex = "-1";
			}
			dispatch("load", {
				event: e
			});
		});

		document.body.scrollIntoView();
	});
	onDestroy(async () => {
		await sleep(250);
		if (map) map.remove();
		map = null;
	});
	// Function to switch map style if style prop changes
	function setStyle(style) {
		if (map) map.setStyle(style);
		dispatch("style", {
			style
		});
	}
	$: setStyle(style);
</script>

<svelte:head>
	<link
		rel="stylesheet"
		href="https://api.mapbox.com/mapbox-gl-js/v2.11.0/mapbox-gl.css"
	/>
</svelte:head>

<div bind:this={container} {id} class="map">
	{#if loaded}
		<slot />
	{/if}
</div>

<style>
	.map {
		width: 100vw;
		height: 100vh;
		position:absolute;
    	left: calc(-50vw + 50%);
	}
</style>