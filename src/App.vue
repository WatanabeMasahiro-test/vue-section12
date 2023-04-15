<template>
  <div class="container">
    <div class="row">
      <div class="main w-75 border mt-2 pt-2 text-center">
        <button @click="show =! show" class="btn btn-primary">切り替え</button>
          <Transition name="fade" appear>
            <p v-if="show">hello</p>
          </Transition>
          <Transition name="slide" type="animation" appear>
            <div v-if="show">
              <p>bye</p>
              <p>HELLO</p>
              <p>HELLO</p>
              <p class="animate__animated animate__bounce">HELLO</p>
            </div>
          </Transition>
          <Transition 
            appear 
            enter-from-class="" 
            enter-active-class="animate__animated animate__bounce" 
            enter-to-class="" 
            leave-class="" 
            leave-active-class="animate__animated animate__shakeX" 
            leave-to-class=""
            >
            <p v-if="show">hello</p>
          </Transition>
      </div>
    </div>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import './assets/animate.css/animate.min.css'

export default {
  name: 'App',
  components: {
    // HelloWorld,
  },
  data: () => ({
    show : true,
}),
computed: {

},
methods: {

},
}
</script>

<style scoped>
.fade-enter-from {
  /* 現れるときの最初の状態 */
  opacity: 0;
}
.fade-enter-active {
  /* 現れるときのトランジションの状態 */
  transition: opacity 5s;
}
.fade-enter-to {
  /* 現れるときの最後の状態 */
  opacity: 1;
}
.fade-leave-from {
  /* 消えるときの最初の状態 */
  opacity: 1;
}
.fade-leave-active {
  /* 消えるときのトランジションの状態 */
  transition: opacity 5s;
}
.fade-leave-to {
  /* 消えるときの最後の状態 */
  opacity: 0;
}

.slide-enter-from {
  opacity: 0;
}
.slide-enter-active {
  animation: slide-in 5s;
  transition: opacity 1s;
}
.slide-enter-to {
  opacity: 1;
}
.slide-leave-from {
  opacity: 1;
}
.slide-leave-active {
  animation: slide-in 5s reverse;
  transition: opacity 1s;
}
.slide-leave-to {
  opacity: 0;
}
@keyframes slide-in {
  from {
    transform: translateX(100px);
  }
  to {
    transform: translateY(0);
  }
}

/* 
.main {

}
*/
</style>
