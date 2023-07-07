<script lang="ts">
	import '@skeletonlabs/skeleton/themes/theme-skeleton.css';
	import '@skeletonlabs/skeleton/styles/skeleton.css';
	import '../app.css';

	import { AppBar, AppShell } from '@skeletonlabs/skeleton';

	import { invalidate } from '$app/navigation';
	import { onMount } from 'svelte';

	export let data;

	let { supabase, session } = data;
	$: ({ supabase, session } = data);

	onMount(() => {
		const { data } = supabase.auth.onAuthStateChange((event, _session) => {
			if (_session?.expires_at !== session?.expires_at) {
				invalidate('supabase:auth');
			}
		});

		return () => data.subscription.unsubscribe();
	});

	async function handleSignIn() {
		const { data, error } = await supabase.auth.signInWithOAuth({
			provider: 'google'
		});
	}

	async function handleSignOut() {
		const { error } = await supabase.auth.signOut();
	}

</script>

<svelte:head>
	<title>User Management</title>
</svelte:head>

<AppShell>
	<svelte:fragment slot="pageHeader">
		<AppBar slotDefault="place-self-center" slotTrail="place-content-end">
			<svelte:fragment slot="lead">
				<a href="/">Bubbles</a>
			</svelte:fragment>
			<svelte:fragment slot="trail">
				{#if session}
					<a href="/account">Account</a>
					<button class="btn btn-sm variant-filled" on:click={handleSignOut}>Sign out</button>
				{:else}
					<button class="btn btn-sm variant-filled" on:click={handleSignIn}>Sign in</button>
				{/if}
			</svelte:fragment>
		</AppBar>
	</svelte:fragment>
	<slot />
</AppShell>
