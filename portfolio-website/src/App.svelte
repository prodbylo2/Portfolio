<script>
  import MusicPlayer from './components/MusicPlayer.svelte'; // Make sure the path is correct
</script>

<main>
  <h1>Music Player</h1>
  <MusicPlayer /> <!-- Include the MusicPlayer component -->
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 800px;
    margin: 0 auto;
    background: #f0f0f0;
    border-radius: 8px;
  }
  
  h1 {
    color: #333;
  }
</style>
