<template>
	<div id="app">
		<div class="container">
			<app-fileupload></app-fileupload>
		</div>
	</div>
</template>

<script>
import AppFileupload from '@/components/AppFileupload';

export default {
	name: 'App',
	components: {
		AppFileupload,
	},
};
</script>

<style>
h1,
h2,
h3,
p {
	margin: 0;
	padding: 0;
}

#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	margin-top: 60px;
}

.container {
	display: block;
	margin: auto;
}

/* Add basic nedia queries for responsiveness */
@media (min-width: 600px) {
	.container {
		width: 80%;
	}
}

@media (min-width: 800px) {
	.container {
		width: 60%;
	}
}

@media (min-width: 1000px) {
	.container {
		width: 50%;
	}
}
</style>
