<!-- MusicPlayer.svelte -->
<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
    import AudioVisualizer from './MusicVisualizer.svelte';

    let audio: HTMLAudioElement;
    let audioContext: AudioContext;
    let analyser: AnalyserNode;
    let dataArray: Uint8Array;
    let isPlaying = false;
    let currentSongIndex = 0;

    const songs = [
        '/music/Air.wav',
        '/music/Glitch.wav',
        '/music/House_Final.wav',
        '/music/Space.wav',
        '/music/Water.wav'
    ];

    const setupAudio = () => {
        audioContext = new (window.AudioContext || (window as any).webkitAudioContext)();
        analyser = audioContext.createAnalyser();
        analyser.fftSize = 256;
        const source = audioContext.createMediaElementSource(audio);
        source.connect(analyser);
        analyser.connect(audioContext.destination);
        dataArray = new Uint8Array(analyser.frequencyBinCount);
    };

    const togglePlay = () => {
        if (isPlaying) {
            audio.pause();
        } else {
            audio.play();
            audioContext.resume();
        }
        isPlaying = !isPlaying;
    };

    const updateVisualizer = () => {
        if (!analyser) return;
        analyser.getByteFrequencyData(dataArray);
        requestAnimationFrame(updateVisualizer);
    };

    const changeSong = (direction: 'next' | 'prev') => {
        if (direction === 'next') {
            currentSongIndex = (currentSongIndex + 1) % songs.length;
        } else if (direction === 'prev') {
            currentSongIndex = (currentSongIndex - 1 + songs.length) % songs.length;
        }

        audio.src = songs[currentSongIndex];
        audio.load();
        audioContext.resume().then(() => {
            audio.play();
            isPlaying = true;
        }).catch((error) => {
            console.error('Failed to resume audio context:', error);
        });
    };

    onMount(() => {
        setupAudio();
        updateVisualizer();
    });

    onDestroy(() => {
        audioContext.close();
    });
</script>

<style>
    .visualizer-container {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .bars {
        display: flex;
        gap: 4px;
    }
    .bar {
        width: 4px;
        background-color: #ff9900;
    }
    .controls {
        display: flex;
        justify-content: center;
        margin-top: 10px;
    }
    button {
        margin: 0 10px;
        padding: 8px 16px;
    }
</style>

<div class="visualizer-container">
    <audio bind:this={audio} src={songs[currentSongIndex]}></audio>
    <AudioVisualizer {audio} />
    <div class="bars">
        {#each Array.from({ length: dataArray?.length ?? 0 }) as _, i}
            <div class="bar" style="height: {dataArray[i] / 2}px"></div>
        {/each}
    </div>
</div>

<div class="controls">
    <button on:click={() => changeSong('prev')}>Previous</button>
    <button on:click={togglePlay}>{isPlaying ? 'Pause' : 'Play'}</button>
    <button on:click={() => changeSong('next')}>Next</button>
</div>
