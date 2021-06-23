<template>
<!-- должен быть ТРИГГЕР(!), который присуждает ИНОЕ ЗНАЧЕНИЕ для CSS-свойства. Или :hover, или from JS... -->
<!--  v-1-->
<!-- ТРИГГЕР - изменение степени скрулла страницы отлавливается by npm-пакетом "vue-check-view", и далее дочкам добавляется новый CSS-класс. -->
  <div v-view="viewHandler" class="container">
    <div class="cleaners">
      <div v-for="(cleaner, index) in 4" :key="index"
           class="cleaner"
      >
        go! - {{index}}
      </div>
    </div>
  </div>
</template>

<!--v-2-->
<!-- ТРИГГЕР - команда via JS by @click, которая присуждает runCascadeAppearance=true. -->
<template>
  <div class="container">
    <div class="cleaners" :class="{'show': runCascadeAppearance}">
      <div v-for="(cleaner, index) in 4" :key="index"
           class="cleaner"
      >
        go! - {{index}}
      </div>
    </div>
  </div>
</template>


<!--v-3-->
<!-- ТРИГГЕР - в mounted() -->
<div class="wrapper" :class="{'show': runCascadeAppearance}" :key="`${$route.params.shelf}`">
  <div v-for="(product, ind) in GET_PRODUCTS($route.params.shelf)"
     :key="ind"
     @click.exact="onGoToProductDescription(product._id)"
     class="cart"
  >
    <product-cart :product="product"/>
  </div>
</div>




<script>
  export default {
    data: () => ({
      runCascadeAppearance: false,  //for v-2   //for v-3
    }),
    methods: {
      viewHandler(e) {    //for v-1
        const coefficient = window.innerWidth < 1024 ? 0.1 : 0.3  //можно не изголяться и стандартно выбрать 0.2
        
        if (e.percentInView > coefficient)     // e.percentInView = 0..1, how much element overlap/перекрывает/ the viewport.
          e.target.element.classList.add('show')
      }
    },
    mounted() {      //for v-3
      setTimeout(() => this.runCascadeAppearance = true, 70)
    }
  }
</script>



<style lang="scss" scoped>
  @mixin tr-reset() {
    opacity: 1;
    transform: translate(0, 0);
  }

  @function trns($how, $properties...) {
    $result: '';
    @each $property in $properties {                      //$properties - это ['opacity', 'transform'].
      $result: $result + $property + ' ' + $how + ', ';
    }
    @return #{$result};
  }
  //эта функция выдаст:
  //"opacity linear .5s, transform  linear .5s, "

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

