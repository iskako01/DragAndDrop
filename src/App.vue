<template>
  <div
    v-for="category in categories"
    :key="category.id"
    @drop="onDrop($event, category.id)"
    class="droppable"
    @dragover.prevent
    @dragenter.prevent
  >
    <h4>{{ category.title }}</h4>
    <div
      v-for="item in items.filter((i) => i.categoryId === category.id)"
      :key="item.id"
      @dragstart="onDragStart($event, item)"
      class="graggable"
      draggable="true"
    >
      <h5>{{ item.title }}</h5>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "App",
  components: {},
  setup() {
    const items = ref([
      {
        id: 0,
        title: "Alisher",
        categoryId: 0,
      },
      {
        id: 1,
        title: "Dog",
        categoryId: 1,
      },
      {
        id: 2,
        title: "cat",
        categoryId: 1,
      },
    ]);
    const categories = ref([
      {
        id: 0,
        title: "People",
      },
      {
        id: 1,
        title: "Animals",
      },
    ]);

    function onDragStart(e, item) {
      e.dataTransfer.dropEffect = "move";
      e.dataTransfer.effectAllowed = "move";
      e.dataTransfer.setData("itemId", item.id.toString());
    }
    function onDrop(e, categoryId) {
      const itemId = parseInt(e.dataTransfer.getData("itemId"));
      items.value = items.value.map((x) => {
        if (x.id == itemId) x.categoryId = categoryId;
        return x;
      });
    }
    return { items, onDragStart, onDrop, categories };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.droppable {
  margin-bottom: 20px;
  padding: 20px;
  border-radius: 20px;
  background-color: #2c3e50;
  color: #fff;
}

.graggable {
  color: #000;
  padding: 5px;
  background-color: #fff;
  border-radius: 20px;
  border: 1px solid #2c3e50;
  margin-bottom: 20px;
}
</style>
