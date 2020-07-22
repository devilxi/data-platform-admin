<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="热源名称">
            <el-input v-model="form.feedname" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="大屏展示名称">
            <el-input v-model="form.shortname" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="热源类型">
            未设置字典，请手动设置 Select
          </el-form-item>
          <el-form-item label="热源位置">
            <el-input v-model="form.position" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="设计负荷">
            <el-input v-model="form.sjfh" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="设计采暖面积">
            <el-input v-model="form.tcnmj" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="实际采暖面积">
            <el-input v-model="form.fcnmj" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="热源性质">
            未设置字典，请手动设置 Select
          </el-form-item>
          <el-form-item label="所属分公司">
            未设置字典，请手动设置 Select
          </el-form-item>
          <el-form-item label="责任人">
            <el-input v-model="form.principal" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="状态">
            <el-input v-model="form.deleted" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="收费面积">
            <el-input v-model="form.sfmj" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="区域名称">
            未设置字典，请手动设置 Select
          </el-form-item>
          <el-form-item label="排序">
            <el-input v-model="form.orderid" style="width: 370px;" />
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button type="text" @click="crud.cancelCU">取消</el-button>
          <el-button :loading="crud.cu === 2" type="primary" @click="crud.submitCU">确认</el-button>
        </div>
      </el-dialog>
      <!--表格渲染-->
      <el-table ref="table" v-loading="crud.loading" :data="crud.data" size="small" style="width: 100%;" @selection-change="crud.selectionChangeHandler">
        <el-table-column type="selection" width="55" />
        <el-table-column prop="feedname" label="热源名称" />
        <el-table-column prop="shortname" label="大屏展示名称" />
        <el-table-column prop="type" label="热源类型" />
        <el-table-column prop="position" label="热源位置" />
        <el-table-column prop="sjfh" label="设计负荷" />
        <el-table-column prop="tcnmj" label="设计采暖面积" />
        <el-table-column prop="fcnmj" label="实际采暖面积" />
        <el-table-column prop="energytype" label="热源性质" />
        <el-table-column prop="companycode" label="所属分公司" />
        <el-table-column prop="principal" label="责任人" />
        <el-table-column prop="deleted" label="状态" />
        <el-table-column prop="sfmj" label="收费面积" />
        <el-table-column prop="qymc" label="区域名称" />
        <el-table-column prop="orderid" label="排序" />
        <el-table-column v-permission="['admin','scBasFeed:edit','scBasFeed:del']" label="操作" width="150px" align="center">
          <template slot-scope="scope">
            <udOperation
              :data="scope.row"
              :permission="permission"
            />
          </template>
        </el-table-column>
      </el-table>
      <!--分页组件-->
      <pagination />
    </div>
  </div>
</template>

<script>
import crudScBasFeed from '@/api/scBasFeed'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, feedcode: null, feedname: null, shortname: null, type: null, position: null, sjfh: null, tcnmj: null, fcnmj: null, energytype: null, companycode: null, principal: null, deleted: null, sfmj: null, xNum: null, yNum: null, qymc: null, orderid: null }
export default {
  name: 'ScBasFeed',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  cruds() {
    return CRUD({ title: '热源基础信息', url: 'api/scBasFeed', sort: 'feedcode,desc', crudMethod: { ...crudScBasFeed }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'scBasFeed:add'],
        edit: ['admin', 'scBasFeed:edit'],
        del: ['admin', 'scBasFeed:del']
      },
      rules: {
      }}
  },
  methods: {
    // 获取数据前设置好接口地址
    [CRUD.HOOK.beforeRefresh]() {
      return true
    }
  }
}
</script>

<style scoped>

</style>
