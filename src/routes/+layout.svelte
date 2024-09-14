<script>
	import "$lib/style.css";
	import Header from "./Header.svelte";
	import Nav from "../lib/nav/Nav.svelte";
	import { onMount } from "svelte";

	onMount(() => {
		const dialog = document.getElementById("dialog");
		// @ts-ignore
		dialog.showModal();
	})
</script>

<dialog id="dialog" style="border: none; border-radius: 15px; background-color: #1d1d1d; color: white; outline: none;">
	<h2>Willkommen bei EasyGuard 2.0 BETA</h2>
	<p>EasyGuard 2.0 ist noch in der Entwicklung und kann Fehler enthalten.</p>
	<p>Bitte nicht als einzigen Router produktiv einsetzen, sonst knallt es irgendwann und du bist traurig.</p>
	<p>Funktionierende Funktionen dieser funktionalen Software:</p>
	<pre>Firewall (Limes) - EZGDISP (S1 Display) - DNS Blocker (Blocky, noch nicht einstellbar!) - DHCP (ebenfalls nicht einstellbar)</pre>
	<p>Wichtige, noch fehlende Funktionen (ausschnitt):</p>
	<pre>WireGuard - Geräte mit MacID (Mac Address Info) - EasyGuard Controller - OOBE</pre>

	<button on:click={() => {
		const dialog = document.getElementById("dialog");
		// @ts-ignore
		dialog.close();
	}}>Verstanden</button>
</dialog>

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
	#dialog::backdrop {
		background-color: rgba(0, 0, 0, 0.5);
	}
</style>