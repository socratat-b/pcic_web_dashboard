<script>
	import { invalidate } from '$app/navigation';
	import { onMount } from 'svelte';
	import modeobserver from '$lib/utils/modeobserver';
	export let data;

	$: ({ session, supabase } = data);

	onMount(() => {
		const { data } = supabase.auth.onAuthStateChange((_, newSession) => {
			if (newSession?.expires_at !== session?.expires_at) {
				invalidate('supabase:auth');
			}
		});

		return () => data.subscription.unsubscribe();
	});

	onMount(modeobserver);
</script>

<slot />
