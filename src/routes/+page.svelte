<svelte:head>
    <link rel="stylesheet" href="https://unpkg.com/@picocss/pico@latest/css/pico.min.css">
</svelte:head>

<script>
	import { each } from "svelte/internal";
    import jsonWikiList from '../smileys.json';
	import aubin from ../aubin.svelte;
    let wikiList = jsonWikiList;

    let inTitle = '';
    let inUrl = '';
    let inDesc = '';
    let inRarity = '';

    function addSmiley() {
        wikiList = [...wikiList, {id: Math.max(...wikiList.map(smiley => smiley.id)) + 1, image: inUrl, title: inTitle, rarity: inRarity, description: inDesc, edit: false}]
    }

    function setEditing(i, isEditing) {
        wikiList[i].edit = isEditing;
    }

    function deleteSmiley(i) {
        wikiList.splice(i, 1);
        wikiList = wikiList;
    }
</script>

<!-- Le haut -->
<div style="margin: 0 auto; padidng: 20px; max-width: 900px; margin-bottom: 50px;">
    <h1 style="text-align: center;">Smiley wiki</h1>
    <p>Ajoute ton smiley</p>

    <div style="display: flex;">
        <input type="text" placeholder="Nom" bind:value={inTitle}>
        <input type="text" placeholder="URL" bind:value={inUrl}>
        <input type="text" placeholder="Description" bind:value={inDesc}>
        <input type="text" placeholder="Rareté" bind:value={inRarity}>
        <button style="width: 200px" on:click={addSmiley}>👌</button>
    </div>
</div>

<!-- Le milieu -->
<div style="display: flex; align-items: center; justify-content: center; max-width: 1500px; margin: 0 auto; flex-flow: wrap;">
    {#each wikiList as smiley, i}
        <article style="padding: 30px; margin: 30px; justify-content: center; display: flex; align-items: center; flex-direction: column">
            {#if smiley.edit}
            <input type="text" bind:value={smiley.title}>
            <input type="text" bind:value={smiley.rarity}>
            <input type="text" bind:value={smiley.image}>
            <input type="text" bind:value={smiley.description}>
            <button on:click={() => setEditing(i, false)}>Valider</button>
            <button on:click={() => deleteSmiley(i)}>Supprimer</button>

            {:else}
            <h4 style="max-width: 200px; text-align: center;">#{smiley.id} : {smiley.title}</h4>
            <img style="max-width: 200px; max-height: 200px; align: center;" alt="Image du smiley {smiley.title}" src="{smiley.image}">
            <h4 style="max-width: 200px; text-align: center;">{smiley.rarity}</h4>
            <p style="max-width: 200px; text-align: center">{smiley.description}</p>
            <button on:click={() => setEditing(i, true)}>Modifier</button>
		<aubin />
            
            {/if}
        </article>
    {/each}
</div>
