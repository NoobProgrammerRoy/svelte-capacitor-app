<script>
    import Card from '../Components/Card.svelte';
    import Title from '../Components/Title.svelte';
    import Button from '../Components/Button.svelte';
    import TextMessage from '../Components/TextMessage.svelte';
    import { pokemonTypes } from '../pokemonTypes';
    
    let move = '';
    let moveData;
    let isError;

    // Function to get information for a given move
    const getMoveInfo = async () => {
        isError = false;
        const res = await fetch(`https://pokeapi.co/api/v2/move/${move.toLowerCase().trim().replaceAll(' ', '-')}`);
        if (res.ok)
            moveData = await res.json();
        else
            isError = true;
    }
</script>

<!-- Move search page -->
<div>
    <Title title={'Search Move'} />
    <form on:submit|preventDefault={getMoveInfo}>
        <div class="flex flex-col items-center">
            <input bind:value={move} type="text" class="w-11/12 text-gray-700 p-1 border-0 border-b-2 border-blue-600 outline-none" placeholder="Enter Move Name" required />
            <Button name={'Search Move'} />
        </div>
    </form>
    <div>
        {#if !isError}
            {#if !moveData}
                <TextMessage color={'text-gray-900'} message={'Searching For Your Pokemon Move...'} />
            {:else}
                <TextMessage color={'text-green-700'} message={'Watch Out For That Attack !'} />
                <Card>
                    <p class="text-center text-md font-bold text-gray-900">
                        {moveData.name.charAt(0).toUpperCase() + moveData.name.replaceAll('-', ' ').slice(1)}
                    </p>
                    <div class="flex flex-row justify-center items-center mx-auto">
                        <p class="px-2 py-1 rounded-md m-1 text-gray-50 font-bold" style="background-color: {pokemonTypes[moveData.type.name]};">
                            {moveData.type.name.charAt(0).toUpperCase() + moveData.type.name.replaceAll('-', ' ').slice(1)}
                        </p>
                    </div>
                    <table class="mx-auto my-1">
                        <tr>
                            <td class="text-gray-900">Power</td>
                            <td class="text-gray-700 text-sm font-bold pl-2">{moveData.power}</td>
                        </tr>
                        <tr>
                            <td class="text-gray-900">Accuracy</td>
                            <td class="text-gray-700 text-sm font-bold pl-2">{moveData.accuracy} %</td>
                        </tr>
                        <tr>
                            <td class="text-gray-900">PP</td>
                            <td class="text-gray-700 text-sm font-bold pl-2">{moveData.pp}</td>
                        </tr>
                        <tr>
                            <td class="text-gray-900">Priority</td>
                            <td class="text-gray-700 text-sm font-bold pl-2">{moveData.priority}</td>
                        </tr>
                    </table>
                    <p class="text-center text-gray-900 mb-1">
                        Description
                    </p>
                    <p class="text-center text-gray-700 text-sm">
                        {moveData.effect_entries[0].effect}
                    </p>
                </Card>
            {/if}
        {:else}
            <TextMessage color={'text-red-700'} message={'Pokemon Move Unknown...'} />
        {/if}
    </div>
</div>