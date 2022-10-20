<template>
  <div class="inputBox shadow">
    <input type="text" v-model="newTodoItem" placeholder="할 일을 입력하세요!">
    일 할 시간: <vue-timepicker v-model="newTodoItem2"></vue-timepicker>
    <!-- <input type="time" v-model="newTodoItem2" placeholder="일 할 시간을 입력하세요!" v-on:keypress.enter="addTodo"> -->
    <span class="addContainer" v-on:click="addTodo">
      <i class="addBtn fas fa-plus" aria-hidden="true"></i>
    </span>
    <modal v-if="showModal" @close="showModal = false">
      <h3 slot="header">경고</h3>
      <span slot="footer" @click="showModal = false">할 일을 입력하세요.
        <i class="closeModalBtn fas fa-times" aria-hidden="true"></i>
      </span>
    </modal>
  </div>
</template>

<script>
import Modal from './common/Modal.vue'
import VueTimepicker from 'vue2-timepicker/src/vue-timepicker.vue'
export default {
  data() {
    return {
      newTodoItem: '',
      newTodoItem2: '',
      showModal: false
    }
  },
  methods: {
    addTodo() {
      if (this.newTodoItem !== "") {
        var value = this.newTodoItem && this.newTodoItem.trim();
        var value2 = this.newTodoItem2 && this.newTodoItem2.trim();
				this.$emit('addTodo', value, value2)
        this.clearInput();
      } else {
        this.showModal = !this.showModal;
      }
    },
    clearInput() {
      this.newTodoItem = '';
      this.newTodoItem2 = '';
    }
  },
  components: {
    Modal: Modal,
    VueTimepicker
  }
}
</script>

<style scoped>
input:focus {
  outline: none;
}
.inputBox {
  background: white;
  height: 50px;
  line-height: 50px;
  border-radius: 5px;
}
.inputBox input {
  border-style: none;
  font-size: 0.9rem;
}
.addContainer {
  float: right;
  background: linear-gradient(to right, #6478FB, #8763FB);
  display: inline-block;
  width: 3rem;
  border-radius: 0 5px 5px 0;
}
.addBtn {
  color: white;
  vertical-align: middle;
}
</style>
