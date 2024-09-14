<script lang="ts">
	import Card from "$lib/Card.svelte";
	import Chip from "$lib/Chip.svelte";
    import { API_ROOT } from "$lib/consts";
	import { onMount } from "svelte";

	let dns: { protectionLevel: "normal" | "high" | "aggressive", tif: boolean, badware: boolean, nrd: boolean, extra: boolean } = { protectionLevel: "normal", tif: true, badware: true, nrd: true, extra: false };

	async function fetchDNS() {
		await fetch(API_ROOT + "dns")
			.then(response => response.json())
			.then(data => {
				console.log(data);
				dns = data;
			});
	}

	async function patchDNS(option: string, value: string | boolean) {
		await fetch(API_ROOT + "dns", {
			method: "PATCH",
			headers: {
				"Content-Type": "application/json"
			},
			body: JSON.stringify({ option, value: value.toString() })
		});
	}

	function checkboxChange(e: Event) {
		if(!e.target) return void alert("Error: No target found.");
		const checkbox = e.target;
		if(!(checkbox instanceof HTMLInputElement)) return void alert("Error: Target is not an input element.");
		const checkboxId = checkbox.id;
		const checkboxState = checkbox.checked;
		patchDNS(checkboxId, checkboxState);
	}

	function selectChange(e: Event) {
		if(!e.target) return void alert("Error: No target found.");
		const checkbox = e.target;
		if(!(checkbox instanceof HTMLSelectElement)) return void alert("Error: Target is not a select element.");
		const id = checkbox.id;
		const value = checkbox.value;
		patchDNS(id, value);
	}

	onMount(() => {
		// fetchDNS();
	})
</script>

<!-- <Card width="100%">
	<div class="card-title">
		<svg width="40" height="40" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M10 4a2 2 0 1 1 4 0v1.24a8.24 8.24 0 0 1 6.25 8.008.75.75 0 0 1-.75.75h-15a.75.75 0 0 1-.75-.75c0-3.86 2.668-7.098 6.25-7.999V4Zm3.25 3.5a.75.75 0 0 0 0 1.5c1.253 0 2.143.618 2.886 1.68a.75.75 0 0 0 1.229-.86c-.93-1.33-2.229-2.32-4.115-2.32ZM2 17a2 2 0 0 1 2-2h16a2 2 0 1 1 0 4H4a2 2 0 0 1-2-2Z" fill="#ffffff"/></svg>
		Gesperrte Dienste
	</div>
	<div class="blocked" style="display: flex; gap: 5px; flex-wrap: wrap; margin-bottom: 10px; margin-top: 10px;">
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
		<Chip>Amazon</Chip>
	</div>
	<button>
		<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M13.94 5 19 10.06 9.062 20a2.25 2.25 0 0 1-.999.58l-5.116 1.395a.75.75 0 0 1-.92-.921l1.395-5.116a2.25 2.25 0 0 1 .58-.999L13.938 5Zm7.09-2.03a3.578 3.578 0 0 1 0 5.06l-.97.97L15 3.94l.97-.97a3.578 3.578 0 0 1 5.06 0Z" fill="#ffffff"/></svg>
	</button>
</Card> -->

<Card width="100%" style="background-color: #ff5c5c; border-color: #ff2424; color: black;">
	Betafunktion: Diese Funktion ist noch in der Entwicklung und kann Fehler enthalten.<br>
	<b>EasyGuard 2.0: Diese Funktion ist noch nicht einstellbar!!</b>
</Card>

