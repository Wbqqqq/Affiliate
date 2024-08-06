<template>
  <div id="app">
    <HeaderBar />
    <MainContent />
  </div>
</template>

<script setup>
import HeaderBar from "./components/HeaderBar.vue";
import MainContent from "./components/MainContent.vue";
import { onMounted } from "vue";

onMounted(() => {
  window.onload = function () {
    document.addEventListener("touchstart", function (event) {
      if (event.touches.length > 1) {
        event.preventDefault();
      }
    });
    document.addEventListener("gesturestart", function (event) {
      event.preventDefault();
    });
  };

  window.onload = function () {
    document.addEventListener("gesturestart", function (e) {
      e.preventDefault();
    });
    document.addEventListener("dblclick", function (e) {
      e.preventDefault();
    });
    document.addEventListener("touchstart", function (event) {
      if (event.touches.length > 1) {
        event.preventDefault();
      }
    });
    let lastTouchEnd = 0;
    document.addEventListener(
      "touchend",
      function (event) {
        var now = new Date().getTime();
        if (now - lastTouchEnd <= 300) {
          event.preventDefault();
        }
        lastTouchEnd = now;
      },
      false
    );
  };
});
</script>
