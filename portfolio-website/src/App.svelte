<!-- App.svelte -->
<script lang="ts">
  import MainLayout from './components/MainLayout.svelte';
  import MusicPlayer from './components/MusicPlayer.svelte';

  const name = "Your Name"; // Replace with your actual name
</script>

<MainLayout {name}>
  <MusicPlayer slot="controls" />
</MainLayout>

