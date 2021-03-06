## 1.Description:
 In single page application on Mobile by using Vue.js, I suggest using `window.sessionStorage` and `localStorage` 
 to build the transition-controller to show the forward or backward actions.


2.Main:

 ```  
  watch: {
    '$route' (to, from) {
      let history = window.sessionStorage
      if (!history.getItem(from.path)) {
        history.setItem(from.path, $lstore.get('PageCount'))
      }
      if (!history.getItem(to.path)) {
        let count = $lstore.get('PageCount')
        count = count + 1
        history.setItem(to.path, count)
        $lstore.set('PageCount', count)
      }
      if (history.getItem(to.path) > history.getItem(from.path)) {
        this.transitionName = 'slide-left' // 前进
      } else {
        this.transitionName = 'slide-right' // 后退
      }
    }
  },

  created() {
    window.sessionStorage.clear()
    $lstore.set('PageCount', 1)
  }

```

3.Demo shows: 

// app.vue

```
 <template>
     <div id="app">
         <transition :name="transitionName">
           <keep-alive>
             <router-view class="childView" v-if="!$route.meta"></router-view>
           </keep-alive>
         </transition>
         <transition :name="transitionName">
           <router-view class="childView" v-if="$route && !$route.meta.keepAlive"></router-view>
         </transition>
     </div>
 </template>
 <script>
 export default {
   name: 'app',
   data() {
     return {
       transitionName: 'slide-right'
     }
   },
   watch: {
     '$route' (to, from) {
       let history = window.sessionStorage
       if (!history.getItem(from.path)) {
         history.setItem(from.path, $lstore.get('PageCount'))
       }
       if (!history.getItem(to.path)) {
         let count = $lstore.get('PageCount')
         count = count + 1
         history.setItem(to.path, count)
         $lstore.set('PageCount', count)
       }
       if (history.getItem(to.path) > history.getItem(from.path)) {
         this.transitionName = 'slide-left' // 前进
       } else {
         this.transitionName = 'slide-right' // 后退
       }
     }
   },
   created() {
     window.sessionStorage.clear()
     $lstore.set('PageCount', 1)
   }
 }
 </script>
 <style scoped>
 #app {
     font-family: 'Avenir', Helvetica, Arial, sans-serif;
     -webkit-font-smoothing: antialiased;
     -moz-osx-font-smoothing: grayscale;
     color: #2c3e50;
     font-size: 0.25rem;
 }

 .childView {
     position: absolute;
     left: 0;
     top: 0;
     width: 100%;
     height: 100%;
     transition: all .35s cubic-bezier(.55, 0, .1, 1);
 }

 .slide-left-enter,
 .slide-right-leave-active {
     opacity: 0;
     -webkit-transform: translate(100%, 0);
     transform: translate(100%, 0);
 }

 .slide-left-leave-active,
 .slide-right-enter {
     opacity: 0;
     -webkit-transform: translate(-100%, 0);
     transform: translate(-100% 0);
 }
 </style>

```
