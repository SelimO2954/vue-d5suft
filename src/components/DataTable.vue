<template>
  <div class="card" id="datatable">
      <div v-for="entry in varFromParent">
      <div class="card">
        <div class = "row">
          <div class="col element">{{entry[0]}}</div>
          <div class="col element">{{entry[1]}}</div>
          <div class="col element">{{entry[2]}}</div>
          <div class="col element">{{entry[3]}}</div>
          <div v-if="!entry[4]" class="col element">
            <input class="form-check-input element" type="checkbox" @click="completeToggle(entry)" value="" id="completeBox">
          </div>
          <div v-if="entry[4]" class="col element">
            <input class="form-check-input element" type="checkbox" @click="completeToggle(entry)" value="" id="completeBox" checked>
          </div>
          <div class="col" style="padding-top: 1%; padding-bottom: 1%;">
            <div v-if="!entry[4]" class = "row" id="updateRow" style="margin-left:10%; margin-right:10%">
              <AddUpdateForm :varFromHeader="[varFromParent, false, entry]" />
            </div>
            <div class = "row" style="margin-left:10%; margin-right:10%">
            <button type="button" class="tablebtn btn btn-danger btn-sm" @click="deleteEntry(entry)">
                <i class="fa fa-times-circle"></i> DELETE
            </button> 
          </div>
        </div>
      </div>
    </div>
  </div>
  
</template>
<script>
import AddUpdateForm from './AddUpdateForm.vue';
export default {
  name: 'TableHeadings',
  components: {
    AddUpdateForm,
  },
  props: ['varFromParent'],
  data: () => ({
  }),
  methods: {
    completeToggle(entry) {
      for (let i = 0; i < this.varFromParent.length; i++) {
        if(this.varFromParent[i] == entry) {
          this.varFromParent[i][4] = !this.varFromParent[i][4]
          break;
        }
      }
    },
    deleteEntry(entry) {
      for (let i = 0; i < this.varFromParent.length; i++) {
        if(this.varFromParent[i] == entry) {
          this.varFromParent.splice(i,1)
          toastr.success("Task deleted sucessfully.");
          console.log("toastr displayed")
          break;
        }
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.element {
  font-size: 14px;
  padding-top: 1%;
  padding-bottom: 1%;
  margin: auto;
}
</style>
