<script>
	import Scroller from '@sveltejs/svelte-scroller';
	import LoremIpsum from './LoremIpsum.svelte';
	import Map from './Map.svelte';
	
	let count;
	let index;
	let progress;

	// Element bindings
	let map = null; // Bound to mapbox 'map' instance once initialised

	$: if (map) {
			// Update the map based on the index
			switch(index) {
				case 0:
					map.easeTo({
						center: [0,0],
						zoom: 1.8,
						duration: 50000
					});
					break;
				case 1:
					map.easeTo({
						center: [100,-10],
						zoom: 1.8
					});
					break;
				case 2:
					map.easeTo({
						center: [-10,10],
						zoom: 1.8
					});

					map.setFog({
						'range': [-1, 10],
						'horizon-blend': 0.01,
						'color': '#ff3e00',
						'high-color': '#ff3e00',
						'space-color': 'rgba(255,255,255,0)',
						'star-intensity': 0.0
					});

					map.setPaintProperty('continent-label', 'text-color', '#e63900');
					map.setPaintProperty('country-label', 'text-color', '#e63900');
					map.setPaintProperty('admin-0-boundary', 'line-color', '#ff7547');
					map.setPaintProperty('water', 'fill-color', '#fbb59d');
					map.setPaintProperty('land', 'background-color', '#ffece5');

					break;
				case 3:
					map.easeTo({
						center: [-100,10],
						zoom: 3
					});

					map.setFog({
						'range': [-1, 10],
						'horizon-blend': 0.005,
						'color': '#7f00ff',
						'high-color': '#7f00ff',
						'space-color': 'rgba(255,255,255,0)',
						'star-intensity': 0.0
					});


					map.setPaintProperty('continent-label', 'text-color', '#7300e6');
					map.setPaintProperty('country-label', 'text-color', '#7300e6');
					map.setPaintProperty('admin-0-boundary', 'line-color', '#a347ff');
					map.setPaintProperty('water', 'fill-color', '#cc9dfb');
					map.setPaintProperty('land', 'background-color', '#f2e5ff');

					
					break;
				case 4:
					map.easeTo({
						center: [0,0],
						zoom: 3,
						duration: 30000
					});

					

					break;
				default:
					console.log('default');
			}	
	}

</script>

<div class='demo'>
	<LoremIpsum/> 

	<Scroller
		bind:count
		bind:index
		bind:progress
	>
		<div slot="background">
			<Map bind:map interactive={false} location={{
				lng: 115,
				lat: 45,
				zoom: 1.8
			}}/>
		</div>

		<div slot="foreground">
			<section data-id="map1">
				<div class="col-medium">
				</div>
			</section>
			<section data-id="map2">
				<div class="col-medium">
					<p>
						Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
					</p>
				</div>
			</section>
			<section data-id="map3">
				<div class="col-medium">
					<p>
						Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
					</p>
				</div>
			</section>
			<section data-id="map4">
				<div class="col-medium">
					<p>
						Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
					</p>
			</section>
			<section data-id="map5">
				<div class="col-medium">
					<p>
						Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
					</p>
			</section>
		</div>
	</Scroller>
	<LoremIpsum/>
</div>

<style>
	.demo {
		padding: 0;
	}
	
	[slot="background"] {
		font-size: 1.4em;
		overflow: hidden;
	}
	
	[slot="foreground"] {
		pointer-events: none;
	}
	
	[slot="foreground"] section {
		pointer-events: all;
	}
	
	section {
		height: 80vh;
		background-color: rgba(0,0,0,0);
		padding: 1em;
		margin: 0 0 3em 0;
	}

	[slot="foreground"] section {
		width: 70%;
		margin: auto;
		text-align: center;
		vertical-align: middle;
	}

	[slot="foreground"] p {
		background: rgba(255,255,255,0.35);
		padding: 2em;
		border-radius: 0.5em;
	}
	
</style>