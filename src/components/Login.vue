<template>
    <div class="page-loader bg-dark" v-if="!isloaded">
    
      <img src="https://cdn.pixabay.com/animation/2022/07/29/03/42/03-42-11-849_512.gif" alt="Computer man" style="width:100px;height:100px;">
      <h3 class="text-light">Loading . . .</h3>
    </div> 
  </template>
  
  <script>
    export default {
      data: () => {
        return {
          isloaded: false
        }
      },
      mounted() {
        document.onreadystatechange = () => {
          if (document.readyState == "complete") { 
            setTimeout(()=>{
                this.isloaded = true
            },2500)
            
          } 
        }
      },
    }
  </script>
  
  <style>
     



.page-loader {
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    z-index: 999;
}



.cube{
    width: 40px;
    height: 40px;
    margin-right: 10px;
    
}

  </style>