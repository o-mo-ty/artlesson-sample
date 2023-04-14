<template>
  <div>
    <MultiSelectPulldown
      label="大項目"
      :items="majorItem"
      :selectedItems="selectedMajorItems"
      @select="onSelectMajor"
    />
    <div>{{ selectedMajorItems }}</div>
  </div>
</template>

<script>
import MultiSelectPulldown from "./MultiSelectPulldown.vue";
export default {
  components: {
    MultiSelectPulldown,
  },
  data() {
    return {
      selectedMajorItems: [],
      majorItem: [
        {
          id: 1,
          name: "スクール",
          display: true,
          emittable: true,
          default: false,
        },
        {
          id: 2,
          name: "スクール以外",
          display: true,
          emittable: true,
          default: false,
        },
      ],
      selectedKPIItems: [],
    };
  },
  methods: {
    manageItems(item, items) {
      if (items.find((i) => i.id === item.id)) {
        items = items.filter((i) => {
          console.log("aaaa", i.id, item.id);
          i.id !== item.id;
        });
      } else {
        items.push(item);
      }
    },
    onSelectMajor(item) {
      this.manageItems(item, this.selectedMajorItems);
    },
  },
};
</script>

<style scoped>
</style>