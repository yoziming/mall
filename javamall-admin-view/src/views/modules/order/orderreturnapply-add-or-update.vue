<template>
  <el-dialog :title="!dataForm.id ? '新增' : '修改'" :close-on-click-modal="false" :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="120px">
      <el-form-item label="order_id" prop="orderId">
        <el-input v-model="dataForm.orderId" placeholder="order_id" :disabled="true"></el-input>
      </el-form-item>
      <el-form-item label="訂單編號" prop="orderSn">
        <el-input v-model="dataForm.orderSn" placeholder="訂單編號" :disabled="true"></el-input>
      </el-form-item>
      <el-form-item label="申請時間" prop="createTime">
        <el-input v-model="dataForm.createTime" placeholder="申請時間" :disabled="true"></el-input>
      </el-form-item>
      <el-form-item label="會員用户名" prop="memberUsername">
        <el-input v-model="dataForm.memberUsername" placeholder="會員用户名" :disabled="true"></el-input>
      </el-form-item>
      <el-form-item label="退款金額" prop="returnAmount">
        <el-input v-model="dataForm.returnAmount" placeholder="退款金額"></el-input>
      </el-form-item>
      <el-form-item label="退貨人姓名" prop="returnName">
        <el-input v-model="dataForm.returnName" placeholder="退貨人姓名"></el-input>
      </el-form-item>
      <el-form-item label="退貨人電話" prop="returnPhone">
        <el-input v-model="dataForm.returnPhone" placeholder="退貨人電話"></el-input>
      </el-form-item>
      <el-form-item label="申請狀態" prop="status">
        <el-select v-model="dataForm.status" placeholder="訂單狀態">
          <el-option label="待處理" :value="0"></el-option>
          <el-option label="退貨中" :value="1"></el-option>
          <el-option label="已完成" :value="2"></el-option>
          <el-option label="已拒絕" :value="3"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="原因" prop="reason">
        <el-input v-model="dataForm.reason" placeholder="原因"></el-input>
      </el-form-item>
      <el-form-item label="描述" prop="description">
        <el-input v-model="dataForm.description" placeholder="描述"></el-input>
      </el-form-item>
      <el-form-item label="處理備註" prop="handleNote">
        <el-input v-model="dataForm.handleNote" placeholder="處理備註"></el-input>
      </el-form-item>
      <el-form-item label="收貨人" prop="receiveMan">
        <el-input v-model="dataForm.receiveMan" placeholder="收貨人"></el-input>
      </el-form-item>
      <el-form-item label="收貨電話" prop="receivePhone">
        <el-input v-model="dataForm.receivePhone" placeholder="收貨電話"></el-input>
      </el-form-item>
      <el-form-item label="公司收貨地址" prop="companyAddress">
        <el-input v-model="dataForm.companyAddress" placeholder="公司收貨地址"></el-input>
      </el-form-item>
      <el-form-item label="收貨時間" prop="receiveTime">
        <el-input v-model="dataForm.receiveTime" placeholder="收貨時間"></el-input>
      </el-form-item>
      <el-form-item label="收貨備註" prop="receiveNote">
        <el-input v-model="dataForm.receiveNote" placeholder="收貨備註"></el-input>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">確定</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data() {
    return {
      visible: false,
      dataForm: {
        id: 0,
        orderId: "",
        skuId: "",
        orderSn: "",
        createTime: "",
        memberUsername: "",
        returnAmount: "",
        returnName: "",
        returnPhone: "",
        status: "",
        handleTime: "",
        skuImg: "",
        skuName: "",
        skuBrand: "",
        skuAttrsVals: "",
        skuCount: "",
        skuPrice: "",
        skuRealPrice: "",
        reason: "",
        description: "",
        descPics: "",
        handleNote: "",
        handleMan: "",
        receiveMan: "",
        receiveTime: "",
        receiveNote: "",
        receivePhone: "",
        companyAddress: "",
      },
      dataRule: {
        orderId: [
          { required: true, message: "order_id不能為空", trigger: "blur" },
        ],
        skuId: [
          { required: true, message: "退貨商品id不能為空", trigger: "blur" },
        ],
        orderSn: [
          { required: true, message: "訂單編號不能為空", trigger: "blur" },
        ],
        createTime: [
          { required: true, message: "申請時間不能為空", trigger: "blur" },
        ],
        memberUsername: [
          { required: true, message: "會員用户名不能為空", trigger: "blur" },
        ],
        returnAmount: [
          { required: true, message: "退款金額不能為空", trigger: "blur" },
        ],
        status: [
          { required: true, message: "申請狀態不能為空", trigger: "blur" },
        ],
        skuImg: [
          { required: true, message: "商品圖片不能為空", trigger: "blur" },
        ],
        skuName: [
          { required: true, message: "商品名稱不能為空", trigger: "blur" },
        ],
        skuBrand: [
          { required: true, message: "商品品牌不能為空", trigger: "blur" },
        ],
        skuAttrsVals: [
          {
            required: true,
            message: "商品銷售屬性(JSON)不能為空",
            trigger: "blur",
          },
        ],
        skuCount: [
          { required: true, message: "退貨數量不能為空", trigger: "blur" },
        ],
        skuPrice: [
          { required: true, message: "商品單價不能為空", trigger: "blur" },
        ],
        skuRealPrice: [
          {
            required: true,
            message: "商品實際支付單價不能為空",
            trigger: "blur",
          },
        ],
        reason: [{ required: true, message: "原因不能為空", trigger: "blur" }],
        descPics: [
          {
            required: true,
            message: "憑證圖片，以逗號隔開不能為空",
            trigger: "blur",
          },
        ],
        handleMan: [
          { required: true, message: "處理人員不能為空", trigger: "blur" },
        ],
        receiveMan: [
          { required: true, message: "收貨人不能為空", trigger: "blur" },
        ],
        receivePhone: [
          { required: true, message: "收貨電話不能為空", trigger: "blur" },
        ],
        companyAddress: [
          { required: true, message: "公司收貨地址不能為空", trigger: "blur" },
        ],
      },
    };
  },
  methods: {
    init(id) {
      this.dataForm.id = id || 0;
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.id) {
          this.$http({
            url: this.$http.adornUrl(
              `/order/orderreturnapply/info/${this.dataForm.id}`
            ),
            method: "get",
            params: this.$http.adornParams(),
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.orderId = data.orderReturnApply.orderId;
              this.dataForm.skuId = data.orderReturnApply.skuId;
              this.dataForm.orderSn = data.orderReturnApply.orderSn;
              this.dataForm.createTime = data.orderReturnApply.createTime;
              this.dataForm.memberUsername =
                data.orderReturnApply.memberUsername;
              this.dataForm.returnAmount = data.orderReturnApply.returnAmount;
              this.dataForm.returnName = data.orderReturnApply.returnName;
              this.dataForm.returnPhone = data.orderReturnApply.returnPhone;
              this.dataForm.status = data.orderReturnApply.status;
              this.dataForm.handleTime = data.orderReturnApply.handleTime;
              this.dataForm.skuImg = data.orderReturnApply.skuImg;
              this.dataForm.skuName = data.orderReturnApply.skuName;
              this.dataForm.skuBrand = data.orderReturnApply.skuBrand;
              this.dataForm.skuAttrsVals = data.orderReturnApply.skuAttrsVals;
              this.dataForm.skuCount = data.orderReturnApply.skuCount;
              this.dataForm.skuPrice = data.orderReturnApply.skuPrice;
              this.dataForm.skuRealPrice = data.orderReturnApply.skuRealPrice;
              this.dataForm.reason = data.orderReturnApply.reason;
              this.dataForm.description述 = data.orderReturnApply.description述;
              this.dataForm.descPics = data.orderReturnApply.descPics;
              this.dataForm.handleNote = data.orderReturnApply.handleNote;
              this.dataForm.handleMan = data.orderReturnApply.handleMan;
              this.dataForm.receiveMan = data.orderReturnApply.receiveMan;
              this.dataForm.receiveTime = data.orderReturnApply.receiveTime;
              this.dataForm.receiveNote = data.orderReturnApply.receiveNote;
              this.dataForm.receivePhone = data.orderReturnApply.receivePhone;
              this.dataForm.companyAddress =
                data.orderReturnApply.companyAddress;
            }
          });
        }
      });
    },
    // 表單提交
    dataFormSubmit() {
      this.$refs["dataForm"].validate((valid) => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl(
              `/order/orderreturnapply/${!this.dataForm.id ? "save" : "update"}`
            ),
            method: "post",
            data: this.$http.adornData({
              id: this.dataForm.id || undefined,
              orderId: this.dataForm.orderId,
              skuId: this.dataForm.skuId,
              orderSn: this.dataForm.orderSn,
              createTime: this.dataForm.createTime,
              memberUsername: this.dataForm.memberUsername,
              returnAmount: this.dataForm.returnAmount,
              returnName: this.dataForm.returnName,
              returnPhone: this.dataForm.returnPhone,
              status: this.dataForm.status,
              handleTime: this.dataForm.handleTime,
              skuImg: this.dataForm.skuImg,
              skuName: this.dataForm.skuName,
              skuBrand: this.dataForm.skuBrand,
              skuAttrsVals: this.dataForm.skuAttrsVals,
              skuCount: this.dataForm.skuCount,
              skuPrice: this.dataForm.skuPrice,
              skuRealPrice: this.dataForm.skuRealPrice,
              reason: this.dataForm.reason,
              description述: this.dataForm.description述,
              descPics: this.dataForm.descPics,
              handleNote: this.dataForm.handleNote,
              handleMan: this.dataForm.handleMan,
              receiveMan: this.dataForm.receiveMan,
              receiveTime: this.dataForm.receiveTime,
              receiveNote: this.dataForm.receiveNote,
              receivePhone: this.dataForm.receivePhone,
              companyAddress: this.dataForm.companyAddress,
            }),
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.$message({
                message: "操作成功",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.visible = false;
                  this.$emit("refreshDataList");
                },
              });
            } else {
              this.$message.error(data.msg);
            }
          });
        }
      });
    },
  },
};
</script>
