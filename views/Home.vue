<template>
  <div class="home">
    <h1>Todoリスト</h1>
      <form id="taskid">
        <input type="radio" name="task" value="all" v-model="judge" checked>すべて
        <input type="radio" name="task" value="working" v-model="judge" >作業中
        <input type="radio" name="task" value="done" v-model="judge">完了
      </form>
    <table>
      <thead>
        <div id="colhead">
          <tr>
            <th>ID</th>
            <th>コメント</th>
            <th>状態</th>
            <th></th>
          </tr>
        </div>
      </thead>
      <tbody>
        <div v-if="judge == 'all'">
          <tr v-for="(todo, index) in tasklist" :key="index">
            <td>{{ index }}</td>
            <td>{{ todo.comment }}</td>
            <button v-on:click="statusChange(index)">{{ todo.status }}</button>
            <button v-on:click="deleteTask(index)">削除</button>
          </tr>
        </div>
        <div v-if="judge == 'working'">
          <tr v-for="(todo, index) in tasklist" :key="index">
            <td v-if="todo.status == '作業中'">{{ index }}</td>
            <td v-if="todo.status == '作業中'">{{ todo.comment }}</td>
            <button v-if="todo.status == '作業中'" v-on:click="statusChange(index)">{{ todo.status }}</button>
            <button v-if="todo.status == '作業中'" v-on:click="deleteTask(index)">削除</button>
          </tr>
        </div>
        <div v-if="judge == 'done'">
          <tr v-for="(todo, index) in tasklist" :key="index">
            <td v-if="todo.status == '完了'">{{ index }}</td>
            <td v-if="todo.status == '完了'">{{ todo.comment }}</td>
            <button v-if="todo.status == '完了'" v-on:click="statusChange(index)">{{ todo.status }}</button>
            <button v-if="todo.status == '完了'" v-on:click="deleteTask(index)">削除</button>
          </tr>
        </div>
      </tbody>
    </table>
    <h2>新規タスクの追加</h2>
    <form id="newtask">
      <input type="text" id="newtask" name="newtask" v-model.trim="newtask" v-on:keyup.enter="addTask">
      <button v-on:click="addTask">追加</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {    
    return {
      newtask: '',
      tasklist: [],
      judge: 'all'
    }
  },
  computed: {
    remakeTasklist(){
      if (this.judge === 'working'){
        return this.tasklist.filter(element => element.status === '作業中');
      } else if (this.judge === 'done'){
        return this.tasklist.filter(element => element.status === '完了');
      } else {
        return this.tasklist;
      }
    }
  },
  methods: {
    addTask(){
      if (this.newtask){
        this.tasklist.push({comment: this.newtask, status: '作業中'});
        this.newtask = '';
      }
    },
    deleteTask(index){
      this.tasklist.splice(index, 1);
    },
    statusChange(index){
      if (this.tasklist[index].status === '作業中'){
        this.tasklist[index].status = '完了';
      } else { 
        this.tasklist[index].status = '作業中';
      }
    }
  }
}
</script>
