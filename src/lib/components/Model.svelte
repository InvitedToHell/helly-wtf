<script>
	import { onMount } from 'svelte';
	let logoElement;

	onMount(() => {
		const script = document.createElement('script');
		script.type = 'module';
		script.src = './spline-viewer.js';
		document.head.appendChild(script);
	});
</script>

<spline-viewer loading="eager" url="https://prod.spline.design/aghPxpUjHieumGOR/scene.splinecode"
></spline-viewer>
