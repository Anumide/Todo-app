<template>
<div v-bind:class="theme" id="theme">
  <div class="jumbotron bg-primary">
    <div class="container">
        <div class="error-message" :class="{'error-message-display': errorMessage}"> {{ errorMessage }}</div>
        <!-- for the todo logo and changing of theme  -->
      <div class="logoTheme">
        <h1 class="h1">TODO</h1>
        <span class="themeImage" @click ="changeTheme"></span>
      </div>
          <!-- for adding new todo -->
    <div class="newTodo">
      <input type="text" required placeholder="Create a new todo..." @keypress.enter="addTodo" v-model="todoItem">
      <span class="newTodoBtn" @click="addTodo"><span class="iconify" data-icon="fluent:add-12-filled" ></span></span>
    </div>
      <!-- list of new todos -->
    <div class="todoList">
      <div class="subTodoList">
        <draggable v-model="todoList"
          group="tasks" 
          @start="drag=true" 
          @end="drag=false" 
          item-key="todo"
          :disabled="!enabled">
         <template #item="{ element, index }">
          <div class="individualTodoList">
            <span>
              <label v-if="!element.editable" @dblclick="isEditable(element, index)" for="todocheckbox" class="todoText" v-bind:class="{completed: element.isCompleted}">{{element.name}}</label>
              <input v-else type="text" v-model="editedText" id="todoTextEdit" @keypress="editCompleted(element)" @blur="editCompleted(element)" @focusout="editCompleted(element)">
            </span>
            <span class="checkbox" @click="todoCompleted(element, index)" v-bind:class="{completed: element.isCompleted}">
                <img src="./assets/images/icon-check.svg" alt="">
            </span>
            
            <span class="deleteTodo" @click="deleteTodo(element, index)">
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18"><path fill="#494C6B" fill-rule="evenodd" d="M16.97 0l.708.707L9.546 8.84l8.132 8.132-.707.707-8.132-8.132-8.132 8.132L0 16.97l8.132-8.132L0 .707.707 0 8.84 8.132 16.971 0z"/></svg>
            </span>
          </div>
         </template>
        </draggable>
      </div>
      <!-- todo sublinks -->
    <div class="todoFootLinks">
      <p class="numOfItems">{{todoNum}} item(s) left</p>
      <div class="links">
        <span @click="showAll" class="active" ref="showalldesktop">all</span>
        <span @click="showActive" ref="showactivedesktop">active</span>
        <span @click="showCompleted" ref="showcompleteddesktop">completed</span>
      </div>
      <p class="clearComplete" @click="clearComplete">clear complete</p>
    </div>
    </div>

       <div class="mobileLinks">
        <span @click="showAll" class="active" ref="showall">all</span>
        <span @click="showActive" ref="showactive">active</span>
        <span @click="showCompleted" ref="showcompleted">completed</span>
      </div>

      <p class="dragInfo">Drag and drop to reorder list</p>
    </div>
  </div>
</div>
</template>

<script>
import draggable from 'vuedraggable';

