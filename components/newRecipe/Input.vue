<template>
  <div :class="text ? 'input-group mb-3 flex' : 'mb - 3 flex'">
    <label for="exampleFormControlInput1" class="form-label">{{
      labels
    }}</label>
    <div class="flex-row">
      <input
        type="text"
        class="form-control"
        id="exampleFormControlInput1"
        placeholder="name@example.com"
        @change="model($event)"
        :value="value"
      />
      <button
        class="btn btn-success"
        type="button"
        id="button-addon2"
        v-show="text"
        @click="click"
      >
        {{ text }}
      </button>
      <button
        class="btn btn-danger"
        type="button"
        id="button-addon2"
        v-show="count > 1"
        @click="Delete(index)"
      >
        Delete
      </button>
    </div>
  </div>
</template>
<script>
export default {
  props: [
    "labels",
    "text",
    "model",
    "click",
    "count",
    "index",
    "Delete",
    "value",
  ],
};
</script>
<style scoped>
.flex {
  display: flex;
  flex-direction: column;
}
.flex-row {
  display: flex;
  flex-direction: row;
}
</style>
