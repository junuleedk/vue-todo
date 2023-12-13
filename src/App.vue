<script setup>
// vue3 컴포턴트 api이며 reactive :반응 연결 해주는 함수, computed: 함수를 캐슁해주는 함수의 연산을 해주는 것
import { reactive, computed } from 'vue';

// reactive함수: 객체를 반응성으로 만들며 값이 밑에 빈 곳으로 들어오면서 리렌더링이 됨
const data = reactive({
  newItem: '',
  items: [],
});

// computed함수: data.items가 변경될 때만 함수가 실행
const totalItems = computed(() => data.items.length);
const isComplete = computed(() => data.items.filter((item) => item.completed).length);
// 할 일 완료 여부

// 할 일 추가, 이건 함수표현식, 함수선언식은 function () {}, 둘의 차이는 호이스팅
const addItem = () => {
  // console.log('함수실행');
  if (data.newItem !== '') {
    data.items.push({
      id: data.items.length + 1,
      text: data.newItem,
      completed: false,
    });
    data.newItem = '';
  }
};

const deleteItem = (id) => {
  const itemToDelete = data.items.find((item) => item.id === id);

  let index = data.items.indexOf(itemToDelete);
  console.log(index);

  data.items.splice(index, 1);
};
</script>

<template>
  <main class="app">
    <h1>Simple to-do list</h1>
    <div class="todo_count">완료: {{ isComplete }} / 할 일: {{ totalItems }}</div>
    <div class="todo_add">
      <!-- v-model 디렉티브로  폼요소와 데이터를 양방향 연결 -->
      <input
        type="text"
        v-on:keyup.enter="addItem()"
        v-model="data.newItem"
        placeholder="할 일을 입력하세요"
        title="할 일을 입력하세요"
      />
      <button type="button" class="add_btn" v-on:click="addItem()">Add</button>
    </div>
    <ul class="todo_list">
      <!-- 리스트마다 고유 id를 key속성에 단방향 연결 -->
      <li v-for="(item, index) in data.items" v-bind:key="item.id" v-bind:class="{ completed: item.completed }">
        <!-- 변수데이터와 속성 연결시 v-bind사용 -->
        <!-- 라벨 클릭시 for로 연결되어 체크박스가 클릭되며 true, false가 발생하며 v-model로 연결된 속성에 들어감 -->
        <input v-bind:id="`check${item.id}`" v-model="item.completed" type="checkbox" />
        <label v-bind:for="`check${item.id}`">{{ item.text }}</label>
        <button v-on:click="deleteItem(item.id)" class="remove_btn" type="button">Remove</button>
      </li>
    </ul>
  </main>
</template>

<style scoped>
.app {
  padding: 40px 20px;
}
.app h1 {
  font-size: 30px;
  font-weight: 700;
  color: var(--text-color-900);
}
.todo_count {
  margin: 10px 0;
}
/* .todo_add {
  display: flex;
} */
.todo_add input[type='text'] {
  height: 40px;
  border: 1px solid #ddd;
  width: calc(100% - 60px);
  /* flex-grow: 1; */
  padding: 0 10px;
  border-radius: 4px;
  margin-right: 10px;
}
.todo_add .add_btn {
  height: 40px;
  padding: 0 10px;
  background: #333;
  color: var(--text-color-100);
  border: none;
  border-radius: 4px;
}
.todo_list {
  margin-top: 20px;
}
.todo_list li {
  margin-bottom: 10px;
  display: flex;
}
.remove_btn {
  height: 32px;
  border: 1px solid var(--primary-color);
  color: var(--primary-color);
  border-radius: 4px;
  padding: 0 5px;
  background: none;
  margin-left: 20px;
}
.todo_list li.completed label {
  text-decoration: line-through;
  color: #ccc;
}
.todo_list label {
  flex-grow: 1;
  line-height: 32px;
}
</style>
