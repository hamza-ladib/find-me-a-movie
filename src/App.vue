<script setup>
import { isObject } from '@vue/shared';
import { ref,reactive,watch} from 'vue';

 let statut=ref(true);
 let showDesc=ref(false);
 let showActors=ref(false);
 let title=ref("");
 let url=ref("");
 let noresult=ref("false")
 let response=ref({});
 let movie = reactive({
    img:"./src/assets/spider.png",
    title:"spider man",
    type: "action",
    rate: "6.9",
    duration:300,
    category:"action / super",
    sameinfo:" TV-14 2022- N/A",
    description:"Pariatur officia nisi proident excepteur ad reprehenderit minim. Velit do in fugiat pariatur. Ut magna id voluptate Lorem aute esse in laboris qui laborum. Sint quis tempor ut laborum et adipisicing labore nostrud reprehenderit enim enim laboris. Ullamco eu ea ipsum Lorem deserunt labore nostrud dolore amet veniam. Commodo est do tempor eu incididunt excepteur officia sunt.",
    actors:["wafae abdou","hamza ladib","tom hardy","emilia clark"]
 })

 watch(showDesc,()=>{if(showDesc.value) showActors.value=false})
 watch(showActors,()=>{if(showActors.value) showDesc.value=false})
 /**
  *! api info 
    * */
   watch(title,()=>{url.value="http://www.omdbapi.com/?t="+title.value+"&apikey=29ffe900"});
   let findMovie= async()=>{
    fetch(url.value).then(result=> result.json()).then(data=>{
    response.value={
      img:data.Poster,
      title:data.Title,
      type:data.Genre,
      rate:data.imdbRating,
      duration:data.Runtime.substring(0,data.Runtime.length-3),
      sameinfo: data.Type+" "+ data.Year+"-"+" "+(data.Production || ""),
      description:data.Plot,
      actors:data.Actors
    }
    
    if(response.value.title== undefined){
      noresult.value=false;
      response.value={};
    }
       
       else {
       noresult.value=true;
      response.value.actors=response.value.actors.split(",");
       console.log(response.actors.value,response.actos.value.split(","));
       
      }
      
    
      
    }
    
    
    );
   }

</script>
<template>
<div class=" p-4 flex justify-center w-screen h-screen bg-gradient-to-br from-yellow-300 to-orange-600 ">

  <div class="  p-6 rounded-2xl h-fit w-full lg:w-1/2 bg-[#1e293b] text-white font-poppins text-base flex flex-col gap-5 items-center">
   <div class=" w-full flex">
    <input v-model="title" placeholder="search a movie " class=" p-1 rounded-md hover:border-emerald-600 focus:border-emerald-500 w-full  bg-transparent border border-white"/>
     <button @click="findMovie" class=" -mx-6 bg-[#1e293b] rounded-full "><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class=" hover:text-green-400 w-10 h-10 text-white ">
  <path d="M8.25 10.875a2.625 2.625 0 115.25 0 2.625 2.625 0 01-5.25 0z" />
  <path fill-rule="evenodd" d="M12 2.25c-5.385 0-9.75 4.365-9.75 9.75s4.365 9.75 9.75 9.75 9.75-4.365 9.75-9.75S17.385 2.25 12 2.25zm-1.125 4.5a4.125 4.125 0 102.338 7.524l2.007 2.006a.75.75 0 101.06-1.06l-2.006-2.007a4.125 4.125 0 00-3.399-6.463z" clip-rule="evenodd" />
</svg>
</button>
   </div>
 <!-- movie not found-->
   <div v-if="noresult==false"  class=" text-lg  p-6 font-quicksand flex gap-2">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
  <path fill-rule="evenodd" d="M9.401 3.003c1.155-2 4.043-2 5.197 0l7.355 12.748c1.154 2-.29 4.5-2.599 4.5H4.645c-2.309 0-3.752-2.5-2.598-4.5L9.4 3.003zM12 8.25a.75.75 0 01.75.75v3.75a.75.75 0 01-1.5 0V9a.75.75 0 01.75-.75zm0 8.25a.75.75 0 100-1.5.75.75 0 000 1.5z" clip-rule="evenodd" />
