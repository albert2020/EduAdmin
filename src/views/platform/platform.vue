<template>
  <div v-cloak class="font16 hgt_full">
    <div class="flex_column hgt_full">
      <el-table
        ref="refElTabel"
        height="100%"
        :data="$store.getters.app.platformList"
        tooltip-effect="light"
        border
        style="width: 100%"
      >
        <el-table-column prop="Id" label="ID" width="50" />
        <el-table-column prop="Label" label="校区名称" width="310">
          <template slot-scope="scope">
            <span
              class="color-1f85aa font-w6 cursor"
              @click="openMoreOperationDialog(scope.$index, scope.row)"
            >{{ scope.row.Label }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="MasterLabel" label="负责人" width="80" />
        <el-table-column prop="Telephone" label="联系电话" width="100" />
        <el-table-column prop="MaxPerYear" label="年招生上限" width="100">
          <template slot-scope="scope">
            <span>{{ formatStudentNumber( scope.row.MaxPerYear) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="MaxAllYear" label="总招生上限" width="100">
          <template slot-scope="scope">
            <span>{{ formatStudentNumber( scope.row.MaxAllYear) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="Address" label="地址" :show-overflow-tooltip="true" width="300" />
        <el-table-column prop="Description" label="备注" :show-overflow-tooltip="true" />
        <el-table-column label="网址" :show-overflow-tooltip="true" width="250">
          <template slot-scope="scope">
            <a :href="scope.row.Domain" target="_blank">{{scope.row.Domain}}</a>
          </template>
        </el-table-column>
      </el-table>
      <div class="between-center m-v-15">
        <el-button type="primary" @click="openNewItem( )">新增校区</el-button>
      </div>
    </div>
    <!-- 弹出框 -->
    <!-- 更多操作弹窗 -->
    <my-dialog :visible.sync="moreOperationDialog" :close-show="true" :title="customFormData.Label">
      <!-- 展示校区的基本信息 -->
      <div slot="left_content">
        <platformRowDetail :formItemData="customFormData" />
      </div>
      <div slot="right_content" class="p_both20 p-b-20">
        <el-tabs @tab-click="onChangeTabs">
          <el-tab-pane id="zlxzqx" label="资料下载权限" name="zlxzqx">
            <platformRight :formItemData="customFormData" />
          </el-tab-pane>
          <el-tab-pane id="gly" label="设置负责人" name="gly">
            <setPlatformMaster :formItemData="customFormData" />
          </el-tab-pane>
        </el-tabs>
      </div>
    </my-dialog>

    <!-- 新增校区信息弹出框 -->
    <el-dialog
      :visible.sync="editDialog"
      width="500px"
      :title="customFormData.Id>0?'编辑'+customFormData.Label:'新增校区'"
    >
      <platformRowDetail :editEnable="true" :formItemData="customFormData" />
    </el-dialog>
  </div>
</template>

<script>
import platformRight from "@/views/system/component/platformRight";
import platformRowDetail from "@/views/system/component/platformRowDetail";
import setPlatformMaster from "@/views/system/component/setPlatformMaster";
import myDialog from "@/components/myDialog/myDialog";
export default {
  name: "setPlatform",
  components: {
    myDialog,
    platformRowDetail,
    setPlatformMaster,
    platformRight
  },
  data() {
    return {
      // 更多操作弹窗
      moreOperationDialog: false,
      // 更多操作弹窗
      editDialog: false,
      // 模态框获得的单条数据
      customFormData: {},
      // 当前操作平台的索引
      activeName: "zlxzqx"
    };
  },
  mounted() {
    this.getAllPlatform();
  },
  methods: {
    onChangeTabs(item) {
      item.$children[0].fire();
    },
    formatStudentNumber(studentnum) {
      if (studentnum == 0) {
        return "无限制";
      }
      return "上限:" + studentnum;
    },

    // 打开校区的弹出框
    openNewItem() {
      this.editDialog = true;
      this.customFormData = {};
    },
    // 获取所有平台的信息
    getAllPlatform() {
      this.$store.dispatch("app/getPlatformList").then(() => {});
    },
    // 打开更多操作的弹出框
    openMoreOperationDialog(index, row) {
      this.currentPlatformIndex = index;
      this.customFormData = row;
      this.moreOperationDialog = true;
    }
  }
};
</script>
<style scoped>
</style>
