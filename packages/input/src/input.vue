<template>
  <div
    class="star-input"
    :class="{
      'star-input--mini': size === 'mini',
      'star-input--hover': isHover && !isFocus,
      'star-input--focus': isFocus
    }"
    :style="styleVo"
    @mouseover="contentMouseover"
    @mouseout="contentMouseout"
  >
    <div
      class="star-input__place"
      :class="isHover ? 'star-input__place--hover' : ''"
      v-if="!isFocus"
      :style="{
        textAlign: !value ? placeAlign : align,
        width: width + 'px'
      }"
    >
      {{value || placeholder}}
    </div>
    <input
      v-else
      ref="input"
      class="star-input__input"
      :value="valueCopy"
      @input="valueCopy = $event.target.value"
      v-bind="$attrs"
      @keyup.enter.nativa="handleConfirm"
    />
    <i v-if="isHover && !isFocus" class="iconfont icon-icon-edit star-input__edit" :title="editTitle"></i>
    <i v-if="isFocus" class="star-input__opt" ref="opt">
      <span @click="handleConfirm" :title="confirmTitle"><i class="iconfont icon-yes-copy"></i></span>
      <span @click="handleCancel" :title="cancelTitle"><i class="iconfont icon-no-copy"></i></span>
    </i>
  </div>
</template>

<script>
export default {
  name: 'sst-input',
  inheritAttrs: false,
  props: {
    width: { type: [String, Number], default: 200 },
    placeholder: { type: String, default: '－－' },
    value: { type: [String, Number], default: null },
    size: { type: String, default: 'mini' },
    editTitle: { type: String, default: 'Edit' },
    confirmTitle: { type: String, default: 'Save value' },
    cancelTitle: { type: String, default: 'To cancel' },
    placeAlign: { type: String, default: 'left' },
    align: { type: String, default: 'left' },
    border: { type: String, default: false }
  },
  data() {
    return {
      isHover: false,
      isFocus: false,
      opt: false,
      valueCopy: null
    }
  },
  watch: {
    value: {
      handler(val) {
        this.valueCopy = val
      },
      immediate: true
    }
  },
  computed: {
    styleVo() {
      return {
        width: this.width + 'px',
        border: this.border && !this.isFocus ? '1px solid #dcdfe6' : 'none',
        borderRadius: this.border ? '5px' : 'none'
      }
    }
  },
  methods: {
    contentMouseover() {
      this.isHover = true
    },
    contentMouseout() {
      this.isHover = false
    },
    handleConfirm() {
      this.isFocus = false
      this.isHover = false
      if (this.valueCopy !== this.value) {
        this.$emit('input', this.valueCopy)
        this.$emit('confirm', this.valueCopy)
      }
    },
    handleCancel() {
      this.valueCopy = this.value
      this.isFocus = false
      this.isHover = false
      this.$emit('cancel')
    }
  },
  mounted() {
    document.addEventListener('click', (e) => {
      const contain = this.$el.contains(e.target)
      if (contain) {
        if (this.$refs.opt && this.$refs.opt.contains(e.target)) {

        } else {
          this.isFocus = true
          this.opt = true
          this.$nextTick(() => {
            this.$refs['input'].focus()
            this.$refs['input'].select()
          })
        }
      } else {
        this.isFocus = false
      }
    })
  }
}
</script>

<style lang="scss" scoped>
  .star-input {
    height: 24px;
    line-height: 24px;
    position: relative;
    text-align: left;

    &.star-input--mini {

      box-sizing: border-box;
    }

    &__place {
      display: inline-block;
    }

    &__edit {
      font-size: 12px;
      width: 20px;
      box-sizing: border-box;
      text-align: center;
      color: #6f6f6f;
      position: absolute;
      right: 0;
      top: 0px;
      height: 100%;
      background-color: #f0f0f0;
      border-left: 1px solid #ccc;
    }

    &__opt {
      position: absolute;
      right: 5px;
      background-color: #f0f0f0;
      display: flex;
      border: 1px solid #ccc;
      border-top: 0;
      border-radius: 0 0 3px 3px;
      box-shadow: 0 3px 6px rgba(111,111,111,.2);
      padding: 3px;
      box-sizing: border-box;
      line-height: 18px;
      span {
        height: 20px;
        border: 1px solid #ccc;
        padding: 0 4px;
        background-color: #f5f5f5;
        i {
          font-size: 14px;
        }
        &:hover{
          background-color: #f0f0f0;
          box-shadow: inset 0 3px 6px 0 rgba(0,0,0,.1);
        }
      }
      span+span{
        margin-left: 3px;
      }
    }

    &--hover{
      background-color: #fff;
      border: 1px solid #dcdfe6;
      border-radius: 5px;
      &.star-input--mini{
        /*padding: 0 3px;*/
      }
    }

    &.star-input--focus {
      height: 54px;
      .star-input__opt {
        bottom:2px;
      }
    }
  }

  .star-input__input{
    -webkit-appearance: none;
    background-color: #fff;
    border-radius: 4px;
    border: 1px solid #dcdfe6;
    box-sizing: border-box;
    display: inline-block;
    font-size: 14px;
    outline: 0;
    transition: border-color .2s cubic-bezier(.645, .045, .355, 1);
    padding: 0 5px;
    color: #000;
    width: 100%;

    &:hover {
      border-color: #c0c4cc;
    }

    &:focus {
      border-color: #409eff;
      outline: 0;
    }

    &::placeholder {
      color: #C0C4CC;
    }
  }

  .star-input--mini .star-input__input {
    height: 24px;
    line-height: 24px;
  }

</style>
