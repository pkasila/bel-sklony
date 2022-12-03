<script lang="ts">
  import WordForm from './lib/WordForm.svelte'
</script>

<svelte:head>
  <title>Месны склон 2 скланення</title>
</svelte:head>

<main>
  <div>
    <a href="https://lyceum.by" target="_blank" rel="noreferrer">
      <img src="/lyceum.svg" class="logo" alt="Vite Logo" />
    </a>
  </div>
  <h1>Месны склон</h1>

  <div class="card">
    <WordForm />
  </div>

  <p class="read-the-docs">
    &copy; {new Date().getFullYear()} <a class="no-highlight" href="https://pkasila.net/" target="_blank" rel="noreferrer">Павел Касіла</a>.
    <a class="no-highlight" href="https://github.com/pkasila/bel-sklony" target="_blank" rel="noreferrer">Зыходныя коды</a>
  </p>
</main>

<style>
  a.no-highlight {
    color: #b4b4b4 !important;
  }
  a.no-highlight:hover {
    color: #9f9f9f !important;
  }
  .logo {
    height: 8em;
    padding: 1.5em;
    will-change: filter;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em rgb(102, 106, 162));
  }
  .read-the-docs {
    color: #888;
  }
</style>