</svg>
 movie not found </div>
   <!-- movie found-->
   <div v-if="Object.values( response).length!=0"  class="relative w-full bg-white flex flex-col md:flex-row bg-opacity-50 justify-center items-center rounded-lg">
    <!-- left side -->
  <div class=" p-2 text-center text-xl font-semibold flex flex-col justify-center items-center relative    gap-1"> 
    <h1 class=" justify-center  z-50 bg-[#1e293b] flex gap-2 items-center p-2 text-white bg-opacity-80 mb-2 rounded-md">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 text-indigo-600">
  <path stroke-linecap="round" stroke-linejoin="round" d="M3.375 19.5h17.25m-17.25 0a1.125 1.125 0 01-1.125-1.125M3.375 19.5h1.5C5.496 19.5 6 18.996 6 18.375m-3.75 0V5.625m0 12.75v-1.5c0-.621.504-1.125 1.125-1.125m18.375 2.625V5.625m0 12.75c0 .621-.504 1.125-1.125 1.125m1.125-1.125v-1.5c0-.621-.504-1.125-1.125-1.125m0 3.75h-1.5A1.125 1.125 0 0118 18.375M20.625 4.5H3.375m17.25 0c.621 0 1.125.504 1.125 1.125M20.625 4.5h-1.5C18.504 4.5 18 5.004 18 5.625m3.75 0v1.5c0 .621-.504 1.125-1.125 1.125M3.375 4.5c-.621 0-1.125.504-1.125 1.125M3.375 4.5h1.5C5.496 4.5 6 5.004 6 5.625m-3.75 0v1.5c0 .621.504 1.125 1.125 1.125m0 0h1.5m-1.5 0c-.621 0-1.125.504-1.125 1.125v1.5c0 .621.504 1.125 1.125 1.125m1.5-3.75C5.496 8.25 6 7.746 6 7.125v-1.5M4.875 8.25C5.496 8.25 6 8.754 6 9.375v1.5m0-5.25v5.25m0-5.25C6 5.004 6.504 4.5 7.125 4.5h9.75c.621 0 1.125.504 1.125 1.125m1.125 2.625h1.5m-1.5 0A1.125 1.125 0 0118 7.125v-1.5m1.125 2.625c-.621 0-1.125.504-1.125 1.125v1.5m2.625-2.625c.621 0 1.125.504 1.125 1.125v1.5c0 .621-.504 1.125-1.125 1.125M18 5.625v5.25M7.125 12h9.75m-9.75 0A1.125 1.125 0 016 10.875M7.125 12C6.504 12 6 12.504 6 13.125m0-2.25C6 11.496 5.496 12 4.875 12M18 10.875c0 .621-.504 1.125-1.125 1.125M18 10.875c0 .621.504 1.125 1.125 1.125m-2.25 0c.621 0 1.125.504 1.125 1.125m-12 5.25v-5.25m0 5.25c0 .621.504 1.125 1.125 1.125h9.75c.621 0 1.125-.504 1.125-1.125m-12 0v-1.5c0-.621-.504-1.125-1.125-1.125M18 18.375v-5.25m0 5.25v-1.5c0-.621.504-1.125 1.125-1.125M18 13.125v1.5c0 .621.504 1.125 1.125 1.125M18 13.125c0-.621.504-1.125 1.125-1.125M6 13.125v1.5c0 .621-.504 1.125-1.125 1.125M6 13.125C6 12.504 5.496 12 4.875 12m-1.5 0h1.5m-1.5 0c-.621 0-1.125.504-1.125 1.125v1.5c0 .621.504 1.125 1.125 1.125M19.125 12h1.5m0 0c.621 0 1.125.504 1.125 1.125v1.5c0 .621-.504 1.125-1.125 1.125m-17.25 0h1.5m14.25 0h1.5" />
