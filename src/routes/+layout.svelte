<style lang="scss">
    nav {
        display: flex;
        justify-content: space-between;
        min-height: 5rem;
        background-color: #218225;
        padding-right: 1rem;
        padding-left: 1rem;
        .title {
            display: flex;
            align-items: center;
            justify-content: center;
            a {
                font-size: 2rem;
                color: white;
                text-decoration: none;
            }
        }
        .links {
            ul {
                margin: 0;
                padding: 0;
                height: 100%;
                display: flex;
                justify-content: space-between;
                gap: 1rem;
                li {
                    display: flex;
                    list-style-type: none;
                    a {
                        display: flex;
                        justify-content: center;
                        align-items: center;
                        text-decoration: none;
                        padding: 1rem;
                        color: white;
                        font-size: 1.5rem;
                    }
                }
            }
        }
    }
</style>

<nav>
    <div class="title">
        <a href="/">Cocktail</a>
    </div>
    <div class="links">
        <ul>
        </ul>
    </div>
</nav>

<slot />