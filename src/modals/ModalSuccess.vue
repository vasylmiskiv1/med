<template>
  <div class="modal__success">
    Відгук збережено успішно	&#10003;
  </div>
</template>

<script>
export default {
  name: 'ModalSuccess'
}
</script>

<style>
  .modal__success {
    background: #b3f8cdb7;
    padding: 30px;
    border: 1px solid #979797;
    border-radius: 5px;
    font-size: 20px;
    position: fixed;
    bottom: 20px;
    right: 20px;
  }
</style>