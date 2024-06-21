<template>
    <div class="common-layout">
    <el-container>
      <el-header>
       <div class="header">
       <div class="ai">
        <span>AI工具箱</span>
       </div>
        <div class="user">
          <img src="./assets/logo.png" alt="">
          <!-- <span class="userName">{{  }}</span> -->
          <el-dropdown :hide-on-click="false">
          <span class="el-dropdown-link" style="min-width: 50px; padding:2px 5px; ">
            {{ userName }}
          </span>
          <template #dropdown>
            <el-dropdown-menu>
              <el-dropdown-item v-for="item in personList" :key="item.id" @click="xuan(item.name,item.id)">{{ item.name }}</el-dropdown-item>
             </el-dropdown-menu>
          </template>
        </el-dropdown>
        </div>
       </div>
      </el-header>
      <el-container>
        <el-aside width="200px">
          <div class="slider">  
           <!-- <i class="DeleteFilled"></i> -->
            <div class="jilu" v-for="(item,index) in shijianList" :key="index">
            <div class="Tim">{{ item.timeDate }}</div>
            <div class="juzi_txt">
              <p class="juzi" v-for="(item2,index2) in item.textList" :key="index2">
              {{ item2}}
              <el-icon @click="shanchu(index,index2)"><DeleteFilled /></el-icon>
              </p>
              <!-- <Edit style="width: 1em; height: 1em; margin-right: 8px" /> -->
            </div>
          </div>
          </div>
        </el-aside>
        <el-main>
         <div class="box_main">
          <div class="text">
            <el-input
              v-model="textarea"
              maxlength="16000"
              style="width: 35vw;"
              placeholder="原始文本内容。。。"
              show-word-limit
              type="textarea"
          />
          <div class="btn">
            <el-button type="primary" @click="startZong">开始总结</el-button>
            <el-button type="success" @click="quxiaoZon">取消总结</el-button>
            <el-button type="info" @click="startZong">重新总结</el-button>
            <span class="state">状态...</span>
          </div>
          </div>
          <div class="summarize">
            <!-- <img class="aiImg" src="./assets/logo.png" alt=""> -->
            <div class="sum_text" v-for="(item,index) in rightTxt" :key="index"> 
              <span>{{item}}</span>
            </div>
          </div>
         </div>
        </el-main>
      </el-container>
    </el-container>
  </div>
  <!-- <el-button :plain="true" @click="open1">Message</el-button> -->
</template>

<script setup name="App">
import { ElMessage ,ElIcon} from 'element-plus';
import { DeleteFilled } from '@element-plus/icons-vue'
import { reactive, ref} from 'vue';
let textarea = ref('')
let rightTxt = reactive([])
 let personList = reactive([
  { id:1,
    name:'123',
    timeList:[
      {
        timeDate:'4月6日',
        textList:[
          '1条','2条','三条'
        ]
      }
    ]
  },
  { id:2,
    name:'迟',
    timeList:[
      {
        timeDate:'4月6日',
        textList:[
          '1111','2111','333'
        ]
      }
    ]},
  { id:3,
    name:'456',
    timeList:[
      {
        timeDate:'4月6日',
        textList:[
          '1122','22211'
        ]
      },
      {
        timeDate:'4月6日',
        textList:[
          '1122','22211'
        ]
      }
    ]}
])
let userName = ref('123')
let shijianList = reactive([
      {
        timeDate:'4月6日',
        textList:[
          '1111','2111','333'
        ]
      }
    ])

function xuan (val,index){
  console.log(val,index)
  let num = shijianList.length
  shijianList.splice(0,num)
  let B_list = localStorage.getItem('shijianList') || []
  if(B_list.length === 0 || userName.value != val){
   userName.value = val
   console.log(userName.value)
   let Peritem = personList.find(item => item.id === index);
   Peritem.timeList.forEach(element => {
    shijianList.push({timeDate:element.timeDate,textList:element.textList})
   });
   console.log(shijianList)
   localStorage.setItem('shijianList', JSON.stringify(shijianList));
  } else {
    shijianList = reactive( JSON.parse(B_list));
  }
}
const open1 = () => {
   ElMessage({
    showClose: true,
    message: '目前最高支持 8k 中文文本和 16k 英文文本输入',
  })
}
function startZong(){
  if(textarea.value === ''){
// 点弹出对话框
   open1()
    return open1
  }
  let item = '增加一条记录'
  // item.push("增加一条记录")
  const date = new Date();
  const month = date.getMonth() + 1;  // 获取当前月份（注意月份从 0 开始，需要加 1）
  const day = date.getDate();  // 获取当前日期
  const Today = `${month}月${day}日`
  // console.log(Today)
  // shijianList.push()
  let len = shijianList.length - 1
  if(shijianList[len].timeDate === Today){
    shijianList[len].textList.push(item)
  } else {
    const newItem = {
      timeDate: Today,
      textList:['增加一条记录']

    }
    shijianList.push(newItem)
  }
      // textarea.value = ''
      rightTxt.push("这是一条总结记录，这是一条总结记录，这是一条总结记录，这是一条总结记录，")
      localStorage.setItem('shijianList', JSON.stringify(shijianList));
}
function quxiaoZon(){
  // console.log(111)
  textarea.value = ''
}
function shanchu(index1,index2){
  if(index2 === 0){
  shijianList.splice(index1,1)
  } else{
  shijianList[index1].textList.splice(index2,1)
  console.log(shijianList)
  }
  localStorage.setItem('shijianList', JSON.stringify(shijianList));
}

</script>

<style>
*{
  padding: 0;
  margin: 0;
}
body{
  background-color: #fafafa;
}
.header{
  width: 100%;
  height: 100%;
  margin-top: 10px;
  background-color: #fff;
  display: flex;
  align-content: center;
  justify-content: space-between;
}
.header .ai{
  font-size: 1.2vw;
  font-weight: 700;
  display: flex;
  align-items: center;
  margin-left: 20px;
}
.user {
  display: flex;
  align-items: center;
  cursor: pointer; 
}
.user img{
  width: 2vw;
  height:2vw;
}
.box_main{
  width: 100%;
  height: auto;
  display: flex;
}
.text{
  width: 40vw;
  height: 600px;
  /* background-color: white; */
}
.el-textarea__inner{
   height: 500px !important;
}
.btn{
   width: 35vw;
   height: 100px;
   display: flex;
   justify-content: space-around;
   align-items: center;
}
.summarize{
  flex: 1;
  padding: 20px;
  /* width: 42vw; */
  background-color: white;
}
/* .summarize:first-child{
  margin-top: 0;
} */
.aiImg{
  width:2vw;
}
.sum_text{
  /* width: 100%; */
  width: fit-content;
  height: auto;
  max-width: 100%;
  word-break: break-all;
  background-color:#f7f7ff;
  padding: .5vw 1vw;
  border-radius: .5vw;
  margin-top: 20px;
}
.slider{
  width: 100%;
  height: 600px;
  margin: 20px 0;
  margin-left: 20px;
  background-color: #fff;
  overflow: auto;
}
.jilu{
  width: 100%;
  margin: 20px 10px;
  background-color:white;
}
.jilu:first-child{
  margin-top: 0;
}
.Tim{
  font-size: 16px;
  font-weight: 700;
  margin-bottom:10px;
}
.juzi{
  width: 160px;
  margin: 5px 0;
  margin-left: 10px;
  background-color: #ebedf0;
  display: flex;
  justify-content: space-between;
}
.el-icon{
  width: 20px !important;
  height: 20px !important;
  color: #333 !important;
}
</style>
