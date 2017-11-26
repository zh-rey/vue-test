<template>
  <div id="app">
    <h1>{{title}}</h1>
    <input v-model="newItem" @keyup.13="addNew" type="text">
    <ul>
      <li v-for='item in items' :class='{finished:item.isFinished}' @click="toggleFinish(item)">{{item.label}}</li>
    </ul>
    <p>child tells me: {{childWords}}</p>
    <component-a msgfromfather="you die!" @child-tell="listenToMyBoy"></component-a>
  </div>
</template>

<script>
import Store from './store.js';
import ComponentA from './components/componentA'
export default {
  data () {
    return {
      title:'this is todo list',
      items:Store.fetch(),
      newItem:'',
      childWords:''
    }
  },
  components:{ComponentA},
  watch:{
    items:{
      handler(items){
        Store.save(items);
      },
      deep:true
    }
  },
  methods:{
    toggleFinish(item){
      item.isFinished = !item.isFinished;
    },
    addNew(){
      this.items.push({
        label:this.newItem,
        isFinished:false
      });
      this.newItem = '';
    },
    listenToMyBoy(msg){
      this.childWords = msg;
    }
  }
}
</script>

<style>
.finished{text-decoration: underline;}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
