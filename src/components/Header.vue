<template>
  <div class="card text-white bg-primary" id="banner">
    <div class="card-body">
      <div class="row">
        <div class="col"></div>
        <div class="col">
          <i class="fa fa-bars" style="font-size: 18px"></i>
          FRAMEWORKS
        </div>
        <div class="col">
          <AddUpdateForm :varFromHeader="[varFromParent, true]" ; />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import AddUpdateForm from './AddUpdateForm.vue';
export default {
  name: 'Header',
  components: {
    AddUpdateForm,
  },
  props: ['varFromParent'],
  data: () => ({}),
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#banner {
  position: relative;
  background-color: #1666bf;
  top: 0;
}
</style>
