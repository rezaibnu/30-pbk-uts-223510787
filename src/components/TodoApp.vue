<template>
  <div class ="container">
    <h2 class ="text-center mt-5"> LIST KEGIATAN </h2>

    <!-- input -->
    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Masukkan List" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-0">TEKAN</button>
    </div>

    <!-- Task Table -->
    <table class="table table-bordered mt-5">
  <thead>
    <tr>
      <th scope="col">Task</th>
      <th scope="col">Status</th>
      <th scope="col" class="text-center">#</th>
      <th scope="col" class="text-center">#</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task, index) in tasks" :key="index">
      <td>
        <span :class="{'selesai': task.status === 'selesai'}">{{task.name}}</span>
      </td>
      <td style="width: 120px">
        <span @click="changeStatus(index)" class="pointer"
          :class="{'text-danger': task.status === 'melakukan',
          'text-warning': task.status === 'proses melakukan',
          'text-success': task.status === 'selesai'
          }"
        >
          {{ firstCharUpper(task.status) }}
        </span>
      </td>
      <td>
        <div class="text-center" @click="editTask(index)">
          <span class="fa fa-pen"></span>
        </div>
      </td>
      <td>
        <div class="text-center" @click="deleteTask(index)">
          <span class="fa fa-trash"></span>
        </div>
      </td>
    </tr>
  </tbody>
</table>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data(){
    return {
      task: '',
      editedTask: null,
      availableStatuses: ['melakukan', 'proses melaksanakan', 'selesai'],

      tasks: [
        {
          name: 'Masuk kuliah besok pagi',
          status: 'melakukan'
        },
        {
          name: 'Merakit PC',
          status: 'proses melakukan'
        },
        {
          name: 'Senam di pagi hari',
          status: 'selesai'
        }
      ]
    }
  },

  methods: {
    submitTask(){
      if(this.task.length === 0) return;

      if(this.editedTask === null){
        this.tasks.push({
          name: this.task,
          status: 'melakukan'
        });
      }else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = '';
    },

    deleteTask(index){
      this.tasks.splice(index, 1);
    },

    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },

    firstCharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    }

    }
  }

</script>


<style scoped>
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>
