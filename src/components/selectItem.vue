<template>
  <div class="selectItem bg-dark text-light">
    <div class="nav_box">
        <nav class="navbar navbar-light bg-light">
          <div class="container-fluid">
            <a class="navbar-brand fw-bold" href="#" style="font-size: 25px;"><span >Chill</span><span class="text-warning">Zone</span></a>
          </div>
        </nav>
        <button type="button" class="btn-close close-btn" aria-label="Close" @click.prevent="closeThis()" ></button>
    </div>
    <!--Contain-->
    <div class="containt" >

      <img :src="'https://www.themoviedb.org/t/p/w300_and_h450_bestv2'+selectData.poster_path" alt="" class="imageBox">

      <div class="containBox">
        <span style="display: flex; gap: 10px; align-items: baseline;">
         
          
            <h2 class="mName" v-if="selectData.title != null">{{selectData.title}}</h2>
          <h2 class="mName" v-if="selectData.title == null">{{selectData.name}}</h2>
          

         
           <h2 style="color: gray; font-weight: normal; font-style: italic;" v-if="selectData.release_date != null">{{ selectData.release_date.split("-")[0] }}</h2>
          <h2 style="color: gray; font-weight: normal; font-style: italic;" v-if="selectData.release_date == null">{{ selectData.first_air_date.split("-")[0] }}</h2>

         
        </span>
        <!--uerBox-->
        <div class="userBox" style="margin-bottom: 10px;">
          <div class="UserScore">
                <div class="circle-2 spinner-border text-dark" style="background-color: #20c997;" ></div>
                <div class="circle-2IN bg-primary text-dark" style=" font-size: 20px; font-weight: bold;" >{{ (selectData.vote_average*10).toFixed() }}% </div>
          </div>
          <span class="userScoreText">
            <div>User</div>
            <div>Score</div>
          </span>
          <div class="trailer" @click.prevent="videoClose()">
            <svg xmlns="http://www.w3.org/2000/svg" width="25" height="25" fill="currentColor" class="bi bi-play-circle-fill" viewBox="0 0 16 16">
              <path d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0zM6.79 5.093A.5.5 0 0 0 6 5.5v5a.5.5 0 0 0 .79.407l3.5-2.5a.5.5 0 0 0 0-.814l-3.5-2.5z"/>
            </svg>
            <span>Play Trailer</span>
          </div>
        </div>


        <!--original Title-->

        <h3 class="original_title" v-if="selectData.original_title!=null" >{{ selectData.original_title }}</h3>
        <h3 class="original_title" v-if="selectData.original_title==null">{{ selectData.original_name }}</h3>

        <h3 class="Overview">Overview</h3>
        <p class="pText">
          {{ selectData.overview }}
        </p>
        
          

          
            <div class="credit">
            <div class="creditBox">
            <h3 class="dname">{{ newData.director.name }}</h3>
            <span class="position">Director</span>
          </div>
          <div class="creditBox">
            <h3 class="dname">{{ newData.producer.name }}</h3>
            <span class="position">Producer</span>
          </div>
          </div>
        
        
        
      </div>
    </div>


    <!--Cast-->
    <div class="container " style="margin-top: 100px;">
      <h2 style="margin-bottom: 14px; font-weight: normal; color: lightgrey; font-style: italic;">Casts</h2>
      <div class="castMainBox">


       
         <div class="castCard bg-light text-dark" v-for="(item, index) in newData.cast.slice(0,10)" :key="index" v-show="item.profile_path!=null">
          <img :src="'https://image.tmdb.org/t/p/w138_and_h175_face'+item.profile_path"   style="border-top-left-radius: 10px; border-top-right-radius: 10px;">
          <h5>{{ item.name }}</h5>
          <span style="font-style: italic;">{{ item.character }}</span>
        </div>

        
       

      </div>
      </div>


      <div class="videobox" v-if="isVideoOn">
        <div class="container-md">
          <iframe 
            width="75%" 
            height="50%" 
            :src="'https://www.youtube.com/embed/'+trailer" 
            title="YouTube video player" 
            frameborder="0" 
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
            allowfullscreen>
          </iframe>
        </div>
        <button type="button " class="btn-close btn-close-white" aria-label="Close" style="position: absolute; top: 10px; right: 20px;" @click.prevent="videoClose"></button>
      </div>




    <footer>
      <a class="FooterItem bg-dark" href="" style="text-decoration: none;">
        <span class="text-light">&copy; 2023 Copyright. All right </span><span style="color: #F39C12;">reserved</span>
    </a>
    </footer>

    
  </div>
</template>

