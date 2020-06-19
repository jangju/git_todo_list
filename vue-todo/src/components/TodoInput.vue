<template>
  <div class="inputBox shadow">
    <input type="text" v-model="newTodoItem" v-on:keyup.enter="addTodo">
    <!-- <button v-on:click="addTodo">add</button> -->
    <span class="addContainer" v-on:click="addTodo">
      <i class="fas fa-plus addBtn"></i>
    </span>

    <Modal v-if="showModal" @close="showModal = false">
      <h3 slot="header">
        경고!
        <i class="closeModalBtn fas fa-times" @click="showModal = false"></i>
      </h3>
      <div slot="body">아무것도 입력되지 않았습니다.</div>
      
    </Modal>
  </div>
</template>

<script>
import Modal from './common/Modal.vue'

export default {
  data() {
    return {
      newTodoItem: "",
      showModal: false
    }
  },
  methods: {
    addTodo() {
      if(this.newTodoItem !== ''){

        //상위 컴포넌트로 이벤트 보냄 this.$emit('이벤트 이름', 인자1, 인자2 ....)
        this.$emit('addTodoItem', this.newTodoItem)
        this.clearInput();
      }else{
        this.showModal = !this.showModal
      }
    },
    clearInput() {
      this.newTodoItem =  '';
    }
  },
  components: {
    Modal
  }
}
</script>

<style scoped>
  input:focus {
    outline: none;
  }
  .inputBox {
    height: 50px;
    border-radius: 5px;
    line-height: 50px;
    background: white;
  }
  .inputBox input {
    border-style: none;
    font-size: 0.9rem;
  }
  .addContainer {
    display: block;
    float: right;
    width: 3rem;
    border-radius: 0 5px 5px 0;
    background: linear-gradient(to right, #6478FB, #8763FB);
  }
  .addBtn {
    color: white;
    vertical-align: middle;
  }
  .closeModalBtn {
    color: #42b983;
  }
</style>