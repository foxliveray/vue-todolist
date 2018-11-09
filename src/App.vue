
<template>
  <div id="app"">
    <h1>{{title}}</h1>
    <ul class="todos">
    <li>
    	<input v-model="newTodo"
    		   @keyup.13="addItem"
    		   placeholder="write down your todo things."
    		   autofocus="true"/>
    </li>
	<li v-for="(todo,index) in todos" :id="index" :class="{'checked':todo.done}">
	<input type="checkbox"
                       @change="saveToStore"
                       v-model="todo.done"
/>
		<label>{{index+1}}.{{todo.value}}</label>
		<time>{{todo.created | date}}</time>
		<button @click.prevent="delItem(todo)"></button>
	</li>
    </ul>
  </div>
</template>

<script type="text/ecmascript-6">
import './assets/site.less'
import './assets/todo.less'	
import moment from 'moment'
import 'moment/locale/zh-cn'

moment.locale('zh-cn')
export default {
  name: 'app',
    filters:{
	  	date(val){
	  		return moment(val).calendar()
	  	}
  } ,
  data () {
    return {
      title: 'vue-todos',
      newTodo:"",
      todos:[]
    }
  }	,
  created(){
  	if(this.is_initialized){
  		this.todos = JSON.parse(localStorage.getItem('VUE-TODOS'))
  	}
  } ,
  computed:{
  	is_initialized(){
  		return localstorage.getItem('VUE-TODOS')
  	}
  } ,
  methods:{
  	addItem(){
  		this.todos.push({
  			value:this.newTodo,
  			created:Date.now(),
  			done:false
  		});
  		this.saveToStore();
  		this.newTodo=''
  	},
  	delItem(todo){
  		this.todos = this.todos.filter((x)=>x!==todo)
  		this.saveToStore()
  	},
  	saveToStore(){
  		localStorage.setItem('VUE-TODOS',JSON.stringify(this.todos))
  	}
  }
}
</script>
