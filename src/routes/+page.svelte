<script>
    async function fetchCategories() {
        let response = await (await fetch("https://www.thecocktaildb.com/api/json/v1/1/list.php?c=list")).json();

        return response.drinks;
    }

    async function fetchAlcoholicDrinks() {
        let response = await (await fetch("https://www.thecocktaildb.com/api/json/v1/1/filter.php?a=Non_Alcoholic")).json();

        return response.drinks;
    }
    async function fetchDrinksByCategory(category, showAlcoholic) {
        if(category == "none") return [];

        let response = await (await fetch("https://www.thecocktaildb.com/api/json/v1/1/filter.php?c=" + category)).json();

        // Filter
        if(showAlcoholic == false) {
            let alcoholicDrinks = await fetchAlcoholicDrinks();

            // Remove alcoholic drinks from response
        }

        return response.drinks;
    }

    let showAlcoholic = true;
    let currentCategory = "none";

    let categories = fetchCategories();
    $: drinks = fetchDrinksByCategory(currentCategory, showAlcoholic); 
</script>

<style lang="scss">
    .optionbar {
        display: flex;
        justify-content: space-between;
        background-color: lightgrey;
        gap: 1rem;
        p { 
            font-family: sans-serif;
            margin: 0;
            margin-left: 1rem;
        }
        .options {
            display: flex;
            margin-right: 1rem;
            gap: 1rem;
            .alcohol, .category {
                display: flex;
            }
            .category {
                select {
                    width: 150px;
                }
            }
        }
    }
</style>

<div class="optionbar">
    <p>Options</p>
    <div class="options">
        <div class="alcohol">
            <label>Show alcoholic drinks</label>
            <input type="checkbox" bind:checked={showAlcoholic}>
        </div>
        <div class="category">
            <label>Category</label>
            <select name="category" bind:value={currentCategory}>
                {#await categories}
                    <option value="none">Loading categories...</option>
                {:then categories}
                    <option value="none">None</option>
                    {#each categories as category, index}
                        <option value="{category.strCategory}">{category.strCategory}</option>
                    {/each}
                {:catch error}
                    <option value="failed">Failed fetching categories</option>
                {/await}
            </select>
        </div>
    </div>
</div>

<div class="container">
    <table>
        <thead>
            <td>Image</td>
            <td>Name</td>
            <td>Details</td>
        </thead>
        <tbody>
            {#await drinks}
                <option value="none">Loading drinks...</option>
            {:then drinks}
                {#each drinks as drink, index}
                    <tr>
                        <td><img src={drink.strDrinkThumb + "/preview"} alt={drink.strDrink}></td>
                        <td>{drink.strDrink}</td>
                        <td><a href="/drink/{drink.idDrink}">Details</a></td>
                    </tr>
                {/each}
            {:catch error}

            {/await}
        </tbody>
    </table>
</div>