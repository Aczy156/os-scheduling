<template>
    <a-card title="Running">
        <a-table :columns="columns" :dataSource="childrunning">
            <a slot="PID" slot-scope="text" href="javascript:;">{{text}}</a>
            <span slot="customTitle">
                PID
            </span>
            <span slot="action" slot-scope="text">
                <a @click="pend(text.PID)" href="javascript:;">挂起</a>
            </span>
        </a-table>
        
    </a-card>
</template>
<script>
const columns = [
{
    dataIndex: "PID",
    key: "PID",
    slots: { title: "customTitle" },
    scopedSlots: { customRender: "PID" }
  },
  {
    title: "名称",
    dataIndex: "name",
    key: "name"
  },
  {
    title: "时间",
    dataIndex: "time",
    key: "time"
  },
  {
    title: "优先权",
    key: "priority",
    dataIndex: "priority",
  },
  {
    title: "操作",
    key: "action",
    scopedSlots: { customRender: 'action' },
  }
];
export default {
    props:{
        childrunning:Array
    },
    data(){
        return{
            columns
        }
    },
    methods:{
        pend:function (PID) {
            console.log(PID);
            this.$emit("pend",PID);
        }
    }
};
</script>