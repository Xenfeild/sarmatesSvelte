<style lang="scss">
    @import "../src/style/style.scss";


</style>

<script lang="ts">
    import { onMount } from "svelte";
    import Header from "../components/header.svelte";
    import CarousselHero from "../components/carousselHero.svelte";
    import { translations, loadTranslations } from '../stores/translationStore';

    onMount(() => {
        const userLang = navigator.language || navigator.language;
        const lang = userLang.split('-')[0]; // 'fr', 'en', 'es'
        loadTranslations(lang);
    });

    // function changeLanguage(lang: string) {
    //     loadTranslations(lang);
    // }
</script>

<Header/>
<main>
    <!-- <div class="useless"></div> -->
    <!-- <Carousel autoplay autoplayDuration={2000} style="height: 100vh;">        
        <img src="https://picsum.photos/200/300" alt="image1"/>
        <img src="src/lib/img/bifrost2022.jpg" alt="image2"/>
    </Carousel> -->
    <!-- <CarousselHero /> -->
    <div class="test">
        <CarousselHero />
        <!-- <CarousselSlider /> -->
        
    </div>
    <div class="test2">
        <h1>{$translations.welcome}</h1>
        <p>{$translations.working}</p>
    </div>
</main>


