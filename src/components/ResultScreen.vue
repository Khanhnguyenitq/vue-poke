<template>
  <div class="screen">
    <h1>Congratulation</h1>
    <p>{{ Math.round(timer / 1000) }} seconds</p>
    <button>Start Again</button>
  </div>
</template>

<script>
export default {
  props: {
    timer: {
      type: Number,
      required: true,
    },
  },
};
</script>
