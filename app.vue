<template>
  <div>
    <h1>Camera Access</h1>
    <video ref="video" autoplay playsinline></video>
    <div>
      <button @click="startCamera('user')">Front Camera</button>
      <button @click="startBackCamera">Back Camera</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const video = ref(null);

// Function to start the front camera
const startCamera = async (facingMode) => {
  try {
    const stream = await navigator.mediaDevices.getUserMedia({
      video: { facingMode },
    });
    video.value.srcObject = stream;
  } catch (error) {
    console.error('Error accessing the camera: ', error);
  }
};

// Function to check for back camera and start it
const startBackCamera = async () => {
  const devices = await navigator.mediaDevices.enumerateDevices();
  const hasBackCamera = devices.some(device => device.kind === 'videoinput' && device.label.toLowerCase().includes('back'));

  if (hasBackCamera) {
    startCamera('environment');
  } else {
    alert("Your device does not have a back camera.");
  }
};
</script>

<style scoped>
video {
  width: 100%;
  max-width: 600px;
  border: 2px solid #ccc;
}
</style>
