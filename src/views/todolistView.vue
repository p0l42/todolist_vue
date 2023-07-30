<template>
    <div class="container">
        <el-row style="margin-bottom: 8px;" class="light-bg"><h1>Todo List</h1></el-row>
        <el-row style="margin-bottom: 8px;" class="light-bg">
            <el-col :span="11">
                <el-input
                    placeholder="请输入代办事项"
                    v-model="input"
                    clearable
                    @keypress.enter.native="addTodo">
                </el-input>
            </el-col>
            <el-col :span="6">
                <el-button type="success" round @click="addTodo">添加事项</el-button>
            </el-col>
            <el-col :span="5">
                <el-button type="danger" round @click="clearAll">清空事项</el-button>
            </el-col>

        </el-row>
        <el-row style="text-align: left; margin-bottom: 10px;" class="light-bg">
            <h3 style="margin-left:5px; margin-bottom: 3px;">
                <i class="el-icon-s-order"></i>
                待办事项
                <el-badge :value="todovalue" :max="10" class="item" type="primary"></el-badge>
            </h3>
        </el-row>
        <div v-for='(todo, index) in todolist' :key="index">
          <el-row class="tip">
            <!-- <span v-show="todo"> -->
              <el-col :span="17" @dblclick.native="editItem(index)">
                <span v-if="!edit[index]">
                  <el-checkbox @change="addDone(index)" :label="todo"></el-checkbox>
                </span>
                <span v-else>
                  <el-input
                    placeholder="新内容"
                    v-model="editinput"
                    clearable
                    @keypress.enter.native="submitItem(index)"
                    >
                  </el-input>
                </span>
              </el-col>
              <el-col :span="6" style="margin-left:10px;">
                <el-button type="primary" icon="el-icon-edit" circle size="mini" @click="editItem(index)"></el-button>
                <el-button type="danger" icon="el-icon-delete" circle size="mini" @click="deleteItem(index)"></el-button>
              </el-col>
              <!-- {{todo}} -->
            <!-- </span> -->
          </el-row>
        </div>
        <el-row style="text-align: left;" class="light-bg">
            <h3 style="margin-left:5px; margin-bottom: 3px;">
                <i class="el-icon-s-claim"></i>
                已完成事项
                <el-badge :value="donevalue" :max="10" class="item" type="success"></el-badge>
            </h3>
        </el-row>
        <div v-for='done in donelist' :key="done">
          <div class="tip">
            <!-- <span v-show="todo"> -->
              <s>{{done}}</s>
            <!-- </span> -->
          </div>
        </div>
    </div>
</template>


<script>
    
    export default{
        data() {
            return {
                input: '',
                todovalue: 0,
                donevalue: 0,
                todolist: [],
                donelist: [],
                edit: [],
                editinput: ''
            }
        },
        methods:{
            addTodo: function() {
              if(this.input === '' || !isNaN(this.input)){ 
                alert("输入为空或纯数字");
                return;
              }
              this.todolist.push(this.input);
              this.input = '';
              this.todovalue++;
              this.edit.push(false);
            },
            addDone: function(index){
              // console.log(index);
              var text = this.todolist[index];
              this.todolist.splice(index,1);
              this.todovalue--;
              this.donelist.push(text);
              this.donevalue++;
            },
            clearAll: function(){
              this.todovalue = 0;
              this.donevalue = 0;
              this.todolist = [];
              this.donelist = [];
            },
            deleteItem: function(index){
              this.todolist.splice(index,1);
              this.todovalue--;
            },
            editItem: function(index){
              this.edit.splice(index,1,true);
            },
            submitItem: function(index){
              this.todolist.splice(index, 1, this.editinput);
              this.edit.splice(index, 1, false);
              this.editinput = '';
            }
        }
    }
</script>

<style>
  .container{
    width:500px;
    margin: auto;
  }
  .color-gr{
    border:#67C23A 1px solid;
    background: #67C23A;
    width: 3px;
  }
  .light-bg{
    background: transparent;
  }

  .el-header, .el-footer {
    /* background-color: #B3C0D1; */
    color: #333;
    text-align: center;
    line-height: 60px;
  }
  
  .el-aside {
    background-color: #D3DCE6;
    color: #333;
    text-align: center;
    line-height: 200px;
  }
  
  .el-main {
    /* background-color: #E9EEF3; */
    color: #333;
    text-align: center;
    line-height: 160px;
  }
  
  body > .el-container {
    margin-bottom: 40px;
  }
  
  .el-container:nth-child(5) .el-aside,
  .el-container:nth-child(6) .el-aside {
    line-height: 260px;
  }
  
  .el-container:nth-child(7) .el-aside {
    line-height: 320px;
  }
  
  .el-row {
    margin-bottom: 8px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;
    margin-left: 2px;
    margin-right: 2px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
  .item {
    margin-top: 3px;
    margin-right: 40px;
   }

   .tip{
    padding: 8px 16px;
    background-color: transparent;
    border-radius: 4px;
    border-left: 5px solid #67C23A;
    text-align: left;
    margin-top: 0px;
    margin-bottom: 8px;
   }
</style>