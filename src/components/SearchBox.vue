<template>
  <div class="searchBox">
    <!--Nav bar for the search box-->
    <div class="searchNav">
        <nav class="navbar  navbar-dark bg-dark">
          <div class="container-fluid">
            <form class="d-flex" @submit.prevent="getSearchData(searchInput)">
              <input class="form-control input" type="search" placeholder="Search" aria-label="Search" v-model="searchInput">
              <button class="btn btn-success" type="submit" onclick="window.location='#mainBox';" >Search</button>
            </form>
          </div>
        </nav>
        <span class="closeIcon" @click="close()">
            <button type="button" class="btn-close btn-close-white" aria-label="Close"></button>
        </span>
    </div>
    <!---->
    <div class="mainDataBox" id="mainBox">
        <img class="mainBoximg" :src="'https://image.tmdb.org/t/p/w300_and_h450_bestv2'+dataBox.mainImgURL">
        <div class="mini-box2">
            <span class="headerBox">
                <h1 class="text-light" >{{ mainTitle }}</h1>
                <h1 style="font-weight: normal; font-size: 30px; color: lightgray;">{{ dataBox.year.split("-")[0] }}</h1>
            </span>
           <!-- 
            <div class="date">{{ dataBox.year.split("-")[0]+"/"+dataBox.year.split("-")[1]+"/"+dataBox.year.split("-")[2] }}</div> 
            -->
            <span class="date" style="left: 140px;">{{  }}</span> 
            <div class="spBox">
                <div class="circle spinner-border" style="background-color: #20c997; top: 100px; left: 0px;" ></div>
                <div class="circleIN bg-primary text-dark" style="top: 104px; left: 4px; font-size: 25px;" >{{ dataBox.score }}% </div>
            </div>
            <p class="score text-light" style="position: absolute; font-size: 17px; font-weight: bold; top: 109px; left: 76px;">
                User <br> Score
            </p>
            <h1 class="language">{{ dataBox.media }}</h1>
        

            <h4 class="originalName">
                {{ dataBox.full_title }}
            </h4>

            <h5 class="overview text-light">
                Overview
            </h5>

            <p class="Ptext text-light">
                {{ dataBox.full_text }}
            </p>


        </div>
    </div>

    <div class="listBox">

        <ul class="ul">

            <li class="li bg-light" style="list-style: none;"  v-for="(item, index) in jsonDataSearch" :key="index" @click="clickLI(index)" v-show="item.poster_path != null" onclick="window.location='#mainBox';">
                
                
                    <a href="#mainBox">
                        <img :src="'https://www.themoviedb.org/t/p/w300_and_h450_bestv2'+ item.poster_path" v-if="item.poster_path != null" alt="" class="liimg">
                        <img :src="'https://www.themoviedb.org/t/p/w300_and_h450_bestv2'+ item.backdrop_path" v-if="item.poster_path == null" alt="" class="liimg">
                    </a>
                    <div class="liText">
                        <div class="liHeaderbox">
                            <!---->
                            <h1 class="liheader" v-if="item.title != null" style="font-size: 20px;">{{ item.title }} <span style="font-size: 15px; color: gray;">{{ item.media_type }}</span></h1>
                            <h1 class="liheader" v-if="item.title == null" style="font-size: 20px;">{{ item.name }} <span style="font-size: 15px; color: gray;">{{ item.media_type }}</span></h1>
                            <!---->
                            
                            <h2 class="liYear" style="font-size: 18px;" v-if="item.media_type == 'movie'">{{ item.release_date.replace("-","/")}} </h2>
                            <h2 class="liYear" style="font-size: 18px;" v-if="item.media_type == 'tv'">{{ item.first_air_date.replace("-","/")}}</h2>
                            
                        </div>
                    

                            <p class="p" style="font-size: 15px;">
                            {{(item.overview+".").split(".")[0]+"...." }}
                        </p>

                        
                    </div>                    
                
            
            </li>

        </ul>

    </div>




    
    
    <div class="container-fluid searchFooter bg-dark pt-3 fs-5 pb-3" style="height: 50px; text-align: center; " >
    <span class="text-light">&copy; 2023 Copyright. All right </span><span style="color: #F39C12;">reserved</span>
  </div>
  </div>

