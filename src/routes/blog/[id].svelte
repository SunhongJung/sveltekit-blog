<script context="module">
    export const load = async ({ page, fetch }) => {
        const postId = page.params.id;
        const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${postId}?_expand=user`)
        const post = await res.json()

        // console.log(post)
        // const userId = post.userId;
        // const userRes = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`)
        // const user = await userRes.json()

        const user = post.user

        // console.log(page)

        return {
            props: {
                post,
                user
            }
        }
    }
</script>

<script>
    export let post;
    export let user;
</script>

<h1>{post.title}</h1>
<p>{post.body}</p>
<p>- Writer by <a sveltekit:prefetch href={`/authors/${user.id}`}>{user.name}</a></p>