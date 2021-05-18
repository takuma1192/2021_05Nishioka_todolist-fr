<template>
  <div id="app">
    <div class="todolist">
      <div class="todocard">
        <p class="title">Todo List</p>
        <div class="todo">
          <div class="flex-todo">
            <input type="text" v-model="inputAdd" class="input-todo"/>
            <button @click="addItem" class="input-btn">追加</button>
          </div>
          <div class="flex-add">
            <div v-for="(todo, index) in todos" :key="todo" class="todo-listcontents">
              <label class="label" v-if="!todo.isEditMode">
                  <input type="checkbox" @click="saveCheckState(index);" v-model="todo.isCheck" class="checkbox"/>
                  <span class="todo-list_item">{{todo.item}}</span>
              </label>
                <input type="text" v-if="todo.isEditMode" class="input-edit" :id="'editTextArea'+index"/>
                <div class="btn-left">
                  <button v-if="!todo.isEditMode" @click="editItem(index);" class="btn">更新</button>
                </div>
                <div class="btn-right">
                  <button @click="deleteItem(index);" class="btn delete" v-if="!todo.isEditMode">削除</button>
                </div>
            </div>  
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      inputAdd: "",
      todos: [],
    };
  },
  methods: {
    async addItem() {
      const resData = await axios.get("http://127.0.0.1:8000/api/todo/");
      this.todos = resData.data.data;
    },
    async saveCheckState() {
      const sendData = {
        todo: this.inputAdd,
      };
      await axios.post("http://127.0.0.1:8000/api/todo/", sendData);
      await this.gettodo();
    },
    async editItem(editTextArea) {
      const sendData = {
        todo:editTextArea,
      };
      await axios.put("http://127.0.0.1:8000/api/todo/" , sendData);
      await this.gettodo();
    },
    async deleteItem() {
      await axios.delete("http://127.0.0.1:8000/api/todo/" );
      await this.gettodo();
    },
  },
  created() {
    this.getodo();
  },
};
</script>

<style>
.todolist {
  background-color: #2D187C;
  height: 100vh;
  width: 100vw;
  position: relative;
}

.todocard {
  background-color: #fff;
  width: 50%;
  padding: 30px;
  position: absolute;
  top: 50%;
  left: 25%;
  border-radius: 8px;
}

.title {
  font-size: 25px;
  font-weight: bold;
  margin-bottom: 15px;
}

.flex-todo {
  margin-bottom: 15px;
  display: flex;
  justify-content: space-between;
}

.input-todo {
  width: 70%;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid #ccc;
  font-size: 15px;
}

.input-btn {
  border: 1px solid #EAA2F7;
  border-radius: 5px;
  font-size: 13px;
  font-weight: bold;
  color: #EAA2F7;
  background-color: #fff;
  padding: 5px 15px;
  margin-left: 5px;
  cursor: pointer;
  transition: 0.5s;
}

.flex-add {
  display: flex;
  margin-bottom: 10px;
  justify-content: space-between;
}

.input-update {
  width: 30%;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid #ccc;
  font-size: 15px;
}

.button-update {
  font-size: 13px;
  font-weight: bold;
  color: #FBAB8E;
  border: 1px solid #FBAB8E;
  background-color: #fff;
  border-radius: 5px;
  padding: 5px 15px;
  cursor: pointer;
  transition: 0.5s;
  margin-left: 5px;
}

.button-delete {
font-size: 13px;
  font-weight: bold;
  color: #7EFADF;
  border: 1px solid #7EFADF;
  background-color: #fff;
  border-radius: 5px;
  padding: 5px 15px;
  cursor: pointer;
  transition: 0.5s;
  margin-left: 5px;
}
</style>
