<template>
<div v-bind:class="theme" id="theme">
  <div class="jumbotron bg-primary">
    <div class="container">
        <!-- for the todo logo and changing of theme  -->
      <div class="logoTheme">
        <h1 class="h1">TODO</h1>
        <img @click ="changeTheme" class="themeImage" alt="change to dark or light theme">
      </div>
          <!-- for adding new todo -->
    <div class="newTodo">
      <input type="text" required placeholder="Create a new todo..." @keydown.enter="addTodo" v-model="todoItem">
    </div>
      <!-- list of new todos -->
    <div class="todoList">
      <div class="subTodoList">
        <div class="individualTodoList" v-show="changeComplete()" v-for="(todo, index) in todoList" v-bind:key="todo">
        <span>
          <label v-if="!todo.editable" @dblclick="isEditable(todo)" for="todocheckbox" class="todoText" v-bind:class="{completed: todo.isCompleted}">{{todo.name}}</label>
          <input v-else type="text" v-model="todo.name" id="todoTextEdit" @keydown.enter="editCompleted(todo)" @blur="editCompleted(todo)">
        </span>
        <span class="checkbox" @click="todoCompleted(todo)" v-bind:class="{completed: todo.isCompleted}">
            <img src="./assets/images/icon-check.svg" alt="">
        </span>
        <!-- delete todo -->
        <span class="deleteTodo" @click="deleteTodo(index)">
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18"><path fill="#494C6B" fill-rule="evenodd" d="M16.97 0l.708.707L9.546 8.84l8.132 8.132-.707.707-8.132-8.132-8.132 8.132L0 16.97l8.132-8.132L0 .707.707 0 8.84 8.132 16.971 0z"/></svg>
        </span>
      </div>
      </div>
      <!-- todo sublinks -->
    <div class="todoFootLinks">
      <p class="numOfItems">3 items left</p>
      <div class="links">
        <span @click="showAll" v-bind:class="{active: showAll}">all</span>
        <span @click="showActive">active</span>
        <span @click="showCompleted">completed</span>
      </div>
      <p class="clearComplete">clear complete</p>
    </div>
    </div>

       <div class="mobileLinks">
        <span>all</span>
        <span>active</span>
        <span>completed</span>
      </div>

      <p class="dragInfo">Drag and drop to reorder list</p>
    </div>
  </div>
</div>
  {{todoList}}
</template>

<script>

export default {
  data(){
    return{
      theme: 'lightTheme',
      completed: true,
      todoItem: '',
      todoList: [],
      id: 0,
      todoDummyObject: {}
    }
  },
  methods:{
    todoObject(obj, id){
      this.name = obj,
      this.editable = false,
      this.isCompleted = false,
      this.id = id
    },
    changeTheme(){
      if(this.theme === 'lightTheme'){
        this.theme = 'darkTheme'
      } else{
        this.theme = 'lightTheme'
      }
    },
    addTodo(){
      this.todoDummyObject = new this.todoObject(this.todoItem, this.id)
      this.todoList.push(this.todoDummyObject)
      this.id++
      this.todoItem = ''
    },
    todoCompleted(todo){
      if(!todo.isCompleted){
        todo.isCompleted = true
      } else{
        todo.isCompleted = false
      }
    },
    isEditable(todo){
      todo.editable = true;
    },
    editCompleted(todo){
      todo.editable = false;
    },
    changeComplete(){
      return this.completed
    },
    showAll(){
      if(this.todoDummyObject.isCompleted || !this.todoDummyObject.isCompleted){
        this.completed = true
        console.log(this.todoDummyObject.isCompleted, this.completed)
      }else{
        this.completed = false
        console.log('failed');
      }
    },
    showActive(){
      if(!this.todoDummyObject.isCompleted){
        this.completed = true
        console.log(this.todoDummyObject.isCompleted, this.completed)
      }else{
        this.completed = false
        console.log('failed');
      }
    },
    showCompleted(){
      if(this.todoDummyObject.isCompleted){
        this.completed = true
        console.log(this.todoDummyObject.isCompleted, this.completed)
      }else{
        this.completed = false
        console.log(this.todoDummyObject.isCompleted, this.completed)
      }
    },

    deleteTodo(index){
      this.todoList.splice(index, 1);
    },

  }
  
}
</script>

<style>
:root{
  --checkboxBackgroundColor: linear-gradient(135deg, hsl(192, 100%, 67%),hsl(280, 87%, 65%));
  --textHoverColor: hsl(220, 98%, 61%);
  --bodyBackgroundColor: hsl(236, 33%, 92%);
  --todoBackgroundColor: hsl(0, 0%, 98%);
  --lineThroughTextColor: hsl(233, 11%, 84%);
  --textColor: hsl(236, 9%, 61%);
  --todoTextColor: hsl(235, 19%, 35%);
  --jumbotronBackground: url('./assets/images/bg-desktop-light.jpg');
  --mobilejumbotronBackground: url('./assets/images/bg-mobile-light.jpg');
  --themeImage: url('./assets/images/icon-moon.svg');
}

.darkTheme{
  --bodyBackgroundColor: hsl(235, 21%, 11%);
  --todoBackgroundColor: hsl(235, 24%, 19%);
   --todoTextColor: hsl(234, 39%, 85%);
   --jumbotronBackground: url('./assets/images/bg-desktop-dark.jpg');
   --mobilejumbotronBackground: url('./assets/images/bg-mobile-dark.jpg');
   --themeImage: url('./assets/images/icon-sun.svg');
}

