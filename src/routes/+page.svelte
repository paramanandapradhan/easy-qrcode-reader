<script lang="ts">
	import EasyQrcodeReader from '$lib';
	import { onMount } from 'svelte';

	let qrcodeReaderRef: EasyQrcodeReader;

	let clientWidth: number;

	const handleQrcode = (ev: CustomEvent) => {
		let data = ev.detail;
		if (data) {
			console.log(data);

			//TODO: Handle the code and resume or close the camera
			qrcodeReaderRef.pause();

			setTimeout(() => {
				qrcodeReaderRef.resume();
			}, 15000);
		}
	};

	onMount(() => {
		return () => {
			qrcodeReaderRef && qrcodeReaderRef.close();
		};
	});
</script>

<div class="camera-container" bind:clientWidth>
	<EasyQrcodeReader bind:this={qrcodeReaderRef} width={clientWidth} on:qrcode={handleQrcode} />
</div>

<style>
	.camera-container {
		display: flex;
		align-items: center;
		justify-content: center;
	}
</style>
