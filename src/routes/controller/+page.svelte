<script>
	import Card from "$lib/Card.svelte";

	const controllerAdmin = "Max Mustermann";
	const controllerIP = "123.123.123.123";
</script>

<Card width="100%" style="background-color: #ff5c5c; border-color: #ff2424; color: black;">
	<b>EasyGuard 2.0: Diese Funktion ist noch nicht verfügbar!</b>
	<span>Die Werte entsprechen aktuell nicht der Wahrheit!</span>
</Card>

<h2 style="margin: 0;">Diese EasyGuard-Firewall wird von einem externen Server verwaltet.</h2>
<span>Betreiber des Controllers: {controllerAdmin}</span>
<span style="display: flex; align-items: center; gap: 5px;">IP-Adresse des Controllers: <pre style="margin: 0;">{controllerIP}</pre></span>
<span>Zugewiesener IP-Bereich vom Controller: 10.10.14.0/24</span>
<button style="display: flex; align-items: center; gap: 10px; padding: 15px; width: fit-content;" class="bg-primary">
	<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M6.25 4.75a1.5 1.5 0 0 0-1.5 1.5v11.5a1.5 1.5 0 0 0 1.5 1.5h11.5a1.5 1.5 0 0 0 1.5-1.5v-4a1 1 0 1 1 2 0v4a3.5 3.5 0 0 1-3.5 3.5H6.25a3.5 3.5 0 0 1-3.5-3.5V6.25a3.5 3.5 0 0 1 3.5-3.5h4a1 1 0 1 1 0 2h-4Zm6.5-1a1 1 0 0 1 1-1h6.5a1 1 0 0 1 1 1v6.5a1 1 0 1 1-2 0V6.164l-4.793 4.793a1 1 0 1 1-1.414-1.414l4.793-4.793H13.75a1 1 0 0 1-1-1Z" fill="#ffffff"/></svg>
	Zur Controllerverwaltung
</button>