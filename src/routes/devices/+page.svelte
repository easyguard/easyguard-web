<script lang="ts">
	import Card from "$lib/Card.svelte";
import { API_ROOT } from "$lib/consts";
	import { onMount } from "svelte";

	let leases: {
		[key: string]: {
			ip: string;
			mac: string;
			hostname: string;
			starts: string;
			ends: string;
		}
	} = {};

	async function fetchLeases() {
		await fetch(API_ROOT + "leases")
			.then(response => response.json())
			.then(data => {
				console.log(data);
				leases = data;
			});
	}

	async function fetchVendor(mac: string) {
		const data = await fetch(API_ROOT + "mac", {
			method: "POST",
			headers: {
				"Content-Type": "text/plain",
			},
			body: mac,
		})
			.then(response => response.text())
		console.log(data);
		return data;
	}

	onMount(() => {
		// fetchLeases();
	})
</script>

<Card width="100%" style="background-color: #ff5c5c; border-color: #ff2424; color: black;">
	<b>EasyGuard 2.0: Diese Funktion ist noch nicht verfügbar!</b>
</Card>
{#each Object.keys(leases) as lease}
	<Card>
		<h2>{leases[lease].hostname}</h2>
		<p>
			<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M12 3.5A7.5 7.5 0 0 0 4.5 11v.25a.75.75 0 0 1-1.5 0V11a9 9 0 0 1 18 0v.25a.75.75 0 0 1-1.5 0V11A7.5 7.5 0 0 0 12 3.5Z" fill="#ffffff"/><path d="M12 6.5A4.5 4.5 0 0 0 7.5 11v.25a.75.75 0 0 1-1.5 0V11a6 6 0 1 1 12 0v.25a.75.75 0 0 1-1.5 0V11A4.5 4.5 0 0 0 12 6.5Z" fill="#ffffff"/><path d="M9 11a3 3 0 1 1 3.75 2.905V16H18a3 3 0 1 1 0 6H6a3 3 0 1 1 0-6h5.25v-2.095A3.001 3.001 0 0 1 9 11Z" fill="#ffffff"/></svg>
			IP: {lease}
		</p>
		<p>
			<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M19.75 4A2.25 2.25 0 0 1 22 6.25v11.505a2.25 2.25 0 0 1-2.25 2.25H4.25A2.25 2.25 0 0 1 2 17.755V6.25A2.25 2.25 0 0 1 4.25 4h15.5Zm-10 8.5h-3.5a.75.75 0 0 0-.743.648l-.007.102v.492l.008.108C5.67 14.964 6.602 15.5 8 15.5c1.331 0 2.24-.486 2.464-1.494l.028-.155.008-.108v-.493a.75.75 0 0 0-.648-.743L9.75 12.5Zm8 .496h-4.498l-.102.007a.75.75 0 0 0 0 1.486l.102.007h4.498l.102-.007a.75.75 0 0 0 0-1.486l-.102-.007ZM8 8.502a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Zm9.75.998h-4.498l-.102.007a.75.75 0 0 0 0 1.486l.102.007h4.498l.102-.007a.75.75 0 0 0 0-1.486L17.75 9.5Z" fill="#ffffff"/></svg>
			MAC: {leases[lease].mac}
		</p>
		<p>
			<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M2.924 4.129A2.25 2.25 0 0 1 5.171 2h1.158a2.25 2.25 0 0 1 2.246 2.129l.838 15.5A2.25 2.25 0 0 1 7.167 22H4.333a2.25 2.25 0 0 1-2.247-2.371l.838-15.5ZM9.512 22c.6-.625.95-1.488.9-2.425L9.855 9.284l4.666-3.862A.75.75 0 0 1 15.75 6v4.24l4.98-4.781A.75.75 0 0 1 22 6v13.75A2.25 2.25 0 0 1 19.75 22H9.512ZM13 20.5h5V16a1 1 0 0 0-1-1h-3a1 1 0 0 0-1 1v4.5Z" fill="#ffffff"/></svg>
			Vendor:
			{#await fetchVendor(leases[lease].mac)}
				Loading...
			{:then vendor}
				{vendor}
			{:catch error}
				Unknown
			{/await}
		</p>
		<!-- <p>Starts: {leases[lease].starts}</p> -->
		<!-- <p>Ends: {leases[lease].ends}</p> -->
	</Card>
{/each}

<style>
	h2 {
		margin: 0;
	}

	p {
		display: flex;
		gap: 0.5rem;
	}
</style>