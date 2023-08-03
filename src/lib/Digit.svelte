<script lang="ts">
  import "swiper/css";
  import Swiper from "swiper";
  import { onMount, createEventDispatcher } from "svelte";

  export let resetDigit = 0;

  let swiperEl;
  let swiper;

  $: {
    if (swiper)
      swiper.slideTo(resetDigit);
  }

  const dispatch = createEventDispatcher();

  onMount(async () => {
    swiper = new Swiper(swiperEl, {
      slidesPerView: 1,
      spaceBetween: 10,
      loop: true,
      direction: "vertical",
      pagination: {
        el: ".swiper-pagination",
        clickable: true,
      },
      navigation: {
        nextEl: ".swiper-button-next",
        prevEl: ".swiper-button-prev",
      },
    });

    dispatch("swiper_init", {
      swiper,
    });
  });
</script>

<div class="digit">
  <div class="swiper mySwiper" bind:this={swiperEl}>
    <div class="swiper-wrapper">
      {#each [...Array(10).keys()] as i}
        <div class="swiper-slide">{i}</div>
      {/each}
    </div>
  </div>
</div>

<style lang="scss">
  .digit {
    background-color: #ddd;
    border-radius: 4px;
    padding: 6px;
  }

  .mySwiper {
    height: 40px;
  }

  .swiper-slide {
    height: 40px;
    font-size: 20px;
  }
</style>
