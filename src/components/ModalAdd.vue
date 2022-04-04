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
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {  
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header {
  margin-top: 0;
  color: #42b983;
  font-size: 1.5rem;
}


.modal-body {
  margin: 20px 0;
}
.modal-body input {
  width: 100%;
}

.modal-default-button {
  float: right;
  padding: 10px;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

</style>