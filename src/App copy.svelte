<script>
  import FaReact from 'svelte-icons/fa/FaReact.svelte'
  import FaAngular from 'svelte-icons/fa/FaAngular.svelte'
  import Button from "./lib/Button.svelte";


  
</script>


<!-- <Button size="large" shadow bgColor="blue" textColor="yellow">Hello</Button> -->
<Button
      on:click={() => alert('Hello')}
      size="large" 
      shadow  
      let:isEsquerdo > 
  <span slot="leftContent">
    {#if isEsquerdo}
      <FaAngular />
    {:else}
      <FaReact />
    {/if}
  </span>
  Hello 
</Button>

<style>
  
</style>