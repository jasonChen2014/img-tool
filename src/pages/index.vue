<template>
    <div id="container">
        <el-container v-if="!dialogFormVisible">
            <el-header>
                <el-row>
                <el-form ref="form" :model="form" label-width="80px">
                    <el-col :span="6">
                        <el-form-item label="企业名称">
                            <el-select v-model="form.ent_name" @change="ent_namechange" placeholder="请选择企业">
                                <el-option v-for="ent_name in ent_names" :key="ent_name" :label="ent_name" :value="ent_name"></el-option>
                            </el-select>
                        </el-form-item>
                    </el-col>
                    <el-col :span="6">
                        <el-form-item label="企业用户">
                            <el-select v-model="form.ent_username" @change="ent_usernamechange" placeholder="请选择用户">
                                <el-option v-for="ent_username in ent_usernames" :key="ent_username" :label="ent_username" :value="ent_username"></el-option>
                            </el-select>
                        </el-form-item>
                    </el-col>
                    <el-col :span="6">
                        <el-form-item label="店内码">
                            <el-select v-model="form.ent_goods_code" @change="ent_goods_codechange" placeholder="请选择用户">
                                <el-option v-for="ent_goods_code in ent_goods_codes" :key="ent_goods_code" :label="ent_goods_code" :value="ent_goods_code"></el-option>
                            </el-select>
                        </el-form-item>
                    </el-col>
                </el-form>
                </el-row>
            </el-header>
            <el-container>
                <el-aside width="300px">
                    <ul class="ent_goods_imgs">
                        <li class="ent_good_img" v-for="ent_goods_pic in ent_goods_pics">
                            <img :src="ent_goods_pic" />
                        </li>
                    </ul>
                </el-aside>
                <el-main>
                    <div class="ent_good_form">
                        <el-form ref="form" :model="form" label-width="80px">
                            <el-form-item label="通用名">
                                <el-input v-model="form.picGoodsCommonName"></el-input>
                            </el-form-item>
                            <el-form-item label="生产厂家">
                                <el-input v-model="form.picFactoryName"></el-input>
                            </el-form-item>
                            <el-form-item label="规格">
                                <el-input v-model="form.picSpec"></el-input>
                            </el-form-item>
                            <el-form-item label="条形码">
                                <el-input v-model="form.picBarCode"></el-input>
                            </el-form-item>
                            <el-form-item label="批准文号">
                                <el-input v-model="form.picApprovalNo"></el-input>
                            </el-form-item>

                            <el-form-item label="商标">
                                <el-input v-model="form.picGoodsBrand"></el-input>
                            </el-form-item>
                            <el-form-item label="商品名">
                                <el-input v-model="form.picProductName"></el-input>
                            </el-form-item>

                            <el-form-item>
                                <el-button type="primary" @click="onSubmit">确认</el-button>
                                <el-button>取消</el-button>
                            </el-form-item>
                        </el-form>
                    </div>
                </el-main>
            </el-container>
        </el-container>
        <el-dialog title="设置用户名" :visible.sync="dialogFormVisible">
            <el-form :model="form">
                <el-form-item label="用户名" :label-width="formLabelWidth">
                    <el-input v-model="form.name" auto-complete="off"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="submitName">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>

import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

<script>

