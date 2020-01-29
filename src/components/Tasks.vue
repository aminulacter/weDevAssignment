<template>
  <div>
    <div class="headline">
      <h1>todos</h1>
    </div>
     <div class="container" :class="{containerbottom: tasklist.length == 0}">
   
        <div class="taskinput">
          <template >
              <svg xmlns="http://www.w3.org/2000/svg" height="60" width="60">
                <g fill="none" >
                
                <polyline class="st0" :stroke="strokeColor" stroke-width="02" points="15.5,23.4, 26.7, 30.9, 37.5, 23.4" />
                </g>  
            </svg>
          </template>
        
          <input type="text" placeholder="What needs to be done?" v-model="newTask" @keydown.enter="addList">
          
        </div>
        <template v-if="tasklist.length">
            <div class="tasks">
            <single-task v-for="(task,index) in selectedList" :key="index" 
            :task = task @complete="completeTask" @changename= "changeName(task.name, $event)" > </single-task>
          </div>
          
          <div class="footer">
              <div class="footerContent">
                  
                <div>
                  {{ activeItemString }} 
                </div>
                <div class="activity">
                  <button :class="{selectedButton: allselected}" @click="showAll" >all</button>
                  <button :class="{selectedButton: activeSelected}" @click="showActive">active</button>
                  <button :class="{selectedButton: completedSelected}" @click="showCompleted">completed</button>
                </div>
                <div >
                  <!-- <a   @click.prevent="removeCompleted" v-if="existsComplete">Remove completed</a> -->
                  <label @click.prevent="removeCompleted" v-if="existsComplete">Remove completed</label>
                </div>
              
                </div>
                <div class="shade1"></div>
                <div class="shade2"></div>
            </div>
        </template> 
   
       </div>

    </div>
 
</template>

<script>
import SingleTask from "./SingleTask"
export default {
  name: 'Tasks',
  components:{
    SingleTask
    },
  props: {
    msg: String
  },
  data()
  {
    return{
      tasklist: [],
      newTask: '',
      selectedActive: false,
      selecedCompleted: false
    }
  },
 computed:{
   selectedList(){
     if(this.selectedActive == this.selecedCompleted)
     {
       return this.tasklist
     }
     if(this.selectedActive){
      
       return this.tasklist.filter(task => task.completed == false)
     }
     else{
       return this.tasklist.filter(task => task.completed == true)
     }
   },
   activeLength()
   {
      return this.tasklist.filter(task => task.completed == false).length
   },
   activeItemString()
   {
     return this.activeLength == 1 ? this.activeLength + " item left" : this.activeLength + " items left"
   },
   existsComplete()
   {
     return this.tasklist.filter(task => task.completed == true).length > 0
   },

   strokeColor()
   {
     return this.tasklist.length ? '#d9d9d9' : '#fff' 
   },
   allselected()
   {
     return this.selectedActive == this.selecedCompleted;
   },
   activeSelected()
   {
     return this.selectedActive != this.selecedCompleted && this.selectedActive == true
   },
   completedSelected()
   {
     return this.selectedActive != this.selecedCompleted && this.selecedCompleted == true
   }

   
 },
  methods: {
    addList()
    {
     if (this.newTask.length == 0 ) {
       return
     }
     let addtask = {}
      addtask.name = this.newTask;
     
      addtask.completed = false;
      this.tasklist.push(addtask);
      this.newTask = ''
    },
    showCompleted()
    {
      this.selectedActive= false
      this.selecedCompleted= true
    },
    showActive()
    {
      this.selectedActive= true
      this.selecedCompleted= false
    },
    showAll()
    {
      this.selectedActive= true
      this.selecedCompleted= true
    },
    completeTask($event)
    {
     let index = this.tasklist.findIndex(task => task.name == $event) 
     
      this.tasklist[index].completed = true
     
    },
    changeName(name, $event){
      let index = this.tasklist.findIndex(task => task.name == name) 
     
      this.tasklist[index].name = $event
    },
    removeCompleted()
    {
        this.tasklist = [...this.tasklist.filter(task => task.completed == false)]
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.container {
  display: flex;
  flex-flow: column;
  
  width: 600px;
  align-items: flex-start;


   border-top: no;
   border-right: 2px solid #e6efe5;
  // border-bottom: 1px solid #e6efe5;
   border-left: 1px solid #e6efe5;
  // border: 1px solid #e6efe5;
   padding-left: 30px;
   padding-bottom: 30px;
   padding-right: 30px;
   background: #fff;
}
.containerbottom
{
  border-bottom: 1px solid #e6efe5;
  padding-bottom: 0px;
  
  box-shadow: 0px 2px lightgrey;
}
.taskinput{
  padding-left: -30px;
  margin-left: -25px;
  padding-bottom: 30px;
  display: flex;
  flex: 1;
  width: 100%;
  input{
   border: 0px;
   flex-grow: 1;
   font-size: 25px;
   
 }
  input:focus {
    outline: none;
  }
  
}
::placeholder {
  
  opacity: .2;
  font-size: 25px;
}
 .tasks{
    display: flex;
    flex-flow: column;
    margin-left: -25px;
    padding-left: 0px;
 }
 .footer{
  // padding-left: -90px;
   padding-top: 20px;
   width: 100%;
   position: relative;
   display: flex;
    margin: 0px;
   
   flex-flow: column;
   opacity: .5;
  
  
 }
  .footerContent{
    position: absolute; top: 0; 
    display: flex;
    justify-content: space-around;
    align-items: center;
    background: white;
    flex-flow: row;
    
    z-index: 6;
    padding-left: 0px;
    margin-left: -30px;
    width: 660px;
    height: 60px;
    //border-radius: 15px;
    border: 1px solid #9facb6;
 }
 .shade1{
   position: absolute;
    z-index: 5;
    left: -25px;
    height: 47px;
    width: 650px;
    background: white;
    border-radius: 5px;
    border-bottom: 1px solid #9facb6;
    border-left: 1px solid #9facb6;
    border-right: 1px solid #9facb6;
    

 }
 .shade2{
    position: absolute; top: 0; 
    z-index: -1;
    // background: blue;
    width: 630px;
    height: 74px;
    left: -15px;
   border-radius: 5px;
    border-bottom: 1px solid #9facb6;
    border-left: 1px solid #9facb6;
    border-right: 1px solid #9facb6;
   // border-left: 1px solid #9facb6;
 }
 h1{
   display: block;
   font-size: 80px;
   color: #ffbf80;
   opacity: .5;
   font-weight: 200;
   
 }
 .activity{
   display: flex;
   width: 30%;
   justify-content: space-between;
   flex-flow: row;
  
   button{
     border: none;
     background:white;
     cursor: pointer;
   }
   button:focus {
     border: 1px solid #8c8c8c;
     outline: none;
   }
   .selectedButton{
   border: 1px solid #8c8c8c;
   outline: none;
  }
 }
 
 label{
   text-decoration: none;
   color: black;
 }
 label:hover{
   text-decoration: underline;
    cursor: pointer;
 }
</style>
