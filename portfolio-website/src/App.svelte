<script>
  import Name from './components/DisplayName.svelte';
  import AudioVisualizer from './components/MusicVisualizer.svelte';
</script>

<main>
  <!-- Include the AudioVisualizer component here -->
  <AudioVisualizer />
  <Name />
</main>

<style>
  main {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
}
</style>
