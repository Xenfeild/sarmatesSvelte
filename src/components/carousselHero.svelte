<script lang="ts">
    import { onMount } from "svelte";
    import { writable } from "svelte/store";
    import { translations, loadTranslations } from "../stores/translationStore";


    // Store pour l'index actuel
    const currentIndex = writable(0);

    let items = [
        // Vos éléments de carrousel ici
        { type: 'image', src: 'https://picsum.photos/800/400?image=1', alt: 'Image 1', text: 'caption1', cta: { url: '#', text: 'Learn More' } },
        { type: 'video', src: 'https://www.w3schools.com/html/mov_bbb.mp4', alt: 'Video 1', text: 'caption2', cta: { url: '#', text: 'Watch Now' } },
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
                    <a href={item.cta.url}  class="carousel-cta">{$translations[item.cta.text]}</a>
                </div>
            </div>
        {/each}
    </div>
    <div class="carousel-indicators">
        {#each items as _, index}
            <button class:active={$currentIndex === index} on:click={() => selectItem(index)}></button>
        {/each}
    </div>
    <div class="carouselShadow"></div> <!-- Ajout de l'élément pour l'ombre -->
</div>

<style>
    .carousel {
        position: relative;
        width: 100%;
        height: 100%;
        overflow: hidden;
    }

    .carousel-inner {
        display: flex;
        transition: transform 0.5s ease-in-out;
        height: 100%;
    }

    .carousel-item {
        min-width: 100%;
        height: 100%;
        position: relative;
        display: none; /* Masquer par défaut */
    }

    .carousel-item.active {
        display: block; /* Afficher l'élément actif */
    }

    .carousel-item img,
    .carousel-item video {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .carousel-caption {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: rgba(0, 0, 0, 0.5);
        color: white;
        padding: 10px;
        text-align: center;
    }

    .carousel-cta {
        display: inline-block;
        margin-top: 10px;
        padding: 10px 20px;
        background-color: #007bff;
        color: white;
        text-decoration: none;
        border-radius: 5px;
    }

    .carousel-indicators {
        position: absolute;
        bottom: 10px;
        left: 50%;
        transform: translateX(-50%);
        display: flex;
        gap: 5px;
    }

    .carousel-indicators button {
        width: 10px;
        height: 10px;
        background-color: #fff;
        border: none;
        border-radius: 50%;
        cursor: pointer;
    }

    .carousel-indicators button.active {
        background-color: #007bff;
    }

    .carouselShadow {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: 1;
        background: rgba(0, 0, 0, 0.3);
        box-shadow: inset var(--inset-x, 60px) var(--inset-y, 50px) var(--inset-blur, 150px) #000,
                    inset calc(var(--inset-x, 60px) * -1) calc(var(--inset-y, 50px) * -1) var(--inset-blur, 150px) #000;
        pointer-events: none; /* Pour s'assurer que l'ombre ne bloque pas les interactions */
    }
</style>