<script setup>
import { defineAsyncComponent } from 'vue'
let activeKey = '01'

const comp = defineAsyncComponent(() =>
   import(`./views/${activeKey}.vue`)
)
</script>
<template>
     <component :is="comp"> </component>
</template>

<style>
* {
 padding: 0;
 margin: 0;
 box-sizing: border-box;
}

html {
  height: 100%;

}

.container {
  height: 100%;

}
</style>
