<template>
  <div class="jumbotron bg-primary">
    <div class="container">
        <!-- for the todo logo and changing of theme  -->
      <div class="logoTheme">
        <h1 class="h1">TODO</h1>
        <img class="themeImage" alt="change to dark or light theme">
      </div>
          <!-- for adding new todo -->
    <div class="newTodo">
      <input type="text" placeholder="Create a new todo...">
    </div>
      <!-- list of new todos -->
    <div class="todoList">
      <div class="subTodoList">
        <div class="individualTodoList">
        <span>
          <input type="checkbox" name="todocheckbox">
          <label for="todocheckbox" id="todoText">Do Something...</label>
          <input type="text" style="display: none;">
        </span>
        <span class="checkbox-wrapper">
          <span class="checkbox">
            <img src="./assets/images/icon-check.svg" alt="">
          </span>
        </span>
        <!-- delete todo -->
        <span>
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18"><path fill="#494C6B" fill-rule="evenodd" d="M16.97 0l.708.707L9.546 8.84l8.132 8.132-.707.707-8.132-8.132-8.132 8.132L0 16.97l8.132-8.132L0 .707.707 0 8.84 8.132 16.971 0z"/></svg>
        </span>
      </div>
      </div>
      <!-- todo sublinks -->
    <div class="todoFootLinks">
      <p class="numOfItems">3 items left</p>
      <div class="links">
        <span>all</span>
        <span>active</span>
        <span>completed</span>
      </div>
      <p class="clearComplete">clear complete</p>
    </div>
    </div>

       <div class="mobileLinks" style="display: none;">
        <span>all</span>
        <span>active</span>
        <span>completed</span>
      </div>

      <p>Drag and drop to reorder list</p>
    </div>
  </div>
</template>

<script>

export default {
  
}
</script>

<style>
:root{
  --checkboxBackgroundColor: linear-gradient(45deg, hsl(192, 100%, 67%),hsl(280, 87%, 65%));
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
   --mobilejumbotronBackground: url('./assets/images/bg-mobile-light.jpg');
   --themeImage: url('./assets/images/icon-sun.svg');
}

#app{
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
  width: 40%;
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
}

.newTodo{
  margin: 3em 0 1em 0;
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
}

.individualTodoList{
  border-bottom: 1px solid var(--textColor);
  display: flex;
  justify-content: space-between;
  padding: 0.7em 1em;
  position: relative;
}

.individualTodoList span:nth-child(1){
  display: flex;
  justify-content: center;
  align-items: center;
}

.individualTodoList span:nth-child(1) input{
  margin-right: 2em;
  /* visibility: hidden; */
}

.checkbox-wrapper{
  width: 25px;
  height: 55%;
  border-radius: 50%;
  border: 1px solid var(--lineThroughTextColor);
  position: absolute;
  left: 3%;
  top: 23%;
  pointer-events: none;
}

.checkbox{
  width: 100%;
  height: 100%;
  opacity: 0;
  border-radius: 50%;
  text-align: center;
  background: var(--checkboxBackgroundColor);
}

.checkbox img{
  width: 50%;
}


</style>

