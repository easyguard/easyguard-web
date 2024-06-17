<script lang="ts">
	import RulesCard from "$lib/firewall/RulesCard.svelte";
	import { onMount } from "svelte";

	const API_ROOT = "http://10.10.99.1:8080/api/";
	
	let newRule = {
		port: 1,
		protocol: "tcp"
	};

	let firewall: {
		zones: {
			name: string,
			input: { ports: FirewallRule[] },
			output: { ports: FirewallRule[] },
			forward: {
				dest: string,
				ports: FirewallRule[]
			}[]
		}[]
	} = { zones: [] };
	let chain: FirewallRule[] | null = null;
	let fromZone = "";
	let toZone = "";

	function zoneToFriendlyName(zone: string) {
		switch (zone) {
			case "lan":
				return "Heimnetz";
			case "wan":
				return "Internet";
			case "guest":
				return "Gast";
			default:
				return zone;
		}
	}

	async function fetchFirewall() {
		await fetch(API_ROOT + "firewall")
			.then(response => response.json())
			.then(data => {
				console.log(data);
				// outgoingRules = data.ports.outgoing;
				firewall = data;
			});
	}

	onMount(() => {
		fetchFirewall();
	})

	function selectMenuUpdate() {
		requestAnimationFrame(() => {
			console.log("A");
			
			if(fromZone == "" || toZone == "") {
				console.log("B");
				chain = null;
				return;
			}
			console.log("C");
	
			if(fromZone == "easyguard") {
				console.log("D");
				chain = firewall.zones.find(zone => zone.name == toZone)?.output?.ports || null;
			} else if(toZone == "easyguard") {
				console.log("E");
				chain = firewall.zones.find(zone => zone.name == fromZone)?.input?.ports || null;
			} else {
				console.log("F");
				chain = firewall.zones.find(zone => zone.name == fromZone)?.forward.find(f => f.dest == toZone)?.ports || null;
			}
			console.log(chain);
		})
	}

	function addRule() {
		let zone = fromZone;
		let chain = toZone;
		let protocol = newRule.protocol;
		let port = newRule.port;

		if(fromZone == "easyguard") {
			zone = toZone;
			chain = "output";
		} else if(toZone == "easyguard") {
			zone = fromZone;
			chain = "input";
		}

		fetch(API_ROOT + "firewall/rule", {
			method: "PUT",
			headers: {
				"Content-Type": "application/json"
			},
			body: JSON.stringify({
			zone, chain, rule: {protocol, port}
			})
		}).then(async res => {
			await fetchFirewall();
			selectMenuUpdate();
		})
	}

	function deleteRule(e: { detail: { protocol: string; port: number; } }) {
		const rule = e.detail;
		let zone = fromZone;
		let chain = toZone;
		let protocol = rule.protocol;
		let port = rule.port;

		if(fromZone == "easyguard") {
			zone = toZone;
			chain = "output";
		} else if(toZone == "easyguard") {
			zone = fromZone;
			chain = "input";
		}

		fetch(API_ROOT + "firewall/rule", {
			method: "DELETE",
			headers: {
				"Content-Type": "application/json"
			},
			body: JSON.stringify({
			zone, chain, rule: {protocol, port}
			})
		}).then(async res => {
			await fetchFirewall();
			selectMenuUpdate();
		})
	}
</script>

<!-- [
	{ name: "HTTP", ports: [{ port: 80, protocol: "tcp" }] },
	{ name: "HTTPS", ports: [{ port: 443, protocol: "tcp" }] },
	{ name: "SMTP", ports: [{ port: 25, protocol: "tcp" }, { port: 465, protocol: "tcp" }] },
] -->

<span>
	<select on:change={selectMenuUpdate} bind:value={fromZone}>
		<option value="" hidden selected>---</option>
		{#each firewall.zones as zone}
			<option value={zone.name}>{zoneToFriendlyName(zone.name)}</option>
		{/each}
		<option value="easyguard">EasyGuard</option>
	</select>
	nach
	<select on:change={selectMenuUpdate} bind:value={toZone}>
		<option value="" hidden selected>---</option>
		{#each firewall.zones as zone}
			<option value={zone.name}>{zoneToFriendlyName(zone.name)}</option>
		{/each}
		<option value="easyguard">EasyGuard</option>
	</select>
</span>

{#if chain}
	<RulesCard rules={chain} bind:newRule on:click={addRule} on:delete={deleteRule}>
		<svg slot="card-icon" width="40" height="40" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M2.75 4.504a.75.75 0 0 1 .743.648l.007.102v13.499a.75.75 0 0 1-1.493.101L2 18.753v-13.5a.75.75 0 0 1 .75-.75Zm12.46 1.883.083-.094a1 1 0 0 1 1.32-.083l.094.083 4.997 4.998a1 1 0 0 1 .083 1.32l-.083.093-4.996 5.004a1 1 0 0 1-1.499-1.32l.083-.094L18.581 13H6a1 1 0 0 1-.993-.883L5 12a1 1 0 0 1 .883-.993L6 11h12.584l-3.291-3.293a1 1 0 0 1-.083-1.32l.083-.094-.083.094Z" fill="#ffffff"/></svg>
		<span slot="card-name">Firewallregeln</span>
	</RulesCard>
{:else}
	<span>Keine Regeln gefunden oder ausgewählt.</span>
{/if}

<!-- <RulesCard rules={outgoingRules} bind:newRule={newOutgoingRule} on:click={() => {
	fetch(API_ROOT + "firewall/outgoing", {
		method: "PUT",
		headers: {
			"Content-Type": "application/json"
		},
		body: JSON.stringify(newOutgoingRule)
	})
		.then(res => {
			location.reload();
		});
}}>
	<svg slot="card-icon" width="40" height="40" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M2.75 4.504a.75.75 0 0 1 .743.648l.007.102v13.499a.75.75 0 0 1-1.493.101L2 18.753v-13.5a.75.75 0 0 1 .75-.75Zm12.46 1.883.083-.094a1 1 0 0 1 1.32-.083l.094.083 4.997 4.998a1 1 0 0 1 .083 1.32l-.083.093-4.996 5.004a1 1 0 0 1-1.499-1.32l.083-.094L18.581 13H6a1 1 0 0 1-.993-.883L5 12a1 1 0 0 1 .883-.993L6 11h12.584l-3.291-3.293a1 1 0 0 1-.083-1.32l.083-.094-.083.094Z" fill="#ffffff"/></svg>
	<span slot="card-name">Ausgehend</span>
</RulesCard>

<RulesCard rules={[
	{ name: "WireGuard", ports: [{ port: 54321, protocol: "udp" }] },
]}>
	<svg slot="card-icon" width="40" height="40" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M21.25 4.5a.75.75 0 0 1 .743.648L22 5.25v13.5a.75.75 0 0 1-1.493.102l-.007-.102V5.25a.75.75 0 0 1 .75-.75Zm-9.04 1.887.083-.094a1 1 0 0 1 1.32-.083l.094.083 4.997 4.998a1 1 0 0 1 .083 1.32l-.083.093-4.996 5.004a1 1 0 0 1-1.499-1.32l.083-.094L15.581 13H3a1 1 0 0 1-.993-.883L2 12a1 1 0 0 1 .883-.993L3 11h12.584l-3.291-3.293a1 1 0 0 1-.083-1.32l.083-.094-.083.094Z" fill="#ffffff"/></svg>
	<span slot="card-name">Eingehend</span>
</RulesCard> -->