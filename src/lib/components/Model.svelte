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

<a title="made by @jassibuum">
	<spline-viewer
		loading-anim-type="spinner-small-light"
		url="https://prod.spline.design/aghPxpUjHieumGOR/scene.splinecode"
	></spline-viewer>
</a>
