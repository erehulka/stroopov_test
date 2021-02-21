<script>
  import { createEventDispatcher } from 'svelte';

  export let correct;

  let sides = [
    'Naľavo',
    'V strede',
    'Napravo'
  ];
  const dispatch = createEventDispatcher();

  $: other = correct;
  $: {
    while (other == correct) {
      other = Math.floor(Math.random() * 3);
    }
  }

  const nextStep = (box) => {
    dispatch('nextBox', box);
  };
</script>

<div class="box_text">
  <h1
    class:center={other == 1}
    class:left={other == 0}
    class:right={other == 2}>
    {sides[correct]}
  </h1>
</div>
<div class="boxes">
  <button on:click={() => nextStep(0)} class="box">Naľavo</button>
  <button on:click={() => nextStep(1)} class="box">V strede</button>
  <button on:click={() => nextStep(2)} class="box">Napravo</button>
</div>

<style lang="scss">
  .box_text {
    border: 2px solid black;
    margin: 2em;
    .center {
      text-align: center;
    }
    .left {
      text-align: left;
    }
    .right {
      text-align: right;
    }
  }
  .boxes {
    margin: 2em;
    display: flex;
    justify-content: center;
    .box {
      margin: 0px 15px;
      font-size: 1.2em;
      border: 1px solid #333;
      font-weight: bold;
    }
  }
</style>