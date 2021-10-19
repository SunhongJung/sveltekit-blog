<script context="module">
    export const load = async ({ page, fetch }) => {
        const authorId = page.params.authorId;
        // const res = await fetch(`https://jsonplaceholder.typicode.com/users/${authorId}`)
        // const user = await res.json()

        // const resPosts = await fetch('https://jsonplaceholder.typicode.com/posts')
        // const allPosts = await resPosts.json()

        // const [resUser, resPosts] = await Promise.all([
        //     fetch(`https://jsonplaceholder.typicode.com/users/${authorId}`),
        //     fetch('https://jsonplaceholder.typicode.com/posts')
        // ])

        const res = await fetch(`https://jsonplaceholder.typicode.com/users/${authorId}?_embed=posts`)

        const user = await res.json()
        const posts = user.posts;

        // const posts = allPosts.filter((p) => p.userId === user.id)
        // console.log(page)

        return {
            props: {
                user,
                posts
            }
        }
    }
</script>

<script>
    export let user;
    export let posts;
</script>


<h1>{user.name}</h1>
<p>{user.company.catchPhrase}</p>
<p>{user.email}</p>

<h2>Posts by {user.name}</h2>
<ul>
{#each posts as post (post.id)}
    <li>
        <a sveltekit:prefetch href={`/blog/${post.id}`}>{post.title}</a>
    </li>
{/each}
</ul>

<style>
    li {
        margin-bottom: 10px;;
    }
</style>

