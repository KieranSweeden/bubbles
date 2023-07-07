<script lang="ts">
	import { Auth } from '@supabase/auth-ui-svelte';
	import { ThemeSupa } from '@supabase/auth-ui-shared';

	export let data;
</script>

<svelte:head>
	<title>Bubbles</title>
</svelte:head>

<div class="row flex-center flex">
	<div class="col-6 form-widget">
		<Auth
			supabaseClient={data.supabase}
			providers={['google']}
			onlyThirdPartyProviders={true}
            appearance={{ theme: ThemeSupa }}
            theme='dark'
            additionalData={{ }}
		/>
	</div>
</div>
