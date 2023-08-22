<script lang="ts">
	import type { QRCodeErrorCorrectionLevel } from 'qrcode';
	import QRCode from 'qrcode';

	let content = '';
	let errorCorrectionLevel = 'H';
  let colorDark = '#000000';
  let colorLight = '#ffffff';

	let qrUrl: string;

	$: {
		if (content.length > 0) {
			QRCode.toDataURL(
				content,
				{
					errorCorrectionLevel: errorCorrectionLevel as QRCodeErrorCorrectionLevel,
          color: {
            dark: colorDark,
            light: colorLight
          }
				},
				(err, url) => {
					if (err) console.error(err);
					qrUrl = url;
				}
			);
		}
	}
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<main class="w-full h-full flex flex-row gap-4">
	<section class="flex-1 p-4">
		{#if qrUrl}
			<img
				src={qrUrl}
				alt="The generated QR code"
				class="w-full h-full aspect-square object-contain"
				style="image-rendering: pixelated"
			/>
		{/if}
	</section>
	<section class="w-96 p-4 bg-stone-300 flex flex-col gap-4">
		<h1 class="text-2xl font-bold">QR Coder</h1>
		<textarea bind:value={content} class="w-full p-4 rounded-lg bg-stone-50" />
		<select bind:value={errorCorrectionLevel} class="w-full rounded-lg px-4 py-2 bg-stone-50">
			<option value="L">L - 7%</option>
			<option value="M">M - 15%</option>
			<option value="Q">Q - 25%</option>
			<option value="H">H - 30%</option>
		</select>
    <div class="flex flex-row gap-4">
      <input type="color" bind:value={colorDark} class="w-full rounded-lg bg-stone-50" />
      <input type="color" bind:value={colorLight} class="w-full rounded-lg bg-stone-50" />
    </div>
	</section>
</main>
