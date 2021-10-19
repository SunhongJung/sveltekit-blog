<div class="container">
    <nav>
        <ul>
            <li>
                <a href="/">Home</a>
            </li>
        </ul>
    </nav>
    <slot />
</div>


<style>
    .container {
        max-width: 600px;
        margin: 50px auto;
    }

    ul {
        display: flex;
        list-style:none;
        padding: 0;
    }
</style>