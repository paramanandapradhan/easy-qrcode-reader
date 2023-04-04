# easy-qrcode-reader-svelte

Using the Svelte library, you can easily read QR codes from a webcam or mobile camera while maintaining control over camera operations. This method relies on the CDN version of the jsQR.js library for QR code decoding. Alternatively, you can provide the URL path for your own 'jsQR.min.js' file.

# Install

```
npm install @cloudparker/easy-qrcode-reader-svelte --save-dev
```

# Screenshot

<img src="" width="340">


# Usage

```html
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

```

# Props

### width:number

_required_

Width of the camera view, height can be adjusted automatically.

### jsQrUrl: string

_default_: `https://cdn.jsdelivr.net/npm/jsqr@1.4.0/dist/jsQR.min.js`

Update your `jsQR.js` library path.

### autoScan: boolean

_default_: true

Automatically start the camera and scan qrcode


# Events

### on:qrcode

It will triggers when a qrcode scanned succesfully. it return the qrdcode data in sting format.

# Public Methods

### open(): void

Open camera for scan qrcode.

### close(): void

Close camera.

### pause(): void

Pause camera.

### resume(): void

Resume camera.

### captureImage(): Promise<string>

Capture image and return a promise with base64 image data. Also it will trigger on:image event when image captured.
