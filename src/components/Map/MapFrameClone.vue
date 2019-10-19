<template>
	<div id="map"></div>	
</template>
<script>
import mapboxgl from 'mapbox-gl';
require('../../../node_modules/mapbox-gl/dist/mapbox-gl.css');


export default {
	name: "MapFrame",
	props: [ "activeObj" ],
	data() {
		return {
			accessKey: 'pk.eyJ1IjoiZGVydHJvY2t4IiwiYSI6ImNrMXcwZHB0bjBmb2gzY216ODA0NDZ3MWsifQ.IoDpTejvyHpWvvj_cjjRlw',
			// center: [-122.662323, 45.523751]
			center: [120.9858621, 14.5709086],
		}
	},
	mounted() {
    	this.createMap();
	},
	methods: {
		createMap() {
			mapboxgl.accessToken = this.accessKey;
				// init the map

			this.map = new mapboxgl.Map({
				container: 'map',
				style: 'mapbox://styles/mapbox/streets-v9',
				
				center: this.center, // Manhattan
				zoom: 14,
			});
			this.canvas = this.map.getCanvasContainer();
			// var bounds = [[-123.069003, 45.395273], [-122.303707, 45.612333]];
			// this.map.setMaxBounds(bounds);

		  	this.map.on('load', () => this.handleInitialLoad())
		  	this.map.on('click', (event) => this.handleClick(event) )
		},
		// handles the click event when adding a new marker i.e. the end marker
		handleClick: async function(event) {
			let coordsObj = event.lngLat;
			this.canvas.style.cursor = '';
			let coords = Object.keys(coordsObj).map( (key) => {
				return coordsObj[key]
			})
			let end = {
				type: 'FeatureCollection',
				features: [{
					type: 'Feature',
					properties: {},
					geometry: {
						type: 'Point',
						coordinates: coords
					}
				}]
			}
			if (this.map.getLayer('end')){
				this.map.getSource('end').setData(end)
			} else {
				this.addMark('end', coords, 'red')
			}
			await this.getRoute(coords);
		},
		handleInitialLoad: async function() {
			await this.getRoute(this.center);
			this.addMark('start', this.center, 'blue');
		},
		// handles the pathfinding part.. draws a line that serves as the path to the destination.
		getRoute: async function(end){
			let start = this.center;
			var url = 'https://api.mapbox.com/directions/v5/mapbox/cycling/' + start[0] + ',' + start[1] + ';' + end[0] + ',' + end[1] + '?steps=true&geometries=geojson&access_token=' + this.accessKey;

			let res = await this.$http.get(url)
			let route = res.data.routes[0].geometry.coordinates;
			let geojson = {
				type: 'Feature',
				properties: {},
				geometry: {
					type: 'LineString',
					coordinates: route
				}
			};
			console.log(geojson)
			this.addRoute(geojson)
		},
		addRoute(source) {
			if(this.map.getSource('route')) {
				this.map.getSource('route').setData(source)
			} else {
				this.map.addLayer({
					id: 'route',
					type: 'line',
					source: {
						type: 'geojson',
						data: {
							type: 'Feature',
							properties: {},
							geometry: {
								type: 'LineString',
								coordinates: source
							}
						}
					},
					layout: {
						'line-join': 'round',
						'line-cap': 'round'
					},
					paint: {
						'line-color': '#3887be',
						'line-width': 5,
						'line-opacity': 0.75
					}
				})
			}
		},
		addMark(id, endpoints, type) {
			// adds a marker with id and endpoints as coordinates
			const colors = {
				'red': 'red',
				'blue': '#3887be'
			}
			let geojson = {
				type: 'Feature',
				properties: {},
				geometry: {
					type: 'LineString',
					coordinates: endpoints
				}
			};
			const color = colors[type]
			console.log(this.map.getSource(id))
			if(this.map.getSource(id)){
				let end = {
					type: "FeatureCollection",
					features: [{
						type: 'Feature',
						properties: {},
						geometry: {
							type: 'Point',
							coordinates: endpoints
						}
					}]
				};
				this.map.getSource(id).setData(end)
			} else {
				console.log("ADDING NEW MARK 2")
				this.map.addLayer({
					id: id,
					type: 'circle',
					source: {
						type: 'geojson',
						data : {
							type: 'FeatureCollection',
							features: [{
								type: 'Feature',
								properties: {},
								geometry: {
									type: 'Point',
									coordinates: endpoints
								}
							}]
						}
					},
					paint: {
						'circle-radius': 10,
						'circle-color': color
					}
				})
			}
		}
	},
	watch: {
		activeObj(value){
			console.log(value)
			if(value !== null){
				const newMark = value;
				const endpoints = [newMark.lon, newMark.lat]
				console.log("ADDING NEW MARK")
				this.addMark('hospital1', endpoints, 'red')
			}
		}
	}
}
</script>
</script>
<style scoped>
#map {
	width: 100%;
	height: inherit;
}
</style>