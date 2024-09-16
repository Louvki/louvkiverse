<template>
    <div ref="matrixCanvas" class="canvas-container"></div>
  </template>
  
  <script>
  import * as THREE from 'three';
  
  export default {
    mounted() {
      this.initMatrixEffect();
    },
    methods: {
      initMatrixEffect() {
        const container = this.$refs.matrixCanvas;
        const { width, height, scene, camera, renderer, drops, matrixCharacters, fontSize } = this.initializeScene(container);
        const canvas = this.createCanvas(width, height);
        const ctx = canvas.getContext('2d');
        const texture = new THREE.CanvasTexture(canvas);
        const material = new THREE.MeshBasicMaterial({ map: texture });
        material.transparent = true;
  
        const plane = this.createPlane(width, height, material);
        scene.add(plane);
        camera.position.z = 100;
  
        const animate = () => {
          this.drawMatrix(ctx, canvas, drops, matrixCharacters, fontSize, height);
          texture.needsUpdate = true;
          renderer.render(scene, camera);
          requestAnimationFrame(animate);
        };
  
        animate();
  
        this.handleResize(renderer, camera, canvas);
      },
  
      // Initializes the scene, camera, renderer, and important variables
      initializeScene(container) {
        const width = window.innerWidth;
        const height = window.innerHeight;
  
        const scene = new THREE.Scene();
        const camera = new THREE.OrthographicCamera(
          width / -2, width / 2, height / 2, height / -2, 1, 1000
        );
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(width, height);
        container.appendChild(renderer.domElement);
  
        const matrixCharacters =
          'ABCDEFGHIJKLMNOPQRSTUVWXYZ' +
          'ΑΒΓΔΕΖΗΘΙΚΛΜΝΞΟΠΡΣΤΥΦΧΨΩ' +
          'АБВГДЕЁЖЗИЙКЛМНОПРСТУФХЦЧШЩЬЭЮЯ' +
          'ｱｲｳｴｵｶｷｸｹｺｻｼｽｾｿﾀﾁﾂﾃﾄﾅﾆﾇﾈﾉﾊﾋﾌﾍﾎﾏﾐﾑﾒﾓﾔﾕﾖﾜﾝ' +
          '0123456789';
  
        const fontSize = 20;
        const columns = Math.floor(width / fontSize);
        const drops = Array(columns).fill(0);
  
        return { width, height, scene, camera, renderer, drops, matrixCharacters, fontSize };
      },
  
      // Creates a 2D canvas element for drawing the matrix effect
      createCanvas(width, height) {
        const canvas = document.createElement('canvas');
        canvas.width = width;
        canvas.height = height;
        return canvas;
      },
  
      // Creates the plane geometry and mesh
      createPlane(width, height, material) {
        const planeGeometry = new THREE.PlaneGeometry(width, height);
        return new THREE.Mesh(planeGeometry, material);
      },
  
      // Draws the matrix effect
      drawMatrix(ctx, canvas, drops, matrixCharacters, fontSize, height) {
        // Increase the opacity for faster clearing of trails
        ctx.fillStyle = 'rgba(0, 0, 0, 0.6)'; // Faster trail clearing
        ctx.fillRect(0, 0, canvas.width, canvas.height);
  
        ctx.font = `${fontSize}px monospace`;
  
        // Draw each column
        drops.forEach((y, x) => {
          // Get the head character (white)
          const randomIndex = Math.floor(Math.random() * matrixCharacters.length)
          const headCharacter = matrixCharacters.charAt(randomIndex);
  
          // Draw the head of the cascade in white
          ctx.fillStyle = '#fff';
          ctx.fillText(headCharacter, x * fontSize, y * fontSize);
  
          // Draw the trailing characters in green
          ctx.fillStyle = '#0f0';
          for (let i = 1; i < 10; i++) {
            const randomIndex = Math.floor(Math.random() * matrixCharacters.length)
            const trailingCharacter = matrixCharacters.charAt(randomIndex)
            ctx.fillText(trailingCharacter, x * fontSize, (y - i) * fontSize);
          }
  
          // Slower falling speed by incrementing less
          if (y * fontSize > height && Math.random() > 0.975) {
            drops[x] = 0;
          } else {
            drops[x] += 0.4; // Slower fall speed by moving 0.75px per frame
          }
        });
      },
  
      // Handles resizing of the window and updates renderer and camera
      handleResize(renderer, camera, canvas) {
        window.addEventListener('resize', () => {
          const width = window.innerWidth;
          const height = window.innerHeight;
  
          renderer.setSize(width, height);
          camera.left = width / -2;
          camera.right = width / 2;
          camera.top = height / 2;
          camera.bottom = height / -2;
          camera.updateProjectionMatrix();
  
          canvas.width = width;
          canvas.height = height;
        });
      },
    },
  };
  </script>
  
  <style scoped>
  .canvas-container {
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    margin: 0;
    padding: 0;
    position: fixed;
    top: 0;
    left: 0;
    background-color: black;
  }
  </style>
  