<template>
  <!-- 封装右侧弹出框 -->
  <div>
    <el-drawer
      :wrapperClosable="false"
      :title="title"
      :visible.sync="visible"
      direction="rtl"
      class="myDrawer"
      :before-close="handleClose"
      :modal-append-to-body="false"
      size="70%"
    >
      <div class="flex_dom drawer_body">
        <div v-if="showLeft" class="left_content flex_column">
          <div class="is-scroll-left hgt_100 left_body_div flex_1">
            <slot name="left_content">展示基本信息区域</slot>
          </div>
        </div>
        <div v-if="showLeft" style="width:5px;height:100%;  background:#e0e3ea; "></div> 
        <div class="right_content" :class="{'no_left':!showLeft}">
          <div class="is-scroll-left hgt_100 right_body_div">
            <slot name="right_content">default left_content</slot>
          </div>
        </div>
      </div>
    </el-drawer>
  </div>
</template>
<script>
export default {
  props: {
    // 是否显示模态框
    visible: {
      type: Boolean,
      default: false
    },

    showLeft: {
      type: Boolean,
      default: true
    },
    title: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      showDialog: false
    };
  },
  computed: {
    style() {
      const style = {};
      if (this.width) {
        style.width = this.width;
      }
      return style;
    }
  },
  mounted() {},
  methods: {
    handleClose(done) {
      this.$emit("update:visible", false);
    }
  }
};
</script>
<style scoped>
.drawer_body {
  overflow: hidden;
  height: 100%;
  width: 100%;
  color: #303133;
  font-size: 14px;
}
.left_content {
  overflow: hidden;
  width: 300px;
  height: 100%;
  margin: 0px; 
  border-right: none;
}
.right_content {
  overflow: hidden;
  width: calc(100% - 320px);
  height: 100%;
  margin-right: 0px;
  /* border: 5px solid #e0e3ea; */
}

.no_left {
  width: calc(100% - 20px);
  margin-left: 0px;
}
.left_body_div {
  overflow-y: scroll;
  width: 100%;
}
.right_body_div {
  width: 100%;
  overflow-y: scroll;
}
.myDrawer >>> .el-drawer__header {
  padding: 15px;
  background: rgb(48, 65, 86);
  margin-bottom: 1px;
  color: #ffffff;
}
.myDrawer >>> .el-drawer__header>span{
   outline: none;
}
.myDrawer >>> .el-dialog__close {
  color: #ffffff;
}
.myDrawer >>> .el-drawer__body {
  height: 100%;
  overflow: hidden;
}
.myDrawer >>> .el-drawer__close-btn {
  outline: none;
}
</style>
