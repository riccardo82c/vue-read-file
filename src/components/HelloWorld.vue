<template>
	<div class="hello">
		<h1>{{ msg }}</h1>

		<label for="csv">Insert csv file</label>
		<input
			type="file"
			name="csv"
			id="csv"
			@change="previewFiles"
		>

		<read-file v-model="value"></read-file>
		<button @click="read">read</button>

	</div>
</template>




<script>
	import ReadFile from 'vue-read-file';

	export default {
		name: 'HelloWorld',
		components: {
			ReadFile,
		},
		props: {
			msg: String,
		},
		data() {
			return {
				value: '',
			};
		},

		methods: {
			previewFiles(event) {
				let file = event.target.files[0];
				console.log(event.target.files[0]);

				this.csvJSON(file);
			},
			read: function () {
				// console.log('ciao');
				// console.log(this.value);

				this.csvJSON(this.value);
			},

			csvJSON: function (file) {
				console.log(file);
				var lines = file.split('\n');

				var result = [];

				// NOTE: If your columns contain commas in their values, you'll need
				// to deal with those before doing the next step
				// (you might convert them to &&& or something, then covert them back later)
				// jsfiddle showing the issue https://jsfiddle.net/
				var headers = lines[0].split(',');

				for (var i = 1; i < lines.length; i++) {
					var obj = {};
					var currentline = lines[i].split(',');

					for (var j = 0; j < headers.length; j++) {
						obj[headers[j]] = currentline[j];
					}

					result.push(obj);
				}

				//return result; //JavaScript object
				return JSON.stringify(result); //JSON
			},
		},
		mounted() {
			this.csvJSON('test.csv');
		},
	};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
	h3 {
		margin: 40px 0 0;
	}
	ul {
		list-style-type: none;
		padding: 0;
	}
	li {
		display: inline-block;
		margin: 0 10px;
	}
	a {
		color: #42b983;
	}

	.form-group.file {
		color: black !important;
		cursor: pointer !important;
	}

	.form-control {
	}
</style>
