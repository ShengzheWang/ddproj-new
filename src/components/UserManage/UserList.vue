<template>
    <div id="UserList">
        <el-dialog :visible.sync="dialogVisibleAct" title="设为活动管理员" :modal="false" width="200">
            <div style="width:200px;margin-left:auto;margin-right:auto">
                <el-row style="width:100%;">
                    <el-col style="width:80px;line-height:33px;">
                        <span>活动名称：</span>
                    </el-col>
                    <el-col style="width:120px;">
                        <el-select size="small" style="width:100px;" v-model="choosedActId">
                            <el-option v-for="item in actList" :value="item.id" :label="item.title" :key="item.id"></el-option>
                        </el-select>
                    </el-col>
                </el-row>
                <div style="margin-top:20px;">
                    <el-button type="primary" size="small" @click="useSetManager">确认</el-button>
                    <el-button size="small" @click="()=>{this.$data.dialogVisibleAct=false;}">取消</el-button>
                </div>
            </div>
        </el-dialog>
        <el-dialog :visible.sync="dialogVisible" title="查看简历" :modal="false" width="300">
            <div class="userContentPanel">
                <div class="contentPanel" style="display: flex;flex-direction: row;text-align: left;padding-left: 20px;">
                    <div class="contentTitle" style="width: 100px;">
                        <span>姓名：</span>
                    </div>
                    <div class="contentContent" style="width:200px;">
                        <span>{{resumeContent.name}}</span>
                    </div>
                </div>
                <div class="contentPanel" style="display: flex;flex-direction: row;text-align: left;padding-left: 20px;margin-top: 20px;">
                    <div class="contentTitle" style="width:100px;">
                        <span>性别：</span>
                    </div>
                    <div class="contentContent" style="width:200px;">
                        <span>{{resumeContent.sex}}</span>
                    </div>
                </div>
                <div class="contentPanel" style="display: flex;flex-direction: row;text-align: left;padding-left: 20px;margin-top: 20px;">


                    <div class="contentTitle" style="width:100px;">
                        <span>年级：</span>
                    </div>
                    <div class="contentContent" style="width:200px;">
                        <span>{{resumeContent.grade}}</span>
                    </div>
                </div>
                <div class="contentPanel" style="display: flex;flex-direction: row;text-align: left;padding-left: 20px;margin-top: 20px;">
                    <div class="contentTitle" style="width:100px;">
                        <span>专业：</span>
                    </div>
                    <div class="contentContent" style="width:200px;">
                        <span>{{resumeContent.profession}}</span>
                    </div>
                </div>
                <div class="contentPanel" style="display: flex;flex-direction: row;text-align: left;padding-left: 20px;margin-top: 20px;">
                    <div class="contentTitle" style="width:100px;">
                        <span>常用电话：</span>
                    </div>
                    <div class="contentContent" style="width:200px;">
                        <span>{{resumeContent.phone}}</span>
                    </div>
                </div>
                <div class="contentPanel" style="display: flex;flex-direction: row;text-align: left;padding-left: 20px;margin-top: 20px;">
                    <div class="contentTitle" style="width:100px;">
                        <span>在校情况：</span>
                    </div>
                    <div class="contentContent" style="width:350px;">
                        <span>{{resumeContent.detail}}</span>
                    </div>
                </div>
                <div class="contentPanel" style="display: flex;flex-direction: row;text-align: left;padding-left: 20px;margin-top: 20px;">
                    <div class="contentTitle" style="width:100px;">
                        <span>审核结果：</span>
                    </div>
                    <div class="contentContent" style="width:350px;">
                         
                        <div v-if="resumeContent.status===1">
                            <span>已通过</span>
                        </div>
                        <div v-if="resumeContent.status===2">
                            <span>未通过</span>
                        </div>
                        <div v-if="resumeContent.status===0">
                            <span>未审核</span>
                        </div>
                    </div>
                </div>
                
                <div style="width: 100%;height: 1px;background-color: whitesmoke;margin-top: 30px;"></div>
                <div class="checkPanel" style="margin-top: 20px;">
                    <div class="contentPanel" style="display: flex;flex-direction: row;text-align: left;padding-left: 20px;margin-top: 20px;">
                       <div class="contentTitle" style="width:100px;">
                        <span>操作：</span>
                        </div>
                        <div class="contentContent" v-if="resumeContent.status===0" style="width:350px;">
                            <el-switch
                                    v-model="resumeContent.checkStatus"
                                    active-color="#13ce66"
                                    inactive-color="#ff4949"
                                    active-text="按月付费"
                                    inactive-text="按年付费"
                                    ></el-switch>
                        </div>
                    </div>
                    <div v-if="resumeContent.checkStatus===false" class="contentPanel" style="display: flex;flex-direction: row;text-align: left;padding-left: 20px;margin-top: 20px;">
                        <div class="contentTitle" style="width:100px;">
                            <span>拒绝理由：</span>
                        </div>
                        <div class="contentContent" style="width:350px;">
                            <el-input type="textarea"></el-input>
                        </div>
                    </div>

                </div>
                <div class="buttonPanel" style="width:100%;height: 50px;text-align: center;margin-top: 30px;">
                    <el-button type="primary" >提交</el-button>
                    <el-button type="primary">返回</el-button>

                </div>




            </div>

        </el-dialog>
        <div class="user-list-breadcrumb">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/UserList' }">用户管理</el-breadcrumb-item>
                <el-breadcrumb-item>用户列表</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <el-card class="user-list-table-content">
            <div class="search-conditions-panel">
                <el-row style="width: 100%;overflow: hidden">
                    <el-col style="width: 33%;text-align: left">
                        <div style="font-size: 14px">
                            <span>用户状态:&nbsp;&nbsp;</span>
                            <el-select style="height: 45px;" v-model="role" @change="changeSearch">
                                <el-option value="-1" label="全部"></el-option>
                                <el-option label="待审核" value="0"></el-option>
                                <el-option label="未激活" value="1"></el-option>
                                <el-option label="活动管理员" value="2"></el-option>
                            </el-select>
                        </div>
                    </el-col>
                    <el-col style="width: 33%;text-align: left">
                        <div style="font-size: 14px;margin-left: 1%">
                            <span>排序:&nbsp;&nbsp;</span>
                            <el-select style="height: 45px;" v-model="orderBy" @change="changeSearch">
                                <el-option value="按注册时间升序"></el-option>
                                <el-option value="按注册时间降序"></el-option>
                                <el-option value="按邀请人数升序"></el-option>
                                <el-option value="按邀请人数降序"></el-option>
                            </el-select>
                        </div>
                    </el-col>
                    <el-col style="width: 33%;text-align: left">
                        <div style="font-size: 14px;margin-left: 1%">
                            <span>搜索:&nbsp;&nbsp;</span>
                            <el-input style="height: 45px;" placeholder="输入昵称快速查找" v-model="searchName" @change="changeSearch"></el-input>
                        </div>
                    </el-col>
                    
                </el-row>

            </div>
            <div class="table-panel">
                <el-table :data="tableData"
                    @selection-change="handleSelectionChange"
                >
                    <el-table-column
                            type="selection"
                            width="55">
                    </el-table-column>
                    <el-table-column
                            prop="stageName"
                            label="昵称"
                            width="100">
                    </el-table-column>
                    <el-table-column
                           
                            label="性别"
                            width="60">
                            <template slot-scope="scope">
                                <span v-if="tableData[scope.$index].sex===0">男</span>
                                <span v-else>女</span>
                            </template>
                    </el-table-column>
                    <el-table-column
                            prop="grade"
                            label="年级"
                            width="90">
                    </el-table-column>
                    <el-table-column
                            prop="profession"
                            label="专业"
                            width="150">
                    </el-table-column>
                    <el-table-column
                            prop="telephone"
                            label="常用电话"
                            width="120">
                    </el-table-column>
                    <el-table-column
                            
                            label="状态"
                            width="80">
                            <template slot-scope="scope">
                                <span v-if="tableData[scope.$index].status===0">未审核</span>
                                <span v-else-if="tableData[scope.$index].status===1">已通过</span>
                                <span v-else>未通过</span>
                            </template>
                    </el-table-column>
                    <el-table-column
                            prop="cdk"
                            label="邀请码"
                            width="90">
                    </el-table-column>
                    <el-table-column
                            prop="inviteNum"
                            label="邀请人数"
                            width="90">
                    </el-table-column>
                    <el-table-column
                            
                            label="注册时间"
                            width="120">
                            <template slot-scope="scope">
                                <span>{{scope.row.createdAt.slice(0,10)}} {{scope.row.createdAt.slice(11,19)}}</span>
                            </template>
                    </el-table-column>
                    <el-table-column
                            label="操作"
                            width="180">
                        <template slot-scope="scope">
                            <el-button type="text" size="small" @click="checkUser(scope.$index)">详情</el-button>
                        </template>
                    </el-table-column>
                </el-table>
            </div>
            <div class="bottom-panel">
                <div style="float: left">
                    <el-button type="primary" size="small" @click="setManager">设为活动管理员</el-button>
                    <el-button type="primary" size="small" @click="delManager">取消权限</el-button>

                </div>
                <div style="float:right;margin-right: 5%;">
                    <el-pagination
                            layout="prev, pager, next"
                            :total="pages+1"
                            @current-change="handleCurrentChange"
                            :current-page="currentPage"
                            >
                    </el-pagination>
                </div>
            </div>


        </el-card>
        <div style="width:100%;height:100px;"></div>
    </div>
