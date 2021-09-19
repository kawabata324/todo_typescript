<template>
  <div class="hello">
    <h1 class="text-3xl text-blue-600 m-4">{{ data.msg }}</h1>
    <div class="flex ">
      <div class="pl-2">
        <input type="radio" id="all" value="all" name="radio"
        @click="allShow" checked>
        <label for="all">すべて</label>
      </div>
      <div class="pl-2">
        <input type="radio" id="todo" value="todo" name="radio"
        @click="doingShow">
        <label for="todo" >作業中</label>
      </div>
      <div class="pl-2">
        <input type="radio" id="done" value="done" name="radio">
        <label for="done" >完了</label>
      </div>
    </div>
    <table class="table-auto">
      <tr>
        <th class="px-4 py-2">ID</th>
        <th class="px-12 py-2">コメント</th>
        <th class="px-6 py-2">状態</th>
        <th class="px-4 py-2">-</th>
      </tr>
      <tr v-for="(item,index) in data.items" :key="item.id">
        <td class="border px-4 py-4">{{index}}</td>
        <td class="border px-4 py-4">{{item}}</td>
        <td class="border px-4 py-4"><button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-0.5 px-3 rounded-full" @click.exact="Changecondition" @click.shift="Changecondition2">{{data.condition}}</button></td>
        <td class="border px-4 py-4"> <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-1 px-3 rounded" @click.shift="removeTodo(item)">削除</button></td>
      </tr>
    </table>
    <p class="text-sm">※削除ボタンは シフトキーを押しながらクリックしてください</p>
  </div>
  <div>
    <h2 class="text-2xl my-5">新しい作業の追加</h2>
    <div class="flex items-center">
      <label for="comment" class="mr-6">コメント</label>
      <input class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block  sm:text-sm border-2 border-gray-600  rounded-md" type="text" v-model='data.add'>
      <input class="mt-3 w-full inline-flex items-center justify-center px-4 py-2 border border-transparent shadow-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm" type="submit" value="追加" @click="addTodo">
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import {reactive,onMounted} from 'vue'

interface Data{
  msg:string
  todo:string
  add:string
  addes:string[]
  items:string[]
  condition:string,
  }
export default defineComponent({
  name: 'HelloWorld',
  props:{
    addes:String
  },
  setup(){
    const data=reactive<Data>({
      msg:'My todo in typescript',
      todo:'',
      add:'',
      addes:[],
      items:[],
      condition:'作業中',
    })
    onMounted(()=>{
      let storagedata=localStorage.getItem('data')
      if(storagedata){
        let savedata:string[]=JSON.parse(storagedata)
        data.items=savedata
      }
    })
    const addTodo=function(){
        if(!data.add){
          return
        }
        if(localStorage.getItem('data')){
          let storagedata=localStorage.getItem('data')
        if(storagedata){
          let savedata:string[]=JSON.parse(storagedata)
          data.addes=savedata

          }
        }
        data.addes.push(data.add)
        data.add=''
        saveTodo()
        allShow()
      }
    const saveTodo=function():void{
      const parse = JSON.stringify(data.addes)
      if(parse){
        localStorage.setItem('data', parse);

      }
    }
    const allShow=function(){
      let storagedata=localStorage.getItem('data')
      if(storagedata){
        let savedata:string[]=JSON.parse(storagedata)
        data.items=savedata
      }
    }
    const removeTodo=function(item:string){
      const removeItem=data.items.indexOf(item)
      data.items.splice(removeItem,1)
      const parse = JSON.stringify(data.items)
      if(parse){
        localStorage.setItem('data', parse);
      }
    }
    return {
      data,addTodo,saveTodo,allShow,removeTodo
    }
  }
});
</script>
