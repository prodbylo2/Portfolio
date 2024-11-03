<!-- <script lang="ts">
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
</script> -->

<!-- <style>
    canvas {
        display: block;
        width: 100%;
        height: 100%;
    }
</style> -->


<!-- 
<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
    import * as THREE from 'three';

    let scene, camera, renderer, sphere;
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

        // Create a dotted sphere
        const geometry = new THREE.SphereGeometry(1, 32, 32);
        const material = new THREE.PointsMaterial({ color: 0xff9900, size: 0.05 });
        sphere = new THREE.Points(geometry, material);
        scene.add(sphere);

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

        // Update sphere size based on frequency data
        const average = dataArray.reduce((sum, value) => sum + value) / dataArray.length;
        sphere.scale.set(average / 128, average / 128, average / 128); // Scale sphere based on average frequency data

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
</script> -->



<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
    import * as THREE from 'three';
    import AudioSphere from './audioSphere.js'; // Adjust the path as necessary

    export let audio: HTMLAudioElement; // Receive the audio element from the parent component
    let scene, camera, renderer, audioSphere;
    let analyserNode;
    let frequencyData = new Uint8Array(2048);
    let audioContext;

    // Initialize Audio Context and Analyser Node
    function initAudioContext() {
        audioContext = new (window.AudioContext || (window as any).webkitAudioContext)();
        analyserNode = audioContext.createAnalyser();
        analyserNode.fftSize = 2048;

        const sourceNode = audioContext.createMediaElementSource(audio);
        sourceNode.connect(analyserNode);
        analyserNode.connect(audioContext.destination);
    }

    // Update function to fetch frequency data and update the sphere
    function update() {
        analyserNode.getByteFrequencyData(frequencyData);
        audioSphere.update(frequencyData); // Update the AudioSphere with the latest frequency data
        renderer.render(scene, camera);
        requestAnimationFrame(update);
    }

    // Setup Three.js scene
    const setupScene = () => {
        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // Create an instance of AudioSphere
        audioSphere = new AudioSphere(3, 32, 32); // Adjust size and segments as needed
        audioSphere.addToScene(scene);

        camera.position.z = 5; // Set camera position
    };

    onMount(() => {
        initAudioContext();
        setupScene();
        // Call startAudio with the audio element source
        audio.play();
        update(); // Start the animation loop
    });

    onDestroy(() => {
        if (audioContext) {
            audioContext.close();
        }
        if (renderer) {
            renderer.dispose();
        }
    });
</script>




















