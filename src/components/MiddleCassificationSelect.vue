<!-- 中分類を選択するコンポーネントのサンプル -->
<template>
  <div>
    <MultiSelectPulldownWithGroup
      label="中項目"
      :items="middleClassificationItems"
      :selectedItems="selectedMiddleClassifications"
      @mainItemSelect="onMainItemSelect"
      @subItemSelect="onSubItemSelect"
    />
    <div>{{ selectedMiddleClassifications }}</div>
  </div>
</template>

<script>
import MultiSelectPulldownWithGroup from "./MultiSelectPulldownWithGroup.vue";
export default {
  components: {
    MultiSelectPulldownWithGroup,
  },
  data() {
    return {
      selectedMiddleClassifications: [],
      middleClassificationItems: [
        {
          id: 1,
          name: "【親階層】初期費用",
          subItems: [
            {id: '1-1', name: '月会費'},
            {id: '1-2', name: '教材費'},
            {id: '1-3', name: '入会金'},
            {id: '1-4', name: '楽器プレゼント'},
            {id: '1-5', name: '施設代'},
          ],
          display: true,
          emittable: true,
          default: false,
        },
        {
          id: 2,
          name: "【親階層】何かのグループ",
          subItems: [
            {id: '2-1', name: 'アイテム1'},
            {id: '2-2', name: 'アイテム2'},
            {id: '2-3', name: 'アイテム3'},
            {id: '2-4', name: 'アイテム4'},
            {id: '2-5', name: 'アイテム5'},
          ],
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
        const index = items.findIndex((i) => i.id === item.id);
        items.splice(index,1);
      } else {
        items.push(item);
      }
    },
    // onSelectMajor(item) {
    //   this.manageItems(item, this.selectedMajorItems);
    // },
    onMainItemSelect(item) {
      console.log('called onMainItemSelect');
    },
    onSubItemSelect(item) {
      console.log('called onSubItemSelect');
    }
  },
};
</script>

<style scoped>
</style>