</svg>


     <span class="  "> {{ response.title }}</span></h1>
      <!-- rate-->
      <div class="  absolute items-center bg-opacity-80 hover:bg-opacity-100 transition-all ease-out duration-300 flex  bg-[#1e293b] gap-1 text-center m-auto p-1 px-3 rounded-md  top-16 left-2/3 z-20">
        <span>{{ response.rate.split(".")[0]}}</span>
        <span>.</span>
        <span class="  text-indigo-400 font-bold rotate-12 text-4xl">{{ response.rate.split(",")[1]|| "0"}}</span>
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6  text-yellow-400">
  <path fill-rule="evenodd" d="M10.788 3.21c.448-1.077 1.976-1.077 2.424 0l2.082 5.007 5.404.433c1.164.093 1.636 1.545.749 2.305l-4.117 3.527 1.257 5.273c.271 1.136-.964 2.033-1.96 1.425L12 18.354 7.373 21.18c-.996.608-2.231-.29-1.96-1.425l1.257-5.273-4.117-3.527c-.887-.76-.415-2.212.749-2.305l5.404-.433 2.082-5.006z" clip-rule="evenodd" />
</svg>
      </div>
      <!-- watch now -->
      <div  class=" bg-opacity-80 hover:bg-opacity-100 transition-all ease-out duration-300 absolute top-2/3 left-36 text-indigo-500 flex  bg-[#1e293b] gap-1 text-center m-auto p-1 px-3 rounded-md  -mb-14 z-20 cursor-pointer">
        <span>watch trailer</span>
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-8 h-8 ">
  <path stroke-linecap="round" stroke-linejoin="round" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
  <path stroke-linecap="round" stroke-linejoin="round" d="M15.91 11.672a.375.375 0 010 .656l-5.603 3.113a.375.375 0 01-.557-.328V8.887c0-.286.307-.466.557-.327l5.603 3.112z" />
</svg>

       
      </div>


      <!----->
      
    <img class=" border-2 border-indigo-300 " :src="response.img">
    <div class=" text-white text-center flex  gap-2 ">
    <div class=" text-md font-quicksand p-3 bg-[#1e293b] bg-opacity-70 rounded-md ">{{ response.duration }} <span class=" text-indigo-300 ">min</span></div> 
    <div class=" text-md font-quicksand  p-3 bg-[#1e293b] bg-opacity-70 rounded-md ">{{ response.type }}</div> 
   </div>
   <div class=" text-white text-center m-0 flex ">
    <div class=" F text-md  font-quicksand p-3 bg-[#1e293b] bg-opacity-70 rounded-md ">{{ response.sameinfo}} <span class=" text-indigo-300 ">.</span></div> 
   </div>
  </div>
  <!---right -->
  <div class="md:w-1/2 w-full flex  flex-col justify-center  p-3 items-center gap-2 ">
   

   <div class=" mx-auto text-white text-center p-3 flex flex-col  bg-[#1e293b] rounded-xl bg-opacity-75 w-full transition ease-in duration-1000 hover:bg-yellow-400 gap-2 ">
    <div class=" flex text-indigo-400 justify-around items-center ">
  <span class="w-1/2"> Description</span>
    <svg @click="showDesc=!showDesc" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-10 h-10 text-black bg-white hover:bg-emerald-500 rounded-full text-center">
  
    <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 8.25l-7.5 7.5-7.5-7.5" />
</svg>


</div>
    

    <div v-show="showDesc " class=" transaction   p-3 bg-opacity-70 rounded-md ">{{response.description}}</div> 
   </div>

   <!-- actors -->
   <div class=" text-white text-center p-3 flex flex-col  bg-[#1e293b] rounded-xl bg-opacity-75 w-full transition ease-in duration-1000 hover:bg-yellow-400 gap-2 ">
    <div class=" flex text-indigo-400 justify-around items-center ">
  <span class="w-1/2">Actors</span>
    <svg   @click=" showActors=!showActors" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-10 h-10 text-black bg-white hover:bg-emerald-500 rounded-full text-center">
  
    <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 8.25l-7.5 7.5-7.5-7.5" />
</svg>


</div>
    

    <div  v-if="showActors" class=" transaction   p-3 bg-opacity-70 rounded-md flex flex-wrap gap-2 ">
    <div v-for="actor in response.actors" class=" flex space-y-2">
      <span class=" bg-indigo-400 p-2 rounded-lg bg-opacity-75  ">{{ actor }}</span>

    </div>
    
      
    
    </div> 
    
   </div>

   
   </div>
   
 
   

   </div>
  <!-- movie found end -->
 

  </div>
</div>

</template>
<style scoped></style>