<script>
  import { onMount } from "svelte";
  import axios from "axios";

  import PostForm from "../components/PostForm.svelte";

  const url = "https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev";

  let posts = [];
  const listPosts = async () => {
    const res = await axios({
      method: "GET",
      url: url + "/posts"
    });
    posts = await res.data;
  };

  onMount(async () => {
    listPosts();
  });

  const addPost = ({ detail: post }) => {
    if (posts.find(p => p.id === post.id)) {
      const index = posts.findIndex(p => p.id === post.id);
      let postsUpdated = posts;
      postsUpdated.splice(index, 1, post);
      posts = postsUpdated;
    } else posts = [post, ...posts];
  };

  const deletePost = async postId => {
    if (confirm("Are you sure ?")) {
      await axios({
        method: "DELETE",
        url: url + `/post/${postId}`
      }).then(() => {
        alert("data deleted!");
      });
      await listPosts();
    }
  };

  let postToBeUpdated = {
    id: null,
    title: "",
    body: ""
  };
  const updatePost = post => {
    postToBeUpdated = post;
  };
</script>

<section>
  <div class={'container'}>
    <PostForm on:postCreated={addPost} {postToBeUpdated} />
    <div class={'descriptive'} layer={'posts'}>
      <h2>Posts</h2>
      <div class={'posts'}>
        <div class={'row'}>
          {#if posts.length === 0}
            <div class={'spinner'} />
          {:else}
            {#each posts as post}
              <div class={'column large:four'}>
                <div class={'post'}>
                  <div class={'card'}>
                    <h4>{post.title}</h4>
                    <hr />
                    <p>{post.body}</p>
                    <div class={'buttons'}>
                      <button on:click={() => updatePost(post)}>update</button>
                      <button on:click={() => deletePost(post.id)}>
                        delete
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            {/each}
          {/if}
        </div>
      </div>
    </div>
  </div>
</section>
