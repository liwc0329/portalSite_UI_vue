<template>
  <div class="app-container">
    <!--工具栏-->
    <div>
      <el-button
        class="filter-item"
        size="mini"
        type="primary"
        icon="el-icon-upload"
        style="float:left;margin: -10px 10px 0 0;padding: 5px;padding-right: 8px;"
        @click="toAdd"
      >发布
      </el-button>
      <el-button
        class="filter-item"
        size="mini"
        type="success"
        icon="el-icon-edit"
        style="float:left;margin: -10px 10px 0 0;padding: 5px;padding-right: 8px;"
        :disabled="crud.selections.length !== 1"
        @click="toEdit"
      >修改
      </el-button>
      <crudOperation style="float:left" />
    </div>
    <!--表格渲染-->
    <el-table
      v-loading="crud.loading"
      :data="crud.data"
      style="width: 100%;margin-top: 24px;"
      @selection-change="crud.selectionChangeHandler"
    >
      <el-table-column :selectable="checkboxT" type="selection" width="55" />
      <el-table-column prop="title" :show-overflow-tooltip="true" align="center" label="标题" />
      <el-table-column align="center" prop="pepole" label="员工名" />
      <el-table-column :show-overflow-tooltip="true" prop="honortitle" label="荣誉简介" align="center" />
      <el-table-column prop="author" label="作者" align="center" />
      <el-table-column :show-overflow-tooltip="true" align="center" prop="imageUrl" label="缩略图">
        <template slot-scope="{row}">
          <el-image
            :src="parseUrl(row.imageUrl)"
            :preview-src-list="[parseUrl(row.imageUrl)]"
            fit="contain"
            lazy
            class="el-avatar"
          />
        </template>
      </el-table-column>
      <el-table-column width="135" prop="publishdate" align="center" label="发布日期" />
    </el-table>
    <!--分页组件-->
    <pagination />
  </div>
</template>

<script>
import crudHonourPublish from '@/api/publish/honourPublish'
import { mapGetters } from 'vuex'
import CRUD, { presenter, header, crud } from '@crud/crud'
import crudOperation from '@crud/CRUD.operation'
import pagination from '@crud/Pagination'

// crud交由presenter持有
const defaultCrud = CRUD({ requestType: 'post', url: 'honour/getHonours', crudMethod: { ...crudHonourPublish }})
export default {
  name: 'Pictures',
  components: { crudOperation, pagination },
  mixins: [presenter(defaultCrud), header(), crud()],
  data() {
    return {

    }
  },
  computed: {
    ...mapGetters([
      'baseApi'
    ])
  },
  created() {
    this.crud.optShow.add = false
    this.crud.optShow.edit = false
  },
  methods: {
    toAdd() {
      this.$router.push({
        path: '/publish/honor/create/'
      })
      this.crud.selections.length = 0
    },
    toEdit() {
      this.$router.push({
        path: '/publish/honor/edit/',
        query: {
          row: this.crud.selections[0]
        }
      })
      this.crud.selections.length = 0
    },
    parseUrl(imgUrl) {
      return `${window.g.baseURL}${imgUrl}`
    },
    checkboxT(row, rowIndex) {
      return row
    }
  }
}
</script>

<style lang="scss" scoped>
  .el-dialog-div {
    height: 60vh;
    overflow: auto;
  }

  .drawer-item {
    font-size: 14px;
    padding: 12px 0;
  }

</style>
