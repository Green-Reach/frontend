<template>
	<b-navbar toggleable='lg' variant="success" type="dark">
		<b-container>
			<b-navbar-brand to="/">Green Reach</b-navbar-brand>
			<b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
			<b-collapse id="nav-collapse" is-nav>
				<b-navbar-nav class="ml-auto">
					<b-nav-item to="/map">
						Map
					</b-nav-item>
					<b-nav-item to="/report">
						Report
					</b-nav-item>
				</b-navbar-nav>
			</b-collapse>
		</b-container>
	</b-navbar>	
</template>
<script>
export default {
	name: "Nav"
}
</script>
<style>
	
</style>