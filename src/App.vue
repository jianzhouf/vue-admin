<template>
  <div id="app" style="width: 800px;">
    <el-form inline :model="form" ref="formRef">
      <el-form-item
        label="姓名"
        prop="name"
        :rules="{required: true, message: '域名不能为空', trigger: 'blur'}"
      >
        <el-input v-model="form.name"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
        <el-button @click="reset">重置</el-button>
      </el-form-item>
    </el-form>
    <div style="margin-bottom: 10px;">
      <el-button type="primary" @click="add">新增</el-button>
    </div>
    <el-table border :data="tableData" style="width: 100%">
      <el-table-column type="selection" width="55"></el-table-column>
      <el-table-column prop="date" label="日期" width="180"></el-table-column>
      <el-table-column prop="name" label="姓名" width="180"></el-table-column>
      <el-table-column prop="address" label="地址"></el-table-column>
    </el-table>
    <el-pagination
      style="display: flex;justify-content: space-around;padding-top: 10px;"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage4"
      :page-sizes="[5, 10, 20, 50]"
      :page-size="pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
    ></el-pagination>

    <el-dialog title="提示" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
      <el-form :model="addForm" ref="addFormRef">
        <el-form-item
          label="姓名"
          prop="name"
          :rules="{required: true, message: '域名不能为空', trigger: 'blur'}"
        >
          <el-input style="width: 220px" v-model="addForm.name"></el-input>
        </el-form-item>
        <el-form-item
          label="日期"
          prop="date"
          :rules="{required: true, message: '域名不能为空', trigger: 'blur'}"
        >
          <el-date-picker v-model="addForm.date"></el-date-picker>
        </el-form-item>
        <el-form-item
          label="地址"
          prop="address"
          :rules="{required: true, message: '域名不能为空', trigger: 'blur'}"
        >
          <el-input style="width: 220px" v-model="addForm.address"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogConfirm">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "app",
  components: {},
  data() {
    return {
      dialogVisible: false,
      pageSize: 10,
      currentPage4: 1,
      total: 400,
      form: {},
      addForm: {},
      tableData: [],
      sourceData: []
    };
  },
  created() {
    const arr = [];
    for (let i = 0; i < 1500; i++) {
      arr.push({
        date: "2016-05-02",
        name: "王小虎" + i,
        address: "上海市普陀区金沙江路 1518 弄"
      });
    }
    this.sourceData = arr;
    this.tableData = arr.slice(0, this.pageSize);
  },
  methods: {
    onSubmit() {
      if (this.form.name) {
        this.$refs.formRef.validate(valid => {
          this.tableData = this.sourceData.filter(
            c => c.name === this.form.name
          );
        });
      }
    },
    add() {
      this.dialogVisible = true;
    },
    reset() {
      this.$refs.formRef.resetFields();
      this.tableData = this.sourceData;
    },
    handleClose() {},
    dialogConfirm() {
      console.log(this.$refs.addFormRef);
      this.$refs.addFormRef.validate(valid => {
        this.addForm.date = this.addForm.date.toString();
        this.tableData.unshift(this.addForm);
        this.dialogVisible = false;
      });
    },
    handleSizeChange(val) {
      this.pageSize = val;
    },
    handleCurrentChange(val) {
      this.tableData = this.sourceData.slice(
        (val - 1) * this.pageSize,
        val * this.pageSize
      );
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
