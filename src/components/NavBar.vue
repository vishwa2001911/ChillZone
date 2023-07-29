<template>
  <nav class="navbar navbar-expand-lg  bg-dark top-0 start-0 w-100 "  data-bs-theme="dark">
    <div class="container-fluid">
      <a class="navbar-brand text-light fw-bold" href="#">Chill<span class="text-warning">Zone</span> </a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0" >
            <li class="nav-item " v-for="(item, index) in pages" :key="index" onclick="window.location='#main'">
              <a class="nav-link text-light"  :class="{ active:isActive }" :href="'#'+item.href" @click.prevent="nacClick(index)" >{{ item.name }}</a>
            </li>
          </ul>
            <!-- <button class="btn btn-primary" type="submit" >Login</button> -->
        </div>
      </div>
  </nav>
<!--Search Section-->

    <div class="Container">
      <div class="container d-flex justify-content-center align-items-center flex-column " >
        <h1 class="text-warning">Find your favorite show</h1>
        <form class="d-flex" style="gap: 5px;" role="Search" @submit.prevent="Search()">
          <input class="form-control sm-w-5" type="search" style="width: 230px;" placeholder="Search" aria-label="Search" v-model="searchInput">
          <button class="btn btn-primary" type="submit">Search</button>
        </form>
      </div>  
    </div>



<!--Main Section-->

  <div class="container mt-5" data-bs-theme="dark" id="main">
        <div class="d-flex justify-content-between align-items-center fs-4 ml-3 text-light bg-dark  " >
          <span style="border-left: solid #0d6efd 3px; padding: 0 0 0 5px;" >{{ pageName }}</span>
          <div  style="width: 90px; display: flex; justify-content: space-between; align-items: center;">
            <button type="button" class="btn btn-primary p-1" style=" font-size: 13px;">see all..</button>
          </div>
        </div>
  <div class="container-fluid mt-5">

      <div class="container-fluid d-flex flex-wrap justify" >

          <!---->

          <div class="m-3 position-relative movieBox" style="" v-for="(item, index) in jsonDataTrending" :key="index" v-show="(item.vote_average*10).toFixed()!= 0" @click.prevent="ClickItemTrend(index)" >
            <img :src="'https://www.themoviedb.org/t/p/w600_and_h900_bestv2/'+item.poster_path" class="card-img-top" alt="..." style="border-radius: 10px;">
            <p class=""><span v-if="item.name == null" class="text-light">{{ item.title }}</span> 
              <span v-if="item.title == null" class="text-light">{{ item.name }}</span> <br>  

              <span v-if="item.first_air_date ==null" class="text-secondary" style="font-size: 14px; text-decoration: none;">{{ (item.release_date) }}</span> 
              <span v-if="item.release_date == null" class="text-secondary" style="font-size: 14px; text-decoration: none;">{{ item.first_air_date }}
              </span></p>

            <div class="circle spinner-border" style="background-color: #20c997;"></div>
            <div class="circleIN bg-primary text-dark" style="font-size: 15px;" >{{ (item.vote_average*10).toFixed(1) }}% </div>
          </div>

          <!---->
      </div>
  </div>
  <div class="box">
    <nav aria-label="..." style="float: right;">
      <ul class="pagination pagination-sm">
        <li class="page-item " @click="clickPageCountTrend(index)" :class="{'active':indexNumTrend==index}" aria-current="page" style="cursor: pointer;" v-for="(item, index) in [1,2,3]" :key="index"><span class="page-link">{{ item }}</span></li>
      </ul>
    </nav>
  </div>
</div>



<!--Find similer Shows-->

  <div class="Container2"  id="similer">
  <div class="container d-flex justify-content-center align-items-center flex-column " >
    <h1 class="text-warning">Find Similer Movies</h1>
    <h4 class="text-light">Search heare</h4>
    <form class="d-flex" role="Search" style="gap:10px;" @submit.prevent="similerSearchNow()" target="#similer">
      <input class="form-control" type="search" style="width: 250px;" placeholder="Search" aria-label="Search" v-model="similerSearch.searchInput">
      <a href="#similerShow"><button class="btn btn-primary" type="submit">Search</button></a>
    </form>
  </div>  
</div >



<!--Similer results-->

