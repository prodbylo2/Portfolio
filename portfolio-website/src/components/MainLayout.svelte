<!-- MainLayout.svelte -->
<script lang="ts">
    export let name: string; // Accept name as a prop
</script>

<style>
    .container {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        min-height: 100vh; /* Full height */
        position: relative; /* For absolute positioning of name */
        padding: 20px;
        background-color: #f0f0f0; /* Light background for contrast */
    }
    .name {
        font-size: 5rem; /* Adjust size as needed */
        font-weight: bold;
        color: rgba(0, 0, 0, 0.2); /* Light color for background effect */
        position: absolute; /* Position behind the visualizer */
        z-index: 0; /* Send to back */
        text-align: center; /* Center the name */
    }
    .visualizer {
        z-index: 1; /* Bring visualizer above the name */
        position: relative; /* Ensure visualizer is positioned normally */
        width: 60vw; /* Adjust width to make it smaller */
        height: 30vh; /* Adjust height to make it smaller */
    }
    .music-player {
        width: 100%;
        position: fixed; /* Fix to the bottom of the viewport */
        bottom: 0;
        background-color: #fff; /* Background for the music player */
        padding: 10px;
        box-shadow: 0 -2px 5px rgba(0, 0, 0, 0.2); /* Subtle shadow for elevation */
        z-index: 2; /* Bring the player above everything else */
    }
</style>

<div class="container">
    <div class="name">{name}</div>
    <div class="visualizer">
        <slot /> <!-- Slot for music visualizer component -->
    </div>
    <div class="music-player">
        <slot name="controls" /> <!-- Slot for navigation controls -->
    </div>
</div>
