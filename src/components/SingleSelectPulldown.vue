<template>
  <div class="pulldown">
    {{ displayName }}
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
    pulldownIndex: {
      type: Number,
      required: true,
    },
    items: {
      type: Array,
      required: true,
    },
    selectedItem: {
      type: Object,
      required: false,
    },
  },
  // data() {
  //   return {
  //     selectedItem: null,
  //   };
  // },
  computed: {
    displayItems() {
      return this.items.filter((item) => {
        return item.display;
      });
    },
    displayName() {
      return this.selectedItem ? this.selectedItem.name : "";
    },
  },
  methods: {
    isSelectedItem(item) {
      if (!this.selectedItem) {
        return false;
      }
      return item.id === this.selectedItem.id;
    },
    onClick(item) {
      // 選択前の選択値
      const before = this.selectedItem ? JSON.parse(JSON.stringify(this.selectedItem)) : null;
      if (item.emittable) {
        this.$emit("select", {index: this.pulldownIndex, before, after: item});
      }
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