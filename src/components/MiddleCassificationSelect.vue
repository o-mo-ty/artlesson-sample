<!-- 中分類を選択するコンポーネントのサンプル -->
<template>
  <div>
    <MultiSelectPulldownWithGroup
      label="中項目"
      :items="middleClassificationItems"
      :selectedItems="selectedMiddleClassifications"
      @groupSelect="onGroupSelect"
      @subItemSelect="onSubItemSelect"
    />
    <div>選択状況：{{ selectedMiddleClassifications }}</div>
  </div>
</template>

<script>
// TODO:グループを選択した場合には、子項目全選択/全選択解除のトグル
// => COMPLETED
// TODO:子項目を持たない要素もある
// => COMPLETED
// TODO:異なるグループの要素を同時に選択する事も可能
// => COMPLETED
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
        },
        {
          id: 3,
          name: "【単独1】サブアイテムなし",
        },
        {
          id: 4,
          name: "【単独2】サブアイテムなし",
        },
      ],
    };
  },
  methods: {
    // manageItems(item, items) {
    //   if (items.find((i) => i.id === item.id)) {
    //     const index = items.findIndex((i) => i.id === item.id);
    //     items.splice(index,1);
    //   } else {
    //     items.push(item);
    //   }
    // },
    // onSelectMajor(item) {
    //   this.manageItems(item, this.selectedMajorItems);
    // },
    onGroupSelect(item) {
      const ids = item.subItems.map((i) => i.id);
      const isAlreadySelectedGroup = this.selectedMiddleClassifications.some((item) => ids.includes(item.id));
      if(isAlreadySelectedGroup) {
        console.log('サブアイテムが既に選択済みだよ!')
        this.selectedMiddleClassifications = this.selectedMiddleClassifications.filter((item) => !ids.includes(item.id));
      } else {
        this.selectedMiddleClassifications = this.selectedMiddleClassifications.concat(item.subItems);
        console.log('サブアイテムはまだ選択されてないよ！')
      }
    },
    onSubItemSelect(item) {
      if (this.selectedMiddleClassifications.find((i) => i.id === item.id)) {
        const index = this.selectedMiddleClassifications.findIndex((i) => i.id === item.id);
        this.selectedMiddleClassifications.splice(index,1);
      } else {
        this.selectedMiddleClassifications.push(item);
      }
      console.log('called onSubItemSelect');
    }
  },
};
</script>

<style scoped>
</style>