</template>

<script>





export default {
    props:[
        'input'
    ],
    data() {
        return {
            jsonDataSearch:{},
            searchInput:'',
            mainTitle:'',
            original_title:'',
            mainImgURL:'',
            dataBox:{
                year:'',
                score:0,
                mainImgURL:'',
                media:"",
                full_title:"",
                full_text:""
                
            }
        }
    },
    methods:{
        getSearchData(inputItem){
        let search;


        search = inputItem+" ".replace(" ", "%20")
        this.searchInput = ""
        //search = "john wick".replace(" ", "%20")
 
        let getUrl =`https://api.themoviedb.org/3/search/multi?query=${search}&include_adult=false&api_key=621ceb5bfc8d936080c6993d5052013f&language=en-US&page=1`


        fetch(getUrl)
            .then(res => res.json())
            .then(json => {
                this.jsonDataSearch = json.results
                console.log(json.results)
               
               
                /*
                if(json.results[0].poster_path != null){
                //this.mainImgURL = json.results[0].poster_path
                   this.dataBox.mainImgURL = json.results[0].poster_path
                }else{
                //this.mainImgURL = json.results[0].backdrop_path
                    this.dataBox.mainImgURL = json.results[0].backdrop_path
                }

                if(json.results[0].release_date != null){
                   this.dataBox.year = json.results[0].release_date 

                }else{
                    this.dataBox.year = json.results[0].first_air_date
                }
                
                */
                
                

                this.addInfo(json.results);

                if(json.results[0].title != null){
                    this.mainTitle = json.results[0].title
                }else{
                    this.mainTitle = json.results[0].name   
                }
               
               
              
                
            })
            .catch(err => console.error('error:' + err));
 
            
    },
    addInfo(MainData){

        
        
        if(MainData[0].poster_path != null){
            //this.mainImgURL = json.results[0].poster_path
            this.dataBox.mainImgURL = MainData[0].poster_path
        }else{
            //this.mainImgURL = json.results[0].backdrop_path
            this.dataBox.mainImgURL = MainData[0].backdrop_path
        }

        
        this.dataBox.media = MainData[0].media_type

        this.dataBox.full_text = MainData[0].overview
        

        if(MainData[0].release_date != null){
            this.dataBox.year = MainData[0].release_date 
                       
        }else{
            this.dataBox.year = MainData[0].first_air_date
        }

        this.dataBox.score = (MainData[0].vote_average*10).toFixed()
        
        
        
        
    },
    clickLI(index){
        
        // image url
        if(this.jsonDataSearch[index].poster_path != null){
            this.dataBox.mainImgURL = this.jsonDataSearch[index].poster_path
        }else{
            this.dataBox.mainImgURL = this.jsonDataSearch[index].backdrop_path
        }
        // date
        if(this.jsonDataSearch[index].release_date != null){
            this.dataBox.year = this.jsonDataSearch[index].release_date
        }else{
            this.dataBox.year = this.jsonDataSearch[index].first_air_date
        }

        //title

        if(this.jsonDataSearch[index].title != null){
            this.mainTitle = this.jsonDataSearch[index].title
            this.dataBox.full_title = this.jsonDataSearch[index].original_title
        }else{
            this.mainTitle = this.jsonDataSearch[index].name
            this.dataBox.full_title = this.jsonDataSearch[index].original_name
        }

        // text 
        this.dataBox.full_text = this.jsonDataSearch[index].overview
        
        this.dataBox.score = (this.jsonDataSearch[index].vote_average*10).toFixed()

        // media type

        this.dataBox.media = this.jsonDataSearch[index].media_type

    },
    
    close(){
        this.$emit('close');
    }},
    created() {
        this.mainTitle = this.input;
       this.getSearchData(this.input);
    }
    
    
    
    }
</script>

<style>



.searchNav{
    width: 100vw;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 5;
}



