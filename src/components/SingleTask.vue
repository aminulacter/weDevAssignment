<template>
<div>
         <div v-if="editmode" class="editText">
            <input type="text" v-model="name" @blur="editName" @keypress.enter="editName" ref="search">
        </div>
        
        <div class="task"  @mouseover="showDelete=true" @mouseleave="showDelete=false" v-else>
            <div class="svgContainer">
            
                <svg xmlns="http://www.w3.org/2000/svg" height="60" width="60" @click="completeTask">
                    <g fill="none" >
                    <circle cx="30" cy="30" r="20" :stroke="circleColor" stroke-width="3" fill="white" />
                    <polyline class="st0" :stroke="tickColor" stroke-width="02" points="20.5,35.8 27.7,42.9 38.2,23.4 " />
                    </g>  
                </svg>
                <div class="inputholder">
                    
                    <span :class="strikeThrough" @click="setfocus">
                        {{ task.name }}
                    </span>
                </div>
        
            </div>
            <div class="cross" v-if="showDelete">
                <svg xmlns="http://www.w3.org/2000/svg" height="60" width="60">
            
                    <line x1="22.5" y1="25.8" x2="35.2" y2="37.9" style="stroke:#ffb380; stroke-width:2" />
                    <line x1="35.2" y1="25.8" x2="22.5" y2="37.9" style="stroke:#ffb380; stroke-width:2" />
                
                </svg>
            </div>
        </div>
       

</div>

    
   
</template>

<script>
    
    export default {
        props: ['task'],
       
        data()
        {
            return{
                showEdit: false,
                name: this.task.name,
                editmode: false,
                showDelete: false
            }
        },
        computed:{
            tickColor()
            {
               return this.task.completed ? "#22AE73" : "#fff"
            },
            circleColor()
            {
               return this.task.completed ? "#D3D3D3" : "#F5F5F5" 
            },
            strikeThrough()
            {
                return this.task.completed ? 'strikeThrough': ''
            }
        },
        methods:{
            completeTask()
            {
                this.$emit("complete", this.task.name);
            },
            editName()
            {
                this.editmode = false
                this.$emit("changename", this.name)
            },
            setfocus()
            {
                this.editmode = true
                this.$nextTick(() => this.$refs.search.focus())
            }

        }
    }
</script>

<style lang="scss" scoped>
    .task{
        display: flex;
        flex-flow: row;
        width: 100%;
        
    }
    .cross{
        display: flex;
        flex-flow: row;
        width: 50px;
        justify-content: flex-end;    
    }
    .svgContainer{
        display: flex;
        justify-content: flex-start;
        width: 580px;
        font-size: 25px;
        align-items: center;
        .svg {
            margin-left: -25px;

        }
      
    }
    .strikeThrough
    {
       text-decoration: line-through;
       color: lightgray; 
    }
    .editText{
        display: flex;
        flex-flow: column;
        width: 600px;
        
        padding-left: 30px;
        padding-bottom: 20px;
        input[type=text] {
            padding: 12px 20px;
            margin: 8px 0;
            font-size: 25px;
        }
         input:focus {
            border: 1px solid #000;
            outline: 1px solid #000;
        }
    }
</style>