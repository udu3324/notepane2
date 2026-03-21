<script>
	import { onMount } from 'svelte';
	import Pane from '../Pane.svelte';
	import { page } from '$app/stores';
    
    export let data
    $: note = data?.note
    $: description = note?.markdown ? cutoff(note.markdown) : ''

    function cutoff(str) {
        if (str.length >= 157) {
            return `${str.substring(0, 157)}...`
        }

        return str.substring(0, 157)
    }

    onMount(() => {
        if (localStorage.theme === "dark") {
			document.documentElement.classList.add("dark")
		}
    })
</script>

<svelte:head>
    <meta name="description" content={description} />
    
    <meta property="og:type" content="article"/>
    <meta property="og:url" content={$page.url.origin + $page.url.pathname} />
    <meta property="og:description" content={description} />
    <meta property="article:published_time" content={note ? note.created_at : ''}/>
    <meta property="article:modified_time" content={note ? note.modified_at : ''}/>
</svelte:head>


<div class="grid h-screen w-screen place-content-center text-(--theme)">
    {#if note.error !== undefined}
    <span class="font-mono">{JSON.stringify(note)}</span>
    {:else}
    <Pane note={note} styleClasses="min-w-52 min-h-52 h-80 w-80"/>
    {/if}
</div>