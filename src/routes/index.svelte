<script context="module">
    export const load = async ({ fetch }) => {

        const res = await fetch('https://jsonplaceholder.typicode.com/posts')
        const posts = await res.json();
        return {
            props: {
                posts,
            }
        }
    }
</script>

<script>
    import { paginate, LightPaginationNav, PaginationNav } from 'svelte-paginate'

    export let posts;
    let searchTerm = ""
    let searchedPost = posts

    $: searchedPost = posts.filter((post) => {
        return post.title.includes(searchTerm)
    })

    let currentPage = 1
    let pageSize = 4
    $: paginatedItems = paginate({ 'items': searchedPost, pageSize, currentPage })

</script>

<h1>Posts</h1>

<input type="text" place="search" bind:value={searchTerm}>

<div class="posts">
    {#each paginatedItems as post (post.id)}
        <div class="post">
            <h2>{post.title.substring(0, 20)}</h2>
            <p>{post.body.substring(0,80)}</p>
            <p class="link"><a sveltekit:prefetch href="{`/blog/${post.id}`}">Read More</a></p>
        </div>
    {:else}
        <p>No found</p>
    {/each}
</div>

{#if searchedPost.length}
    <LightPaginationNav
    totalItems="{searchedPost.length}"
    pageSize="{pageSize}"
    currentPage="{currentPage}"
    limit="{1}"
    showStepOptions="{true}"
    on:setPage="{(e) => currentPage = e.detail.page}"
    />
{/if}

<!-- <PaginationNav
  totalItems="{searchedPost.length}"
  pageSize="{pageSize}"
  currentPage="{currentPage}"
  limit="{1}"
  showStepOptions="{true}"
  on:setPage="{(e) => currentPage = e.detail.page}"
/> -->


<style>
    .posts {
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-gap: 20px;
        margin: 30px 0;
    }

    .post {
        border: 1px solid #ddd;
        padding: 10px;
        box-shadow: 0 0 20px #eee;
    }

    h2 {
        margin: 0;
    }

    .link {
        text-align: right;
    }

    @media all and (max-width: 600px) {
        .posts {
            display: grid;
            grid-template-columns: 1fr;
            grid-gap: 20px;
            margin: 30px 0;
        }        
    }
</style>