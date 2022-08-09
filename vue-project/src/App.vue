<script setup>
      
       import { ref } from 'vue' 
        
       let vils = ref([])
       let all = ref([
             { vil: 0, tuman: 0 , comment: "" , tlist: [] } 
       ])
  
       let getVils = ()=>{
         fetch('https://robocontest.uz/api/regions', {
            method: 'GET',
            headers: {
              'Content-Type': 'application/json; charset=utf-8' 
            }}
            ).then(async(data,status)=>{
                vils.value = await data.json()   
          })
       }
      
       let getTuman = (index)=>{
           fetch('https://robocontest.uz/api/regions?q='+all.value[index].vil, {
             method: 'GET',
             headers: {
              'Content-Type': 'application/json; charset=utf-8' 
             }}
             ).then(async(data,status)=>{
                all.value[index].tlist = await data.json();
                all.value[index].tuman = all.value[index].tlist[0].id || 0
          })
      }
  
      let addNew = ()=>{
           all.value.push({ vil: 0 , tuman: 0 , comment: "" , tlist: [] })
      }

       let removeOld = (index)=>{
          all.value.pop(index)
      }

      let updateTList = (index) =>{
          getTuman(index)
      }

      getVils()
      
</script>

<template>

    <div class="container mt-5 " style="height: 100vh">
            <button  class="btn btn-primary" @click="addNew()">
                   <i class="fa fa-plus-circle" aria-hidden="true"></i>
            </button>
             <a class="btn btn-light m-2" href="https://github.com/mensenvau/vuejs_ch" target="_blank" >
                 <i class="fa fa-github" aria-hidden="true"></i>
            </a>
           <div class="row col-12 mt-2" v-for="(item,index) in all">
          
              <div class="form-group col-sm-3"> 
                <label for="">Viloyat {{index}}</label>
                <select class="form-control"  v-model="item.vil" @change="updateTList(index)">
                    <option v-for="vil in vils" :value="vil.id">{{vil.name}}</option> 
                </select>
              </div> 

              <div class="form-group col-sm-3"> 
                <label for="">Tuman</label>
                <select class="form-control"  v-model="item.tuman"  :disabled="item.tlist.length==0">
                     <option v-for="tuman in item.tlist" :value="tuman.id">{{tuman.name}}</option> 
                </select>
              </div>
             
              <div class="form-group col-sm-3">
                <label for="">Comment</label>
                <input type="text"  class="form-control" placeholder="Comment"   v-model="item.comment"> 
              </div>
               
              <div class="form-group col-sm-1"> 
                  <label for="">Delete</label>
                  <button  class="btn btn-danger" @click="removeOld(index)">
                      <i class="fa fa-trash-o" aria-hidden="true"></i>
                  </button>
              </div>
              
          </div>

          <div class="row mt-5">
              <code>{{all}}</code>
          </div>
           
    </div>
     
</template>
 

 