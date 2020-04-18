<template>
  <div>
    <div style="padding:10px">
      <el-button type="success" @click="add=true">添加学生</el-button>
      <el-dialog title="学生信息" :visible.sync="add" :append-to-body="true">
        <el-form :model="form">
          <el-form-item label="学生系别" :label-width="formLabelWidth">
            <el-input v-model="form2.obj" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="学生姓名" :label-width="formLabelWidth">
            <el-input v-model="form2.name" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="学生地址" :label-width="formLabelWidth">
            <el-input v-model="form2.add" autocomplete="off"></el-input>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="add = false">取 消</el-button>
          <el-button type="primary" @click="addone()">确 定</el-button>
        </div>
      </el-dialog>
    </div>

    <el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="obj" label="系别" width="180"></el-table-column>
      <el-table-column prop="name" label="姓名" width="180"></el-table-column>
      <el-table-column prop="address" label="地址"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>

        <el-dialog title="学生信息" :visible.sync="dialogFormVisible" :append-to-body="true">
          <el-form :model="form">
            <el-form-item label="学生系别" :label-width="formLabelWidth">
              <el-input v-model="form.obj" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="学生姓名" :label-width="formLabelWidth">
              <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="学生地址" :label-width="formLabelWidth">
              <el-input v-model="form.add" autocomplete="off"></el-input>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary" @click="edit()">确 定</el-button>
          </div>
        </el-dialog>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [
        {
          obj: "计科系",
          name: "李青",
          address: "上海市普陀区金沙江路 1518 弄"
        },
        {
          obj: "电子系",
          name: "盖伦",
          address: "上海市普陀区金沙江路 1517 弄"
        },
        {
          obj: "汽车工程系",
          name: "亚索",
          address: "上海市普陀区金沙江路 1519 弄"
        },
        {
          obj: "艺术系",
          name: "赵信",
          address: "上海市普陀区金沙江路 1516 弄"
        }
      ],

      dialogFormVisible: false,
      add: false,
      form: {
        name: "",
        obj: "",
        add: ""
      },
      form2: {
        name: "",
        obj: "",
        add: ""
      },
      formLabelWidth: "120px",
      id: ""
    };
  },
  methods: {
    open1() {
      this.$notify({
        title: "修改成功",
        message: "这是一条提示消息",
        type: "success"
      });
    },
    open4() {
      this.$notify.error({
        title: "删除成功",
        message: "这是一条提示消息"
      });
    },
    show() {
      console.log("???");
      this.$ajax
        .get(
          "https://5d644ebb-5faa-4a84-880e-2584fcf37210.mock.pstmn.io/mockget?jay=1"
        )
        .then(res => {
          console.log(res);
        //  this.tableData = res.data
          // var data =  JSON.parse(res.data)
          // console.log(data );
        });
    },
    handleEdit(index, row) {
      this.dialogFormVisible = true;

      this.form.name = row.name;
      this.form.obj = row.obj;
      this.form.add = row.address;
      this.id = index;
    },
    handleDelete(index, row) {
      this.id = index;
      this.tableData.splice(this.id, 1);
      this.open4();
    },
    edit() {
      if (
        this.form.obj.length == 0 ||
        this.form.name.length == 0 ||
        this.form.add.length == 0
      ) {
        return this.$message.error("不能为空");
      } else {
        this.tableData[this.id].name = this.form.name;
        this.tableData[this.id].obj = this.form.obj;
        this.tableData[this.id].address = this.form.add;

        this.dialogFormVisible = false;

        this.open1();
      }
    },

    addone() {
      var student = {};
      student["obj"] = this.form2.obj;
      student["name"] = this.form2.name;
      student["address"] = this.form2.add;

      if (
        this.form2.obj.length == 0 ||
        this.form2.name.length == 0 ||
        this.form2.add.length == 0
      ) {
        return this.$message.error("不能为空");
      }

      this.tableData.push(student);
      this.add = false;
    }
  },
  created() {
    this.show();
  }
};
</script>

<style  scoped>
</style>