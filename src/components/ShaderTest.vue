<template>
  <v-container>
    <v-layout text-xs-center wrap>
      <v-flex xs12 sm6 mb-5>
        <h2>Render</h2>
        <canvas id="glCanvas" width="500" height="350"></canvas>
      </v-flex>
      <v-flex xs12 sm6 mb-5>
        <h2>Fragment shader</h2>
        <v-btn color="info" @click="reRender()">random color</v-btn>
        <v-text-field
				   type="text" 
				   placeholder="TODO" 
				   @keypress="reRender()"
				   ></v-text-field>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    mounted () {
      this.runWebGL()
    },
    data: {
      gl: ""
    },
    methods: {
      runWebGL() {
        this.gl = document.querySelector("#glCanvas").getContext("webgl");

        if (this.gl === null) {
          alert("Unable to initialize WebGL. Your browser or machine may not support it.");
          return;
        }

        this.gl.clearColor(1.0, 0.0, 1.0, 1.0);
        this.gl.clear(this.gl.COLOR_BUFFER_BIT);
      },

      reRender() {
        this.gl.clearColor(Math.random(1), Math.random(1), Math.random(1), 1);
        this.gl.clear(this.gl.COLOR_BUFFER_BIT);
      }
    }
  }
</script>

<style>

</style>
