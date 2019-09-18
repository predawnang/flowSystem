<template>
    <div class="fillcontain">
        <div class="contain">
            <div class="search_container">
                <input type="text" v-model="searchVal" placeholder="请输入搜索关键字" class="searchInput">
                <el-button type="primary">查询</el-button>
                <el-button type="primary">添加设备</el-button>
            </div>
          <div class="table_container">
            <el-table
                 v-loading="loading"
                 :data="tableData"
                 border
                 stripe
                 highlight-current-row
                 header-cell-class-name="table-header-class"
                 style="width:100%">
                <el-table-column
                   label="序号"
                   width="60"
                   align='center'>
                   <template slot-scope="scope">
                       <span>{{scope.$index+(paginations.pageIndex - 1) * paginations.pageSize + 1}} </span>
                    </template>
                </el-table-column>
                <el-table-column
                   property="compangIndex"
                   label="设备ID"
                   align='center'>
                </el-table-column>
                <el-table-column
                   property="company"
                   label="企业名称"
                   align='center'>
                </el-table-column>
                 <el-table-column
                   property="username"
                   label="模块"
                   align='center'>
                </el-table-column> 
                <el-table-column
                   property="tel"
                   label="模块数量"
                   align='center'>
                </el-table-column>
                <el-table-column
                   property="emaill"
                   label="终端ID"
                   align='center'>
                </el-table-column>
                <el-table-column
                   property="address"
                   label="终端类型"
                   align='center'>
                </el-table-column> 
                 <el-table-column
                   property="ip"
                   label="位置"
                   align='center'>
                </el-table-column>
                <el-table-column
                   property="createTime"
                   label="日期"
                   align='center'>
                </el-table-column>
                <el-table-column
                    label="操作"
                    align="center"
                    width="100">
                    <template slot-scope="scope">
                        <el-button @click="handleClick(scope.row)" type="text" size="middle">激活</el-button>
                        <el-button type="text" size="middle">禁用</el-button>
                    </template>
                </el-table-column>
            </el-table>
           <el-row>
                <el-col :span="24">
                    <div class="pagination">
                        <el-pagination
                            v-if='paginations.total > 0'
                            :page-sizes="paginations.pageSizes"
                            :page-size="paginations.pageSize"
                            :layout="paginations.layout"
                            :total="paginations.total"
                            :current-page='paginations.pageIndex'
                            @current-change='handleCurrentChange'
                            @size-change='handleSizeChange'>
                        </el-pagination>
                    </div>
                </el-col>
            </el-row>
          </div>
        </div>
    </div>
</template>

<script>
    import { getUserList } from "@/api/user";
    export default {
        data(){
            return {
                searchVal: '',
                tableData: [],
                loading:true,
                //需要给分页组件传的信息
                paginations: {
                    total: 0,        // 总数
                    pageIndex: 1,  // 当前位于哪页
                    pageSize: 10,   // 1页显示多少条
                    pageSizes: [5, 10, 15, 20],  //每页显示多少条
                    layout: "total, sizes, prev, pager, next, jumper"   // 翻页属性
                },
            }
        },
        created(){
        },
        mounted(){
            this.getUserList();
        },
        methods: {
            getUserList(){
                let para = {
                    limit:this.paginations.pageSize,
                    page:this.paginations.pageIndex
                }
                getUserList(para).then(res => {
                    this.loading = false;
                    // this.paginations.total = res.data.total;
                    // this.tableData = res.data.userList;
                    this.tableData = [];
                })
            },
            // 每页多少条切换
            handleSizeChange(pageSize) {
               this.paginations.pageSize = pageSize;
               this.getUserList();
            },
            // 上下分页
            handleCurrentChange(page) {
               this.paginations.pageIndex = page;
               this.getUserList();
            }
        },
    }
</script>

<style lang="less" scoped>
    .fillcontain{
        padding-bottom: 0;
    }
    .contain{
        background: #fff;
        padding: 10px;
        margin-bottom: 20px;
    }
   .pagination{
       padding: 10px 20px;
       text-align: right;
   }

   .fillcontain {
       .contain {
           .search_container {
               margin-bottom: 20px;
               text-align: center;
               .searchInput {
                   width: 25%;
                   border-radius: 6px;
                   border: 1px solid #e7e7e7;
                   height: 40px;
                   line-height: 40px;
                   margin-right: 6px;
                   padding: 0px 8px;
               }
           }
       }
   }
</style>


