<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-input v-model="dataForm.key" placeholder="參數名" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">查詢</el-button>
        <el-button v-if="isAuth('order:orderitem:save')" type="primary" @click="addOrUpdateHandle()">新增</el-button>
        <el-button v-if="isAuth('order:orderitem:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量刪除</el-button>
      </el-form-item>
    </el-form>
    <el-table :data="dataList" border v-loading="dataListLoading" @selection-change="selectionChangeHandle" style="width: 100%;">
      <el-table-column type="selection" header-align="center" align="center" width="50">
      </el-table-column>
      <el-table-column prop="id" header-align="center" align="center" label="id">
      </el-table-column>
      <el-table-column prop="orderId" header-align="center" align="center" label="order_id">
      </el-table-column>
      <el-table-column prop="orderSn" header-align="center" align="center" label="order_sn">
      </el-table-column>
      <el-table-column prop="spuId" header-align="center" align="center" label="spu_id">
      </el-table-column>
      <el-table-column prop="spuName" header-align="center" align="center" label="spu_name">
      </el-table-column>
      <el-table-column prop="spuPic" header-align="center" align="center" label="spu_pic">
      </el-table-column>
      <el-table-column prop="spuBrand" header-align="center" align="center" label="品牌">
      </el-table-column>
      <el-table-column prop="categoryId" header-align="center" align="center" label="商品分類id">
      </el-table-column>
      <el-table-column prop="skuId" header-align="center" align="center" label="商品sku編號">
      </el-table-column>
      <el-table-column prop="skuName" header-align="center" align="center" label="商品sku名字">
      </el-table-column>
      <el-table-column prop="skuPic" header-align="center" align="center" label="商品sku圖片">
      </el-table-column>
      <el-table-column prop="skuPrice" header-align="center" align="center" label="商品sku價格">
      </el-table-column>
      <el-table-column prop="skuQuantity" header-align="center" align="center" label="商品購買的數量">
      </el-table-column>
      <el-table-column prop="skuAttrsVals" header-align="center" align="center" label="商品銷售屬性組合（JSON）">
      </el-table-column>
      <el-table-column prop="promotionAmount" header-align="center" align="center" label="商品促銷分解金額">
      </el-table-column>
      <el-table-column prop="couponAmount" header-align="center" align="center" label="優惠券優惠分解金額">
      </el-table-column>
      <el-table-column prop="integrationAmount" header-align="center" align="center" label="積分優惠分解金額">
      </el-table-column>
      <el-table-column prop="realAmount" header-align="center" align="center" label="該商品經過優惠後的分解金額">
      </el-table-column>
      <el-table-column prop="giftIntegration" header-align="center" align="center" label="贈送積分">
      </el-table-column>
      <el-table-column prop="giftGrowth" header-align="center" align="center" label="贈送成長值">
      </el-table-column>
      <el-table-column fixed="right" header-align="center" align="center" width="150" label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.id)">修改</el-button>
          <el-button type="text" size="small" @click="deleteHandle(scope.row.id)">刪除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination @size-change="sizeChangeHandle" @current-change="currentChangeHandle" :current-page="pageIndex" :page-sizes="[10, 20, 50, 100]" :page-size="pageSize" :total="totalPage" layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
    <!-- 彈窗, 新增 / 修改 -->
    <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
  </div>
</template>

<script>
import AddOrUpdate from "./orderitem-add-or-update";
export default {
  data() {
    return {
      dataForm: {
        key: "",
      },
      dataList: [],
      pageIndex: 1,
      pageSize: 10,
      totalPage: 0,
      dataListLoading: false,
      dataListSelections: [],
      addOrUpdateVisible: false,
    };
  },
  components: {
    AddOrUpdate,
  },
  activated() {
    this.getDataList();
  },
  methods: {
    // 獲取數據列表
    getDataList() {
      this.dataListLoading = true;
      this.$http({
        url: this.$http.adornUrl("/order/orderitem/list"),
        method: "get",
        params: this.$http.adornParams({
          page: this.pageIndex,
          limit: this.pageSize,
          key: this.dataForm.key,
        }),
      }).then(({ data }) => {
        if (data && data.code === 0) {
          this.dataList = data.page.list;
          this.totalPage = data.page.totalCount;
        } else {
          this.dataList = [];
          this.totalPage = 0;
        }
        this.dataListLoading = false;
      });
    },
    // 每頁數
    sizeChangeHandle(val) {
      this.pageSize = val;
      this.pageIndex = 1;
      this.getDataList();
    },
    // 當前頁
    currentChangeHandle(val) {
      this.pageIndex = val;
      this.getDataList();
    },
    // 多選
    selectionChangeHandle(val) {
      this.dataListSelections = val;
    },
    // 新增 / 修改
    addOrUpdateHandle(id) {
      this.addOrUpdateVisible = true;
      this.$nextTick(() => {
        this.$refs.addOrUpdate.init(id);
      });
    },
    // 刪除
    deleteHandle(id) {
      var ids = id
        ? [id]
        : this.dataListSelections.map((item) => {
            return item.id;
          });
      this.$confirm(
        `確定對[id=${ids.join(",")}]進行[${id ? "刪除" : "批量刪除"}]操作?`,
        "提示",
        {
          confirmButtonText: "確定",
          cancelButtonText: "取消",
          type: "warning",
        }
      ).then(() => {
        this.$http({
          url: this.$http.adornUrl("/order/orderitem/delete"),
          method: "post",
          data: this.$http.adornData(ids, false),
        }).then(({ data }) => {
          if (data && data.code === 0) {
            this.$message({
              message: "操作成功",
              type: "success",
              duration: 1500,
              onClose: () => {
                this.getDataList();
              },
            });
          } else {
            this.$message.error(data.msg);
          }
        });
      });
    },
  },
};
</script>
