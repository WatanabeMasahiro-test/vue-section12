<!--**
    * トランジションをコンポーネントで再利用するときは、SLOTで中のデータを渡す
    **   -->

<template>
  <div class="container">
    <div class="row">
      <div class="main border mt-2 pt-2 text-center">
        <Transition name="fade" mode="out-in">
          <p v-if="show" key="bye">さよなら</p>
          <p v-else key="hello">こんにちは</p>
        </Transition>
        <hr>
          <button @click="add()">追加</button>
          <ul class="mx-auto w-25">
            <TransitionGroup name="fade" tag="div">
              <li 
                style="cursor: pointer; width:200px;" 
                v-for="(number, index) in numbers" 
                :key="number"
                @click="remove(index)"
              >{{ number }}</li>
          </TransitionGroup>
          </ul>
          <hr>
          <button @click="switchAnimation = 'slide'" class="mx-1">スライド</button>
          <button @click="switchAnimation = 'fade'"  class="mx-1">フェイド</button>
          <p>{{ switchAnimation }}</p>
        <Transition
          :name="switchAnimation" 
          apper
        >
        <p v-show="show">BYE</p>
        </Transition>
        <hr>
        <button @click="show = !show" class="btn btn-primary">切り替え</button>
        <Transition 
          :css="false" 
          @before-enter="beforeEnter" 
          @enter="enter" 
          @after-enter="afterEnter" 
          @enter-cancelled="enterCancelled" 
          @before-leave="beforeLeave" 
          @leave="leave" 
          @after-leave="afterLeave" 
          @leave-cancelled="leaveCancelled" 
        >
          <div class="circle mt-3" v-if="show"></div>
        </Transition>
        <Transition name="fade" appear>
          <p v-if="show">hello</p>
        </Transition>
        <Transition :name="slide" type="animation" appear>
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
        <hr>
        <button @click="myComponent = 'ComponentA'" class="mx-1">ComponentA</button>
        <button @click="myComponent = 'ComponentB'" class="mx-1">ComponentB</button>
        <Transition name="fade" mode="out-in" appear>
          <component :is="myComponent" :class="myComponent == 'ComponentA' ? 'animate__animated animate__bounce' : 'animate__animated animate__shakeX'"></component>
        </Transition>
        <hr>
      </div>
    </div>
  </div>
</template>


<script>
  import ComponentA from './components/ComponentA.vue'
  import ComponentB from './components/ComponentB.vue'
  import './assets/animate.css/animate.min.css'

  export default {
    name: 'App',
    components: {
      ComponentA,
      ComponentB,
    },
    data: () => ({
      show            : true,
      switchAnimation : 'slide',
      myComponent     : 'ComponentA',
      numbers         : [0,1,2,],
      nextNumber      : 3,
    }),
    computed: {
    },
    methods: {
      ramdomIndex() {
        return Math.floor(Math.random() * this.numbers.length);
      },
      add() {
        this.numbers.splice(this.ramdomIndex(), 0, this.nextNumber);
        this.nextNumber += 1;
      },
      remove(index) {
        this.numbers.splice(index, 1);
      },
      myComponent_f() {
        if(this.myComponent === 'ComponentA') {
          return 'fade';
        } else if (this.myComponent === 'ComponentB') {
          return 'shakeX';
        }
      },
      beforeEnter(el)   {
         // 現れる前
        el.style.transform = `scale(0)`;
      },
      enter(el, done)   { // doneはCSSを使わないときは付ける？？
         // 現れるとき
         let scale = 0;
         const interval = setInterval(() => {
          el.style.transform = `scale(${scale})`;
          scale += 0.1;
          if(scale > 1) {
            clearInterval(interval);
            done();
          }
         }, 200);
      },
      afterEnter()    {
         // 現れた後
      },
      enterCancelled(){
         // 現れるアニメーションがキャンセルされたとき
      },
      beforeLeave()   {
         // 消える前
      },
      leave(el, done)         {
         // 消えるとき
         let scale = 1;
         const interval = setInterval(() => {
          el.style.transform = `scale(${scale})`;
          scale -= 0.1;
          if(scale < 0) {
            clearInterval(interval);
            done();
          }
         }, 200);
      },
      afterLeave()    {
         // 消えた後
      },
      leaveCancelled(){ // v-showのときのみ
         // 消えるときアニメーションがキャンセルされたとき
      },
    },
  }
</script>


<style scoped>
  .fade-move {
    transition : transform 1s;
  }
  .fade-enter-from {
    /* 現れるときの最初の状態 */
    opacity: 0;
  }
  .fade-enter-active {
    /* 現れるときのトランジションの状態 */
    transition: opacity 0.5s;
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
    transition: opacity 0.5s;
    position  : absolute;
  }
  .fade-leave-to {
    /* 消えるときの最後の状態 */
    opacity: 0;
  }

  .slide-enter-from {
    opacity: 0;
  }
  .slide-enter-active {
    animation: slide-in 0.5s;
    transition: opacity 0.5s;
  }
  .slide-enter-to {
    opacity: 1;
  }
  .slide-leave-from {
    opacity: 1;
  }
  .slide-leave-active {
    animation: slide-in 0.5s reverse;
    transition: opacity 0.5s;
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

  .circle {
    width             : 200px;
    height            : 200px;
    margin            : auto;
    border-radius     : 200px;
    background-color  : deeppink;;
  }
</style>
