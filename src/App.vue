<template>
	<div id="app">
		<img
			alt="Vue logo"
			src="./assets/logo.png"
		>
		<HelloWorld msg="Welcome to Your Vue.js App" />
		<read-file v-model="value"></read-file>
		<button @click="read">read</button>
	</div>
</template>

<script>
	import HelloWorld from './components/HelloWorld.vue';
	import ReadFile from 'vue-read-file';

	export default {
		name: 'App',
		components: {
			HelloWorld,
			ReadFile,
		},
		data() {
			return {
				value: '',
			};
		},
		methods: {
			read: function () {
				// console.log('ciao');
				// console.log(this.value);

				this.csvJSON(this.value);
			},
			csvJSON: function (file) {
				var lines = file.split('\n');

				var result = [];

				// NOTE: If your columns contain commas in their values, you'll need
				// to deal with those before doing the next step
				// (you might convert them to &&& or something, then covert them back later)
				// jsfiddle showing the issue https://jsfiddle.net/
				var headers = lines[0].split(';');

				for (var i = 1; i < lines.length; i++) {
					var obj = {};
					var currentline = lines[i].split(';');

					for (var j = 0; j < headers.length; j++) {
						obj[headers[j]] = currentline[j];
					}

					result.push(obj);
				}

				console.log(result);
				//return result; //JavaScript object
				return JSON.stringify(result); //JSON
			},
		},
	};
</script>

<style>
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}

	/* .form-group.file {
			color: black !important;
		} */
</style>
