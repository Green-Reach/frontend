<template>
	<!--
	<div class="banner">
		<div class="banner-content">
			<h1>Stripe</h1>
			<p>A greate payment API</p>
			<a 
			class="button twitter"
			href="#">
				Use now
			</a>		
		</div>
	</div>
	-->
	
	<div class="report">
		<div class="content">
			<h3>Report Fire</h3>
			<p>You can help us make the world a safer place by providing a photo of a recent wildfire near you.</p>	
			<h5>Choose Photo</h5>
			<b-form @submit="onSubmit">
				<b-form-file
			      v-model="file"
			      :state="Boolean(file)"
			      placeholder="Choose a file or drop it here..."
			      drop-placeholder="Drop file here..."
			    ></b-form-file>
			    <div class="mt-3">Selected file: {{ file ? file.name : '' }}</div>
			    <b-button type="submit" variant="success">Submit</b-button>
			</b-form>
			<!--
			<button @click="openFile" class="button green-button">
				Upload Photo
			</button>
			<input type="file" id="file" ref="file" v-on:change="handleFileUpload()"/>
			<div class='file-uploaded' v-show="file">
				<div class="text">
					Filename: {{ file.name }}
				</div>
				<div class="send-button">
					Send
				</div>
			</div>
			-->
		</div>
	</div>
	
</template>
<script>

export default {
	name: "ReportPage",
	data() {
		return {
			file: null,
		}
	},
	methods: {
		onSubmit(evt) {
	        evt.preventDefault()
	        alert(this.file.name)
	      },
	}
}
	
</script>
<style>

.report {
	height: 100vh; 
	color: green;
	background: linear-gradient(
				135deg,
				rgba(0,0,0, 0.8) 0%,
				rgba(0,0,0, 0.3) 50%), 
				url('https://picsum.photos/seed/picsum/300/300')
				;
	background-size: cover;
	display: flex;
	justify-content: space-around;
	align-items: center;
	text-align: center;
}

.content {
	padding: 2em;
	border-radius: 10px;
	background: rgba(255, 255, 255, 0.8);
	color: black;
}

.report h3 {
	font-size: 2.5em;
	font-weight: bold;
}

.report p {
	font-weight: lighter;
}


.button {
	border: 0;
	width: auto;
	padding: 10px;
	border-radius: 15px;
	background: white;
	color: black;
}

.button:hover {
	background: #f5f5f5;
	transition: 0.3s;
}


.button.green-button {
	background: rgb(0, 160, 0);
	color: white;
	border: 1 solid green;
}

.button.green-button:hover {
	color: white;
	background: green;
	transition: 0.5s;
}

#file {
	display: none;
}

.file-uploaded {
	margin: 0.8em 0;
	background: white;
	color: black;
	padding: 10px;
	border-radius: 10px;
	position: absolute;
}

.file-uploaded .text {
	position: relative;
	top: 0;
	left: 5px;
}

.file-uploaded .send-button {
	position: relative;
	top: 0;
	right: 5px;
}

</style>