<script>
export default {
  props:[
    'selectData',
    'Type'
  ],
  data() {
    return {
      newData:{
        director:{
          name:''
        },
        producer:{
          name:""
        },
        cast:[]
      },
      isVideoOn:false,
      trailer:'',
    }
  },
  methods: {
    getT(){
      let mediaType;
      console.log(this.Type)
      // this method will get trailer data
      if(this.Type == "Movies"){
        mediaType = "movie"
      }else if(this.Type == "TV-Series"){
        mediaType = "tv"
      }else{
        mediaType = this.dataSelect.media_type
      }
      console.log(mediaType)
      let url1 = `https://api.themoviedb.org/3/${mediaType}/${this.selectData.id}/videos?api_key=621ceb5bfc8d936080c6993d5052013f`
      fetch(url1)
        .then(res => res.json())
        .then(json =>this.addDataT(json.results));
      
      
    },
    addDataT(data){
      this.trailer = data[0].key
    },
    videoClose(){
      this.isVideoOn = !this.isVideoOn
      this.getT()
    },
    closeThis(){
      this.$emit('closeSelect')
    },
    getCastData(){

      let mediaType;
  
      // this method will get trailer data
      if(this.Type == "Movies"){
        mediaType = "movie"
      }else if(this.Type == "TV-Series"){
        mediaType = "tv"
      }else{
        mediaType = this.dataSelect.media_type
      }
      
      let url = `https://api.themoviedb.org/3/${mediaType}/${this.selectData.id}/credits?api_key=621ceb5bfc8d936080c6993d5052013f`
      fetch(url)
        .then(res => res.json())
        .then(json =>this.inputData(json))
        .catch(err => console.error('error:' + err));
    
        
    },
    inputData(data){

      for(let i=0; i<data.crew.length;i++){

        if(data.crew[i].job == "Director"){
          this.newData.director = data.crew[i]
        }

        if(data.crew[i].job == "Producer"){
          this.newData.producer = data.crew[i]
        }
      }

      this.newData.cast = data.cast
        
    },
  },
  created(){
    this.dataSelect = this.selectData
    
    this.getCastData();
  

  }
}
</script>

<style>

.videobox{
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: 5;
  display: flex;
  justify-content: center;
  align-items: center;
  background: url('https://www.splitshire.com/wp-content/uploads/2015/03/SplitShire-8098-1800x1200.jpg');
  background-repeat: no-repeat;
  background-size: cover;
}

.ContainerBox{
  height: 370px;
}

.Overview{
  margin-left: 10px;
  font-size: 25px;
}

.original_title{
  margin-left: 10px;
  font-style: italic;
  color: gray;
}

.mName{
  font-size: 30px;
  margin-left: 10px;
}

.castCard{
  width: 140px;
  min-height: 250px;
  display: flex;
  flex-direction: column;
  border: none;
  border-radius: 10px;
  text-align: center;
}

.castMainBox{
  margin-bottom: 100px;
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}

.credit{
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.creditBox{
  display: flex;
  align-items: center;
  gap: 0;
  justify-content: flex-start;
  flex-direction: column;
}

.dname{
  font-size: 15px;
  color: gray;
}

.position{
  font-size: 13px;
  color: gray;
  font-style: italic;
}

.trailer{
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  text-decoration: none;
  transition: 0.4s;
  margin-top: 20px;
}

.trailer:hover{
  color: rgb(255, 195, 32);
}

.pText{
  margin: 10px;
}


.circle-2{
  width: 60px;
  height: 60px;
}

.circle-2IN{
  width: 52px;
  height: 52px;
  border-radius: 50%;
  position: absolute;
  top: 4px;
  left: 4px;
  display: flex;
  justify-content: center;
  align-items: center;

}

.selectItem{
    width: 100vw;
    z-index: 4;
    height: 100vh;
    overflow-y: scroll;
    overflow-x: hidden;
    position: fixed;
    top: 0;
    left: 0;
    
}


.close-btn{
    position: absolute;
    top: 15px;
    right: 40px;
}

.FooterItem{
    width: 100vw;
    height: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 20px;
    position: fixed;
    bottom: 0;
    left: 0;
}

.containt{
  display: flex;
  justify-content: center;
  align-items: flex-start;
  margin-top: 80px;
  gap: 20px;
  
}

.containBox{
  width: 60%;
  min-height: 350px;
  
  position: relative;

}

.userScoreText{
  margin-top: 20px;
}

.UserScore{
  position: relative;
  width: 40px;
  height: 40px;

}

.userBox{
  display: flex;
  width: 300px;
  justify-content: space-around;
  align-items: center;
  margin-left: 10px;
}

.imageBox{
  width: 250px;
}

.nav_box{
  width: 100vw;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 4;
}

@media screen and (max-width: 610px) {
  .imageBox{
  width: 320px;
  border-radius: 10px;
}
  .containt{
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin-top: 70px;
  gap: 20px;
  
}

.containBox{
  width: 90%;
  min-height: 350px;
  
  position: relative;
}

.close-btn{
    top: 20px;
    right: 15px;
}

.castMainBox{
  display: flex;
  align-items: center;
  justify-content: center;
}

}

</style>