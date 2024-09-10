<script lang="ts">
    import { onMount } from "svelte";
    import { writable } from "svelte/store";
    import { translations, loadTranslations } from "../stores/translationStore";


    // Store pour l'index actuel
    const currentIndex = writable(0);

    let items = [
        // Vos éléments de carrousel ici
        { type: 'image', src: 'https://picsum.photos/800/400?image=1', alt: 'Image 1', text: 'caption1', logos: [
            { src: '../src/lib/img/spotify.png', alt: 'Logo 1', url: 'https://site1.com' },
            { src: '../src/lib/img/deezer.png', alt: 'Logo 2', url: 'https://site2.com' }
        ]},
        { type: 'video', src: 'https://www.w3schools.com/html/mov_bbb.mp4', alt: 'Video 1', text: 'caption2', cta: { url: '#', text: 'watchVideo' } },
        { type: 'image', src: 'https://picsum.photos/800/400?image=2', alt: 'Image 1', text: 'caption3', cta: { url: '#', text: 'Learn More' } },
        // Ajoutez d'autres éléments ici
    ];

    let interval : number;

    onMount(() => {
        loadTranslations('fr'); // Charger les traductions par défaut
        startCarousel();
        startCarousel();
        return () => clearInterval(interval);
    });

    function startCarousel() {
        interval = setInterval(() => {
            currentIndex.update(n => (n + 1) % items.length);
        }, 5000); // Change toutes les 5 secondes
    }

    function selectItem(index: number) {
        console.log(`selectItem called with index: ${index}`);
        currentIndex.set(index);
        clearInterval(interval);
        startCarousel();
    }
</script>

<div class="carousel">
    <div class="carousel-inner">
        {#each items as item, index}
            <div class="carousel-item { $currentIndex === index ? 'active' : '' }">
                {#if item.type === 'image'}
                    <img src={item.src} alt={item.alt} />
                {:else if item.type === 'video'}
                    <video src={item.src} muted loop autoplay></video>
                {/if}
                <div class="carousel-caption">
                    <p>{$translations[item.text]}</p>
                    {#if item.logos}
                        <div class="carousel-logos">
                            {#each item.logos as logo}
                                <a href={logo.url} target="_blank" rel="noopener noreferrer">
                                    <img src={logo.src} alt={logo.alt} class="carousel-logo" />
                                </a>
                            {/each}
                        </div>
                    {:else}
                        <a href={item.cta.url} class="carousel-cta">{$translations[item.cta.text]}</a>
                    {/if}
                </div>
            </div>
        {/each}
    </div>
    <div class="carouselShadow"></div>
    <div class="carousel-indicators">
        {#each items as _, index}
        <button class:active={$currentIndex === index} on:click={() => {
            console.log(`Button clicked with index: ${index}`);
            selectItem(index);
        }}></button>
        {/each}
    </div>
    
</div>

<style lang="scss">
    @import "../src/style/carousselHero.scss";

    .carouselShadow {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle, transparent, rgba(0, 0, 0, 0.8) 70%); /* Utiliser un dégradé radial pour un effet circulaire avec le centre clair */

    // z-index: 1;
}

</style>