export default {
  components: { 
    draggable,
  },
  data(){
    return{
      
      theme: 'lightTheme',
      todoItem: '',
      todoList: [],
      finalTodoList: [],
      numOfTodo: [],
      todoNum: '0',
      todoDummyObject: {},
      errorMessage: '',
      editedText: '',
      todos: [],
      enabled: true,
    }
  },
  created(){
    this.theme = localStorage.theme
    if(localStorage.todoList){
      this.todoList = JSON.parse(localStorage.todoList);
      this.finalTodoList = JSON.parse(localStorage.todoList);
      this.numOfTodo = this.todoList.length;
      this.todoNum = this.numOfTodo;
        this.countingTodo();
    }

    if(localStorage.todos){
      this.todos = JSON.parse(localStorage.todos);
    }


  },
  beforeUpdate(){
    if(this.$refs.showalldesktop.classList.contains('active') || this.$refs.showall.classList.contains('active')){
      this.finalTodoList = this.todoList
      localStorage.todoList = JSON.stringify(this.finalTodoList);
      this.countingTodo();
    }
  },
  updated(){
    localStorage.todos = JSON.stringify(this.todos)
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
        localStorage.theme = this.theme;
      } else{
        this.theme = 'lightTheme'
        localStorage.theme = this.theme;
      }
    },
    addTodo(){      
      if(!this.finalTodoList.length == 0){
        if(!this.todoItem == ''){
              if(this.$refs.showcompleteddesktop.classList.contains('active') || this.$refs.showcompleted.classList.contains('active')){
                this.todoDummyObject = new this.todoObject(this.todoItem, this.id)
                let todos = this.finalTodoList.map(todo => todo.name);
                todos = todos.map(todo => todo.toLowerCase())
                if(!todos.includes(this.todoDummyObject.name.toLowerCase())){
                  this.finalTodoList.push(this.todoDummyObject)
                  localStorage.todoList = JSON.stringify(this.finalTodoList)
                  this.finalTodoList = JSON.parse(localStorage.todoList)
                  this.todoItem = ''
                  this.errorMessage ='task added successfully!'
                  setTimeout(() => {
                      this.errorMessage = ''
                    }, 2000);
                  this.countingTodo()
                  return
                }
                    this.errorMessage = 'task already exist!'
                    this.todoItem = ''
                    setTimeout(() => {
                      this.errorMessage = ''
                    }, 2000);
                    return
              }else if(this.$refs.showactivedesktop.classList.contains('active') || this.$refs.showactive.classList.contains('active')){
                this.todoDummyObject = new this.todoObject(this.todoItem, this.id)
                let todos = this.finalTodoList.map(todo => todo.name);
                 todos = todos.map(todo => todo.toLowerCase())
                if(!todos.includes(this.todoDummyObject.name.toLowerCase())){
                    this.finalTodoList.push(this.todoDummyObject)
                    localStorage.todoList = JSON.stringify(this.finalTodoList)
                    this.finalTodoList = JSON.parse(localStorage.todoList)
                    this.todoList = this.finalTodoList
                    this.todoItem = ''
                    this.errorMessage ='task added successfully!'
                  setTimeout(() => {
                      this.errorMessage = ''
                    }, 2000);
                    this.showActive()
                    this.countingTodo()
                    return
                }
                    this.errorMessage = 'task already exist!'
                    this.todoItem = ''
                    setTimeout(() => {
                      this.errorMessage = ''
                    }, 2000);
                    return
                
              }else{
                this.todoDummyObject = new this.todoObject(this.todoItem, this.id)
                let todos = this.finalTodoList.map(todo => todo.name);
                 todos = todos.map(todo => todo.toLowerCase())
                if(!todos.includes(this.todoDummyObject.name.toLowerCase())){
                    this.finalTodoList.push(this.todoDummyObject)
                    localStorage.todoList = JSON.stringify(this.finalTodoList)
                    this.finalTodoList = JSON.parse(localStorage.todoList)
                    this.todoList = this.finalTodoList
                    this.todoItem = ''
                    this.errorMessage ='task added successfully!'
                  setTimeout(() => {
                      this.errorMessage = ''
                    }, 2000);
                    this.countingTodo()
                    return
                }
                    this.errorMessage = 'task already exist!'
                    this.todoItem = ''
                    setTimeout(() => {
                      this.errorMessage = ''
                    }, 2000);
                    return
              }        
        }
          this.errorMessage = 'enter an input!'
          setTimeout(() => {
            this.errorMessage = ''
          }, 2000);
      }else{

      if(!this.todoItem == ''){
            if(this.$refs.showcompleteddesktop.classList.contains('active') ||  this.$refs.showcompleted.classList.contains('active')){
              this.todoDummyObject = new this.todoObject(this.todoItem, this.id)
              this.finalTodoList.push(this.todoDummyObject)
              localStorage.todoList = JSON.stringify(this.finalTodoList)
              this.finalTodoList = JSON.parse(localStorage.todoList)
              this.errorMessage ='task added successfully!'
                  setTimeout(() => {
                      this.errorMessage = ''
                    }, 2000);
              this.countingTodo()
              return
          }else{
            this.todoDummyObject = new this.todoObject(this.todoItem, this.id)
            this.finalTodoList.push(this.todoDummyObject)
            localStorage.todoList = JSON.stringify(this.finalTodoList)
            this.finalTodoList = JSON.parse(localStorage.todoList)
            this.todoList = this.finalTodoList
            this.errorMessage ='task added successfully!'
                  setTimeout(() => {
                      this.errorMessage = ''
                    }, 2000);
          }
          this.todoItem = ''
          this.countingTodo() 
          return
        }
         this.errorMessage = 'enter an input!'
          setTimeout(() => {
            this.errorMessage = ''
          }, 2000);
      }
    },
    todoCompleted(todo, index){
      if(this.$refs.showalldesktop.classList.contains('active') || this.$refs.showall.classList.contains('active')){
        todo.isCompleted = !todo.isCompleted
        this.finalTodoList[index] = todo
        localStorage.todoList = JSON.stringify(this.finalTodoList)
        this.finalTodoList = JSON.parse(localStorage.todoList)
        this.countingTodo()
      }
      
      if(this.$refs.showactivedesktop.classList.contains('active') || this.$refs.showactive.classList.contains('active')){
         for(let i = 0; i < this.finalTodoList.length; i++){
          if(this.finalTodoList[i].name == todo.name){
            todo.isCompleted = true
            this.finalTodoList[i].isCompleted = true
            localStorage.todoList = JSON.stringify(this.finalTodoList)
            this.finalTodoList = JSON.parse(localStorage.todoList)
            this.showActive()
            this.countingTodo()
            return
          }
        }

      }
      if(this.$refs.showcompleteddesktop.classList.contains('active') || this.$refs.showcompleted.classList.contains('active')){
        for(let i = 0; i < this.finalTodoList.length; i++){
          if(this.finalTodoList[i].name == todo.name){
            todo.isCompleted = false
            this.finalTodoList[i].isCompleted = false
            localStorage.todoList = JSON.stringify(this.finalTodoList)
            this.finalTodoList = JSON.parse(localStorage.todoList)
            this.showCompleted()
            this.countingTodo()
            return
          }
        }
      }
    },
    isEditable(todo, index){
      if(this.$refs.showactivedesktop.classList.contains('active') || this.$refs.showactive.classList.contains('active')){
        todo.isEditable = false
        return
      }
       this.todos = this.finalTodoList.map(todo => todo.name)
       this.todos[index] = ''
      for(let i = 0; i < this.finalTodoList.length; i++){
        if(this.finalTodoList[i].name == todo.name){
          this.editedText = todo.name 
          todo.name = ''
          this.finalTodoList[i].name = ''
          todo.editable = true;
          this.finalTodoList[i].editable = true
          return
        }
      }
    },
    editCompleted(todo){ 
        if(!this.todos.includes(this.editedText.toLowerCase())){
          for(let i = 0; i < this.finalTodoList.length; i++){
            if(this.finalTodoList[i].name == todo.name){
              todo.name = this.editedText
              this.finalTodoList[i].name = this.editedText
              todo.editable = false;
              this.finalTodoList[i].editable = false
              localStorage.todoList = JSON.stringify(this.finalTodoList)
              this.finalTodoList = JSON.parse(localStorage.todoList)
              return
            }
          }          
        }else if(this.editedText == ''){
          this.errorMessage = 'enter an input!'
            setTimeout(() => {
              this.errorMessage = ''
            }, 2000);
            return
        }else{
          this.errorMessage = 'task already exist!'
          setTimeout(() => {
            this.errorMessage = ''
          }, 2000);
          return
        }
            
    },
    showAll(){
      this.finalTodoList = JSON.parse(localStorage.todoList)
      this.todoList = this.finalTodoList
      this.$refs.showactive.classList.remove('active')
      this.$refs.showall.classList.add('active')
      this.$refs.showcompleted.classList.remove('active')
      this.$refs.showactivedesktop.classList.remove('active')
      this.$refs.showalldesktop.classList.add('active')
      this.$refs.showcompleteddesktop.classList.remove('active')
      this.enabled = true
      },
    showActive(){
      this.finalTodoList = JSON.parse(localStorage.todoList)
      this.todoList = this.finalTodoList.filter(e => {
        if(!e.isCompleted){
          return e
        }
      })
      this.finalTodoList.forEach(e => e.isEditable = false)
      this.todoList.forEach(e => e.isEditable = false)
      this.$refs.showactive.classList.add('active')
      this.$refs.showall.classList.remove('active')
      this.$refs.showcompleted.classList.remove('active')
      this.$refs.showactivedesktop.classList.add('active')
      this.$refs.showalldesktop.classList.remove('active')
      this.$refs.showcompleteddesktop.classList.remove('active')
      this.countingTodo()
      this.enabled = false
    },
    showCompleted(){
      this.finalTodoList = JSON.parse(localStorage.todoList)
      this.todoList = this.finalTodoList.filter(e => {
        if(e.isCompleted){
          return e
        }
      })
      this.$refs.showactive.classList.remove('active')
      this.$refs.showall.classList.remove('active')
      this.$refs.showcompleted.classList.add('active')
      this.$refs.showactivedesktop.classList.remove('active')
      this.$refs.showalldesktop.classList.remove('active')
      this.$refs.showcompleteddesktop.classList.add('active')
      this.enabled = false
    },

    deleteTodo(todo, index){
      if(this.$refs.showall.classList.contains('active') || this.$refs.showalldesktop.classList.contains('active')){
        this.finalTodoList.splice(index, 1)
        localStorage.todoList = JSON.stringify(this.finalTodoList)
        this.finalTodoList = JSON.parse(localStorage.todoList)
        this.todoList = this.finalTodoList
        this.countingTodo()
        return
      }else{
        for(let i = 0; i < this.finalTodoList.length; i++){
          if(this.finalTodoList[i].name == todo.name){
            this.todoList.splice(index, 1)
            this.finalTodoList.splice(i, 1)
            localStorage.todoList = JSON.stringify(this.finalTodoList)
            this.finalTodoList = JSON.parse(localStorage.todoList)
            this.countingTodo()
            return
          }
        }
      }        
    },
    countingTodo(){
      this.finalTodoList = JSON.parse(localStorage.todoList)
      let filterdTodoList = this.finalTodoList.filter(e => {
        if(!e.isCompleted){
          return e
        }
      })
      this.todoNum = parseInt(filterdTodoList.length)
    },
    clearComplete(){
      let filteredTodoList = this.finalTodoList.filter(e => e.isCompleted) 
      if(filteredTodoList.length == 0){
        this.errorMessage = 'No completed task!'
        setTimeout(() => {
            this.errorMessage = ''
          }, 2000);
        return
      }  

      if(this.$refs.showcompleteddesktop.classList.contains('active') || this.$refs.showcompleted.classList.contains('active')){
          this.todoList = []
          this.finalTodoList = this.finalTodoList.filter(e => !e.isCompleted)
          localStorage.todoList = JSON.stringify(this.finalTodoList)
          this.countingTodo()
      }else{
        this.finalTodoList = this.todoList.filter(e => !e.isCompleted)
        this.todoList = this.finalTodoList
        localStorage.todoList = JSON.stringify(this.finalTodoList)
        this.countingTodo()
      }


    }

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
  --lineThroughTextColor: hsl(234, 15%, 61%);
   --todoTextColor: hsl(234, 39%, 85%);
   --jumbotronBackground: url('./assets/images/bg-desktop-dark.jpg');
   --mobilejumbotronBackground: url('./assets/images/bg-mobile-dark.jpg');
   --themeImage: url('./assets/images/icon-sun.svg');
}