</template>

<script>
import { stringify } from 'querystring';
    export default {
        name: "UserList",
        data(){
            return{
                searchName:'',
                orderBy:'按注册时间升序',
                order:'',
                role:'-1',
                dataList:[],
                choosedList:[],
                dialogVisibleAct:false,
                tableData:[{
                    name:'ss5hane1',
                    gender:'男',
                    grade:'2016',
                    major:'人力资源管理',
                    phone:'18860016206',
                    status:'正式会员',
                    inviteCode:'QHYSX5',
                    inviteNum:13,
                    registerTime:'2019-11',

                }],
                dialogVisible:false,
                resumeContent:{
                    name:'',
                    sex:'',
                    grade:'',
                    profession:'',
                    phone:'',
                    detail:'',
                    status:1,
                },
                actList:[],
                choosedActId:'',
                pages:10,
                pageSize:10,
                currentPage:1,
            }
        },
        mounted(){
            let _this = this;
            console.log(this.global.token);
            fetch(this.global.base_url+'/user/all?role=-1&orderby=0&order=0&key=',{
                method:'get',
                mode:'cors',
                headers:{
                    Authorization: this.global.token
                }
            }).then((res)=>res.json()).then((response)=>{
                console.log(response);
                _this.$data.tableData = response.data.userList;
                _this.$data.pages = response.data.totalNum;
            })
        },
        methods:{
            handleCurrentChange(val){
                console.log(val);
                let _this = this;
                let role = this.$data.role;
                let order = 0;
                let orderBy = 1;
                if(this.$data.orderBy === '按注册时间升序'){
                    order = 0;
                    orderBy = 0;
                }else if(this.$data.orderBy === '按注册时间降序'){
                    order = 1;
                    orderBy = 0;
                }else if(this.$data.orderBy === '按邀请人数升序'){
                    order = 0;
                    orderBy=1;
                }else {
                    order = 1;
                    orderBy = 1;
                }

                let key = this.$data.searchName;
                let page = val-1;
                fetch(this.global.base_url + '/user/all?role='+role+'&orderby='+orderBy+'&order='+order+'&key='+key+'&page='+page+'&limit=10',{
                    method: 'get',
                    headers:{
                        Authorization: this.global.token
                    }
                }).then(res=>res.json()).then((response)=>{
                    _this.$data.tableData = response.data.userList;
                    _this.$data.pages = response.data.totalNum;
                })
            },
            changeSearch(){
                let _this = this;
                let role = this.$data.role;
                let order = 0;
                let orderBy = 1;
                if(this.$data.orderBy === '按注册时间升序'){
                    order = 0;
                    orderBy = 0;
                }else if(this.$data.orderBy === '按注册时间降序'){
                    order = 1;
                    orderBy = 0;
                }else if(this.$data.orderBy === '按邀请人数升序'){
                    order = 0;
                    orderBy=1;
                }else {
                    order = 1;
                    orderBy = 1;
                }

                let key = this.$data.searchName;

                fetch(this.global.base_url + '/user/all?role='+role+'&orderby='+orderBy+'&order='+order+'&key='+key+'&page=0'+'&limit=10',{
                    method: 'get',
                    headers:{
                        Authorization: this.global.token
                    }
                }).then(res=>res.json()).then((response)=>{
                    _this.$data.tableData = response.data.userList;
                    _this.$data.pages = response.data.totalNum;
                })
            },
            checkUser(index){
                // this.$data.dialogVisible = true;
                console.log('dwdwd');
                let id = this.$data.tableData[index].id;

                let _this = this;
                console.log(this.global.token);
                fetch(this.global.base_url+'/user/info/check?id='+id,{
                    method:'get',
                    mode:'cors',
                    headers:{
                        Authorization: this.global.token
                    }
                }).then((res)=>res.json()).then((response)=>{
                    console.log(response);
                    _this.$data.resumeContent = response.data;
                    _this.$data.dialogVisible = true;
                    
                })
            },
            handleSelectionChange(val){
                console.log(val);
                this.$data.choosedList = val;
            },
            setManager(){
                let _this = this;
                if(this.$data.choosedList.length===0){
                    this.$message({
                        type:'error',
                        message:'未选择用户哦~'
                    });

                }else{
                    this.$data.dialogVisibleAct=true;
                }
                
                fetch(this.global.base_url+'/activity/title?key=',{
                    method:'get',
                    headers:{
                        Authorization:this.global.token
                    },

                }).then(res=>res.json()).then((response)=>{
                    console.log(response);
                    _this.$data.actList = response.data.activityList;
                })
            },
            useSetManager(){
                let _this = this;
               let userList = [];
                let chooseUserList = this.$data.choosedList;
                for(let index = 0; index<chooseUserList.length; index++){
                    userList.push(chooseUserList[index].id);
                }
                fetch(this.global.base_url+'/user/manager',{
                    method:'post',
                    body:JOSN.stringify({
                        userList: userList,
                        activityID: this.$data.choosedActId
                    }),
                    headers:{
                        Authorization: this.global.token
                    }
                }).then(res=>res.json()).then((response)=>{
                    _this.$message({
                        type:'success',
                        message:'操作成功！'
                    })
                })
            },
            delManager(){
                let _this = this;
               let userList = [];
                let chooseUserList = this.$data.choosedList;
                for(let index = 0; index<chooseUserList.length; index++){
                    userList.push(chooseUserList[index].id);
                }
                fetch(this.global.base_url+'/user/manager',{
                    method:'delete',
                    body:JOSN.stringify({
                        userList: userList,
                        
                    }),
                    headers:{
                        Authorization: this.global.token
                    }
                }).then(res=>res.json()).then((response)=>{
                    _this.$message({
                        type:'success',
                        message:'操作成功！'
                    })
                })
            }
        }
    }
</script>

<style lang="less">
    #UserList{
        width: 100%;
        height: 100%;


        .el-input, .el-input--suffix, .el-input__inner, .el-select{
             width:150px !important;
         }
    }

    .user-list-breadcrumb{
        width: 100%;
        height: 30px;
        /*background-color: dodgerblue;*/
        margin-top: 20px;
        padding-left: 20px;
    }

    .user-list-table-content{
        width: 97%;
        margin-left: 2%;
        margin-top: 20px;
    }

    .search-conditions-panel{
        width: 100%;
        height: 60px;
        line-height: 60px;
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        align-items: flex-start;
        /*background-color: #42b983;*/
    }


    .bottom-panel{
        margin-top: 20px;
        width: 100%;
        height: 50px;
        padding-left: 20px;
        text-align: left;
    }



</style>
