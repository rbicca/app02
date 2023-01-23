<script>
  export let init = 0;
  export let max = 10;

  console.log('recebi ', $$props);
  console.log('e além disso', $$restProps);

  let count = 0;

  count = init;

  const incr = () => { 
    if (count === max) return ;

    count ++; 
  }

  $: if (count > 4){  console.log(`o counter é ${count}`); }
</script>



<button on:click={incr} >Clicks {count}</button>


<style>
  button {
    padding: 10px 10px;
  }
</style>