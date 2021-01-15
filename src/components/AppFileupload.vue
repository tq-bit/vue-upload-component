<template>
	<section class="upload-container">
		<div class="upload-heading">
			<h2>{{ headingText || 'Fileupload demo' }}</h2>
		</div>

		<div
			v-on:dragover.prevent="handleDragOver"
			v-on:drop.prevent="handleDrop"
			v-on:dragleave.prevent="handleDragLeave"
			class="upload-body"
			:class="{ 'upload-body-dragged': status.over }"
		>
			<svg
				v-if="loading"
				xmlns="http://www.w3.org/2000/svg"
				xmlns:xlink="http://www.w3.org/1999/xlink"
				style="margin: auto; display: block; shape-rendering: auto; animation-play-state: running; animation-delay: 0s;"
				width="160px"
				height="105px"
				viewBox="0 0 100 100"
				preserveAspectRatio="xMidYMid"
			>
				<path
					fill="none"
					stroke="#486684"
					stroke-width="8"
					stroke-dasharray="42.76482137044271 42.76482137044271"
					d="M24.3 30C11.4 30 5 43.3 5 50s6.4 20 19.3 20c19.3 0 32.1-40 51.4-40 C88.6 30 95 43.3 95 50s-6.4 20-19.3 20C56.4 70 43.6 30 24.3 30z"
					stroke-linecap="round"
					style="transform: scale(0.8); transform-origin: 50px 50px; animation-play-state: running; animation-delay: 0s;"
				>
					<animate
						attributeName="stroke-dashoffset"
						repeatCount="indefinite"
						dur="1s"
						keyTimes="0;1"
						values="0;256.58892822265625"
						style="animation-play-state: running; animation-delay: 0s;"
					></animate>
				</path>
			</svg>
			<span v-else>{{ bodyText || 'Drop your files here' }}</span>
		</div>

		<div class="upload-footer">
			<div v-if="status.dropped">
				<!-- Display the information related to the file -->
				<p class="upload-footer-file-name">{{ file.name }}</p>
				<small class="upload-footer-file-size">Size: {{ file.size }} kb</small>
			</div>
			<button @click="handleFileupload" class="upload-footer-button">
				<span>{{ footerText || 'Upload' }}</span>
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
			loading: false,
			// Create a property that holds the file information
			file: {
				name: 'MyScreenshot.jpg',
				size: 281923,
				value: '',
			},
			// Add the drag and drop status as an object
			status: {
				over: false,
				dropped: false,
			},
		};
	},

	methods: {
		handleDragOver() {
			this.status.over = true;
		},
		handleDrop(event) {
			this.status.dropped = true;
			this.status.over = false;
			const fileItem = event.dataTransfer.items[0].getAsFile();
			this.file = {
				name: fileItem.name,
				size: (fileItem.size / 1000).toFixed(2),
			};
			const reader = new FileReader();

			reader.readAsArrayBuffer(fileItem);
			reader.onloadend = event => {
				// Take the reader's result and use it for the next method
				const file = event.target.result;
				this.file.value = file;
				// Emit an event to the parent component
				this.$emit('fileLoaded', this.file);
			};
		},
		handleDragLeave() {
			this.status.over = false;
		},
		async handleFileupload() {
			const url = 'https://vue-upload-server.herokuapp.com/';
			const options = { method: 'post', body: this.file.value };
			try {
				this.loading = true;
				const response = await fetch(url, options);
				const data = await response.json();
				const { bytes, type } = data;
				alert(
					`Success! \nFilesize: ${(bytes / 1000).toFixed(2)} kb \nType: ${
						type.mime
					}`
				);
			} catch (e) {
				alert('Error! \nAn error occured: \n' + e);
			} finally {
				this.loading = false;
			}
		},
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

.upload-body-dragged {
	color: #fff;
	background-color: #b6d1ec;
}
</style>
