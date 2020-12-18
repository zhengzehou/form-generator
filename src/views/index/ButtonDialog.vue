<template>
  <div>
    <el-dialog
      v-bind="$attrs"
      :close-on-click-modal="false"
      :modal-append-to-body="false"
      v-on="$listeners"
      @open="onOpen"
      @close="onClose"
    >
        <el-form ref="elBtnForm" :model="formData" :rules="rules" size="small" label-width="100px" >
        <el-form-item prop="label" label="按钮文字" >
            <el-input v-model="formData.label" placeholder="请输入按钮文字" />
        </el-form-item>
        <el-form-item prop="func" label="方法名称" >
            <el-input v-model="formData.func" placeholder="请输入方法名称" />
        </el-form-item>
        <el-form-item prop="type" label="按钮类型" >
            <el-select v-model="formData.type" :style="{ width: '100%' }">
              <el-option label="primary" value="primary" />
              <el-option label="success" value="success" />
              <el-option label="warning" value="warning" />
              <el-option label="danger" value="danger" />
              <el-option label="info" value="info" />
              <el-option label="text" value="text" />
            </el-select>
          </el-form-item>
          <el-form-item label="按钮图标" >
            <el-input v-model="formData.icon" placeholder="请输入按钮图标名称">
              <el-button slot="append" icon="el-icon-thumb" :current="'icon'" @click="openBtnIconsDialog()">
                选择
              </el-button>
            </el-input>
          </el-form-item>
          <el-form-item label="组件尺寸" >
            <el-radio-group v-model="formData.size">
              <el-radio-button label="medium">
                中等
              </el-radio-button>
              <el-radio-button label="small">
                较小
              </el-radio-button>
              <el-radio-button label="mini">
                迷你
              </el-radio-button>
            </el-radio-group>
          </el-form-item>
        </el-form>
      <div slot="footer">
        <el-button
          type="primary"
          @click="handelConfirm"
        >
          确定
        </el-button>
        <el-button @click="close">
          取消
        </el-button>
      </div>
    </el-dialog>
    <icons-dialog v-if="iconsBtnVisible" :visible.sync="iconsBtnVisible"  @select="setIcon" />
  </div>
</template>
<script>
import { isNumberStr } from '@/utils/index'
import { getTreeNodeId, saveTreeNodeId } from '@/utils/db'
import IconsDialog from './IconsDialog'

const id = getTreeNodeId()

export default {
  components: {
    IconsDialog
  },
  // props: {
  //   openIconsDialog: Function
  // },
  data() {
    return {
      id,
      iconsBtnVisible: false,
      formData: {
        label: undefined,
        type: undefined,
        icon: undefined,
        size: 'mini',
        func: undefined
      },
      rules: {
        label: [
          {
            required: true,
            message: '请输入按钮名称',
            trigger: 'blur'
          }
        ],
        func: [
          {
            required: true,
            message: '请输入操作方法',
            trigger: 'blur'
          }
        ],
        type: [
          {
            required: true,
            message: '请选择按钮类型',
            trigger: 'change'
          }
        ],
        size: [
          {
            required: true,
            message: '请选择按钮尺寸',
            trigger: 'change'
          }
        ]
      },
      dataType: 'string',
      dataTypeOptions: [
        {
          label: '字符串',
          value: 'string'
        },
        {
          label: '数字',
          value: 'number'
        }
      ]
    }
  },
  // computed: {},
  // watch: {
  //   // eslint-disable-next-line func-names
  //   'formData.value': function (val) {
  //     this.dataType = isNumberStr(val) ? 'number' : 'string'
  //   },
  //   id(val) {
  //     saveTreeNodeId(val)
  //   }
  // },
  created() {},
  mounted() {},
  methods: {
    onOpen() {
      this.formData = {
        label: undefined,
        type: undefined,
        icon: undefined,
        size: 'mini',
        func: undefined
      }
    },
    onClose() {},
    close() {
      this.$emit('update:visible', false)
    },
    handelConfirm() {
      this.$refs.elBtnForm.validate(valid => {
        if (!valid) return
        this.$emit('commit', this.formData)
        this.close()
      })
    },
    openBtnIconsDialog() {
      this.iconsBtnVisible = true
    },
    setIcon(val) {
      this.formData.icon = val
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
