<script>
    import { onMount } from "svelte";
    import { translations, loadTranslations } from '../stores/translationStore';

    onMount(() => {
        const userLang = navigator.language || navigator.language;
        const lang = userLang.split('-')[0]; // 'fr', 'en', 'es'
        loadTranslations(lang);
    });
</script>

<section class="news">
    <h2>{$translations.news}</h2>
</section>

<style lang="scss">
    @import "../src/style/style.scss";
</style>