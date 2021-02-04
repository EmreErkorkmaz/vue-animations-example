<template>
  <router-view v-slot="slotProps">
    <transition name="fade-button" mode="out-in">
      <component :is="slotProps.Component"></component>
    </transition>
  </router-view>
  <div class="container">
    <list-data></list-data>
  </div>
  <div class="container">
    <div class="block" :class="{animate: animatedBlock}"></div>
    <button @click="animateBlock">Animate</button>
  </div>
  <div class="container">
    <transition 
    :css="false"
    @before-enter="beforeEnter" 
    @enter="enter" 
    @after-enter="afterEnter" 
    @before-leave="beforeLeave"
    @leave="leave"
    @after-leave="afterLeave"
    @enter-cancelled="enterCancelled"
    @leave-cancelled="leaveCancelled"
    >
      <p v-if="paraIsVisible">This is only sometimes visible...</p>
    </transition>
    <button @click="toggleParagraph">Toggle Paragraph</button>
  </div>
  <div class="container">
    <transition name="fade-button" mode="out-in">  
      <button @click="showUsers" v-if="!usersAreVisible">Show Users</button>
      <button @click="hideUsers" v-else>Hide Users</button>
    </transition>
  </div>
    <base-modal @close="hideDialog" :open="dialogIsVisible">
      <p>This is a test dialog!</p>
      <button @click="hideDialog">Close it!</button>
    </base-modal>
  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>  

<script>
import ListData from './components/ListData';
export default {
  components: {
    ListData
  },
  data() {
    return { 
      dialogIsVisible: false,
      animatedBlock: false,
      paraIsVisible: false,
      usersAreVisible: false,
      enterInterval: null, 
      leaveInterval: null,
      };
  },
  methods: {
    animateBlock(){
      this.animatedBlock = !this.animatedBlock;
    },
    toggleParagraph(){
      this.paraIsVisible = !this.paraIsVisible
    },
    showDialog() {
      this.dialogIsVisible = true;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    },
    showUsers() {
      this.usersAreVisible = true;
    },
    hideUsers(){
      this.usersAreVisible = false;
    },
    beforeEnter(el){
      console.log('beforeEnter()');
      el.style.opacity = 0;
    },
    beforeLeave(el){
      console.log('beforeLeave()');
      el.style.opacity = 1;

    },
    enter(el, done){
      console.log('enter()');
      let round = 1;
      this.enterInterval = setInterval(() => {
        el.style.opacity = round * 0.01;
        round++;
        if(round > 100){
          clearInterval(this.enterInterval);
          done();
        }
      }, 20);
    },
    afterEnter(){
      console.log('afterEnter()');
    },
    leave(el, done){
      console.log('leave()');
      let round = 1;
      this.leaveInterval = setInterval(() => {
        el.style.opacity = 1 - round * 0.01;
        round++;
        if(round > 100){
          clearInterval(this.leaveInterval);
          done();
        }
      }, 20);
    },
    afterLeave(){
      console.log('afterLeave()');
    },
    enterCancelled(){
      clearInterval(this.enterInterval);
    },
    leaveCancelled(){
      clearInterval(this.leaveInterval);
    }
  },
};
</script>

<style>
.route-enter-from{}
.route-enter-active{
  animation: slide-fade 0.4s ease-out;
}
.route-enter-to{}
.route-leave-active{
  animation: slide-fade 0.4s ease-in;
}
.animate {
  /* transform: translateX(-150px); */
  animation: slide-fade 0.3s ease-in forwards;
}
/* .v-enter-from {
  opacity: 0;
  transform: translateY(-30px);
} */
/* .para-enter-active {
  transition: all 0.3s ease-out;
  animation: slide-fade 0.3s ease-out;
} */
/* .v-enter-to {
  opacity: 1;
  transform: translateY(0);
} */
/* .para-leave-from {
  opacity: 1;
  transform: translateY(0);
} */
/* .para-leave-active {
  transition: all 0.3s ease-in;
  animation: slide-fade 0.3s ease-out;
} */
/* .para-leave-to {
  opacity: 0;
  transform: translateY(-30px);
} */
.fade-button-enter-from,
.fade-button-leave-to{
  opacity: 0;
}
.fade-button-enter-active{
  transition: opacity 0.3s ease-out;
}
.fade-button-leave-active{
  transition: opacity 0.3s ease-in;
}
.fade-button-enter-to,
.fade-button-leave-from{
  opacity: 1;
}

@keyframes slide-fade {
  0% {
    transform: translateX(0) scale(1);
  }
  70% {
    transform: translateX(-120px) scale(1.2);
  }
  100% {
    transform: translateX(-150px) scale(1);
  }
}
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
  outline: none;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
  /* transition: transform 0.3s ease-in; */

}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}




</style>