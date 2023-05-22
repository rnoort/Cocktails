<script>
    import { onMount } from 'svelte';
    import { page } from "$app/stores";

    async function fetchDrink(id) {
        let response = await (await fetch("https://thecocktaildb.com/api/json/v1/1/lookup.php?i=" + id)).json();
       
        let drink = response.drinks[0];

        // Ingredients and measurements
        drink.ingredients = [];
        for(let i = 1; drink["strIngredient" + i] != null; i++) {
            drink.ingredients.push({ ingredient: drink["strIngredient" + i], measurement: drink["strMeasure" + i]});
        }

        return drink;
    }
    
    let drink = fetchDrink($page.params.drink);
</script>

<style lang="scss">
    .container {
        display: flex;
        justify-content: center;
        height: calc(100% - 5rem);
        align-items: center;
    }


</style>

<div class="container">
    {#await drink}
        <h2>Loading drink</h2>
    {:then drink}
        <h2>{drink.strDrink}</h2>
        <div class="ingredients">
            <h3>Ingredients</h3>
            <div>
                {#each drink.ingredients as ingredient, index}
                    <div>
                        <p>{ingredient.measurement}</p>
                        <img src="https://thecocktaildb.com/images/ingredients/{ingredient.ingredient}-Small.png">
                    </div>    
                {/each}
            </div>
        </div>
        <div class="instructions">
            <h3>Instructions</h3>
            <div>
                <p>{drink.strInstructions}</p>
            </div>
        </div>
    {:catch error}

    {/await}
</div>