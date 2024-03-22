<template>
  <j-modal
    :title="title"
    :width="width"
    :visible="visible"
    :confirmLoading="confirmLoading"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">
    <a-spin :spinning="confirmLoading">
      <a-form :form="form">
        <a-form-item label="快递单号" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['serial',validatorRules.serial]" placeholder="请输入快递单号"></a-input> <!-- :disabled="!!model.id" -->
        </a-form-item>
        <a-form-item label="快递名称" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['name',validatorRules.name]" placeholder="请输入快递名称"></a-input>
          </a-form-item>
          <a-form-item label="发件人" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <a-input v-decorator="['backup1',validatorRules.backup1]" placeholder="请输入发件人"></a-input>
          </a-form-item>
          <a-form-item label="发件人手机号" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <a-input v-decorator="['ations',validatorRules.ations]" placeholder="请输入发件人手机号"></a-input>
          </a-form-item>
        <a-form-item label="收件人" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['supplier',validatorRules.supplier]" placeholder="请输入收件人"></a-input>
        </a-form-item>
        <a-form-item label="收件人手机号" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['backup2',validatorRules.backup2]" placeholder="请输入收件人手机号"></a-input>
        </a-form-item>
        <a-form-item label="始发地" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['backup3',validatorRules.backup3]" placeholder="请输入始发地"></a-input>
        </a-form-item>
        <a-form-item label="目的地" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['backup4',validatorRules.backup4]" placeholder="请输入目的地"></a-input>
        </a-form-item>
        <a-form-item label="重量" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['backup5',validatorRules.backup5]" placeholder="请输入重量"></a-input>
        </a-form-item>
        <a-form-item label="体积单位" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-select v-decorator="['unit',validatorRules.unit]" placeholder="体积单位">
            <template v-for="(unit,index) in unitList">
              <a-select-option v-bind:value="unit.itemValue">{{unit.itemText}}</a-select-option>
            </template>
          </a-select>
        </a-form-item>
        <a-form-item label="仓库" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-select v-decorator="['warehouse', {rules: [{required: true, message: '请选择仓库'}]}]" placeholder="请选择仓库">
            <a-select-option v-for="warehouse in warehouses" :key="warehouse.id" :value="warehouse.id">
              {{ warehouse.name }}
            </a-select-option>
          </a-select>
        </a-form-item>
        <a-form-item label="库位" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-select v-decorator="['storageLocation', {rules: [{required: true, message: '请选择库位'}]}]" placeholder="请选择库位">
            <a-select-option v-for="location in storageLocations" :key="location.id" :value="location.id">
              {{ location.name }}
            </a-select-option>
          </a-select>
        </a-form-item>

        <!--        <a-form-item label="容积" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
<!--          <a-input v-decorator="['materialVolume',validatorRules.materialVolume]" placeholder="请输入物料容积"></a-input>-->
<!--        </a-form-item>-->
      </a-form>
    </a-spin>
  </j-modal>
</template>

<script>

  import pick from 'lodash.pick'
  import JDate from '@/components/jeecg/JDate'
  import JDictSelectTag from '@/components/dict/JDictSelectTag'
  import {httpAction, getAction} from '@/api/manage'
  import {duplicateCheck} from '@/api/api'

  export default {
    name: "MaterialModal",
    components: {
      JDate,
      JDictSelectTag
    },
    data () {
      return {
        form: this.$form.createForm(this),
        title:"操作",
        width:800,
        visible: false,
        model: {},
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        confirmLoading: false,
        unitList: {},
        validatorRules: {
          serial: { rules: [{ required: true, message: '请输入快递单号' }] },
          name: { rules: [{ required: true, message: '请输入快递名称' }] },
          unit: { rules: [{ required: false, message: '请选择体积单位' }] },
          materialVolume: { rules: [{ required: false, message: '请输入容积' }] },
        },
        url: {
          add: "/cable/material/add",
          edit: "/cable/material/edit",
        }
      }
    },
    created () {
      this.unitLists();
    },
    methods: {
      materialJudge(rule, value, callback){
        if (!value) {
          callback()
        } else {
          var params = {
            tableName: 'material',
            fieldName: 'serial',
            fieldVal: value,
            dataId: this.model.id
          };
          duplicateCheck(params).then((res) => {
            if (res.success) {
              callback()
            } else {
              callback("该快递单号已存在!")
            }
          })

        }
      },
      unitLists() {
        getAction("/sys/dictItem/getDictItemUnit").then((res) => {
          if (res.success) {
            this.unitList = res.result;
          }
        });
      },
      add () {
        this.edit({});
      },
      edit (record) {
        if(!!record.unit) {
          record.unit = record.unit + '';
        }
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'serial','name','ations','supplier','unit', 'materialVolume', 'createTime','updateTime','createBy','updateBy','backup1','backup2','backup3','backup4','backup5'))
        })
      },
      close () {
        this.$emit('close');
        this.visible = false;
      },
      handleOk () {
        const that = this;
        // 触发表单验证
        this.form.validateFields((err, values) => {
          if (!err) {
            that.confirmLoading = true;
            let httpurl = '';
            let method = '';
            if(!this.model.id){
              httpurl+=this.url.add;
              method = 'post';
            }else{
              httpurl+=this.url.edit;
               method = 'put';
            }
            let formData = Object.assign(this.model, values);
            httpAction(httpurl,formData,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
              that.close();
            })
          }

        })
      },
      handleCancel () {
        this.close()
      },
      popupCallback(row){
        this.form.setFieldsValue(pick(row,'serial','name','ations','supplier','unit', 'materialVolume','createTime','updateTime','createBy','updateBy','backup1','backup2','backup3','backup4','backup5'))
      }
    }
  }
</script>