<Card width="100%">
	<div class="card-title">
		<svg width="40" height="40" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M12 8V2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V10h-6a2 2 0 0 1-2-2Zm-5 4.25a.75.75 0 1 1 1.5 0 .75.75 0 0 1-1.5 0Zm0 3a.75.75 0 1 1 1.5 0 .75.75 0 0 1-1.5 0Zm0 3a.75.75 0 1 1 1.5 0 .75.75 0 0 1-1.5 0Zm3-6a.75.75 0 0 1 .75-.75h5.5a.75.75 0 0 1 0 1.5h-5.5a.75.75 0 0 1-.75-.75Zm0 3a.75.75 0 0 1 .75-.75h5.5a.75.75 0 0 1 0 1.5h-5.5a.75.75 0 0 1-.75-.75Zm0 3a.75.75 0 0 1 .75-.75h5.5a.75.75 0 0 1 0 1.5h-5.5a.75.75 0 0 1-.75-.75ZM13.5 8V2.5l6 6H14a.5.5 0 0 1-.5-.5Z" fill="#ffffff"/></svg>
		Sperrungen
	</div>
	<div style="margin-top: 10px;">
		<span>Schutzlevel:</span>
		<select id="protectionLevel" on:change={selectChange} disabled>
			<option value="normal" selected={dns.protectionLevel == "normal"}>Normal</option>
			<option value="high" selected={dns.protectionLevel == "high"}>Hoch (empfohlen)</option>
			<option value="aggressive" selected={dns.protectionLevel == "aggressive"}>Aggressiv</option>
		</select>
	</div>
	<span>Zusatz:</span>
	<div>
		<input type="checkbox" id="tif" checked={dns.tif} on:change={checkboxChange} disabled>
		<label for="tif">Thread Intelligence Feeds (empfohlen)</label>
	</div>
	<div>
		<input type="checkbox" id="badware" checked={dns.badware} on:change={checkboxChange} disabled>
		<label for="badware">Badware Hoster (empfohlen)</label>
	</div>
	<div>
		<input type="checkbox" id="nrd" checked={dns.nrd} on:change={checkboxChange} disabled>
		<label for="nrd">Newly Registered Domains (empfohlen)</label>
	</div>
	<div>
		<input type="checkbox" id="extra" checked={dns.extra} on:change={checkboxChange} disabled>
		<label for="extra">EasyGuard Extra (empfohlen)</label>
	</div>
	<!-- <div style="display: flex; gap: 5px; flex-wrap: wrap; margin-bottom: 10px; margin-top: 10px;">
		<Chip>AdGuard Official</Chip>
		<Chip>AdGuard Official</Chip>
		<Chip>AdGuard Official</Chip>
		<Chip>AdGuard Official</Chip>
		<Chip>AdGuard Official</Chip>
		<Chip>AdGuard Official</Chip>
	</div> -->
</Card>

<!-- 
# zip und mov TLDs
||zip^$important
||mov^$important
# Alle punycode domains
/.*(xn--).*/
# Suchmaschinen-schrott
||softonic.com^
||uptodown.com^
||malavida.com^ -->

<Card width="100%">
	<div class="card-title">
		<svg width="40" height="40" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="m8.086 18.611 5.996-14.004a1 1 0 0 1 1.878.677l-.04.11-5.996 14.004a1 1 0 0 1-1.878-.677l.04-.11 5.996-14.004L8.086 18.61Zm-5.793-7.318 4-4a1 1 0 0 1 1.497 1.32l-.083.094L4.414 12l3.293 3.293a1 1 0 0 1-1.32 1.498l-.094-.084-4-4a1 1 0 0 1-.083-1.32l.083-.094 4-4-4 4Zm14-4.001a1 1 0 0 1 1.32-.083l.093.083 4.001 4.001a1 1 0 0 1 .083 1.32l-.083.095-4.001 3.995a1 1 0 0 1-1.497-1.32l.084-.095L19.584 12l-3.293-3.294a1 1 0 0 1 0-1.414Z" fill="#ffffff"/></svg>
		Extraeinträge (Fortgeschritten)
	</div>
	<textarea rows=18 disabled>
</textarea>
	<button>
		<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M6.75 3h-1A2.75 2.75 0 0 0 3 5.75v12.5A2.75 2.75 0 0 0 5.75 21H6v-6a2.25 2.25 0 0 1 2.25-2.25h7.5A2.25 2.25 0 0 1 18 15v6h.25A2.75 2.75 0 0 0 21 18.25V8.286a3.25 3.25 0 0 0-.952-2.299l-2.035-2.035A3.25 3.25 0 0 0 15.75 3v4.5a2.25 2.25 0 0 1-2.25 2.25H9A2.25 2.25 0 0 1 6.75 7.5V3Z" fill="#ffffff"/><path d="M14.25 3v4.5a.75.75 0 0 1-.75.75H9a.75.75 0 0 1-.75-.75V3h6ZM16.5 21v-6a.75.75 0 0 0-.75-.75h-7.5a.75.75 0 0 0-.75.75v6h9Z" fill="#ffffff"/></svg>
	</button>
</Card>