<template>
  <div class="firefly-container" :style="fireflyStyle">
    <div v-for="i in qty" :key="i" class="firefly"></div>
  </div>
</template>

<script>
export default {
  name: "FireflyCss",
  props: {
    qty: {
      type: Number,
      default: 10
    },
    fireflyBkColor:{
      type: String,
      default: 'aliceblue'
    }, 
    fireflyLightColor: {
      type: String,
      default: 'yellow'
    }, 
  },
  computed:{
    fireflyStyle() {
      return {
        "--bk-color": this.fireflyBkColor,
        "--light-color": this.fireflyLightColor,
      };
    },
  },
  methods: {
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.firefly-container {
  .firefly {
    position: fixed;
    left: 50%;
    top: 50%;
    pointer-events: none;
    width: 0.4vw;
    height: 0.4vw;
    margin: -0.2vw 0 0 9.8vw;
    &::before,
    &::after {
      content: "";
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      transform-origin: -10vw;
    }
    &::before {
      background: var(--bk-color);
      opacity: 0.4;
      animation: drift ease alternate infinite;
    }
    &::after {
      background: white;
      opacity: 0;
      box-shadow: 0 0 0vw 0vw var(--light-color);
      animation: drift ease alternate infinite, flash ease infinite;
    }
    animation: ease 200s alternate infinite;
  }
  $flyQty: 40; // max qty
  @for $i from 1 through $flyQty {
    $steps: random(12) + 16;
    $rotateSpeed: random(10) + 8s;
    @keyframes move#{$i} {
      @for $step from 0 to $steps {
        #{percentage($step / ($steps - 1))} {
          transform: translateX(random(100) - 50vw)
            translateY(random(100) - 50vh)
            scale(random(75) / 100 + 0.25);
        }
      }
    }
    .firefly:nth-child(#{$i}) {
      animation-name: move#{$i};
      &::before {
        animation-duration: $rotateSpeed;
      }
      &::after {
        animation-duration: $rotateSpeed, random(6) + 5s;
        animation-delay: 0s, random(5000) + 0ms;
      }
    }
  }
  @keyframes drift {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
  @keyframes flash {
    0%,
    30%,
    100% {
      opacity: 0;
      box-shadow: 0 0 0vw 0vw var(--light-color);
    }
    5% {
      opacity: 1;
      box-shadow: 0 0 2vw 0.4vw var(--light-color);
    }
  }
}
</style>
