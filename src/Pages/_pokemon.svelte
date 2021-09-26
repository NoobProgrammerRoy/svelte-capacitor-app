<script>
    import Card from '../Components/Card.svelte';
    import Title from '../Components/Title.svelte';
    import Button from '../Components/Button.svelte';
    import TextMessage from '../Components/TextMessage.svelte';
    import { pokemonTypes } from '../pokemonTypes';

    let pokemon = '';
    let pokemonData;
    let isError;

    // Function to get information for a given pokemon
    const getPokemonInfo = async () => {
        isError = false;
        const res = await fetch(`https://pokeapi.co/api/v2/pokemon/${pokemon.toLowerCase().trim()}`);
        if (res.ok)
            pokemonData = await res.json();
        else
            isError = true;
    }
</script>

<!-- Pokemon search page -->
<div>
    <Title title={'Search Pokemon'} />
    <form on:submit|preventDefault={getPokemonInfo}>
        <div class="flex flex-col items-center">
            <input bind:value={pokemon} type="text" class="w-11/12 text-gray-700 p-1 border-0 border-b-2 border-blue-600 outline-none" placeholder="Enter Pokemon Name" required />
            <Button name={'Search Pokemon'}/>
        </div>
    </form>
    <div>
        {#if !isError}
            {#if !pokemonData}
                <TextMessage color={'text-gray-700'} message={'Searching For Your Favorite Pokemon...'} />
            {:else}
                <TextMessage color={'text-green-700'} message={"Who's That Pokemon ?" } />
                <Card>
                    <p class="text-center text-md font-bold text-gray-900">
                        {pokemonData.name.charAt(0).toUpperCase() + pokemonData.name.slice(1)}
                    </p>
                    <img class="w-1/2 block mx-auto" src={pokemonData.sprites.front_default} alt={pokemonData.name} />
                    <p class="text-center text-sm text-gray-700 font-bold mb-1">
                        # {pokemonData.order}
                    </p>
                    <div class="flex flex-row justify-center items-center mx-auto">
                        {#each pokemonData.types as type}
                            <p class="px-2 py-1 rounded-md mx-1 text-gray-50 font-bold" style="background-color: {pokemonTypes[type.type.name]};">
                                {type.type.name.charAt(0).toUpperCase() + type.type.name.slice(1)}
                            </p>
                        {/each}
                    </div>
                    <div class="flex flex-row justify-between items-center my-1">
                        <p class="text-gray-900">Height : 
                            <span class="text-gray-700 text-sm font-bold">
                                {pokemonData.height / 10} m
                            </span>
                        </p>
                        <p class="text-gray-900">Weight : 
                            <span class="text-gray-700 text-sm font-bold">
                                {pokemonData.weight / 10} kg
                            </span>
                        </p>
                    </div>
                    <p class="text-center text-gray-900">Ability : 
                        <span class="text-gray-700 text-sm" style="color: {pokemonTypes[pokemonData.types[0].type.name]};">
                            {pokemonData.abilities[0].ability.name.charAt(0).toUpperCase() + pokemonData.abilities[0].ability.name.slice(1)}
                        </span>
                    </p>
                </Card>
            {/if}
        {:else}
            <TextMessage color={'text-red-700'} message={'Pokemon Data Unknown...'} />
        {/if}
    </div>
</div>