<div v-show="similerSearch.isSimilerActive" >
  <div id="similer" class="container-fluid m-3" style="display:flex; justify-content: center; align-items: center; width: 100vw; min-height: 100vh;">
  
  <div class="w-100 " style="display:flex; justify-content: center; align-items: center; flex-direction: column;  width: 100%; min-height: auto;">






    <div class="container d-flex justify-content-between align-items-center fs-4 ml-3 text-light bg-dark "  >
          <span style="border-left: solid #0d6efd 3px; padding: 0 0 0 5px;" >Similer to {{ similerSearch.searchInput }}</span>
          <div  style="width: 80px; display: flex; justify-content: space-between; align-items: center;">
            
            <button type="button" class="btn btn-primary p-1" style=" font-size: 13px;">see all..</button>
          </div>
        </div>
    <!--
      <div class="container m-3 d-flex  "  style="justify-content: space-between; align-items: center; margin-right: 10px;"> 


    <span class="text-light fs-3" style="border-left:solid #0d6efd; padding-left: 3px;">Similer to eee eee</span>
    <div  style="width: 90px; display: flex; justify-content: space-between; align-items: center;">
      <span class=" text-secondary" style="font-size: 14px;">lenth</span>
      <button type="button" class="btn btn-primary p-1" style=" font-size: 13px;">see all..</button>
    </div>
  </div>
    -->
    <div class="container d-flex flex-wrap justify" style="align-items: center;">
      
          <div class="m-5 position-relative movieBox" v-for="(item, index) in this.similerSearch.searchResults" :key="index" v-show="item.poster_path != null">
            <img :src="'https://www.themoviedb.org/t/p/w600_and_h900_bestv2/'+item.poster_path"  class="card-img-top" alt="..."  style="border-radius: 10px;">
            <p class=""><span v-if="item.name == null" class="text-light">{{ item.title }}</span> 
              <span v-if="item.title == null" class="text-light">{{ item.name }}</span> <br>  

              <span v-if="item.first_air_date ==null" class="text-secondary" style="font-size: 14px; text-decoration: none;">{{ (item.release_date) }}</span> 
              <span v-if="item.release_date == null" class="text-secondary" style="font-size: 14px; text-decoration: none;">{{ item.first_air_date }}
              </span></p>

            <div class="circle spinner-border" style="background-color: #20c997;"></div>
            <div class="circleIN bg-primary text-dark" >{{ (item.vote_average*10).toFixed(1) }}%</div>
          </div>
    </div>
</div>
</div>
<div id="similerShow"></div>

<hr style="margin-left: 5%; color: white;" width="90%">
</div>


<!---->
</template>

<script>