::-webkit-scrollbar{
  width: 5px;
}

::-webkit-scrollbar-thumb{
  background-color: var(--todoBackgroundColor);
  border-radius: 10px;
}

#theme{
  height: 100vh;
  overflow: auto;
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

.error-message{
    position: absolute;
    background-color: var(--bodyBackgroundColor);
    color: var(--todoTextColor);
    width: 480px;
    top: -6%;
    transform: rotateX(180deg);
    text-align: center;
    border-radius: 5px;
    box-shadow: 0 0 15px 1px rgb(0 0 0 / 30%);
}

.error-message-display{
  animation: slide-down 550ms ease-out forwards;
}

.h1{
  font-size: 2.5rem;
  font-weight: 900;
  letter-spacing: 1rem;
  color: #fff;
}

.themeImage{
  background: var(--themeImage) no-repeat;
  background-size: contain;
  transition: background 500ms;
  width: 6%;
  cursor: pointer;
}

.newTodo{
  margin: 3em 0 1.2em 0;
  display: flex;
  align-items: center;
}

.newTodoBtn{
  font-size: 1.5rem;
  margin-left: 5px;
  background-color: var(--todoBackgroundColor);
  border-radius: 5px;
  color: var(--todoTextColor);
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 32px;
  height: 35px;
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
  width: 90%;
}

#todoTextEdit{
  display: inline-block;
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

.list-enter-active,
.list-leave-active{
  transition: all 500ms ease;
}

.list-enter-from{
  opacity: 0;
  transform: translateX(-30px);
}

.list-leave-to{
  opacity: 0;
  transform: translateX(30px);
}

@media screen and (max-width: 550px){
  .container{
    width: 100%;
  }

  .error-message{
    width: 95%;
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

.deleteTodo{
  opacity: 1;
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
    cursor: pointer;
  }

  .mobileLinks span:hover{
    color: var(--todoTextColor);
  }

}

@keyframes slide-down{
  to{
    top: 2%;
    transform: rotateX(0deg);
  }
}

</style>

