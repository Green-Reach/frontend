<template>
	<div class="map">
		<div class="map-sidebar">
			<MapSidebar :locations="locations" 
				v-on:changeActive="handleChange"
				/>
		</div>
		<div class="map-frame">
			<MapFrame :activeObj="activeObj" />
		</div>
	</div>	
</template>
<script>
import MapFrame from '@/components/Map/MapFrame'
import MapSidebar from "@/components/Map/MapSidebar"

export default {
	name: "Map",
	components: {
		MapFrame,
		MapSidebar
	},
	data() {
		return {
			locations: [
			],
			indexOfLastActive: 0,
			activeObj: null,
			center: [120.9858621, 14.5709086]
		}
	},
	mounted() {
		this.getNearby()
	},
	methods: {
		handleChange(index, obj){
			this.indexOfLastActive = index;
			if (index === 0){
				this.activeObj = null;
			} else {
				this.activeObj = obj;
			}
		},
		getNearby: async function() {
			let data = {
				amenity: "hospital",
				latitude: this.center[1],
				longitude: this.center[0],
				radius: 5000
			}
			let res = await this.$http.post('http://0.0.0.0:8000/search/', data)
			let locations = res.data
			locations.unshift({name: "All", active: false})
			this.locations = [...locations]
			console.log(locations)
		}
	}
}
</script>
<style>
.map {
	padding: 10px 15px;
	height: 70vh;
	display: flex;
	justify-content: center;
}

.map-sidebar {
	flex: 0 0 40%;
	background: #f5f5f5;
	height: 100%;
	display: block;
	overflow-y: scroll;
}



.map-frame {
	flex: 0 0 60%;
	padding: 0;
	height: 100%;
}

</style>