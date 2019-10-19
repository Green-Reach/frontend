<template>
	<div>
		<div class="map-sidebar-header">
			<h3>Locations</h3>
		</div>
		<div 
			v-for="(location, index) in locations"
			:key="index"
			class="map-sidebar-item"
			:class="indexOfLastActive == index ? 'active' : ''"
			@click="toggleActive(index)"
		>
			<h4>{{ location.name }}</h4>
		</div>
	</div>
</template>
<script>

export default {
	name: "MapSidebar",
	props: [ "locations",  ],
	data () {
		return {
			indexOfLastActive: 0
		}
	},
	methods: {
		toggleActive(index){
			this.locations[index].active = !this.locations[index].active;
			this.locations[this.indexOfLastActive].active = !this.locations[this.indexOfLastActive].active;
			this.indexOfLastActive = index;
			this.$emit('changeActive', this.indexOfLastActive, this.locations[index]);
		},
	}
}

</script>
<style>
.map-sidebar-header {
	text-align: center;
	padding: 5px 0;
}

.map-sidebar-item {
	padding: 3px 15px;
	text-align: left;
}


.map-sidebar-item:hover {
	background: white;
}

.map-sidebar-item.active {
	background: white;
	color: black;
}
</style>