<template>
  <el-dialog :title="!dataForm.id ? '新增' : '修改'" :close-on-click-modal="false" :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="120px">
      <el-form-item label="訂單id" prop="orderId">
        <el-input v-model="dataForm.orderId" placeholder="訂單id"></el-input>
      </el-form-item>
      <el-form-item label="操作人[用户；系統；後台管理員]" prop="operateMan">
        <el-input v-model="dataForm.operateMan" placeholder="操作人[用户；系統；後台管理員]"></el-input>
      </el-form-item>
      <el-form-item label="操作時間" prop="createTime">
        <el-input v-model="dataForm.createTime" placeholder="操作時間"></el-input>
      </el-form-item>
      <el-form-item label="訂單狀態【0->待付款；1->待發貨；2->已發貨；3->已完成；4->已關閉；5->無效訂單】" prop="orderStatus">
        <el-input v-model="dataForm.orderStatus" placeholder="訂單狀態【0->待付款；1->待發貨；2->已發貨；3->已完成；4->已關閉；5->無效訂單】"></el-input>
      </el-form-item>
      <el-form-item label="備註" prop="note">
        <el-input v-model="dataForm.note" placeholder="備註"></el-input>
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
        operateMan: "",
        createTime: "",
        orderStatus: "",
        note: "",
      },
      dataRule: {
        orderId: [
          { required: true, message: "訂單id不能為空", trigger: "blur" },
        ],
        operateMan: [
          {
            required: true,
            message: "操作人[用户；系統；後台管理員]不能為空",
            trigger: "blur",
          },
        ],
        createTime: [
          { required: true, message: "操作時間不能為空", trigger: "blur" },
        ],
        orderStatus: [
          {
            required: true,
            message:
              "訂單狀態【0->待付款；1->待發貨；2->已發貨；3->已完成；4->已關閉；5->無效訂單】不能為空",
            trigger: "blur",
          },
        ],
        note: [{ required: true, message: "備註不能為空", trigger: "blur" }],
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
              `/order/orderoperatehistory/info/${this.dataForm.id}`
            ),
            method: "get",
            params: this.$http.adornParams(),
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.orderId = data.orderOperateHistory.orderId;
              this.dataForm.operateMan = data.orderOperateHistory.operateMan;
              this.dataForm.createTime = data.orderOperateHistory.createTime;
              this.dataForm.orderStatus = data.orderOperateHistory.orderStatus;
              this.dataForm.note = data.orderOperateHistory.note;
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
              `/order/orderoperatehistory/${
                !this.dataForm.id ? "save" : "update"
              }`
            ),
            method: "post",
            data: this.$http.adornData({
              id: this.dataForm.id || undefined,
              orderId: this.dataForm.orderId,
              operateMan: this.dataForm.operateMan,
              createTime: this.dataForm.createTime,
              orderStatus: this.dataForm.orderStatus,
              note: this.dataForm.note,
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
