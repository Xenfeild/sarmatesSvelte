<style lang="scss">
    @import "../src/style/style.scss";


</style>

<script lang="ts">
    import { onMount } from "svelte";
    import Header from "../components/header.svelte";
    import CarousselHero from "../components/carousselHero.svelte";
    // import CarousselSlider from "../components/CarousselSlider.svelte";

    interface Translations {
        title: string;
        description: string;
        [key:string]: string;
    }

    let translations: Translations = {
        title: "",
        description: "",
    };

    async function loadTranslations(lang: string) {
        try {
            const response = await fetch(`/src/locales/${lang}.json`);
            if (!response.ok) {
                throw new Error(`Failed to load translations for ${lang}`);
            }
            translations = await response.json();
            document.title = translations.title;
            const metaDescription = document.querySelector('meta[name="description"]');
            if (metaDescription) {
                metaDescription.setAttribute('content', translations.description);
            }
            document.documentElement.lang = lang;
        } catch (error) {
            console.error("Error loading translations:", error);
        }
    }

    onMount(() => {
        const userLang = navigator.language || navigator.language;
        const lang = userLang.split('-')[0]; // 'fr', 'en', 'es'
        loadTranslations(lang);
    });

    function handleLanguageChange(event: CustomEvent<{ lang: string }>) {
        const { lang } = event.detail;
        loadTranslations(lang);
    }
</script>

<Header on:languageChange={handleLanguageChange}/>
<main>
    <div class="useless"></div>
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
        <h1>{translations.welcome}</h1>
        <p>{translations.working}</p>
    </div>
</main>


