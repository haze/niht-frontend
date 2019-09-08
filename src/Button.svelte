<script>
  export let enabled;
  export let text;
  export let onSubmit;
  let style = enabled ? 'button enabled' : 'button disabled';
</script>

<style>

  .enabled {
    /* color: hsla(225, 80%, 50%, .8) !important; */
    color: white;
    background-color: hsl(225, 50%, 50%) !important;
  }

  /* .disabled { */
  /* } */

  .button {
    border: none;
    font-size: 75%;
    padding-top: 0.6vw;
    padding-bottom: 0.6vw;
    padding-left: 1vw;
    padding-right: 1vw;
    border-radius: 5px;
    /* background-color: hsla(0, 0%, 50%, .1); */
    /* color: hsla(0, 0%, 50%, .4); */
    background-color: hsla(225, 50%, 90%, .4);
    color: white;
    transition: 0.1s all ease-in-out;
  }

  .enabled:hover {
    cursor: pointer;
    background-color: hsla(225, 50%, 90%, .4);
    color: white;
    box-shadow: 0 0.250em 0.375em rgba(50,50,93,.09), 0 0.063em 0.188em rgba(0,0,0,.08);
    transform: scale(1.05);
  }

  .enabled:active:hover {
    transform: scale(0.9);
    box-shadow: none;
  }
</style>


<button class={enabled ? 'button enabled' : 'button disabled'} on:click={(e) => { if (enabled) { onSubmit(e) }}}>{text}</button>
