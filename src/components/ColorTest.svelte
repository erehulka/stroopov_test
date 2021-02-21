<script>
  import { createEventDispatcher } from 'svelte';

  export let correct;

  let colors = [
    'Modrá',
    'Zelená',
    'Červená',
    'Žltá'
  ];
  const dispatch = createEventDispatcher();

  $: other = correct;
  $: {
    while (other == correct) {
      other = Math.floor(Math.random() * 4);
    }
  }

  const nextStep = (color) => {
    dispatch('nextColor', color);
  };
</script>

<div class="color_text">
  <h1
    class:blue={correct == 0}
    class:green={correct == 1}
    class:red={correct == 2}
    class:yellow={correct == 3}>
    {colors[other]}
  </h1>
</div>
<div class="colors">
  <button on:click={() => nextStep(0)} class="color blue"></button>
  <button on:click={() => nextStep(1)} class="color green"></button>
  <button on:click={() => nextStep(2)} class="color red"></button>
  <button on:click={() => nextStep(3)} class="color yellow"></button>
</div>

<style lang="scss">
  .color_text {
    -webkit-text-stroke: 2px black;
    font-size: 1.5em;
    text-align: center;
    .red {
      color: red;
    }
    .blue {
      color: blue;
    }
    .yellow {
      color: yellow;
    }
    .green { 
      color: green;
    }
  }
  .colors {
    display: flex;
    .color {
      width: 22%;
      margin: 1.5%;
      height: 80px;
    }
    .red {
      background-color: red;
    }
    .blue {
      background-color: blue;
    }
    .yellow {
      background-color: yellow;
    }
    .green { 
      background-color: green;
    }
  }
</style>