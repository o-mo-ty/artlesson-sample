<template>
  <div>
    <SingleSelectPulldown
      v-for="n of 5"
      :key="n"
      :pulldownIndex="n"
      :items="getOrderItems(n)"
      :selectedItem="getSelectedItem(n)"
      @select="onSelect"
    />
    <div>
      {{ selectedOrderItems }}
    </div>
  </div>
</template>

<script>
import SingleSelectPulldown from "./SingleSelectPulldown.vue";
export default {
  components: {
    SingleSelectPulldown
  },
  data() {
    return {
      selectedOrderItems: [],
      orderItems: [
        {
          id: 999,
          name: "表示しない",
          display: true,
          default: true,
          selectable: false,
        },
        { id: 1, name: "科目", display: true, default: false },
        {
          id: 2,
          name: "サービス",
          display: true,
          default: false,
          selectable: true,
        },
        {
          id: 3,
          name: "ジャンル",
          display: true,
          default: false,
          selectable: true,
        },
        {
          id: 4,
          name: "コース",
          display: true,
          default: false,
          selectable: true,
        },
        { id: 5, name: "地域", display: true, default: false, selectable: true, },
        {
          id: 6,
          name: "都道府県",
          display: true,
          default: false,
          selectable: true,
        },
        {
          id: 7,
          name: "スペース",
          display: true,
          default: false,
          selectable: true,
        },
      ],
    };
  },
  created() {
    // 最初のプルダウンのみ初期値を変更
    this.selectedOrderItems.push(this.orderItems.find((item) => item.id === 1));
  },
  computed: {
    defaultSelectItem() {
      return this.orderItems.find((item) => item.default);
    },
  },
  methods: {
    getSelectedItem(pulldownIndex) {
      if (this.selectedOrderItems[pulldownIndex - 1] === undefined) {
        return this.defaultSelectItem;
      }
      return this.selectedOrderItems[pulldownIndex - 1];
    },
    getOrderItems(pulldownIndex) {
      const items = JSON.parse(JSON.stringify(this.orderItems));
      if (pulldownIndex === 1) {
        items[0].display = false;
        return items;
      }
      // 指定pulldownの1つ前までの選択済みの要素を取得
      const selectedItems = this.selectedOrderItems
        .slice(0, pulldownIndex - 1)
        .filter((item) => item.id !== undefined);
      return items.filter((item) => {
        // 選択済みの要素以外を表示
        return !selectedItems.find((i) => i.id === item.id);
      });
    },
    onSelect({ index, before, after }) {
      // 選択されたプルダウンを含む、以下の選択を全てリセット
      this.selectedOrderItems = this.selectedOrderItems.slice(0, index - 1);

      // 選択不可項目の場合、何もせず処理を抜ける
      if (!after.selectable) {
        return;
      }

      // 以前の選択と同じアイテムを選択した場合、選択を解除する
      if (before && before.id === after.id) {
        this.selectedOrderItems = this.selectedOrderItems.filter(
          (i) => i.id !== before.id
        );
        return;
      }

      // 以前の選択と異なるアイテムを選択した場合、以前の選択を削除し、新しい項目を選択状態にする
      if (before && before.id !== after.id) {
        this.selectedOrderItems = this.selectedOrderItems.filter(
          (i) => i.id !== before.id
        );
        this.selectedOrderItems.push(after);
      }
    },
  },
};
</script>

<style scoped>
</style>