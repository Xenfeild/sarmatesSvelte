<script>
    import { onMount } from "svelte";
    import sliderContent from "./sliderContent.js";

    let carousselElement;
    let slides = [];
    let texts = [];
    let videoShadowVisible = false;
    let counter = 0;
    const tempoImage = 10000;

    onMount(() => {
        initializeCaroussel();
        const interval = setInterval(slide, tempoImage);
        return () => clearInterval(interval);
    });

    function initializeCaroussel() {
        const mediaSize = window.innerWidth < 769 ? 'm' : 'xl';
        const imgFolder = mediaSize === 'm' ? './img/m/' : './img/';

        if (mediaSize === 'm') {
            const root = document.querySelector(':root');
            root.style.setProperty('--inset-x', '60px');
            root.style.setProperty('--inset-y', '50px');
            root.style.setProperty('--inset-blur', '150px');
        }

        slides = sliderContent.map((elt, idx) => {
            const fileExtension = elt.imageUrl.split('.').pop();
            const slide = {
                id: 'slide' + idx,
                type: fileExtension === 'mp4' ? 'video' : 'image',
                src: imgFolder + elt.imageUrl,
                trackSrc: fileExtension === 'mp4' ? imgFolder + elt.imageUrl.split('.')[0] + '.vtt' : null,
                transform: idx > 0 ? (idx % 2 === 0 ? "translateX(-100%)" : "translateX(100%)") : "translateX(0)"
            };
            return slide;
        });

        texts = sliderContent.map((elt, idx) => {
            const text = {
                id: 'text' + idx,
                title: elt.title,
                button: elt.bouton,
                buttonUrl: elt.boutonUrl,
                transform: idx % 2 === 0 ? "translateX(100%)" : "translateX(-100%)",
                opacity: 0
            };
            return text;
        });
    }

    function slide() {
        const sliderContentLength = sliderContent.length - 1;

        if (counter === 0) {
            slides[sliderContentLength].transform = 'translateX(100%)';
            texts[sliderContentLength].transform = 'translateX(-100%)';
            texts[sliderContentLength].opacity = 0;
        } else {
            if (counter % 2 === 0) {
                slides[counter - 1].transform = 'translateX(100%)';
                texts[counter - 1].transform = 'translateX(-100%)';
                texts[counter - 1].opacity = 0;
            } else {
                slides[counter - 1].transform = 'translateX(-100%)';
                texts[counter - 1].transform = 'translateX(100%)';
                texts[counter - 1].opacity = 0;
            }
        }

        slides[counter].transform = 'translateX(0)';
        videoShadowVisible = slides[counter].type === 'video';
        texts[counter].transform = 'translateX(0)';
        texts[counter].opacity = 1;

        counter++;
        if (counter > sliderContentLength) counter = 0;
    }
</script>

<style>
    .slider {
    width: 100%;
    height: 100%;
    position: relative;
    overflow-x: hidden;
}

.slideBg{
    position: absolute;
    z-index: -3;
    width: 100%;
    height: 100%;
    background-position: 50%;
    background-size: cover;
    background-repeat: no-repeat;
    transition: transform 0.5s ease-out;
}
.videoBg {
    position: absolute;
    z-index: -3;
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.5s ease-out;
}
.videoShadow {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -2;
    background: rgba(0, 0, 0, 0.3);
    box-shadow: inset var(--inset-x) var(--inset-y) var(--inset-blur) #000,
                inset calc(var(--inset-x) * -1) calc(var(--inset-y) * -1) var(--inset-blur) #000;
    opacity: 0;
}
.slideBg::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -2;
    background: rgba(0, 0, 0, 0.3);
    box-shadow: inset var(--inset-x) var(--inset-y) var(--inset-blur) #000,
                inset calc(var(--inset-x) * -1) calc(var(--inset-y) * -1) var(--inset-blur) #000;
}


.textBg {
    position: absolute;
    user-select: none;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    gap: 1rem;
    transition: transform 0.8s 0.5s ease-out,
                opacity 0.8s 0.2s ease;
    text-align: center;
    font-size: 1.5rem;
    font-weight: 900;
    text-shadow: 1px 1px 2px #000;
    color: #fff;
}
.textBg .btnSlide {
    font-size: 1rem;
    padding: 1rem 1.5rem;
    cursor: pointer;
    background-color: var(--clr-secondary);
    border: none;
    border-radius: 5px;
}
.textBg .btnSlide a {
    text-decoration: none;
    color: var(--clr-primary);
}

@media (min-width: 785px) {
    .textBg {
        font-size: 2.5rem;
    }
    .textBg .btnSlide {
        font-size: 1.5rem;
    }
}
</style>


<div bind:this={carousselElement} class="carousel"></div>