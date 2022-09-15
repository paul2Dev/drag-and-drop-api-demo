<template>
  <div 
    class="drop-zone"
    @drop="onDropItem($event, 1)"
    @dragenter.prevent
    @dragover.prevent
  >
    <div v-for="fruit in getFruitList(1)" :key="fruit.id" 
      class="drag-item"
      draggable="true"
      @dragstart="startDragItem($event, fruit)"
    >{{ fruit.title }}
    </div>
  </div>

  <div 
    class="drop-zone"
    @drop="onDropItem($event, 2)"
    @dragenter.prevent
    @dragover.prevent
  >
    <div v-for="fruit in getFruitList(2)" :key="fruit.id" 
      class="drag-item"
      draggable="true"
      @dragstart="startDragItem($event, fruit)"
    >{{ fruit.title }}
    </div>
  </div>
</template>

<script>

  import { ref } from 'vue'

  export default {
    setup() {
      const fruits = ref([
        { id:1, title: "Apple", listId: 1 },
        { id:2, title: "Mango", listId: 1 },
        { id:3, title: "Kiwi", listId: 2 },
        { id:4, title: "Banana", listId: 2 }
      ])

      const getFruitList = (listId) => {
        return fruits.value.filter(fruit => fruit.listId === listId)
      }

      const startDragItem = (event, item) => {
        event.dataTransfer.dropEffect = 'move'
        event.dataTransfer.effectAllowed = 'move'
        event.dataTransfer.setData('itemId', item.id)
      }

      const onDropItem = (event, list) => {
        const itemId = event.dataTransfer.getData('itemId')
        const item = fruits.value.find((item) => item.id === parseInt(itemId))
        item.listId = list
      }

      return {
        getFruitList,
        fruits,
        startDragItem,
        onDropItem
      }
    }
  }

</script>


<style scoped>
.drop-zone{
  width: 100%;
  margin: 50px auto;
  background-color: #41B883;
  padding: 10px;
  min-height: 10px;
}

.drag-item {
  background-color: #34495E;
  color: white;
  padding: 10px;
  margin-bottom:10px;
}
</style>
