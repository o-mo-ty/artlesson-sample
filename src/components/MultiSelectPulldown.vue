<template>
  <div class="pulldown">
    {{ label }}
    <div
      v-for="item in displayItems"
      :class="{ selected: isSelectedItem(item) }"
      :key="item.id"
      @click="onClick(item)"
    >
      {{ item.name }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      required: true,
    },
    items: {
      type: Array,
      required: true,
    },
    selectedItems: {
      type: Array,
      required: false,
    },
  },
  computed: {
    displayItems() {
      return this.items.filter((item) => {
        return item.display;
      });
    },
  },
  methods: {
    isSelectedItem(item) {
      if (!this.selectedItems) {
        return false;
      }
      if (this.selectedItems.find((i) => item.id === i.id)) {
        return true;
      }
      return false;
    },
    onClick(item) {
      this.$emit("select", item);
    },
  },
};
</script>

<style scoped>
.pulldown {
  display: inline-block;
  margin-right: 30px;
}

.selected{
  color: red;
}
</style>