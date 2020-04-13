<template>
  <div v-view="viewHandler" class="container">
    <div class="cleaners">
      <div v-for="(cleaner, index) in 4" :key="index"
           class="cleaner">
        go! - {{index}}
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    methods: {
      viewHandler(e) {
        const coeff = window.innerWidth < 1024 ? 0.1 : 0.3
        if (e.percentInView > coeff) e.target.element.classList.add('show')
      }
    }
  }
</script>
<style lang="scss" scoped>
  @mixin tr-reset() {
    opacity: 1;
    transform: translate(0, 0);
  }

  @function trns($h, $s...) {
    $f: '';
    @each $t in $s {
      $f: $f + $t + ' ' + $h + ', ';
    }
    @return #{$f};
  }
  /*эта функция выдаст:*/
  /*opacity linear .5s*/
  /*transform  linear .5s*/

  $base-delay: 0.2s;
  $OutCubic: cubic-bezier(0.215, 0.61, 0.355, 1);
  $appear: $OutCubic 0.5s;

  .show {
    .cleaners {
      @for $i from 1 through 6 {
        .cleaner:nth-child(#{$i}) {
          @include tr-reset();
          transition: trns($appear, opacity, transform);
          transition-delay: ($base-delay + $i * $base-delay);
        }
      }
    }
  }

  .container {
    max-height: 400px;

    .cleaners {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      grid-column-gap: 22px;

      .cleaner {
        min-width: 300px;
        height: 300px;
        background: red;
        box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.16);
        border-radius: 8px;
        position: relative;
        opacity: 0;
        transform: translateY(-30px);
      }
    }
  }
</style>

