<template>
  <div class="container">
    <br/>
    <h1>CATATAN TUGAS KULIAH</h1>
    <h3>TO DO LIST</h3>
    <br />
    <div class="tambah-data">
      <div class="form-tambah">
      <h2>Masukkan Data Tugas Anda</h2>
      <form @submit.prevent="addData">
        <input type="text" placeholder="Nama Tugas" v-model="taskData"/>
        <br/>
        <input type="text" placeholder="Mata Kuliah" v-model="coursesData"/>
        <br/>
        <input type="text" placeholder="Deadline" v-model="deadlineData"/>
        <br/>
        <input type="text" placeholder="Deskripsi" v-model="descriptionData"/>
        <br />
        <button class="btn-add" @click="addData">SAVE</button>
      </form>
      </div>
    </div>
    <br/>
    <br/>
    <br/>
    <div class="todo-list">
      <div class="item" v-for="(item, index) in todoList" :key="index">
        <div class="card-todo">
          <div class="col-8">
          <h2 style="text-align: left">{{item.task}}</h2>
          <p style="text-align: left">{{item.courses}} | Due Date : {{item.deadline}}</p>
          <p style="text-align: left">{{item.description}}</p>
          </div>
          <div class="col-4">
            <button class="btn-remove" @click="removeData(item)" >DELETE</button>
          </div>
        </div>
      </div>
      <br/>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    taskData: '',
    coursesData: '',
    deadlineData: '',
    descriptionData: '',
    todoList: [],
  }),
  created() {
    this.loadlocalStorage();
  },
  watch: {
    todoList() {
      localStorage.setItem('todoList', JSON.stringify(this.todoList));
    },
  },
  methods: {
    addData() {
      if (this.taskData !== '') {
        const date = new Date();
        const dataTask = {
          id: date.getTime(),
          task: this.taskData,
          courses: this.coursesData,
          deadline: this.deadlineData,
          description: this.descriptionData,
          complete: false,
          show: false,
        };
        this.todoList.push(dataTask);
      }
      this.taskData = '';
      this.coursesData = '';
      this.deadlineData = '';
      this.descriptionData = '';
    },
    removeData(item) {
      const index = this.todoList.findIndex((Element) => Element.id === item.id);
      this.todoList.splice(index, 1);
    },
    loadlocalStorage() {
      const ls = JSON.parse(localStorage.getItem('todoList'));
      if (ls == null) {
        return;
      }
      this.todoList = ls;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: block;
  text-align: center;
  margin: 10px;
  border: 3px double black;
  background-color: teal;
  color: rgb(1, 1, 48);
}
.tambah-data {
  border: 4px double rgb(219, 216, 216);
  width: 740px;
  display: inline-block;
  background-color: white;
}
.form-tambah {
  box-shadow: 0 15px 15px 0 rgba(0,0,0,0.2);
  padding-top: 10px;
}
.todo-list {
  display: inline-block;
  width: 750px;
}
input[type=text] {
  height: 30px;
  margin: 5px;
  padding: 5px;
  cursor: pointer;
  transition: 0.3s;
  width: 450px;
}
input[type=text]:focus {
  border: 2px solid darkblue;
  border-radius: 0px;
}
.btn-add{
    box-shadow: 5px 5px 5px 5px turquoise;
    background-color: transparent;
    color: darkblue;
    border: 2px solid darkblue;
    border-radius: 0px;
    text-align: center;
    margin: 5px 2px;
    padding: 10px 15px;
    cursor: pointer;
    transition: 0.3s;
    margin: 20px;
    width: 300px;
}
.btn-add:hover{
    background-color: turquoise;
    color: darkblue;
}
.card-todo {
    border: 6px double rgb(219, 216, 216);
    box-shadow: 0 15px 15px 0 rgba(0,0,0,0.2);
    transition: 0.3s;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-auto-rows: minmax(10px, auto);
    margin-bottom: 20px;
    background-color: turquoise;
}
.col-8 {
  padding-left: 10px;
  width: 600px;
}
.card-todo:hover {
    box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}
.btn-remove{
    box-shadow: 5px 5px 5px 5px rgb(252, 32, 32);
    background-color: transparent;
    color: maroon;
    border: 2px solid maroon;
    border-radius: 0px;
    text-align: center;
    margin: 5px 2px;
    padding: 10px 15px;
    cursor: pointer;
    transition: 0.3s;
    margin: 20px;
}
.btn-remove:hover{
    background-color: red;
    color: white;
}
</style>
