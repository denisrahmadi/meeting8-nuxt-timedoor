<template>
  <div class="mb-3">
    <label for="exampleFormControlTextarea1" class="form-label"
      >Recipe Description</label
    >
    <textarea
      class="form-control"
      id="exampleFormControlTextarea1"
      rows="3"
      @change="model($event)"
    ></textarea>
  </div>
</template>
<script>
export default {
  props: ["model"],
};
</script>
<style scoped></style>