export default {

    data() {
        return {
            searchInput:'',
            isActive:true,
            indexNumUp:0,
            indexNumTrend:0,
            pages:[
                {
                    name:"Trending",
                    url:"https://api.themoviedb.org/3/trending/all/day?api_key=621ceb5bfc8d936080c6993d5052013f&language=en-US-US&page=",
                    href:""
                },{
                    name:"Movies",
                    url:"https://api.themoviedb.org/3/movie/popular?api_key=621ceb5bfc8d936080c6993d5052013f&language=en-US&page=",
                    href:""
                },{
                    name:"TV-Series",
                    url:"https://api.themoviedb.org/3/tv/popular?api_key=621ceb5bfc8d936080c6993d5052013f&language=en-US&page=",
                    href:"main"
                },
                
            ],
            pageName:"Trending",
            index:0,
            homeURL:'https://api.themoviedb.org/3/trending/all/day?api_key=621ceb5bfc8d936080c6993d5052013f&language=en-US-US&page=',
            jsonDataTrending:{},
            jsonDataUpcomming:{},
            similerSearch:{
              searchInput:'',
              searchResults:[],
              isSimilerActive:false,
              movie_id:{}
            },
            showThis:false,
        }
    },
    methods: {
      ClickItemTrend(index){
        this.$emit('selectItem',this.jsonDataTrending[index])
      },
      sendType(){
        
      },
      ClickItemUp(index){ 
        /*
        this.selectData = this.jsonDataUpcomming[index]
        this.selectData.media_type = "movie"
        console.log(this.selectData)
        */
        this.$emit('selectItem',this.jsonDataUpcomming[index])
        
      },
      similerSearchNow(){


       
      
        // Get Search results
        //const url1 =`https://api.themoviedb.org/3/search/movie?query=Jack+Reacher&api_key=621ceb5bfc8d936080c6993d5052013f`

        //        https://api.themoviedb.org/3/search/movie?query=pitets%20of%20the&include_adult=false&language=en-US&page=1


        /*
        fetch(url)
                .then(res => res.json())
                .then(json =>  this.similerSearch.searchResults = json.results)
                .catch(err => console.error('error:' + err));
        
        */

        this.similerSearch.searchResults.length  = 0
        
        this.similerSearch.isSimilerActive = false

        let search = this.similerSearch.searchInput+" ".replace(" ", "%20")

        

        let url =`https://api.themoviedb.org/3/search/movie?query=${search}&include_adult=false&api_key=621ceb5bfc8d936080c6993d5052013f&language=en-US&page=1`

        //let getUrl = `https://api.themoviedb.org/3/movie/439079/similar?api_key=621ceb5bfc8d936080c6993d5052013f&language=en-US&page=1`
        
        
          fetch(url)
                .then(res => res.json())
                .then(json => {
                  this.similerSearch.movie_id = json.results[0].id

                  let getUrl = `https://api.themoviedb.org/3/movie/${this.similerSearch.movie_id}/similar?api_key=621ceb5bfc8d936080c6993d5052013f&language=en-US&page=1`

                  fetch(getUrl)
                      .then(res => res.json())
                      .then(json =>   this.similerSearch.searchResults = json.results)
                      .catch(err => console.error('error:' + err));


                } )
                .catch(err => console.error('error:' + err));


          /* 
          
          fetch(getUrl)
                .then(res => res.json())
                .then(json =>   this.similerSearch.searchResults = json.results)
                .catch(err => console.error('error:' + err));
          
          */      

          if(true){
            setTimeout(()=>{
            this.similerSearch.isSimilerActive = true
          }, 3000)
          }else{
            this.showThis = true
          }
        
      },


      clickPageCountUp(index){
        this.indexNumUp = index

        this.getUpcommingData();
      },
      clickPageCountTrend(index){
        this.indexNumTrend = index

        this.getData(this.homeURL,this.indexNumTrend)
      },
        nacClick(index){
            this.pageName = this.pages[index].name
            this.index = index
            this.getData(this.pages[index].url ,index)

            let x = this.pageName

            this.$emit('sendType', x)
            

            
        },

        getData(url,index){
          const pageNum = index+1;
          fetch(url+pageNum)
                .then(res => res.json())
                .then(json => this.jsonDataTrending = json.results)
                .catch(err => console.error('error:' + err));

                

          this.homeURL = url
        },
        getUpcommingData(){
          const url = "https://api.themoviedb.org/3/movie/upcoming?api_key=621ceb5bfc8d936080c6993d5052013f&language=en-US&page="
          fetch(url+(this.indexNumUp+1))
                .then(res => res.json())
                .then(json => {
                  this.jsonDataUpcomming = json.results
                  
                })
                .catch(err => console.error('error:' + err));

               

                
        },
        Search(){
         // if(this.searchInput!=''){
            this.$emit('searchNow',this.searchInput);
            
          //}
        }
    },
    beforeMount(){
        if(this.pageName=this.pages[this.index].name){
                this.isActive = true
        }else{
            this.isActive = true
        }

        this.getData(this.homeURL,this.indexNumTrend);
      
    }
}
</script>

<style>


.movieBox{
  width: 10rem;
  cursor: pointer;
}

@media screen and (max-width:500px) {
  .movieBox{
  width: 20rem;
}
}

.Container{

  width: 100vw;
  height: 80vh;
  background-image: url('https://images2.minutemediacdn.com/image/fetch/w_2000,h_2000,c_fit/https%3A%2F%2Fhiddenremote.com%2Ffiles%2Fimage-exchange%2F2018%2F08%2Fie_13173.jpeg');
  background-size: cover;
  background-repeat: no-repeat;
  display: flex;
  align-items: center;
  justify-content: center;
}

.Container2{

width: 100vw;
height: 100vh;
background-image: url('https://pyxis.nymag.com/v1/imgs/6e7/5ac/f28dc9f0f0709b7c3646ee9aebe1bdb1b3-07-stanger-things-hawkins.rsocial.w1200.jpg');
background-size: cover;
background-repeat: no-repeat;
display: flex;
align-items: center;
justify-content: center;
}

.circle{
  position: absolute;
  top: -18px;
  right: -11px;
  width: 65px;
  height: 65px;
  border-radius: 50%;
  
  z-index: 2;
}

.circleIN{
  width: 57px;
  height: 57px;
  border-radius: 50%;
  z-index: 3;
  position: absolute; 
  top: -14px;
  right: -7px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
}

@media screen and (max-width: 450px) {
  .Container{
    
    height: 25vh;
    
    border-bottom-right-radius: 30px;
    border-bottom-left-radius: 30px;
    margin-left: 5px;
  }
  .Container2{
    height: 30vh;

  }
}




</style>