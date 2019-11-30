<template>
    <div id="PassageManage">
        <el-dialog :visible.sync="dialogVisible" title="创建新文章">
            <div>
                <span>文章标题：</span>
                <el-input placeholder="请输入标题"></el-input>
            </div>
            <div id="editor-wang">
            </div>
        </el-dialog>

        <div class="user-list-breadcrumb">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/UserFeedback' }">用户管理</el-breadcrumb-item>
                <el-breadcrumb-item>文章管理</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <el-card class="user-list-table-content" >
            <div class="use-editor" >
                <div class="search-conditions-panel">
                    <div style="margin-left: 4%">
                        <el-input placeholder="快速搜索文章">
                            <i slot="prefix" class="el-input__icon el-icon-search"></i>
                        </el-input>
                    </div>
                </div>
                <div class="table-panel">
                    <el-table :data="tableData">
                        <el-table-column
                                type="selection"
                                width="55">
                        </el-table-column>
                        <el-table-column
                                prop="name"
                                label="文章名称"
                                width="330">
                        </el-table-column>
                        <el-table-column
                                label="操作"
                                width="180">
                            <template slot-scope="scope">
                                <el-button type="text" size="small" @click="modifyMajor(scope.$index)">编辑</el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                </div>
                <div class="bottom-panel">
                    <div style="float:left;margin-right: 5%;">
                        <el-button type="primary" size="small">新增</el-button>
                        <el-button type="danger" size="small">删除</el-button>
                    </div>
                </div>
            </div>



        </el-card>

        <!--<div id="editor" class="editor"></div>-->
    </div>
</template>

<script>
    // import E from 'wangeditor';

    export default {
        name: "PassageManage",
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
            }
        },
        mounted(){

            let _this = this;
            var E = require('wangeditor');
            var editor = new E('#editor-wang');
            editor.create();



        },
        updated(){

        },
        methods:{
            modifyMajor(index){
                this.$data.modifyForm.modifyIndex = index;
                this.$data.useEditor=true;
                this.$data.dialogVisible=true;
                this.$data.modifyForm.majorName = this.$data.tableData[index].name;
                // this.$router.push('/NewPassage');
                this.$data.dialogVisible = true;
            },
            confirmMajor(){
                this.$data.tableData[this.$data.modifyForm.modifyIndex].name = this.$data.modifyForm.majorName;
                this.$data.dialogVisible=false;
            }
        }
    }
</script>

<style lang="less">
    #PassageManage {
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
