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
          emittable: false,
          default: true,
        },
        { id: 1, name: "科目", display: true, emittable: true, default: false },
        {
          id: 2,
          name: "サービス",
          display: true,
          emittable: true,
          default: false,
        },
        {
          id: 3,
          name: "ジャンル",
          display: true,
          emittable: true,
          default: false,
        },
        {
          id: 4,
          name: "コース",
          display: true,
          emittable: true,
          default: false,
        },
        { id: 5, name: "地域", display: true, emittable: true, default: false },
        {
          id: 6,
          name: "都道府県",
          display: true,
          emittable: true,
          default: false,
        },
        {
          id: 7,
          name: "スペース",
          display: true,
          emittable: true,
          default: false,
        },
      ],
    };
  },
  created() {
    this.selectedOrderItems.push(this.orderItems.find((item) => item.id === 1));
  },
  methods: {
    getSelectedItem(pulldownIndex) {
      // if (pulldownIndex === 1) {
      //   return this.selectedOrderItems[pulldownIndex - 1];
      // }
      if (this.selectedOrderItems[pulldownIndex - 1] === undefined) {
        // if (pulldownIndex === 1) {
        //   return undefined;
        // }
        return this.orderItems.find((item) => item.default);
      }
      return this.selectedOrderItems[pulldownIndex - 1];
    },
    getOrderItems(pulldownIndex) {
      // debugger
      // const items = this.orderItems.map((item) => item);
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
      console.log("before", before);
      console.log("after", after);
      if (before && before.id === after.id) {
        console.log("HITTTTTTTTTTTTT");
        this.selectedOrderItems = this.selectedOrderItems.filter(
          (i) => i.id !== before.id
        );
        return;
      }

      if (before) {
        this.selectedOrderItems = this.selectedOrderItems.filter(
          (i) => i.id !== before.id
        );
      }
      if (after) {
        this.selectedOrderItems.push(after);
      }
      console.log("item?", before, after);
      this.selectedOrderItems = this.selectedOrderItems.slice(0, index);
    },
  },
};
</script>

<style scoped>
</style>