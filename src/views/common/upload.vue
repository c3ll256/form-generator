<template>
  <div>
    <div
      v-if="!imageDefaultUrl"
      class="upload-cover-img"
      :style="`width: ${
        coverImageSize && coverImageSize.width ? coverImageSize.width : 375
      }px; height: ${
        coverImageSize && coverImageSize.height ? coverImageSize.height : 375
      }px`"
      @click="uploadHandle()"
    >
      点击上传图片
    </div>
    <img
      v-else
      class="upload-cover-img"
      :style="`width: ${
        coverImageSize && coverImageSize.width ? coverImageSize.width : 375
      }px; height: ${
        coverImageSize && coverImageSize.height ? coverImageSize.height : 375
      }px`"
      :src="imageDefaultUrl"
      @click="uploadHandle()"
    >
    <div style="color: #f56c6c; font-size: 10px">
      *图片需选择10Mb以下文件，图片大小建议：宽{{
        coverImageSize && coverImageSize.width ? coverImageSize.width : 375
      }}px 高：{{
        coverImageSize && coverImageSize.height ? coverImageSize.height : 375
      }}px
    </div>
    <!-- 引用el的dialog弹框组件，默认data中设置croppaVisible=true -->
    <el-dialog
      ref="uploader"
      :append-to-body="true"
      title="图片上传"
      :visible.sync="croppaVisible"
      :width="`${imageSize.width < 500 ? 500 : imageSize.width + 40}px`"
      :before-close="handleClose"
      :show-close="false"
      :close-on-press-escape="false"
      :close-on-click-modal="false"
    >
      <el-upload
        class="avatar-uploader"
        action
        accept="image"
        :show-file-list="false"
        :http-request="uploadChoice"
      >
        <img
          v-if="imageUrl"
          :src="imageUrl"
          class="avatar"
          :style="`width:${
            imageSize && imageSize.width ? imageSize.width : 500
          }px;height:${imageSize && imageSize.height ? imageSize.height : 500}px;`"
        >
        <i
          v-else
          class="el-icon-plus avatar-uploader-icon"
          :style="`width:${
            imageSize && imageSize.width ? imageSize.width : 500
          }px;height:${
            imageSize && imageSize.height ? imageSize.height : 500
          }px;line-height:${imageSize && imageSize.height ? imageSize.height : 500}px;`"
        />
      </el-upload>
      <span slot="footer" class="dialog-footer">
        <el-button @click="handleClose">取 消</el-button>
        <el-button type="primary" @click="uploadCroppedImage">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'JoxUpload',
  props: {
    // 选择图大小
    imageSize: {
      type: Object,
      default: () => ({ width: 300, height: 300 })
    },
    // 展示图大小
    coverImageSize: {
      type: Object,
      default: () => ({ width: 150, height: 150 })
    },
    // 初始url
    imageDefaultUrl: {
      type: String,
      default: ''
    },
    isCroppa: {
      type: Boolean,
      default: false
    },
    id: {
      type: String,
      default: ''
    },
    // 返回的变量名
    type: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      uploadLoading: false, // 上传加载
      croppa: {},
      tempcroppa: {},
      croppaVisible: false,
      imageUrl: '',
      image: {
        imageUrl: '',
        file: ''
      }
    }
  },
  watch: {
    imageDefaultUrl: (newValue, oldValue) => {
      this.imageUrl = newValue
    }
  },
  methods: {
    // 选择图片
    uploadHandle() {
      this.croppaVisible = true
    },
    // 上传完成
    uploadDone(image) {

    },
    // 确认选择图片
    uploadCroppedImage() {
      this.croppaVisible = false
    },
    handleClose() {
      this.croppaVisible = false
    },
    uploadChoice(param) {

    }
  }
}
</script>
<style>
.croppa_box {
  text-align: center;
}
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
  text-align: center;
}
.avatar {
  object-fit: cover;
}
.upload-cover-img {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  background-color: rgba(0, 0, 0, 0.1);
  object-fit: cover;
}
</style>
