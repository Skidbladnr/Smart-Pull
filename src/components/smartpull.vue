<template>
  <div>
    <div class="container">
      <form @submit.prevent="add">
        <input v-model="todo" />
        <button type="submit">Add Task</button>
      </form>
    </div>
    <div class ="container2">
          TO DO LIST
    </div>
    
    <button @click="saveKeeper()" class="bnt">Save</button> 
    <div v-for="(todo, index) of todos" :key="todo.id">
        {{ todo.todo }}
        
        <button @click="deleteTodo(index)">Remove</button>
    </div>
  </div>
</template>

<script>
import { v4 as uuidv4 } from "uuid";
import db from "@/initfirebase"

    export default {
    name: "App",
    data() {
      return {
        todo: "",
        todos: [],
      };
    },
    mounted(){
        this.gatekeeper()
        
    },
    methods: {
      add() {
        const { todo } = this;
        this.todos.push({
          id: uuidv4(),
          todo,
        });
        this.todo = "";
      },
      gatekeeper(){
        const gate = window.localStorage.getItem("gate");
        console.log(window.localStorage.getItem("gate"));
        if(gate == null){ //if the user has never visited the site before send them to retrieve firestore data
          getFirestore()
        }
        if(gate == 1){ //If the user has visited the site before, retrieve the data from local storage
          getLocal()
        }
      },
      saveKeeper(){
        const savegate = window.localStorage.getItem("savegate");
        if(savegate == null){
          saveFirestore()
        }
        if(savegate == 1){
          saveLocal()
        }
      },
      
      deleteTodo(index) {
        console.log(this.todos[index])
        this.todos.splice(index, 1);
      },
      deleteItem(ids) {
        console.log(this.todos[index])
        this.todos.splice(index, 1);
      },
      saveFirestore(){
          window.localStorage.setItem("savegate", 1)
          this.todos.forEach((task) => {
              db.collection("taskset").doc(task.id).set({
                    ID: task.id,
                    TODO: task.todo,
                    retrieve: true
                    
                })
                .then((docRef) => {
                    console.log("Document written with ID: ", docRef.id);
                })
                .catch((error) => {
                    console.error("Error adding document: ", error);
                });
              
              console.log(task.id + " spacer to todo " + task.todo)
        
          })
          
      },
      getFirestore(){
          window.localStorage.setItem("gate", 1)
          db.collection("taskset").where("retrieve", "==", true)
            .get()
            .then((querySnapshot) => {
                querySnapshot.docs.forEach((doc) => {
                    // doc.data() is never undefined for query doc snapshots
                    console.log(doc.data())
                    renderTodo(doc)
                    this.todos.push({
                      id: doc.id, 
                      todo: doc.data().TODO})

                });
            })
            .catch((error) => {
                console.log("Error getting documents: ", error);
            });
          
      },
      saveLocal(){
          console.log("Cool cool this works!")
          var allIds = "";

          this.todos.forEach((task) => {
              console.log(task.id)
              allIds += task.id + " "
              
              window.localStorage.setItem(task.id, task.todo)
          })
          window.localStorage.setItem("allIds", allIds)
      },
      getLocal(){
          const allIds=window.localStorage.getItem("allIds")
          var Ids=allIds.trim().split(" ")
          console.log(allIds)
          console.log(Ids)
          
          Ids.forEach((id) => {
              const task = window.localStorage.getItem(id)
              
            this.todos.push({
              id: id, 
              todo: task})
          })
          
      },
    

    },
      

    
    };
  function renderTodo(doc){
    let ty = doc.id;
    let tu = doc.data().TODO;
    
    console.log(ty + " " + tu)
  }

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container{
    position:relative; 
    background-color:white;
}
.container2{
    position:relative; 
    
}
.bnt{
    background-color: green;
}
.bnt2{
    background-color: limegreen;
}
</style>
