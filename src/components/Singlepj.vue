<template>
  <div class="project" :class="{complete:project.complete}">
    <div class="flexing">
        <div>
            <h3 @click="showDetail =! showDetail">{{project.title}}</h3>
        </div>
        <div>
        <span class="material-symbols-outlined" @click="deleteProject">
            delete
        </span>
        <router-link :to="{name:'EditProject',params:{id:project.id}}">
            <span class="material-symbols-outlined">
        edit
        </span>
        </router-link>
        <span class="material-symbols-outlined" @click="completeProject">
        done
        </span>
        </div>
    </div>
    <hr>
    <p v-if="showDetail">{{project.detail}}</p>
    {{project.complete}}
  </div>
</template>

<script>
export default {
    data(){
        return{
            showDetail:false,
            api:"http://localhost:3000/projects/"
        }
    },
    props:[
        "project"
    ],
    methods:{
        deleteProject(){
            let deleteRoute = this.api+this.project.id;
            fetch(deleteRoute,{method:"DELETE"})
            .then(()=>{
                this.$emit("delete",this.project.id)
            })
        },
        completeProject(){
            let updateCompleteRoute= this.api+this.project.id;
            fetch(updateCompleteRoute,
                {method:"PATCH",
                headers:{
                    "Content-Type":"application/json"
                },
                body:JSON.stringify(
                        {
                        complete:!this.project.complete
                        }
                    ) 
                }
            )
          .then(()=>{
                this.$emit("complete",this.project.id)
            })
            .catch((err)=>{
                console.log(err)
            })
        }
    }
}  
</script>

<style>
.project{
    padding: 20px;
    background-color: #13024b;
    margin: 10px;
    border-radius: 5px;
    box-shadow: 8px 5px 10px 0px rgba(0,0,0,0.5);
    border-left: 6px solid crimson;
    color: white;
}
h3{
    color:rgb(247, 242, 250);
    font-weight: bold;
    cursor: pointer;
}
p{
    color: white;
}
.material-symbols-outlined{
    color:white;
}
.flexing{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
span{
    margin-left: 10px;
    user-select: none;
}
span:hover{
    color: #777;
    cursor: pointer;
}
.complete{
    border-left-color: greenyellow;
}
</style>