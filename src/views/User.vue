<template>
<div>



    <div style="padding: 10px 0">
      <el-input style="width: 200px" v-model="username"></el-input>
      <el-button style="margin-left: 5px" type="primary" @click="load">搜索</el-button>
      <el-button style="margin-left: 5px" @click="add">新增</el-button>
      <el-button style="margin-left: 5px" @click="exp">导出</el-button>
      <el-popconfirm
          title="这是一段内容确定删除吗？"
          @confirm="delBatch">
        <el-button slot="reference">批量删除</el-button>
      </el-popconfirm>
      <!--        <el-button style="margin-left: 5px" @click="delBatch">批量删除</el-button>-->
    </div>
    <el-table :data="tableData"
              @selection-change="handleSelectionChange">
      <el-table-column
          type="selection"
          width="55">
      </el-table-column>
      <el-table-column prop="id" label="ID" width="140">
      </el-table-column>
      <el-table-column prop="username" label="用户名" width="140">
      </el-table-column>
      <el-table-column prop="nickname" label="昵称" width="120">
      </el-table-column>
      <el-table-column prop="phone" label="电话" width="120">
      </el-table-column>
      <el-table-column prop="address" label="地址" width="140">
      </el-table-column>
      <el-table-column prop="address" label="邮箱" width="140">
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button
              size="mini"
              @click="handleEdit(scope.row)">编辑
          </el-button>
          <el-popconfirm
              title="这是一段内容确定删除吗？"
              @confirm="handleDelete(scope.row)">
            <el-button slot="reference">删除</el-button>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>

  <div style="padding: 10px 0">
    <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="pageNum"
        :page-sizes="[5, 10, 15, 20]"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total">
    </el-pagination>
  </div>

  <el-dialog title="收货地址" :visible.sync="dialogFormVisible">
    <el-form :model="form" label-width="80px">
      <el-form-item label="用户名">
        <el-input v-model="form.username" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="昵称">
        <el-input v-model="form.nickname" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="电话">
        <el-input v-model="form.phone" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="地址">
        <el-input v-model="form.address" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="邮箱">
        <el-input v-model="form.email" autocomplete="off"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="dialogFormVisible = false">取 消</el-button>
      <el-button type="primary" @click="addNew">确 定</el-button>
    </div>
  </el-dialog>
</div>
</template>

<script>
export default {
  name: "User",
  data(){
    return {
      tableData: [],
      total: 0,
      pageNum: 1,
      pageSize: 5,
      username: "",
      dialogFormVisible: false,
      form: {},
      multipleSelection: []
    }
  },
  created() {
    this.load()
  },
  methods:{
    handleSizeChange(pageSize) {
      console.log(pageSize)
      this.pageSize = pageSize
      this.load()
    },
    handleCurrentChange(pageNum) {
      console.log(pageNum)
      this.pageNum = pageNum
      this.load()
    },
    load() {
      this.request.get("/user/page?", {
        params: {
          pageNum: this.pageNum,
          pageSize: this.pageSize,
          username: this.username,
        }
      }).then(
          res => {
            console.log(res)
            this.tableData = res.records;
            this.total = res.total
          }
      )
    },
    add() {
      this.dialogFormVisible = true;
      this.form = {}
    },
    addNew() {
      this.dialogFormVisible = false
      this.request.post("/user", this.form).then(
          res => {
            console.log(res)
            if (res) {
              this.$message.success("保存成功")
            } else {
              this.$message.error("保存失败")
            }
            this.load()
          }
      )
    },
    handleEdit(row) {
      console.log(row)
      this.form = Object.assign({}, row)
      this.dialogFormVisible = true;
    },
    handleDelete(row) {
      console.log("hello")
      this.request.delete("/user/" + row.id).then(
          res => {
            console.log(res)
            if (res) {
              this.$message.success("保存成功")
            } else {
              this.$message.error("保存失败")
            }
            this.load()
          }
      )
    },
    handleSelectionChange(val) {
      console.log(val)
      this.multipleSelection = val
    },
    delBatch() {
      let ids = this.multipleSelection.map(v => v.id)
      this.request.post("/user/delete/batch",ids).then(
          res => {
            console.log(res)
            if (res) {
              this.$message.success("批量删除成功")
            } else {
              this.$message.error("批量删除失败")
            }
            this.load()
          }
      )
    },
    exp(){
      window.open("http://localhost:9090/user/export")
    }
  }
}
</script>

<style scoped>

</style>