<template>
  <div class="welcome container">
    <div class="card">
      <div class="card-content center-align">
        <h2 class="teal-text">Power chat</h2>
        <form @submit.prevent="enterChat">
          <label for="name">Enter your name:</label>
          <input type="text" name="name" placeholder="name" v-model="name" autocomplete="off">
          <p v-if="feedback" class="red-text">{{feedback}}</p>
          <button class="btn teal">Enter chat</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Welcome',
  data () {
    return {
      name: null,
      feedback: ''
    }
  },
  methods: {
    enterChat(){
      if(this.name){
        this.feedback = null
        this.$router.push({name: 'Chat', params: {name: this.name}})
      }else{
        this.feedback = 'You must enter a name'
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.welcome {
  max-width: 400px;
  margin-top: 100px;
}
.welcome h2 {
  font-size: 3em;
}
.welcome button {
  margin: 30px auto;
}
</style>