export default {
    data() {
        return {
            form: {
                name: '',
                ent_name: '',
                ent_username: '',
                ent_goods_code: '',

                picGoodsCommonName: '',
                picFactoryName: '',
                picSpec: '',
                picBarCode: '',
                picApprovalNo: '',

                picGoodsBrand: '',
                picProductName: ''
            },
            ent_names: ['万宁','瑞康'],
            ent_usernames: ['hxchen','canjie'],
            ent_goods_codes: ['111','222','333'],
            ent_goods_pics: [],
            ent_goods: [
                {
                    ent_goods_code: '111',
                    pic_goods_common_name: '云南白药',
                    pic_factory_name: '',
                    pic_spec: '',
                    pic_barcode: '',
                    pic_approval_no: '',
                    imgs: [''],
                    pic_goods_brand: '',
                    pic_product_name: ''
                },
                {
                    ent_goods_code: '222',
                    pic_goods_common_name: '云南白药',
                    pic_factory_name: '',
                    pic_spec: '',
                    pic_barcode: '',
                    pic_approval_no: '',
                    imgs: [''],
                    pic_goods_brand: '',
                    pic_product_name: ''
                },
                {
                    ent_goods_code: '333',
                    pic_goods_common_name: '云南白药',
                    pic_factory_name: '',
                    pic_spec: '',
                    pic_barcode: '',
                    pic_approval_no: '',
                    imgs: [''],
                    pic_goods_brand: '',
                    pic_product_name: ''
                }
            ],
            formLabelWidth: '120px',
            dialogFormVisible: localStorage.gj_username?false:true
        }
    },
    mounted(){ 
        // 权限验证& 并做跳转
        this.router()
    },
    methods: {
        // 做权限跳转
        router: function() {
            var self = this;
            self.$axios.get('https://data.gaojihealth.cn/picdeal/deal/allcompony').then(function(res){
                if(res.data.success) {
                    console.log('hehe');
                    self.ent_names = res.data.list;
                }
            });
        },
        submitName: function(){
            if(this.form.name == ''){
                alert('请输入用户名');
            }else{
                localStorage.gj_username = this.form.name;
                this.dialogFormVisible = false;
            }
        },
        ent_namechange: function(value){
            console.log(value);
            var self = this;
            let ent_name = value;
            self.$axios.get('https://data.gaojihealth.cn/picdeal/deal/username/'+ent_name).then(function(res){
                //console.log(res);
                if(res.data.success) {
                    //console.log('hehe');
                    self.ent_usernames = res.data.list;
                }
            });
        },
        ent_usernamechange: function(value) {
            console.log(value);
            var self = this;
            let ent_username = value;
            let ent_name = self.form.ent_name;
            self.$axios.get('https://data.gaojihealth.cn/picdeal/deal/shop/'+ent_name+'/'+ent_username).then(function(res){
                console.log(res);
                if(res.data.success) {
                    //console.log('hehe');
                    self.ent_goods_codes = res.data.list;
                }
            });
        },
        ent_goods_codechange: function(value){
            var self = this;
            let ent_goods_code = value;
            let ent_name = self.form.ent_name;
            let ent_username = self.form.ent_username;
            self.$axios.all([self.getPics(ent_name,ent_username,ent_goods_code), self.getForm1(ent_name,ent_username,ent_goods_code),self.getForm2(ent_name,ent_username,ent_goods_code)]).then(self.$axios.spread(function (res1, res2,res3) {
                // 三个请求现在都执行完成
                
                if(res1.data.success) {
                    self.ent_goods_pics = res1.data.list.map(function(value,index){
                        return "https://data.gaojihealth.cn/picdeal/deal/img/"+self.form.ent_name+"/"+self.form.ent_username+"/"+self.form.ent_goods_code+"/"+value.replace(/\.jpg$/,'');
                    });
                }
                if(res2.data.success && res3.data.success) {
                    var d2 = res2.data,d3 = res3.data;
                    self.form.picGoodsCommonName = d3.goods_common_name || d2.goods_product_name;
                    self.form.picFactoryName = d3.factory_name || d2.production_company;
                    self.form.picSpec = d3.barcode_spec || d2.ent_goods_spec;
                    self.form.picBarCode = d3.barcode || d2.barcode;
                    self.form.picApprovalNo = d3.approval_no || d2.approval_no;
                }else{
                    self.$alert('不存在该商品', '提示', {
                        confirmButtonText: '确定',
                    });
                }
            }));
            
            
        },
        getPics: function(ent_name,ent_username,ent_goods_code) {
            var self = this;
            return self.$axios.get('https://data.gaojihealth.cn/picdeal/deal/pic/'+ent_name+'/'+ent_username+'/'+ent_goods_code)
        },
        getForm1: function(ent_name,ent_username,ent_goods_code){
            var self = this;
            return self.$axios.get('https://data.gaojihealth.cn/picdeal/deal/formalgaojigoods/'+ent_name+'/'+ent_goods_code+'/20180718')
        },
        getForm2: function(ent_name,ent_username,ent_goods_code){
            var self = this;
            return self.$axios.get('https://data.gaojihealth.cn/picdeal/deal/feedback2/'+ent_name+'/'+ent_goods_code+'/20180718')
        },
        onSubmit: function(){
            let self = this;
            let entName = self.form.ent_name;
            let entGoodsCode = self.form.ent_goods_code;

            let picGoodsCommonName = self.form.picGoodsCommonName;
            let picFactoryName = self.form.picFactoryName;
            let picSpec = self.form.picSpec;
            let picBarCode = self.form.picBarCode;
            let picApprovalNo = self.form.picApprovalNo;

            let picGoodsBrand = self.form.picGoodsBrand;
            let picProductName = self.form.picProductName;
            //拼接路由
            let url = 'https://data.gaojihealth.cn/picdeal/deal/entpic/'+entName+'/'+entGoodsCode+'/'+picGoodsBrand+'/'+picProductName+'/'+picGoodsCommonName+'/'+picFactoryName+'/'+picSpec+'/'+picBarCode+'/'+picApprovalNo;
            self.$axios.get(url).then(function(res){
                if(res.data.success) {
                    self.$message({
                        message: '恭喜你，提交成功',
                        type: 'success'
                    });
                }
            });
        }
    }
}
</script>

<style scoped>
    .el-header {
        margin-top:22px;
        border-bottom: 1px solid #DCDFE6;
        box-shadow: 1px 1px 3px #DCDFE6;
    }
    .el-aside {
        /*border-right: 1px solid #DCDFE6;*/
    }
    .el-row {
        margin-bottom: 20px;
        &:last-child {
        margin-bottom: 0;
        }
    }
    .ent_good_img {
        margin: 20px;
        list-style:none;
    }
    .ent_good_img img{
        width: 260px;
        height: 260px;
    }
    .ent_good_form{
        width: 60%;
        margin: 0 auto;
        margin-top: 22px;
    }

</style>