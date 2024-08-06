<template>
  <div class="no-more-history-container">
    <div class="no-more-history">
      <div class="line"></div>
      <p>No more history!</p>
      <div class="line"></div>
    </div>
  </div>
</template>

<script setup></script>

<style scoped>
.no-more-history-container {
  text-align: center;
  padding: 20px;
}

.no-more-history {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
  margin: auto;
}

.line {
  width: 100px;
  height: 1px;
  background: #edeeef;
  flex-grow: 1;
}

p {
  margin: 0;
  font-weight: 400;
  font-size: 12px;
  color: #707177;
}

@media (max-width: 600px) {
  .line {
    width: 50px;
  }
}
</style>
