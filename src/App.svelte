<script>
  import Button from './Button.svelte';
  import { onMount } from 'svelte';
  import isUrl from 'is-url-superb';

  const ERROR_VANISH_DELAY_MS = 5000;
  
  let inputReference = null;
  let buttonEnabled = false;
  let link = "google.com";
  let error = "invalid url";
  let gotLink = false;

  const httpLink = () => {
    if (link.startsWith("http")) {
      return link;
    }
    return "https://" + link
  };

  const isURL = (url) => {
    const raw = isUrl(url);
    if (!raw && !url.startsWith("http")) {
      const withForcedHttpUrl = "https://" + url;
      const forcedHttpIsUrl = isUrl(withForcedHttpUrl);
      return forcedHttpIsUrl;
    }
    return raw;
  }

  $: buttonEnabled = isURL(link);

  export const onSubmit = (event) => {
    // try to upload
    fetch("http://localhost:1337/", {method: 'PUT', body: httpLink()})
      .then((resp) => resp.json())
      .then((body) => {
        console.log(body);
        if ('url' in body) {
          link = body.url;
          gotLink = true;
        } else if ('error' in body) {
          error = body.error;
          setTimeout(() => {
          }, ERROR_VANISH_DELAY_MS);
        }
      });
  }

  onMount(() => {
    inputReference.focus();
  });
</script>

<style>
  .container {
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .input {
    margin: 0;
    padding: 0;
    border: none;
    font-size: 110%;
  }

  .input:focus {
    outline: none;
  }

  .error {
    font-size: 80%;
    margin: 0;
    color: hsla(0, 80%, 50%, .5);
    float: left;
    width: 100%;
  }
</style>

<div class="container">
  <div>
  <div>
  <input bind:this={inputReference} bind:value={link} type="text" spellcheck="false" class="input" placeholder="example.com"/>
  <Button bind:enabled={buttonEnabled} onSubmit={onSubmit} text="SHORTEN"/>
  </div>
  {#if error != null}
    <p class="error">{error}</p>
  {/if}
  </div>
</div>
