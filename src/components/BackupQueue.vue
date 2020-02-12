<template>
    <a-card title="后备队列" :bordered="false">
        <a-table :columns="columns" :dataSource="childbackup">
            <a slot="PID" slot-scope="text" href="javascript:;">{{text}}</a>
            <span slot="customTitle">PID</span>
            <span slot="action" slot-scope="text">
                <a @click="Enter(text.PID)" href="javascript:;">进入</a>
            </span>
        </a-table>
    </a-card>
</template>
<script>
// define data
const columns = [
    {
        dataIndex:"PID",
        key:"PID",
        slots:{title:"customTitle"},
        scopedSlots:{customRender:"PID"}
    },
    {
        title:"名称",
        dataIndex:"name",
        key:"name"
    },
    {
        title:"时间",
        dataIndex:"time",
        key:"time"
    },
    {
        title:"优先权",
        dataIndex:"priority",
        key:"priority"
    },
    {
        title:"操作",
        key:"action",
        scopedSlots:{customRender:'action'}
    }
];
export default {
    props:{
        childbackup:Array
    },
    data:function() {
        return {
            columns
        }
    },
    methods:{
        Enter:function(PID){
            console.log("go to ready");
            this.$emit("enter",PID);
        }
    }
};
</script>