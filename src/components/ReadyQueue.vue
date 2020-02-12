<template>
    <a-card title="Ready" :bordered="false">
        <a-table :columns="columns" :dataSource="childReady">
            <a slot="PID" slot-scope="text" href="javascripts:;">{{text}}</a>
            <span slot="customTitle">
                PID
            </span>
        </a-table>
    </a-card>
</template>
<script>
const columns=[
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
        childReady:Array
    },
    data(){
        return {
            columns
        };
    },
    methods:{
        test:function (PID) {
            console.log(PID);
            
        }
    }
};
</script>