<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <ShaderTest/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import ShaderTest from '@/components/ShaderTest.vue'; // @ is an alias to /src

@Component({
  components: {
    ShaderTest,
  },
})
export default class Home extends Vue {}
</script>