#theme{
  height: 100vh;
  background-color: var(--bodyBackgroundColor);
  transition: background-color 500ms;
}

*{
  font-family: 'Josefin Sans', sans-serif !important;
  transition: all 500ms;
}

.jumbotron{
  height: 18em;
  background-image: var(--jumbotronBackground) !important;
  background-size: cover;
  transition: background-image 500ms;
}

.container{
  width: 500px;
  padding-top: 5em;
  padding-bottom: 5em; 
}

.logoTheme{
  display: flex;
  justify-content: space-between;

}

.h1{
  font-size: 2.5rem;
  font-weight: 900;
  letter-spacing: 1rem;
  color: #fff;
}

.themeImage{
  content: var(--themeImage);
  transition: content 500ms;
  width: 7%;
  height: 10%;
  cursor: pointer;
}

.newTodo{
  margin: 3em 0 1.2em 0;
}

.newTodo input{
  width: 100%;
  padding: .8em 1em;
  border-radius: 7px;
  border: none;
  font-weight: 600;
  background-color: var(--todoBackgroundColor);
  color: var(--todoTextColor);
  opacity: .8;

}

 .newTodo input:focus{
  outline: none;
  transition: box-shadow 500ms;
  box-shadow: 0 0 15px 1px rgba(0, 0, 0, .3) 
 }

.newTodo input::placeholder{
  font-weight: 600;
  opacity: .6;
}

.todoList{
  border-radius: 7px;
  background-color: var(--todoBackgroundColor);
  box-shadow: 0 0 15px 1px rgba(0, 0, 0, .3) ;
}

.individualTodoList{
  border-bottom: 1px solid var(--textColor);
  display: flex;
  justify-content: space-between;
  padding: 0.7em 1em;
  position: relative;
  cursor: grabbing;
}

.individualTodoList:hover .deleteTodo{
  opacity: 1;
}

.individualTodoList span:nth-child(1){
  display: flex;
  justify-content: flex-start;
  align-items: center;
  width: 100%;
  margin-left: 40px;
}

.individualTodoList span:nth-child(1) input{
  margin-left: 5px;
  visibility: hidden;
}

.todoText{
  display: inline-block;
  font-weight: 500;
  font-size: 20px;
  cursor: text;
  color: var(--todoTextColor);
  overflow-y: scroll;
  width: 90%;
}

#todoTextEdit{
  display: inline-block;
  margin-left: 40px;
  visibility: visible;
}

.todoText.completed{
  color: var(--lineThroughTextColor);
  text-decoration: line-through;
  pointer-events: none;
}

.checkbox{
  width: 22px;
  height: 22px;
  border-radius: 50%;
  text-align: center;
  border: 1px solid var(--lineThroughTextColor);
  position: absolute;
  left: 18px;
  top: 14px;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  background: transparent;
  cursor: pointer;
}

.checkbox:hover{
  border-radius:50%;
  border:1px solid transparent; 
  background: var(--checkboxBackgroundColor) border-box;
  -webkit-mask:
     linear-gradient(#fff 0 0) padding-box, 
     linear-gradient(#fff 0 0);
  mask:
     linear-gradient(#fff 0 0) padding-box, 
     linear-gradient(#fff 0 0);
  -webkit-mask-composite: destination-out; 
  mask-composite: exclude; 
}

.checkbox.completed{
  background: var(--checkboxBackgroundColor);
  border: none;
}

.checkbox.completed:hover{
  background: var(--checkboxBackgroundColor);
  border: none;
  -webkit-mask-composite: destination-over;
  mask-composite: destination-over;
}

.checkbox img{
  width: 50%;
  opacity: 0;
}

.completed img{
  opacity: 1;
}

.deleteTodo{
  opacity: 0;
  transition: opacity 200ms;
  cursor: pointer;
}

.todoFootLinks{
  display: flex;
  justify-content: space-between;
  padding: 10px;
}

.numOfItems, .clearComplete{
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0;
  font-size: .9rem;
  font-weight: 500;
  color: var(--textColor);
}

.clearComplete{
  cursor: pointer;
}

.clearComplete:hover{
  color: var(--todoTextColor)
}

.mobileLinks{
  display: none;
}

.links{
  display: flex;
  justify-content: space-between;
  width: 35%;
  text-transform: capitalize;
  font-size: .9rem;
  font-weight: 500;
  color: var(--textColor);
}

.links span{
  cursor: pointer;
  transition: color 200ms;
}

.links span:hover{
  color: var(--todoTextColor);
}

.active{
  color: var(--textHoverColor);
}

.dragInfo{
  margin: 2em;
  text-align: center;
  opacity: .7;
  color: var(--textColor);
}

@media screen and (max-width: 550px){
  .container{
    width: 100%;
  }

  .jumbotron{
    background-image: var(--mobilejumbotronBackground) !important;
  }

  .logoTheme{
    width: 90%;
    margin: auto;
  }

  .newTodo{
    width: 90%;
    margin: 4em auto 1.2em auto;
  }

  .todoList{
    width: 90%;
    margin: auto;
  }

  .h1{
    font-size: calc(1.375rem + 1.5vw);
  }

  .links{
    display: none;
  }

  .mobileLinks{
    width: 90%;
    text-transform: capitalize;
    font-size: .9rem;
    font-weight: 500;
    color: var(--textColor);
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    background: var(--todoBackgroundColor);
    margin: 15px auto;
    border-radius: 7px;
    padding: 0.8em 4em;
  }

  .mobileLinks span{
    transition: color 200ms;
  }

  .mobileLinks span:hover{
    color: var(--todoTextColor);
  }

}

</style>

