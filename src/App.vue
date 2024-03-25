<template>
  <FormComponent />
</template>

<script>
import FormComponent from '@/components/FormComponent.vue'

export default {
  name: 'App',
  components: {
    FormComponent
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Montserrat&display=swap');

body {
  font-family: 'Montserrat', sans-serif;
  margin: 0;
}
* {
  box-sizing: border-box;
}
</style>
