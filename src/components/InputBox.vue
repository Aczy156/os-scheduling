<template>
    <a-card title="新增进程" :bordered="false">
        <!-- form to present information -->
        <a-form :form="form" @submit="handlesubmit">
            <a-form-item label="PID">
                <a-input disabled v-decorator="['PID',{initialValue:count}]"></a-input>
            </a-form-item>
            <a-form-item label="进程名称">
                <a-input 
                    v-decorator="[
                    'name',{rules:[{required:true,message:'请输入进程名称'}]},
                    ]"
                />
            </a-form-item>
            <a-form-item label="执行时间">
                <a-input-number
                    :min="1"
                    :max="10"
                    v-decorator="[
                        'time',
                        {   
                            initialValue:5,
                            rules:[{
                            required:true,message:'请输入执行时间',type:'number'
                        }]}
                    ]"
                />
                <a-form-item label="优先权">
                    <a-input-number
                        v-decorator="[
                            'priority',
                            {
                                initialValue:3,
                                rules:[{
                                    required:true,
                                    message:'请输入优先权',
                                    type:'number'
                                }]
                            }
                        ]"
                    />
                </a-form-item>
                <a-form-item>
                    <a-button class="add-form" type="primary" html-type="submit">添加进程</a-button>
                </a-form-item>
            </a-form-item>
        </a-form>
    </a-card>
</template>
<script>
export default {
    props:{
        count:Number
    },
    name:"InputBox",
    data:function(){
        return {
            formLayout:"vertical",
            form:this.$form.createForm(this,{name:"coordinated"})
        };
    },
    methods:{
        handlesubmit:function (e) {
            console.log("click button");
            e.preventDefault();
            this.form.validateFields((err,values)=>{
                if (!err) {
                    this.$emit("addBackup",values);
                }
            });
        }
    }
};
</script>
<style>
.add-form{
    width:100%;
}
</style>