<script lang="ts">
	import EasyQrcodeReader from '$lib';
	import { onMount } from 'svelte';

	let qrcodeReaderRef: EasyQrcodeReader;

	let clientWidth: number = $state(340);

	const handleQrcode = (data: string) => {
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
	<EasyQrcodeReader bind:this={qrcodeReaderRef} bind:width={clientWidth} onQrcode={handleQrcode} />
</div>

<style>
	.camera-container {
		display: flex;
		align-items: center;
		justify-content: center;
	}
</style>
