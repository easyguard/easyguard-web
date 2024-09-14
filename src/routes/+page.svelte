<script>
	import Spinner from "$lib/Spinner.svelte";
	import Card from "$lib/Card.svelte";

	let checks = [
		{ name: "Firewall", status: "loading" },
		{ name: "DNS", status: "loading" },
		{ name: "Tunnel", status: "loading" }
	];

	function fiftyfifty() {
		return Math.random() < 0.8 ? "ok" : "fail";
	}

	setTimeout(() => {
		checks[0].status = fiftyfifty();
	}, 1000);

	setTimeout(() => {
		checks[1].status = fiftyfifty();
	}, 600);

	setTimeout(() => {
		checks[2].status = fiftyfifty();
	}, 1600);

</script>

<Card width="100%" style="background-color: #ff5c5c; border-color: #ff2424; color: black;">
	<b>EasyGuard 2.0: Startseite zeigt momentan Zufallswerte an.</b>
</Card>

<div id="status">
	{#if checks.some(({ status }) => status === "fail")}
		<!-- if any failed -->
		<svg width="200" height="200" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M12.45 2.15C14.992 4.057 17.587 5 20.25 5a.75.75 0 0 1 .75.75V11c0 5.001-2.958 8.676-8.725 10.948a.75.75 0 0 1-.55 0C5.958 19.676 3 16 3 11V5.75A.75.75 0 0 1 3.75 5c2.663 0 5.258-.943 7.8-2.85a.75.75 0 0 1 .9 0ZM12 16a.75.75 0 1 0 0 1.5.75.75 0 0 0 0-1.5Zm0-9.018a.75.75 0 0 0-.743.648l-.007.102v6.5l.007.102a.75.75 0 0 0 1.486 0l.007-.102v-6.5l-.007-.102A.75.75 0 0 0 12 6.982Z" fill="#db1c1c"/></svg>
		<h2>Es gibt Probleme!</h2>
	{:else if checks.filter(({ status }) => status === "ok").length === checks.length}
		<!-- if all passed -->
		<svg width="200" height="200" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M20.25 5c-2.663 0-5.258-.943-7.8-2.85a.75.75 0 0 0-.9 0C9.008 4.057 6.413 5 3.75 5a.75.75 0 0 0-.75.75V11c0 5.001 2.958 8.676 8.725 10.948a.75.75 0 0 0 .55 0C18.042 19.676 21 16 21 11V5.75a.75.75 0 0 0-.75-.75Zm-3.493 4.303-6 5.5a.75.75 0 0 1-1.037-.023l-2.5-2.5a.75.75 0 1 1 1.06-1.06l1.993 1.992 5.47-5.015a.75.75 0 0 1 1.014 1.106Z" fill="#20DC1C"/></svg>
		<h2>Keine Probleme erkannt!</h2>
	{:else}
		<!-- if still loading -->
		<Spinner size={200} />
	{/if}
</div>
<div id="checks">
	{#each checks as { name, status }}
		<div class="check">
			{#if status === "ok"}
				<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M12 2c5.523 0 10 4.477 10 10s-4.477 10-10 10S2 17.523 2 12 6.477 2 12 2Zm3.22 6.97-4.47 4.47-1.97-1.97a.75.75 0 0 0-1.06 1.06l2.5 2.5a.75.75 0 0 0 1.06 0l5-5a.75.75 0 1 0-1.06-1.06Z" fill="#ffffff"/></svg>
			{:else if status === "fail"}
				<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M12 2c5.523 0 10 4.477 10 10s-4.477 10-10 10S2 17.523 2 12 6.477 2 12 2Zm3.53 6.47-.084-.073a.75.75 0 0 0-.882-.007l-.094.08L12 10.939l-2.47-2.47-.084-.072a.75.75 0 0 0-.882-.007l-.094.08-.073.084a.75.75 0 0 0-.007.882l.08.094L10.939 12l-2.47 2.47-.072.084a.75.75 0 0 0-.007.882l.08.094.084.073a.75.75 0 0 0 .882.007l.094-.08L12 13.061l2.47 2.47.084.072a.75.75 0 0 0 .882.007l.094-.08.073-.084a.75.75 0 0 0 .007-.882l-.08-.094L13.061 12l2.47-2.47.072-.084a.75.75 0 0 0 .007-.882l-.08-.094-.084-.073.084.073Z" fill="#ffffff"/></svg>
			{:else}
				<Spinner size={24} />
			{/if}
			<span>{name}</span>
		</div>
	{/each}
</div>

<style>
	#status {
		display: flex;
		align-items: center;
		flex-direction: column;
		width: 100%;
	}

	.check {
		display: flex;
		align-items: center;
		gap: 10px;
	}
</style>