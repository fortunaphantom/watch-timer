<script lang="ts">
  import { onMount } from "svelte/internal";
  import Swiper from "swiper";
  import Digit from "./Digit.svelte";
  import { withPrevious } from "svelte-previous";

  export let resetTime;
  const [current, previous, oldest] = withPrevious(resetTime, {
    numToTrack: 2,
  });
  $: $current = resetTime;

  let secs = resetTime % 60;
  let mins = ((resetTime - secs) / 60) % 60;
  let hours = ((resetTime - secs - mins * 60) / 3600) % 24;
  let days = (resetTime - secs - mins * 60 - hours * 3600) / 86400;
  let digits1: number[] = [];
  let digits2: number[] = [];

  let swiper: Swiper[] = [];

  let resetTimeState = 0;

  $: {
    resetTimeState = resetTime;
  }

  const calculateDigits = (time: number) => {
    const secs1 = time % 60;
    const mins1 = ((time - secs1) / 60) % 60;
    const hours1 = ((time - secs1 - mins1 * 60) / 3600) % 24;
    const days1 = (time - secs1 - mins1 * 60 - hours1 * 3600) / 86400;

    let res: number[] = [];
    res[0] = Math.floor(days1 / 10);
    res[1] = Math.floor(days1 % 10);
    res[2] = Math.floor(hours1 / 10);
    res[3] = Math.floor(hours1 % 10);
    res[4] = Math.floor(mins1 / 10);
    res[5] = Math.floor(mins1 % 10);
    res[6] = Math.floor(secs1 / 10);
    res[7] = Math.floor(secs1 % 10);
    return res;
  };

  const focusHandler = () => {
    console.log("onfocus");
    digits2 = calculateDigits(resetTimeState);
    for (let i = 0; i < 8; i++) {
      swiper[i].slideTo(digits2[i]);
    }
  };

  onMount(() => {
    const timerId = setInterval(() => {
      console.log(resetTimeState);
      resetTimeState--;
      if (resetTimeState < 0) {
        return;
      }
      digits2 = calculateDigits(resetTimeState);

      console.log(digits1);
      console.log(digits2);

      for (let i = 0; i < 8; i++) {
        console.log(`swiper${i}: `, swiper[i].activeIndex);
        if (digits1[i] != digits2[i]) {
          if (
            (digits1[i] + 9) % 10 == digits2[i]
          ) {
            swiper[i].slidePrev();
          } else {
            swiper[i].slideTo(digits2[i]);
          }
        }
      }

      digits1 = [...digits2];
    }, 1000);
    // window.addEventListener("focus", focusHandler);
    return () => {
      clearInterval(timerId);
      // window.removeEventListener("focus", focusHandler);
    }
  });
</script>

<div class="watch-timer">
  <Digit
    on:swiper_init={(e) => (swiper[0] = e.detail.swiper)}
    resetDigit={Math.floor(days / 10)}
  />
  <Digit
    on:swiper_init={(e) => (swiper[1] = e.detail.swiper)}
    resetDigit={Math.floor(days % 10)}
  />
  :
  <Digit
    on:swiper_init={(e) => (swiper[2] = e.detail.swiper)}
    resetDigit={Math.floor(hours / 10)}
  />
  <Digit
    on:swiper_init={(e) => (swiper[3] = e.detail.swiper)}
    resetDigit={Math.floor(hours % 10)}
  />
  :
  <Digit
    on:swiper_init={(e) => (swiper[4] = e.detail.swiper)}
    resetDigit={Math.floor(mins / 10)}
  />
  <Digit
    on:swiper_init={(e) => (swiper[5] = e.detail.swiper)}
    resetDigit={Math.floor(mins % 10)}
  />
  :
  <Digit
    on:swiper_init={(e) => (swiper[6] = e.detail.swiper)}
    resetDigit={Math.floor(secs / 10)}
  />
  <Digit
    on:swiper_init={(e) => (swiper[7] = e.detail.swiper)}
    resetDigit={Math.floor(secs % 10)}
  />
</div>

<style lang="scss">
  .watch-timer {
    display: flex;
    gap: 6px;
  }
</style>
