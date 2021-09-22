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
        <td class="border px-4 py-4">{{item.comment}}</td>
        <td class="border px-4 py-4"><input type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-0.5 px-3 rounded-full" @click.exact="item.status='完了'" @click.shift="item.status='作業中'" v-model="item.status"></td>
        <td class="border px-4 py-4"> <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-1 px-3 rounded" @click.shift="removeTodo(index)">削除</button></td>
      </tr>
    </table>
    <p class="text-sm">※削除ボタンは シフトキーを押しながらクリックしてください</p>
    <p class="text-sm">※完了から作業中に戻すには、シフトキーを押しながらクリックしてください</p>
  </div>
  <div>
    <h2 class="text-2xl my-5">新しい作業の追加</h2>
    <div class="flex items-center">
      <label for="comment" class="mr-6">コメント</label>
      <input class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block  sm:text-sm border-2 border-gray-600  rounded-md" type="text" v-model='data.add'>
      <input class="mt-3  inline-flex items-center justify-center px-4 py-2 border border-transparent shadow-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500" type="submit" value="追加" @click="addTodo">
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
  addes:[{comment:string,status:string}]
  items:[{comment:string,status:string}]
  }
export default defineComponent({
  name: 'HelloWorld',
  setup(){
    const data=reactive<Data>({
      msg:'My todo in typescript',
      todo:'',
      add:'',
      addes:[{comment:'',status:''}],
      items:[{comment:'',status:''}],
    })
    onMounted(()=>{
      let storagedata=localStorage.getItem('data')
      if(storagedata){
        let savedata:[{comment:string,status:string}]=JSON.parse(storagedata)
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
          let savedata:[{comment:string,status:string}]=JSON.parse(storagedata)
          data.addes=savedata
          }
        }
        let pushData={
          comment:data.add,
          status:'作業中'
        }
        data.addes.push(pushData)
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
        let savedata:[{comment:string,status:string}]=JSON.parse(storagedata)
        data.items=savedata
      }
    }
    const removeTodo=function(item:{comment:string,status:string}){
      const removeItem=data.items.indexOf(item)
      data.items.splice(removeItem,1)
      const parse = JSON.stringify(data.items)
      if(parse){
        localStorage.setItem('data', parse);
      }
    }
    const doingShow=function(){
      data.addes=[{comment:'',status:''}]
      if(localStorage.getItem('data')){
        let storagedata=localStorage.getItem('data')
        if(storagedata){
        let savedata:[{comment:string,status:string}]=JSON.parse(storagedata)
          const findresult=savedata.filter(elment=>
          elment.status==='作業中')
          console.log(findresult)
          // data.addes=findresult
        }
      }

    }
    return {
      data,addTodo,saveTodo,allShow,doingShow,removeTodo
    }
  }
});
</script>
