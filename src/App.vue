<template>
  <div class="main" :style="background">
    <div class="background"><img src="@/assets/img/bg-desktop-dark.jpg" v-if="switchDay == false" alt="background-image"><img src="@/assets/img/bg-desktop-light.jpg"  v-else-if="switchDay" alt="background-image"></div>
    <div class="container">
      <div class="header"><h1 class="title">Todo</h1> <span @click="switchMode()" class="switch-theme"><img v-if="switchDay == false" src="@/assets/svg/icon-sun.svg" alt="icon-night and sun"><img v-else-if="switchDay" src="@/assets/svg/icon-moon.svg" alt="icon-night and sun"></span></div>
      <div class="main-todo">
         
          <div class="add-task-cont"  :style="background">
            
            <input type="text" name="add-task" id="add-task" placeholder="Ajouter une tache" v-model="txtToAdd">
            <input type="checkbox" name="button-add" id="button-add" v-model="addBtn" @change="addTask()">
          </div>
          <ul class="tasks" v-for="(task, index) in tasks" :key='index'>
            <li class="task" :style="background">

               <span class="task-cont">{{task.content}}</span>
               <div class="delete" @click="deleteTask(index)"><img src="@/assets/svg/icon-cross.svg" alt="svg cross"></div>
            </li>
            
          </ul>

        
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data(){
    return {
      txtToAdd:null,
      addBtn:false,
      tasks:[{content: "gregegr"}],
      switchDay:false,
      background:"background:#25273D;color:white;",
      modeActive:null
    }
  },mounted() {
    if (localStorage.getItem('tasks')) {
      try {
          this.tasks = JSON.parse(localStorage.getItem('tasks'));
        } catch(e) {
          localStorage.removeItem('tasks');
        }
    }
  }
  ,methods:{
    addTask(){
        if(this.addBtn) {
          this.tasks.push({content: this.txtToAdd})
          this.txtToAdd = null
          this.addBtn = false
          this.saveTasks();
        }

       
    },saveTasks(){
      const parsed = JSON.stringify(this.tasks);
      localStorage.setItem('tasks', parsed);
    }
    ,deleteTask(task){

      this.tasks.splice(task,1)
      this.saveTasks();

    },switchMode(){
      if(this.switchDay) {
        this.switchDay = false
        this.background ="background:#25273D;color:white;"

      } else {
        this.switchDay = true
        this.background ="background:white;color:#25273D;"
      }
    }
  },computed:{

  }
}
</script>

<style>
:root {
  --title:25px;
  --txt-task:16px;

}
body {
  margin: 0;
  text-decoration: none;

}
*, *::before,*::after{
  box-sizing: border-box;
  list-style: none;

}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}

.main {
  position: relative;
  background: #181824;
  width: 100%;
  height: 100%;
}
.background {
width: 100%;
height: 300px;
}
.container {
  padding: 39px 24px;
  position: relative;
  transform: translatey(-200px);
   margin: 0 auto;
  min-width: 326px;
  max-width: 541px;
  max-height: 528px;
  min-height: 495px;
}
.background  img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.header h1 {
  font-size: var(--title);
  text-transform: uppercase;
  letter-spacing: 3px;
  font-weight: 800;
  color: white;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20pxv;
}
 .add-task-cont {

  box-shadow: 1px 1px 7px solid;
  display: flex;
  gap: 20px;
  align-items: center;
  padding: 13px 19px;
  border-radius: 5px;
}
input[type='text']{
  width: 100%;
  background: rgba(0, 0, 0, 0);
  border: none;
  height: 48px;
color: inherit;
}

input[type=checkbox] {
  -moz-appearance: none;
    -ms-appearance: none;
    border-radius: 50%;
    height: 18px;
    width: 18px;

}
.tasks {
  background: #25273D;
  width: 100%;
  padding: 0;
  border-radius: 5px;
}
.task {
  padding: 13px 19px;
  display: flex;
  justify-content: space-between;
  gap: 1px;
  align-items: center;
  color: inherit;
  border-bottom: 1px solid grey;
  height: 55px;
  box-shadow: 1px 1px 7px rgba(0, 0, 0, .3);
  border-radius: 5px;

}
.task-cont {
  font-size: var(--txt-task);

}


</style>
