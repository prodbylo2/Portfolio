<script>
  // import AudioControls from './components/MusicPlayer.svelte';
  import AudioVisualizer from './components/MusicVisualizer.svelte';
</script>

<main>
  <!-- Include the AudioVisualizer component here -->
  <AudioVisualizer />
  <!-- <AudioVisualizer audioFilePath='' /> -->
  <!-- <AudioControls /> -->
</main>

<style>
  /* main {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100%;
      width: 100%;
      font-family: Arial, sans-serif;
      background-color: #121212;
      color: #ffffff;
      text-align: center;
      overflow-x: hidden;
      margin: 0%;
  } */

  main {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
}


</style>
