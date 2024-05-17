<template>
  <div class="home">
      <h1>Home</h1>
      <FilterNav @filterValue="current=$event" :current="current"></FilterNav>
      <div v-for="project in filteredProjects" :key="project.id">
          <Singlepj :project="project" @delete="deletePj" @complete="completeProject"></Singlepj>
      </div>
  </div>
</template>

<script>
import FilterNav from '../components/FilterNav'
import Singlepj from '../components/Singlepj'
export default {
  components: {
    FilterNav, 
      Singlepj 
  },
  data(){
    return{
      projects:[],//three projects
      current:"all"
    }
  },
  computed:{
    filteredProjects(){
     if(this.current==="complete"){
      return this.projects.filter((p)=>{
        return p.complete
      })
     }
     if(this.current==="ongoing"){
      return this.projects.filter((p)=>{
        return !p.complete
      })
     }
     return this.projects;
    }
  },
 mounted(){
  fetch("http://localhost:3000/projects")
  .then((response)=>{
     return response.json()
  })
  .then((data)=>{
    this.projects=data
  })
  .catch(()=>{

  })
 },
 methods:{
  deletePj(delId){
    this.projects = this.projects.filter((project)=>{
      return project.id!= delId;
    })
  },
  completeProject(id){
    let findProject = this.projects.find(project=>{
      return project.id === id;
    })
    findProject.complete =! findProject.complete
  }
 }
}
</script>
