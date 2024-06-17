<script lang="ts">
	import Card from "$lib/Card.svelte";
	import ports from "$lib/ports.js";
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	/**
	 * @type {Array<{name: string, ports: Array<{port: number, protocol: "tcp" | "udp"}>}>}
	 */
	// export let rules = [
	// 	{ name: "HTTP", ports: [{ port: 80, protocol: "tcp" }] },
	// 	{ name: "HTTP", ports: [{ port: 80, protocol: "tcp" }] }
	// ];
	export let rules: FirewallRule[] = [];

	export let newRule = {
		port: 80,
		protocol: "tcp"
	};
</script>

<Card width="100%">
	<div class="card-title">
		<slot name="card-icon"></slot>
		<slot name="card-name"></slot>
	</div>
	<table style="width: 100%;">
		<tr>
			<th>Beschreibung</th>
			<th>Port</th>
			<th></th>
		</tr>
		{#each rules as rule}
			<tr>
				<td>{ports[rule.port] || ""}</td>
				<td>
					{rule.protocol != "icmp" ? rule.port + "/" + rule.protocol : rule.protocol}
				</td>
				<td>
					<button class="bg-danger" on:click={() => {
						dispatch("delete", rule)
					}}>
						<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M21.5 6a1 1 0 0 1-.883.993L20.5 7h-.845l-1.231 12.52A2.75 2.75 0 0 1 15.687 22H8.313a2.75 2.75 0 0 1-2.737-2.48L4.345 7H3.5a1 1 0 0 1 0-2h5a3.5 3.5 0 1 1 7 0h5a1 1 0 0 1 1 1Zm-7.25 3.25a.75.75 0 0 0-.743.648L13.5 10v7l.007.102a.75.75 0 0 0 1.486 0L15 17v-7l-.007-.102a.75.75 0 0 0-.743-.648Zm-4.5 0a.75.75 0 0 0-.743.648L9 10v7l.007.102a.75.75 0 0 0 1.486 0L10.5 17v-7l-.007-.102a.75.75 0 0 0-.743-.648ZM12 3.5A1.5 1.5 0 0 0 10.5 5h3A1.5 1.5 0 0 0 12 3.5Z" fill="#ffffff"/></svg>
					</button>
				</td>
			</tr>
		{/each}
		<!-- <tr>
			<td>HTTP</td>
			<td>80/tcp, 443/tcp</td>
			<td>
				<button class="bg-danger">
					<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M21.5 6a1 1 0 0 1-.883.993L20.5 7h-.845l-1.231 12.52A2.75 2.75 0 0 1 15.687 22H8.313a2.75 2.75 0 0 1-2.737-2.48L4.345 7H3.5a1 1 0 0 1 0-2h5a3.5 3.5 0 1 1 7 0h5a1 1 0 0 1 1 1Zm-7.25 3.25a.75.75 0 0 0-.743.648L13.5 10v7l.007.102a.75.75 0 0 0 1.486 0L15 17v-7l-.007-.102a.75.75 0 0 0-.743-.648Zm-4.5 0a.75.75 0 0 0-.743.648L9 10v7l.007.102a.75.75 0 0 0 1.486 0L10.5 17v-7l-.007-.102a.75.75 0 0 0-.743-.648ZM12 3.5A1.5 1.5 0 0 0 10.5 5h3A1.5 1.5 0 0 0 12 3.5Z" fill="#ffffff"/></svg>
				</button>
			</td>
		</tr>
		<tr>
			<td>HTTP</td>
			<td>80/tcp, 443/tcp</td>
			<td>
				<button class="bg-danger">
					<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M21.5 6a1 1 0 0 1-.883.993L20.5 7h-.845l-1.231 12.52A2.75 2.75 0 0 1 15.687 22H8.313a2.75 2.75 0 0 1-2.737-2.48L4.345 7H3.5a1 1 0 0 1 0-2h5a3.5 3.5 0 1 1 7 0h5a1 1 0 0 1 1 1Zm-7.25 3.25a.75.75 0 0 0-.743.648L13.5 10v7l.007.102a.75.75 0 0 0 1.486 0L15 17v-7l-.007-.102a.75.75 0 0 0-.743-.648Zm-4.5 0a.75.75 0 0 0-.743.648L9 10v7l.007.102a.75.75 0 0 0 1.486 0L10.5 17v-7l-.007-.102a.75.75 0 0 0-.743-.648ZM12 3.5A1.5 1.5 0 0 0 10.5 5h3A1.5 1.5 0 0 0 12 3.5Z" fill="#ffffff"/></svg>
				</button>
			</td>
		</tr> -->
		<tr>
			<td>
				<!-- <input type="text" placeholder="Neue Regel..." bind:value={newRule.name}> -->
			</td>
			<td>
				<input type="number" bind:value={newRule.port} style="width: 5ch !important; -moz-appearance: textfield;" max=65535, min=1>
				<select bind:value={newRule.protocol}>
					<option value="tcp">TCP</option>
					<option value="udp">UDP</option>
				</select>
			</td>
			<td>
				<button class="bg-primary" on:click>Anwenden</button>
			</td>
		</tr>
	</table>
</Card>