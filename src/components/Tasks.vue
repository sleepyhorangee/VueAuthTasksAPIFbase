<template>
  <div class="hello">
    <div class="holder">
       <!-- <p v-if = "tasks.length = 1">You have more than 1 task</p>
        <p v-else>You have less than or = 1 task</p> -->
      {{ name }}
      {{ btnState ? 'The button is enabled' : 'Disabled'}}
      <button v-on:click="changeName" v-bind:enabled="btnState">Change Name</button>        
 
         <!-- Prevent the form from reloading -->
      <form @submit.prevent="addTask"> 
        <h5>Please Enter a New Tasks:</h5>
          <input type="checkbox" id="check" v-model="checked">
          <label for="check">Important</label>
        <!-- <input type="text" placeholder="Enter a task (2-way data-binding below).." v-model="task" v-validate="'required|alpha|min:3'" data-vv-delay="2000" name="task"> -->
        <input type="text" placeholder="Enter a task (2-way data-binding below).." v-model="task" v-validate="'min:3'" data-vv-delay="2000" name="task">
          {{ task }}
        <transition name="alert-in" enter-active-class="animated flipInX" leave-active-class="animated flipOutX">
          <p class="alert" v-if="errors.has('task')">{{ errors.first('task') }}</p>
        </transition>  
      </form>

      <ul>
        <div>
          <h5 v-if="tasks.length == 0"  >You're done with your tasks!!!</h5>
          <h5 v-else>Your {{tasks.length}} Tasks:</h5>
          <transition-group name="list" enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
          <li v-for="(data, index) in tasks" :key='index'>
            {{ index +1 }}. {{ data.task }}
            <i class="fa fa-minus-circle" v-on:click="remove(index)"></i>
          </li>
          </transition-group>
        </div>
      </ul>
        <div v-bind:class="{ alert: showAlert, 'another-class': showClass }"></div>
        <div v-bind:class="alertObject"></div>
        <div v-bind:style="{ backgroundColor: bgColor, width: bgWidth, height:bgHeight }"></div>
    </div>
  </div>
</template>
  
<script>
export default {
  name: 'Tasks',
  data() {
    return {
      checked: true,
      name: 'Steveo',
      btnState: true,
       showAlert: true,
       showClass: true,
        // ...or use
      alertObject: {
        alert: true,
       anotherClass: true
     },
      task: '',
      tasks: [
        { "task": "Vue.js"},
        { "task": "Frontend Developer"},
        { "task": "bOBO"},
        { "task": "popo"},
        { "task": "bidieauyhe"}
      ],
       bgColor: 'red',
       bgWidth:'100%',
       bgHeight:'30px',
    }
  },
  methods: {
    addTask() {
      // this.tasks.push({task: this.task})
      console.log('Checkbox value: ' +this.checked);
      this.$validator.validateAll().then((result) => {
        if (result) {
          this.tasks.push({task: this.task})
          this.task = '';
        } else {
          console.log('Not valid');
        }
      })
    },
    remove(id) {
      this.tasks.splice(id,1);
    },
    changeName() {
      // name = "George"
      // btnState: !btnState
      console.log('changeName')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src='./Tasks.css' scoped>

</style>
