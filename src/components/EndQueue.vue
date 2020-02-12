<template>
    <a-card title="End" :bordered="false">
        <a-table :columns="columns" :dataSource="childEnd">
            <a slot="PID" slot-scope="text" href="javascript:;">{{text}}</a>
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
        childEnd:Array
    },
    data(){
        return{
            columns
        }
    },
    methods:{

    }
}
</script>