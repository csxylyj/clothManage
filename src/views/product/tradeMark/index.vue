<template>
  <div>
    <el-button
      type="primary"
      icon="el-icon-plus"
      style="margin: 10px 0px"
      @click="showDialog"
      >添加</el-button
    >
    <el-dialog title="添加品牌" :visible.sync="dialogFormVisible">
      <el-form style="width: 80%">
        <el-form-item label="品牌名称" label-width="100px">
          <el-input autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="品牌LOGO" label-width="100px">
          <el-upload
            class="avatar-uploader"
            action=""
            :show-file-list="false"
            :on-success="handleAvatarSuccess"
            :before-upload="beforeAvatarUpload"
          >
            <img v-if="imageUrl" :src="imageUrl" class="avatar" />
            <i v-else class="el-icon-plus avatar-uploader-icon"></i>
            <div slot="tip" class="el-upload__tip">
              只能上传JPG文件，且不能超过2MB
            </div>
          </el-upload>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogFormVisible = false"
          >确 定</el-button
        >
      </div>
    </el-dialog>

    <el-table style="width: 100%" border :data="list">
      <el-table-column type="index" label="序号" width="80px" align="center">
      </el-table-column>
      <el-table-column prop="tmName" label="品牌名称" width="width">
      </el-table-column>
      <el-table-column prop="prop" label="品牌LOGO" width="width">
        <template slot-scope="{ row }">
          <el-image
            style="width: 100px; height: 100px"
            :src="row.logoUrl"
          ></el-image>
        </template>
      </el-table-column>
      <el-table-column prop="prop" label="操作" width="width">
        <el-button
          type="warning"
          icon="el-icon-edit"
          size="mini"
          @click="updateTrademark"
          >修改</el-button
        >
        <el-button type="danger" icon="el-icon-delete" size="mini"
          >删除</el-button
        >
      </el-table-column>
    </el-table>
    <el-pagination
      style="margin: 20px; textalign: center"
      :current-page="page"
      :page-size="limit"
      :total="total"
      :page-sizes="[5, 10, 20]"
      layout="prev, pager, next, jumper,->, sizes, total"
      @current-change="getTradeList"
      @size-change="handleSizeChange"
    >
    </el-pagination>
  </div>
</template>

<script>
export default {
  name: "TradeMark",
  data() {
    return {
      page: 1,
      limit: 5,
      total: 0,
      list: [],
      dialogFormVisible: false,
      imageUrl: "",
    };
  },
  mounted() {
    this.getTradeList();
  },
  methods: {
    async getTradeList(pager = 1) {
      this.page = pager;
      const { page, limit } = this;
      let result = await this.$API.tradeMark.reqTradeMarkList(page, limit);
      console.log(result);
      if (result.code == 200) {
        this.total = result.data.total;
        this.list = result.data.records;
      }
    },
    handleSizeChange(limit) {
      this.limit = limit;
      this.getTradeList();
    },
    showDialog() {
      this.dialogFormVisible = true;
    },
    updateTrademark() {
      this.dialogFormVisible = true;
    },
    handleAvatarSuccess(res, file) {
      this.imageUrl = URL.createObjectURL(file.raw);
    },
    beforeAvatarUpload(file) {
      const isJPG = file.type === "image/jpeg";
      const isLt2M = file.size / 1024 / 1024 < 2;
      if (!isJPG) {
        this.$message.error("上传图片只能是JPG格式");
      }
      if (!isLt2M) {
        this.$message.error("上传图片大小不能超过2MB");
      }
      return isJPG && isLt2M;
    },
  },
};
</script>

<style>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>