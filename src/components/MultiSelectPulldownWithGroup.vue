<!-- グループ内での複数選択を行うコンポーネント -->
<!-- 親-子以上深い階層は受け入れない -->
<template>
  <div class="pulldown">
    <div class="header">
      {{ label }}
    </div>
    <div>
      <div
        v-for="item in displayItems"
        :class="{ selected: isSelectedItem(item) }"
        :key="item.id"
        @click="onClick(item)">
        {{ item.name }}
      </div>
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
    // ここでサブアイテムも平たく配列に展開
    displayItems() {
      let displayItems = [];
      this.items.forEach((item) => {
        displayItems.push(item);
        if (item.subItems) {
          item.subItems.forEach((subItem) => {
            displayItems.push(subItem);
          });
        }
      })
      return displayItems
    },
  },
  methods: {
    hasSubItem(item) {
      return item.hasOwnProperty("subItems");
    },
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
      if (this.hasSubItem(item)) {
        console.log('oya!')
      } else {
        console.log('sub!')
      }
    },
    // onClickMain(mainItem) {
    //   this.$emit("mainItemSelect", mainItem);
    // },
    // onClickSub(subItem) {
    //   this.$emit("subItemSelect", subItem);
    // }
  },
};
</script>

<style scoped>
.pulldown {
  display: inline-block;
  margin-right: 30px;
}
.header {
  border: 1px solid #ccc;
}
.selected{
  color: red;
}
</style>