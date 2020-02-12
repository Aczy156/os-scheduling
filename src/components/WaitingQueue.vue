<template>
<a-card title="Waiting" :bordered="false">
    <a-table :columns="columns" :dataSource="childWaiting">
        <a slot="PID" slot-scope="text" href="javascript:;">{{text}}</a>
        <span slot="customtitle">
            PID
        </span>
        <span slot="action" slot-scope="text">
            <a @click="hangOutAction(text.PID)" href="javascript:;">解挂</a>
        </span>
    </a-table>
</a-card>
</template>

<script>
const columns = [{
        dataIndex: "PID",
        key: "PID",
        slots: {
            title: "customTitle"
        },
        scopedSlots: {
            customRender: "PID"
        }
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
        scopedSlots: {
            customRender: 'action'
        },
    }
];
export default {
    props: {
        childWaiting: Array
    },
    data() {
        return {
            columns
        }
    },
    methods: {
        hangOutAction: function (PID) {
            this.$emit("hangOut", PID);
        }
    }
};
</script>
