<template>
  <Transition>
    <div v-if="show" class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container" :style="styleContainer">
          <div class="modal-header">
            <slot name="header"></slot>
          </div>

          <div class="modal-body">
            <slot name="body"></slot>
          </div>

          <div class="modal-footer">
            <slot name="footer">

            </slot>
          </div>
        </div>
      </div>
    </div>

  </Transition>

</template>

<script>
export default {
  name: "ModalAdd",
  props: {
    show: {
      type: Boolean,
      default: false
    },
    styleContainer: {
      type: String,
      default: 'width: 550px;height: 510px;'
    }
  }
}
</script>

<style scoped>
@import '@/css/ModalAdd.scss'

</style>