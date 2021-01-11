<template>
	<section class="upload-container">
		<div class="upload-heading">
			<h2>{{ headingText || 'Fileupload demo' }}</h2>
		</div>

    <!-- The body will serve as our actual drag and drop zone -->
		<div class="upload-body">
			{{ bodyText || 'Drop your files here' }}
		</div>

		<div class="upload-footer">
			<div>
				<!-- Display the information related to the file -->
				<p class="upload-footer-file-name">{{ file.name }}</p>
				<small class="upload-footer-file-size">Size: {{ file.size }} kb</small>
			</div>
			<button @click="handleFileupload" class="upload-footer-button">
				{{ footerText || 'Upload' }}
			</button>
		</div>
	</section>
</template>

<script>
export default {
	// Accept the heading from parent component
	props: {
		heading: String,
	},

	data() {
		return {
			// Create a property that holds the file information
			file: {
				name: 'MyScreenshot.jpg',
				size: 281923,
				value: ''
			},
		};
	},
};
</script>

<style scoped>
/* Add basic styles for the upload container */
.upload-container {
	border-radius: 0.25rem;
	border: 1px solid#486684;
	display: block;
	margin: auto;
	width: 100%;
}

.upload-heading,
.upload-footer {
	background: #486684;
	color: #fff;
	padding: 1rem 2rem;
	font-weight: 600;
}

.upload-body {
	align-items: center;
	background-color: #fafafa;
	color: #486684;
  display: flex;
  font-size: 1.5rem;
	justify-content: center;
	min-height: 25vh;
}

.upload-footer {
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.upload-footer-file-name {
	margin-bottom: 0.25rem;
}

.upload-footer-file-size {
	opacity: 0.95;
}

.upload-footer-button {
	background-color: transparent;
	border: 2px solid #fff;
	border-color: #fff;
	border-radius: 0.25rem;
	color: #fff;
	cursor: pointer;
	font-weight: 600;
	padding: 0.5rem 1rem;

	transition: 0.25s all;
}

.upload-footer-button:hover {
	background-color: #fff;
	color: #486684;
	transition: 0.25s all;
}
</style>
