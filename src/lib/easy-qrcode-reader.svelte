<script lang="ts">
	import EasyCamera from '@cloudparker/easy-camera-svelte';
	import EasyScriptLoader from '@cloudparker/easy-script-loader-svelte';

	type Props = {
		width?: number;
		autoScan?: boolean;
		onQrcode?: (data: string) => void;
	};

	let { width = $bindable(340), autoScan = true, onQrcode }: Props = $props();

	let cameraRef: EasyCamera | null = $state(null);

	let jsQR: any;

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

	const handleFrame = (imageData: ImageData) => {
		if (jsQR) {
			const decodedQRCode = jsQR(imageData.data, imageData.width, imageData.height);
			if (decodedQRCode) {
				onQrcode && onQrcode(decodedQRCode.data);
			}
		}
	};

	function handleJsQRLoad(lib: any) {
		jsQR = lib;
	}

	$effect(() => {
		return () => {
			cameraRef && cameraRef.close();
		};
	});
</script>

<EasyScriptLoader
	scriptName="jsQR"
	scriptUrl="https://cdn.jsdelivr.net/npm/jsqr@1.4.0/dist/jsQR.min.js"
	onLoad={handleJsQRLoad}
/>
<EasyCamera bind:this={cameraRef} bind:width onFrame={handleFrame} needFrames autoOpen={autoScan} />
