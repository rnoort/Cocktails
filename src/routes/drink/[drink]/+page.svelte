<script>
    import { onMount } from 'svelte';
    import { page } from "$app/stores";

    async function fetchDrink(id) {
        let response =  await fetch("https://thecocktaildb.com/api/json/v1/1/lookup.php?i=" + id);

        let decodedResponse = await response.json();
       
        let drink = decodedResponse.drinks[0];

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
        justify-content: center;
        height: calc(100% - 5rem);
        align-items: center;
        h2 {
            text-align: center;
        }
    }

    .ingredient {
        display: flex;
        align-items: center;
        flex-direction: column;
    }

    .ingredientsbox {
        display: flex;
    }

</style>

<div class="container">
    {#await drink}
        <h2>Loading drink</h2>
    {:then drink}
        <h2>{drink.strDrink}</h2>
        <div class="ingredients">
            <h3>Ingredients</h3>
            <div class="ingredientsbox">
                {#each drink.ingredients as ingredient, index}
                    <div class="ingredient">
                        <img src="https://thecocktaildb.com/images/ingredients/{ingredient.ingredient}-Small.png">
                        <p>{ingredient.ingredient}</p>
                        <p>{ingredient.measurement}</p>
                    </div>    
                {/each}
            </div>
        </div>
        <div class="instructions">
            <h3>Instructions</h3>
            <p>{drink.strInstructions}</p>
        </div>
    {:catch error}

    {/await}
</div>