<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
    import * as THREE from 'three';

    let scene, camera, renderer, cube;
    let analyser;
    let dataArray = new Uint8Array(256); // Size to match the FFT size in the main component
    let audioContext;

    export let audio: HTMLAudioElement; // Make sure to accept 'audio' as the prop

    const setupScene = () => {
        // Scene setup
        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // Create a cube
        const geometry = new THREE.BoxGeometry();
        const material = new THREE.MeshBasicMaterial({ color: 0xff9900 });
        cube = new THREE.Mesh(geometry, material);
        scene.add(cube);

        camera.position.z = 5;

        // Set up audio analyser
        audioContext = new (window.AudioContext || (window as any).webkitAudioContext)();
        analyser = audioContext.createAnalyser();
        analyser.fftSize = 256; // Match this with your main component
        const source = audioContext.createMediaElementSource(audio);
        source.connect(analyser);
        analyser.connect(audioContext.destination);
    };

    const updateVisualizer = () => {
        if (!analyser) return;

        // Get frequency data
        analyser.getByteFrequencyData(dataArray);

        // Update cube size based on frequency data
        const average = dataArray.reduce((sum, value) => sum + value) / dataArray.length;
        cube.scale.set(1, average / 128, 1); // Scale cube based on average frequency data

        // Render the scene
        renderer.render(scene, camera);
        requestAnimationFrame(updateVisualizer);
    };

    onMount(() => {
        setupScene();
        updateVisualizer();
    });

    onDestroy(() => {
        renderer.dispose();
        audioContext.close();
    });
</script>

<!-- <style>
    canvas {
        display: block;
        width: 100%;
        height: 100%;
    }
</style> -->
