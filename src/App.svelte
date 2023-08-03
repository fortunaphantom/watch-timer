<script lang="ts">
  import svelteLogo from './assets/svelte.svg'
  import WatchTimer from './lib/WatchTimer.svelte';

  let time = 1000;
</script>

<main>
  <div class="card">
    <WatchTimer resetTime={time} />
    <input on:change={(e) => {
      time = Number(e.target.value);
    }} />
  </div>
  <div>3</div>
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
</style>