<template>
  <NavBar  @searchNow="search" @selectItem="SelectedItem" @sendType="Type"></NavBar>
  <About></About>
  <SearchBox :input="inputSearch" v-if="isSearchClick" @close="close"></SearchBox>
  <selectItem v-if="isSelect" :selectData="selectData" @closeSelect="closeSelect" :Type="type"></selectItem>
  <Login></Login>
</template>

<script>
import NavBar from './components/NavBar.vue'
import About from './components/About.vue'
import SearchBox from './components/SearchBox.vue'
import selectItem from './components/selectItem.vue'
import Login from './components/Login.vue'




export default {
  name: 'App',
  components: {
    NavBar,
    About,
    SearchBox,
    selectItem,
    Login
  },
  data() {
    return {
      isSearchClick:false,
      inputSearch:'',
      isSelect:false,
      selectData:undefined,
      type:'',
    }
  },
  methods: {
    Type(data){
      this.type = data
      
    },
    SelectedItem(data){
      this.selectData = data
      this.isSelect = true
    },
    search(searchInput){
        this.isSearchClick = true;
        this.inputSearch = searchInput;
    },
    close(){
      this.isSearchClick = false
    },
    closeSelect(){
      this.isSelect = false;
    } 
  },
}
</script>

<style>

body{
  overflow-x: hidden;
}

html{
  scroll-behavior: smooth;
}
</style>
