<script>
    import { onMount } from 'svelte';
    import { gsap } from 'gsap';

    let leftTextRef;
    let rightTextRef;

    const containerStyle = `

        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 100vh;
        background-color: transparent;
        color: white;
        font-weight: bolder;
    `;

    const textStyle = `
        font-size: 150px;
        position: relative;
        white-space: nowrap;
        font-family: 'Doto', sans-serif;
        font-weight: 1000;
        color: black;
        overflow: hidden;
    `;

    const randomFlicker = (element) => {
        const tl = gsap.timeline({ repeat: -1 });
        tl.to(element, {
        opacity: 0.5,
        duration: Math.random() * 0.1 + 0.05,
        ease: 'power1.inOut',
        });
        tl.to(element, {
        opacity: 1,
        duration: Math.random() * 0.1 + 0.05,
        ease: 'power1.inOut',
        });
        tl.duration(Math.random() * 2 + 1);
    };

    onMount(() => {
        if (leftTextRef && rightTextRef) {
        gsap.fromTo(leftTextRef, { x: '-100%', opacity: 0 }, { x: '0%', opacity: 1, duration: 2.5 });
        gsap.fromTo(rightTextRef, { x: '100%', opacity: 0 }, { x: '0%', opacity: 1, duration: 2.5 });

        const applyRandomFlicker = (element) => {
            Array.from(element.children).forEach((letter) => {
            randomFlicker(letter);
            });
        };

        applyRandomFlicker(leftTextRef);
        applyRandomFlicker(rightTextRef);
        }

        return () => {
        gsap.globalTimeline.clear();
        };
    });
</script>

<div style={containerStyle}>
    <div bind:this={leftTextRef} style={textStyle}>
        {#each Array.from('NEERAJ') as letter}
        <span>{letter}</span>
        {/each}
    </div>
    <div bind:this={rightTextRef} style={textStyle}>
        {#each Array.from('MENON') as letter}
        <span>{letter}</span>
        {/each}
    </div>
</div>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Barlow+Condensed&family=Doto:wght@100..900&display=swap');
</style>