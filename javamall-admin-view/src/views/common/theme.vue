<template>
  <el-form>
    <h2>佈局設置</h2>
    <el-form-item label="導航條類型">
      <el-radio-group v-model="navbarLayoutType">
        <el-radio label="default" border>default</el-radio>
        <el-radio label="inverse" border>inverse</el-radio>
      </el-radio-group>
    </el-form-item>
    <el-form-item label="側邊欄皮膚">
      <el-radio-group v-model="sidebarLayoutSkin">
        <el-radio label="light" border>light</el-radio>
        <el-radio label="dark" border>dark</el-radio>
      </el-radio-group>
    </el-form-item>
  </el-form>
</template>

<script>
export default {
  computed: {
    navbarLayoutType: {
      get() {
        return this.$store.state.common.navbarLayoutType;
      },
      set(val) {
        this.$store.commit("common/updateNavbarLayoutType", val);
      },
    },
    sidebarLayoutSkin: {
      get() {
        return this.$store.state.common.sidebarLayoutSkin;
      },
      set(val) {
        this.$store.commit("common/updateSidebarLayoutSkin", val);
      },
    },
  },
};
</script>

