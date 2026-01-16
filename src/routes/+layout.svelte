<script lang="ts">
    import './layout.css';
    import favicon from '$lib/assets/favicon.svg';
    import 'xp.css/dist/XP.css';

    let { children } = $props();
</script>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>
<div style="height: 100dvh; width: 100dvw;">
    {@render children()}
</div>