.liHeaderbox{
    display: flex;
    justify-content: center;
    flex-direction: column;
}

.liText{
    display: flex;
    justify-content: space-around;
    align-items:start;
    flex-direction: column;

}

.li{
    width: 70%;
    min-height: 150px;
    text-decoration: none;
    display: flex;
    gap: 30px;
    border-radius: 10px;
    -webkit-box-shadow: 2px 3px 11px -1px rgba(0,0,0,0.75);
    -moz-box-shadow: 2px 3px 11px -1px rgba(0,0,0,0.75);
    box-shadow: 2px 3px 11px -1px rgba(0,0,0,0.75);
    cursor: pointer;
}

.ul{
    display: flex;
    justify-content: center;
    align-items: center;
    padding-top: 30px;
    flex-direction: column;
    gap: 30px;
    
    
}

.liimg{
    width: 100px;
    border-top-left-radius: 10px;
    border-bottom-left-radius: 10px;
}

.Ptext{
    font-size: 15px;
    position: absolute;
    top: 250px;
    left: 10px;
}

.overview{
    position: absolute;
    top: 220px;
    left: 10px;
    font-size: 20px;
    font-style: italic;
        
}

.originalName{
    position: absolute;
    font-weight: normal;
    top: 190px;
    left: 10px;
    font-size: 20px;
    font-style: italic;
    color: rgb(141, 141, 141);
}

.language{
 
    position: absolute;
    top: 120px;
    left: 150px;
    font-size: 25px;
    font-style: italic;
    cursor: pointer;
    color: gray;
}



.headerBox{
    display: flex;
    gap: 10px;
    align-items: baseline;
    position: absolute;
    top: 0;
    left: 10px;
}

.date{
    position: absolute;
    top: 50px;
    left: 15px;
    font-size: 20px;
    font-weight: bold;
}



.mini-box2{
    width: 60%;
    height: 450px;
    position: relative;
    
}

.mainBoximg{
    max-width: 320px;
    border-radius:10px ;
}

.mainDataBox{
    width: 100vw;
    min-height: 500px;
    background-color: lightgray;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 20px;
    background-image: url("https://www.splitshire.com/wp-content/uploads/2015/03/SplitShire-8098-1800x1200.jpg");
    background-repeat: no-repeat;
    background-size: cover;
    margin-top: 50px;
    padding-top: 3px;
    
}

.closeIcon{
    position: absolute;
    top: 15px;
    right: 20px;
}

.closeIcon:hover{
    color: gray;
}

.searchBox{
    z-index: 4;
    width: 100vw;
    height: 100vh;
    overflow-x: hidden;
    overflow-y: scroll;
    position: fixed;
    top: 0;
    bottom: 0;
    background-color: whitesmoke;
    scroll-behavior: smooth;
}

.input{
    margin-right: 10px;
}


@media screen and (max-width: 700px) {
    .li{
        width: 90vw;
    }

    .liimg{
        width: 150px;
        
    }

    .mainBoximg{
        width: 450px;
    }

    .date{
        top: 72px;
    }

    .mainDataBox{
        flex-direction: column;
        padding-top: 50px;
        padding-bottom: 10px;
        height: auto;
    }
    .mini-box2{
        width: 90%;
    }
}

@media screen and (max-width: 500px) {
    .li{
        width: 90vw;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        padding: 10px;
    }

    .liimg{
        width: 300px;
        border-radius: 10px;
        
    }

    .p{
        width: 90%;
    }

    .ul{
        padding-left: 0;
    }

    .mainDataBox{
        flex-direction: column;
        padding-top: 50px;
        padding-bottom: 10px;
        min-height: 300px;
    }

}

@media screen and (max-width: 440px) {
    

    .p{
        width: 90%;
    }

    .ul{
        padding-left: 0;
    }

    .mainDataBox{
        flex-direction: column;
        padding-top: 30px;
        padding-bottom: 10px;
        height: auto;
    }

    .mini-box2{
        height: 550px;
    }

    .input{
        width: 200px;
    }

}

</style>