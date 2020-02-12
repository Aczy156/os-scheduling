<template>
  <div id="app">
    <a-layout id="components-layout-demo-top" class="layout">
      <a-layout-header>
        <span class="title">CPU Scheduling  Limit: {{limitprocess}}, Process: {{totalprocess}}</span>
      </a-layout-header>
      <a-layout-content style="padding:20px 50px">
        <div :style="{overflow:'auto',height:'100%',left:0,padding:'24px'}">
          <a-row :gutter="10">
            <a-col :span="6">
              <InputBox :count.sync="parentCount" @addBackup="addBackup"/>
            </a-col>
            <a-col :span="9">
              <BackupQueue v-bind:childbackup="backup" @enter="backupToReady"></BackupQueue>
            </a-col>
            <a-col :span="7">
              <End v-bind:childEnd="end"/>
            </a-col>
          </a-row>
          <a-row :gutter="16">
            <a-col :span="8">
              <ReadyQueue v-bind:childReady="ready" />
            </a-col>
            <a-col :span="8">
              <running v-bind:childrunning="running" @pend="runningToWaiting"/>
            </a-col>
            <a-col :span="8">
              <Waiting v-bind:childWaiting="waiting" @hangOut="waitingToReady"/>
            </a-col>
          </a-row>
        </div>
      </a-layout-content>
      <a-layout-bottom style="text-align:center">
        Made by Aczy156
      </a-layout-bottom>
    </a-layout>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import BackupQueue from './components/BackupQueue.vue'
import InputBox from './components/InputBox.vue'
import ReadyQueue from './components/ReadyQueue.vue'
import Running from './components/Running.vue'
import Waiting from './components/WaitingQueue.vue'
import End from './components/EndQueue.vue'

export default {
  name: 'app',
  // init pram
  data:function () {
    return{
      limitprocess:6,
      totalprocess:0,
      backup:[],  // sync with BackupQueue
      ready:[],
      running:[],
      waiting:[],
      end:[],
      parentCount:0
    };
  },
  watch:{
    // parentCount(newV,oldV){

    // }
  },
  methods:{
    isNotFull:function () {
      return this.totalprocess < this.limitprocess;
    },
    backupToReady:function (PID) {
      console.log(PID+"is select");
      
      if (this.isNotFull()) {
        // add the process to ready
        this.ready.push(this.backup.filter(item=>item.key==PID)[0]);
        // remove the process from backup
        this.backup = this.backup.filter(item=>item.key!==PID);
        this.totalprocess++;
      }
    },
    addBackup:function (values) {
      values["key"] = values.PID;
      this.backup.push(values);
      this.parentCount++;
    },
    // onclick Action
    runningToWaiting:function (PID) {
      /* notice that the process transfer from ready to running 
      is automatic,so don't need to make a function to react 
      */
      // this.running.push(this.ready.filter(item=>item.key==PID)[0]);
      // this.ready = this.ready.filter(item=>item.key!==PID);
      console.log("Pend "+PID);
      this.waiting.push(this.running.pop());
    },
    waitingToReady:function (PID) {
      console.log("hangout "+PID);
      this.ready.push(this.waiting.pop());
    },
    // autochange
    changeRunningStatus:function () {
      this.running[0].time--;
      this.running[0].priority--;
    },
    checkRunning:function () {
      if (this.running[0].time == 0) {
        this.end.push(this.running.shift());
        this.totalprocess--;
      }
    },
    sortReady:function () {
      this.ready.sort((a,b)=>a.priority > b.priority ? -1:1);
    }
  },
  mounted:function(){
    let _this = this;
    setInterval(() => {
      if (_this.running.length > 0) {
        // change the first in running => only have one process
        _this.changeRunningStatus();
        // check if the time is zero
        _this.checkRunning();
      }
      // select the right process from ready
      if (_this.ready.length > 0) {
        if (_this.running.length == 0) {
          _this.running.push(this.ready.shift());
        }
        // compare the priority Ready[0] & Running[0] ,keep the Running
        else if (_this.ready[0].priority > _this) {
          _this.ready.push(_this.running.shift());
          _this.running.push(_this.ready.shift());
          _this.sortReady();
        }
      }
    }, 1000);
    
  },
  components: {
    // HelloWorld
    BackupQueue,
    InputBox,
    ReadyQueue,
    Running,
    Waiting,
    End
  }
};
</script>

<style>
/* #app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
html,
body,
#app{
  width:100%;
  height:100%;
}
.ant-layout {
  height: 100%;
}
.ant-row {
  margin-bottom: 1em;
}
.title {
  color: #fff;
  font-size: large;
}
</style>
