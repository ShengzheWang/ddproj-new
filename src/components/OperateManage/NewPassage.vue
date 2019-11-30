<template>
    <div id="NewPassage">
        <div class="user-list-breadcrumb">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/UserFeedback' }">用户管理</el-breadcrumb-item>
                <el-breadcrumb-item>文章管理</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <el-card class="user-list-table-content" >
            <div class="the-editor" style="overflow-y: scroll">
                <div class="editor-title" style="width: 100%;height:24px;line-height: 24px;">
                    <div style="float: left">
                        <span style="font-size: 18px;">编辑文章</span>
                    </div>
                    <div style="float: right;margin-right: 30px;">
                        <el-button type="primary" @click="getContent">提交</el-button>
                        <el-button type="danger" >返回</el-button>
                    </div>
                </div>

                <div class="the-editor-divider"></div>
                <div class="the-editor-title-panel">
                    <span>编辑标题：</span>

                    <el-input v-model="modifyForm.majorName" size="small" style="width:100%"></el-input>
                </div>
                <div class="the-editor-title-panel">
                    <el-row style="width:100%;height: 40px;">
                        <el-col style="width: 15%;line-height: 40px;text-align: left">
                            <span>编辑内容：</span>
                        </el-col>
                        <el-col style="width:85%;text-align: left;line-height: 40px;">

                        </el-col>
                    </el-row>
                </div>
                <div class="the-editor-title-panel">
                    <div id="editor-wang" style="width: 100%;height: 100px;">
                    </div>
                </div>
                <div style="width: 100%;height: 130px;"></div>

            </div>


        </el-card>

        <!--<div id="editor" class="editor"></div>-->
    </div>
</template>

<script>
    // import E from 'wangeditor';

    export default {
        name: "NewPanage",
        data(){
            return{
                tableData:[{
                    name:'ss5hane1',
                    id:''
                }],
                dialogVisible:false,
                modifyForm:{
                    modifyIndex:'',
                    id:'',
                    majorName:''
                },
                useEditor: true,
                editor:''
            }
        },
        mounted(){

            let _this = this;
            var E = require('wangeditor');
            var editor = new E('#editor-wang');
            editor.create();
            this.$data.editor = editor;


        },
        updated(){

        },
        methods:{
            modifyMajor(index){
                this.$data.modifyForm.modifyIndex = index;
                this.$data.useEditor=true;
                this.$data.dialogVisible=true;
                this.$data.modifyForm.majorName = this.$data.tableData[index].name
            },
            confirmMajor(){
                this.$data.tableData[this.$data.modifyForm.modifyIndex].name = this.$data.modifyForm.majorName;
                this.$data.dialogVisible=false;
            },
            getContent(){
                console.log(this.$data.editor.txt.html());
            }
        }
    }
</script>

<style lang="less">
    #NewPassage {
        width: 100%;
        height: 100%;


        .user-list-breadcrumb {
            width: 100%;
            height: 30px;
            /*background-color: dodgerblue;*/
            margin-top: 20px;
            padding-left: 20px;
        }

        .user-list-table-content {
            width: 97%;
            margin-left: 2%;
            margin-top: 20px;
        }

        .search-conditions-panel {
            width: 100%;
            height: 60px;
            line-height: 60px;
            display: flex;
            flex-direction: row;
            justify-content: flex-start;
            align-items: flex-start;
            /*background-color: #42b983;*/
        }


        .bottom-panel {
            margin-top: 20px;
            width: 100%;
            height: 50px;
            padding-left: 20px;
            text-align: left;
        }

        .dialogContent {
            display: flex;
            width: 100%;
            height: 100px;
            align-items: center;
            justify-content: center;
        }


        .editor {
            width: 300px;
            height: 100px;
        }


        .the-editor {
            text-align: left;
            font-weight: bold;
            padding-left: 20px;





            .the-editor-divider {
                margin-top: 20px;
                width: 100%;
                height: 0.5px;
                background-color: whitesmoke;
                margin-left: auto;
                margin-right: auto;

            }

            .the-editor-title-panel {
                margin-top: 20px;
                font-weight: normal;
            }

            .el-input, .el-input__inner {
                width: 70% !important;
            }


        }



    }
</style>
