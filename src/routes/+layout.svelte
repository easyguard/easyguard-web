<script>
	import "$lib/style.css";
    import Header from "./Header.svelte";
    import Nav from "../lib/nav/Nav.svelte";
</script>

<div class="app">
	<Header />
	<div style="display: flex; gap: 25px;">
		<Nav />
	
		<main>
			<slot></slot>
		</main>	
	</div>
</div>

<style>
	main {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		gap: 15px;
	}
</style>