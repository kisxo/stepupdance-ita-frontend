<script>
	import '../app.css';
	import '../../static/custom-theme.css';

	let { children } = $props();
	import Navbar from './Navbar.svelte';
</script>

<Navbar/>
{@render children()}
