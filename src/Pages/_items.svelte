<script>
    import Card from '../Components/Card.svelte';
    import Title from '../Components/Title.svelte';
    import Button from '../Components/Button.svelte';
    import TextMessage from '../Components/TextMessage.svelte';
    
    let item = '';
    let itemData;
    let isError;

    // Function to get information for a given item
    const getItemInfo = async () => {
        isError = false;
        const res = await fetch(`https://pokeapi.co/api/v2/item/${item.toLowerCase().trim().replaceAll(' ', '-')}`);
        if (res.ok)
            itemData = await res.json();
        else
            isError = true;
    }
</script>

<!-- Item search page -->
<div>
    <Title title={'Search Item'} />
    <form on:submit|preventDefault={getItemInfo}>
        <div class="flex flex-col items-center">
            <input bind:value={item} type="text" class="w-11/12 text-gray-700 p-1 border-0 border-b-2 border-blue-600 outline-none" placeholder="Enter Item Name" required />
            <Button name={'Search Item'} />
        </div>
    </form>
    <div>
        {#if !isError}
            {#if !itemData}
                <TextMessage color={'text-gray-900'} message={'Searching For Your Pokemon Item...'} />
            {:else}
                <TextMessage color={'text-green-700'} message={'Pokemon Item Found !'} />
                <Card>
                    <p class="text-center text-md font-bold text-gray-900">
                        {itemData.name.charAt(0).toUpperCase() + itemData.name.replaceAll('-', ' ').slice(1)}
                    </p>
                    <img class="w-1/4 block mx-auto" src={itemData.sprites.default} alt={itemData.name} />
                    <p class="text-center text-sm text-gray-700 font-bold mb-1">
                        ${itemData.cost}
                    </p>
                    <p class="text-center text-gray-900 mb-1">
                        Description
                    </p>
                    <p class="text-center text-gray-700 text-sm">
                        {itemData.effect_entries[0].effect}
                    </p>
                </Card>
            {/if}
        {:else}
            <TextMessage color={'text-red-700'} message={'Pokemon Item Unknown...'} />
        {/if}
    </div>
</div>