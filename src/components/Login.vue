<template>
  <div class="hello">
    <form>
      <div class="form-group">
        <label for="exampleInputEmail1">Email address</label>
        <input
          type="email"
          class="form-control"
          id="exampleInputEmail1"
          aria-describedby="emailHelp"
          placeholder="Enter email"
          v-model="email"
        />
        <small id="emailHelp" class="form-text text-muted"
          >We'll never share your email with anyone else.</small
        >
      </div>
      <div class="form-group">
        <label for="exampleInputPassword1">Password</label>
        <input
          type="password"
          class="form-control"
          id="exampleInputPassword1"
          placeholder="Password"
          v-model="pass"
        />
      </div>
      <button type="submit" class="btn btn-primary" @click.prevent="login">Submit</button>
    </form>
  </div>
</template>

<script>
import firebase from 'firebase'
import {mapActions} from 'vuex'
export default {
    data(){
        return{
            email: '',
            pass: '',
            alert: {
             text:"",
             state: false,
             type:""
             },
        }
    },
  methods:{
    ...mapActions(['updateUser']),
      login(){
          if(this.email != "" && this.pass != ""){
              firebase.auth().signInWithEmailAndPassword(this.email, this.pass)
              .then(() => {
                this.alert.state = false;
                this.$router.push("home");
                this.updateUser(firebase.auth().currentUser)

        })
        .catch(()=> {
          this.alert.state = true 
          this.alert.text = "Usuario o password con error, intente nuevamente"
        })
      } else{ 
        this.alert.state = true 
        this.alert.text = "Debe ingresar usuario y password"
      }
      }
  }
  
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
