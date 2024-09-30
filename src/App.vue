<template>
  <div
    class="container d-flex flex-row align-items-center justify-content-around"
    style="height: 100% !important"
  >
    <div
      id="boxShadowPreview"
      class="d-flex align-items-center justify-content-center"
      style="height: 400px; width: 400px"
      :style="boxShadowStyle"
    >
      <div
        @click="copyCodeHandle"
        class="bg-dark text-light d-flex flex-row align-items-center justify-content-center p-2 border rounded-3"
        role="button"
      >
        <i class="bi bi-copy me-2"></i>
        <p ref="boxShadowCode" class="m-0">
          box-shadow : {{ shadowX }}px {{ shadowY }}px {{ blurRadius }}px
          {{ shadowSize }}px {{ shadowColor }};
        </p>
      </div>
    </div>
    <div id="boxShadowEditor" style="width: 300px">
      <div id="X-shadow">
        <label class="form-label">Shadoe-X :</label>
        <input
          type="range"
          v-model="shadowX"
          min="-150"
          max="158"
          class="form-range"
        />
      </div>
      <div id="Y-shadow">
        <label class="form-label">Shadoe-Y :</label>
        <input
          type="range"
          v-model="shadowY"
          min="-150"
          max="158"
          class="form-range"
        />
      </div>
      <div id="Blur">
        <label class="form-label">Blur :</label>
        <input type="range" v-model="blurRadius" class="form-range" />
      </div>
      <div id="size">
        <label class="form-label">Size :</label>
        <input type="range" v-model="shadowSize" class="form-range" />
      </div>
      <div
        id="boxBgColor"
        class="d-flex flex-row align-items-center justify-content-between mb-2"
      >
        <label class="form-label me-3">background :</label>
        <input
          type="color"
          class="form-control form-control-color"
          v-model="boxBgColor"
        />
      </div>
      <div
        id="shadowColor"
        class="d-flex flex-row align-items-center justify-content-between"
      >
        <label class="form-label me-3">shadow color :</label>
        <input
          type="color"
          class="form-control form-control-color"
          v-model="shadowColor"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from "vue";
import Swal from "sweetalert2";

const shadowX = ref(0);
const shadowY = ref(0);
const blurRadius = ref(0);
const shadowSize = ref(0);
const boxBgColor = ref("#007bff");
const shadowColor = ref("#000");

const boxShadowStyle = computed(() => {
  return {
    backgroundColor: boxBgColor.value,
    boxShadow: `${shadowX.value}px ${shadowY.value}px ${blurRadius.value}px ${shadowSize.value}px ${shadowColor.value}`,
  };
});

const boxShadowCode = ref(null);
const copiedMessage = ref(null);
function copyCodeHandle() {
  const text = boxShadowCode.value.innerHTML;

  // for test error
  // navigator.clipboard.writeText = () => Promise.reject(new Error("Simulated error")); 
  
  navigator.clipboard
    .writeText(text)
    .then(() => {
      copiedMessage.value = "Code copied successfully!";
      const Toast = Swal.mixin({
        toast: true,
        position: "top-end",
        showConfirmButton: false,
        timer: 2000,
        timerProgressBar: true,
      });
      Toast.fire({
        icon: "success",
        title: copiedMessage.value,
      });
    })
    .catch((err) => {
      console.error("Error copying the code!", err);
      copiedMessage.value = "Error copying the code!";  
      const Toast = Swal.mixin({
        toast: true,
        position: "top-end",
        showConfirmButton: false,
        timer: 2000,
        timerProgressBar: true,
      });
      Toast.fire({
        icon: "error",
        title: copiedMessage.value,
      });
    });
}
</script>

<style>
body {
  background-color: #ecf0f1;
}
</style>
