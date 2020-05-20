<script>
  import { createEventDispatcher } from "svelte";
  import axios from "axios";
  const dispatch = createEventDispatcher();

  const url = "https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev";

  export let postToBeUpdated;
  $: title = postToBeUpdated.title;
  $: body = postToBeUpdated.body;
  let loading = false;
  const onSubmit = async e => {
    e.preventDefault();
    if (title.trim() === "" || body.trim() === "") {
      alert("fill the form correctly");
    }

    loading = true;

    const data = {
      title: title,
      body: body
	};
	
	let requestURL, method;
	if (postToBeUpdated.id) {
		requestURL = `${url}/post/${postToBeUpdated.id}`;
		method =  "PUT"
	} else {
		requestURL = `${url}/post`;
		method =  "POST"
	}

    const res = await axios({
      method,
      url: requestURL,
      data
    });
    const post = await res.data;
    loading = false;
    dispatch("postCreated", post);
    title = body = "";
  };
</script>

<div class={'descriptive'} layer={'form'}>
  <h2>Add Data</h2>
  {#if !loading}
    <form on:submit={e => onSubmit(e)}>
      <div class={'group'}>
        <label for={'title'}>Title</label>
        <input
          type="text"
          placeholder={'title'}
          bind:value={postToBeUpdated.title} />
      </div>
      <div class={'group'}>
        <label for={'body'}>Body</label>
        <input
          type="text"
          placeholder={'body'}
          bind:value={postToBeUpdated.body} />
      </div>
      <div class={'group'}>
        <button type={'submit'}>{postToBeUpdated.id ? 'Update' : 'Add'}</button>
      </div>
    </form>
  {:else}
    <div class={'spinner'} />
  {/if}
</div>
