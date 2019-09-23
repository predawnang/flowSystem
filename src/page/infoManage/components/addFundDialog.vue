<template>
    <el-dialog 
        :visible.sync="isVisible"
        :title="addFundDialog.title" 
        :close-on-click-modal='false'
        :close-on-press-escape='false'
        :modal-append-to-body="false"
        @close="closeDialog">
        <div class="form">
            <el-form 
                ref="form" 
                :model="form"
                :rules="form_rules"
                :label-width="dialog.formLabelWidth" 
                style="margin:10px;width:auto;">

                <el-form-item prop='deviceId' label="设备ID:">
                    <el-input type="text" v-model="form.deviceId"></el-input>
                </el-form-item>

                <el-form-item prop='module'  label="模块名称:">
                    <el-input type="text" v-model.number="form.module"></el-input>
                </el-form-item>

                <el-form-item prop='terminal' label="终端类型:">
                    <el-input type="text" v-model.number="form.terminal"></el-input>
                </el-form-item>

                <el-form-item prop='companyName' label="企业名称:">
                    <el-input type="text" v-model.number="form.companyName"></el-input>
                </el-form-item>
                
                <el-form-item prop='account' label="企业主账号:">
                    <el-input type="text" v-model.number="form.account"></el-input>
                </el-form-item>

                <el-form-item  class="text_right">
                    <el-button @click="isVisible = false">取 消</el-button>
                    <el-button type="primary" @click='onSubmit("form")'>提  交</el-button>
                </el-form-item>

            </el-form>
        </div>
    </el-dialog>
</template>

<script>
   import { mapState, mapGetters } from 'vuex'
   import { addMoney,updateMoney } from "@/api/money";

  export default {
      name:'addFundDialogs',
      data(){
          let validateData = (rule, value, callback) => {
                if(value === ''){
                    let text;
                    if(rule.field == "module"){
                        text='模块名称';
                    }else if(rule.field == "terminal"){
                        text='终端类型';
                    }else if(rule.field == "companyName"){
                        text='企业名称';
                    }else if(rule.field == "account"){
                        text='企业主账号';
                    }
                    callback(new Error(text+'不能为空'));
                }
            };
          return {
            isVisible: this.isShow,
            form:{
                deviceId:'',
                module: '',
                terminal: '',
                companyName:'',
                account:''
            },
            payType:[
                {label:'提现',value:'0'},
                {label:'提现手续费',value:'1'},
                {label:'提现锁定',value:'2'},
                {label:'理财服务退出',value:'3'},
                {label:'购买宜定盈',value:'4'},
                {label:'充值',value:'5'},
                {label:'优惠券',value:'6'},
                {label:'充值礼券',value:'7'},
                {label:'转账',value:'8'}
            ],
            form_rules: {
                deviceId   : [
                    {required: true, message : '设备ID不能为空',trigger : 'blur'}
                ],
                module   : [
                    { required: true, validator:validateData,trigger: 'blur'},
                ],
                terminal   : [
                    { required: true, validator:validateData,trigger: 'blur'},
                ],
                companyName   : [
                    { required: true, validator:validateData,trigger: 'blur'},
                ],
                account   : [
                    { required: true, validator:validateData,trigger: 'blur'},
                ]
            },
            //详情弹框信息
            dialog: {
                width:'400px',
                formLabelWidth:'120px'
            }
          }
      },
      props:{
          isShow:Boolean,
          dialogRow:Object
      },
      computed:{
        ...mapGetters(['addFundDialog']),

      },
      created(){
           if(this.addFundDialog.type === 'edit'){
                this.form = this.dialogRow;
                // this.form.incomePayType = (this.dialogRow.incomePayType).toString();
                // this.form.pay = -this.dialogRow.pay;
            }else{
                // this.$refs['form'].resetFields();
                // console.log(this.$refs['form']);
            }                
      },
      mounted(){

      },
      methods:{
          onScreeoutMoney(){

          },
          onAddMoney(){

          },
          closeDialog(){
              this.$emit('closeDialog');
          },
          //表单提交
          onSubmit(form){
            this.$refs['form'].validate((valid) => {
                if (valid) {//表单数据验证完成之后，提交数据;
                    let formData = this[form];
                    const para = Object.assign({}, formData)
                    // edit
                    if(this.addFundDialog.type === 'edit'){
                        updateMoney(para).then(res => {
                            this.$message({
                                message: '修改成功',
                                type: 'success'
                            })
                            this.$refs['form'].resetFields()
                            this.isVisible = false
                            this.$emit('getFundList');
                       })
                    }else{
                        // add
                        addMoney(para).then(res => {
                            this.$message({
                                message: '新增成功',
                                type: 'success'
                            })
                            this.$refs['form'].resetFields()
                            this.isVisible = false
                            this.$emit('getFundList');
                       })
                    }
                }
            })
          },
      }
  }
</script>

<style lang="less" scoped>
    .search_container{
        margin-bottom: 20px;
    }
    .btnRight{
        float: right;
        margin-right: 0px !important;
    }
    .searchArea{
        background:rgba(255,255,255,1);
        border-radius:2px;
        padding: 18px 18px 0;
    }
</style>
