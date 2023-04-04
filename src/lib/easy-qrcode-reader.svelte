<script lang="ts" context="module">
	declare var window: any;
</script>

<script lang="ts">
	import { browser } from '$app/environment';
	import EasyCamera from '@cloudparker/easy-camera-svelte';
	import { createEventDispatcher, onMount } from 'svelte';

	export let width: number;
	export let jsQrUrl = 'https://cdn.jsdelivr.net/npm/jsqr@1.4.0/dist/jsQR.min.js';
	export let autoScan: boolean = true;

	let cameraRef: EasyCamera;
	let dispatch = createEventDispatcher();

	export function pause() {
		cameraRef && cameraRef.pause();
	}

	export function resume() {
		cameraRef && cameraRef.resume();
	}

	export function open() {
		cameraRef && cameraRef.open();
	}

	export function close() {
		cameraRef && cameraRef.close();
	}

	export function captureImage() {
		cameraRef && cameraRef.captureImage();
	}

	const handleFrame = (ev: CustomEvent) => {
		let imageData = ev.detail;
		if (window['jsQR']) {
			const decodedQRCode = window['jsQR'](imageData.data, imageData.width, imageData.height);
			if (decodedQRCode) {
				dispatch('qrcode', decodedQRCode.data);
			}
		}
	};

	onMount(() => {
		return () => {
			cameraRef && cameraRef.close();
		};
	});
</script>

<svelte:head>
	{#if browser && !window['jsQR']}
		<script src={jsQrUrl} type="text/javascript"></script>
	{/if}
</svelte:head>

<EasyCamera
	bind:this={cameraRef}
	bind:width
	on:frame={handleFrame}
	needFrames
	on:image
	bind:autoOpen={autoScan}
/>
