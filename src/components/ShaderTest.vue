<template>
  <v-container>
    <v-layout text-xs-center wrap>
      <v-flex xs12 sm6 mb-5>
        <h2>Render</h2>
        <canvas id="glCanvas" width="500" height="350"></canvas>
      </v-flex>
      <v-flex xs12 sm6 mb-5>
        <h2>Fragment shader</h2>
        <v-flex xs12 mb-1>
          <v-btn color="info" @click="randomColor()">random color</v-btn> or start typing
        </v-flex>
          <textarea id="text-inp" width="" height="200" v-on:keyup="shader()">
void main(void) {
  gl_FragColor = vec4(gl_FragCoord.x / 640.0, gl_FragCoord.y / 480.0, 0, 1);
}
          </textarea>
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

      randomColor() {
        this.gl.clearColor(Math.random(1), Math.random(1), Math.random(1), 1);
        this.gl.clear(this.gl.COLOR_BUFFER_BIT);
      },

      shader() {
        console.log("Its shader time");
        // Vertex Shader
        var vertCode =
          'attribute vec2 a_position;' +
          
          'void main(void) {' +
              ' gl_Position = vec4(a_position, 0, 1);' +
          '}';

        var vertexShader = this.gl.createShader(this.gl.VERTEX_SHADER);
        this.gl.shaderSource(vertexShader, vertCode);
        this.gl.compileShader(vertexShader);
        
        // Fragment Shader
        var fragCode = String(document.getElementById("text-inp").value);

        var fragmentShader = this.gl.createShader(this.gl.FRAGMENT_SHADER);
        this.gl.shaderSource(fragmentShader, fragCode);
        this.gl.compileShader(fragmentShader);

        var success = this.gl.getShaderParameter(fragmentShader, this.gl.COMPILE_STATUS);
        if (!success) { // If the shader is wrong we 
          this.gl.clearColor(1.0, 0.0, 1.0, 1.0);
          this.gl.clear(this.gl.COLOR_BUFFER_BIT);
          return
        }

        var program = this.gl.createProgram();
        this.gl.attachShader(program, vertexShader);
        this.gl.attachShader(program, fragmentShader);
        this.gl.linkProgram(program);	
        this.gl.useProgram(program);

        var buffer = this.gl.createBuffer();
        this.gl.bindBuffer(this.gl.ARRAY_BUFFER, buffer);
        this.gl.bufferData(
          this.gl.ARRAY_BUFFER, 
          new Float32Array([
            -1.0, -1.0, 
            1.0, -1.0, 
            -1.0,  1.0, 
            -1.0,  1.0, 
            1.0, -1.0, 
            1.0,  1.0]), 
          this.gl.STATIC_DRAW
        );

        var positionLocation = this.gl.getAttribLocation(program, "a_position");
        this.gl.enableVertexAttribArray(positionLocation);
        this.gl.vertexAttribPointer(positionLocation, 2, this.gl.FLOAT, false, 0, 0);

        this.gl.drawArrays(this.gl.TRIANGLES, 0, 6);
      }
    }
  }
</script>

<style>
  #text-inp { z-index:999; width: 300px; height: 300px;   }
</style>
