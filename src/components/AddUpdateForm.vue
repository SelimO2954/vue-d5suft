<template>
  <button 
    v-if="varFromHeader[1]"
    type="button"
    class="btn btn-primary float-end shadow btn-sm"
    data-bs-toggle="modal"
    data-bs-target="#dialog"
    @click=clearModal()
  >
    <i class="fa fa-plus-circle" style="font-size: 18px"></i>
     ADD
  </button>
  <button
    v-if="!varFromHeader[1]"
    type="button"
    class="btn btn-primary float-end shadow btn-sm"
    data-bs-toggle="modal"
    data-bs-target="#dialog"
    @click=clearModal()
  >
    <i class="fa fa-edit"></i>
     UPDATE
  </button>
  <div class="modal fade text-dark" id="dialog" tabindex="-1">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header bg-primary text-white">
          <h5 class="modal-title" id="modaltitle">
          </h5>
        </div>
        <div class="modal-body">
          <form>
            <div class="form-group" id="titlevalidation">
              <input
                type="text"
                class="form-control"
                id="titleinput"
                placeholder="Title"
                required
              />
              <div class="invalid-feedback">New Title is Required!</div>
            </div>
            <div class="form-group" id="restvalidation">
              <input
                type="text"
                class="form-control"
                id="descriptioninput"
                placeholder="Description"
                required
              />
              <div class="invalid-feedback">Description is Required!</div>
              <div class="input-group date">
                <input type="date" class="form-control" id="deadlineinput" placeholder="" required/>
                <div class="invalid-feedback">Deadline is Required!</div>
              </div>
            </div>
            <div class="row">
              <div class="text-muted d-flex flex-row" style="font-size:14px">Priority</div>
            </div>
            <div id="radio">
              <div class="row">
                <div class="col">
                  <div class="form-check form-check-inline">
                    <input class="form-check-input" type="radio" name="priorityradio" id="low">
                    <label class="form-check-label" for="low">
                      Low
                    </label>
                  </div>
                </div>
              <div class="col">
                <div class="form-check form-check-inline">
                  <input class="form-check-input" type="radio" name="priorityradio" id="med">
                  <label class="form-check-label" for="medium">
                  Med
                  </label>
                </div>
              </div>
              <div class="col">
                <div class="form-check form-check-inline">
                  <input class="form-check-input" type="radio" name="priorityradio" id="high">
                  <label class="form-check-label" for="high">
                  High
                  </label>
                </div>
              </div>
            </div>
          </form>
        </div>
        <div class="modal-footer">
        <button
          type="button"
          id="submitbutton"
          class="btn btn-primary btn-sm"
          @click="submit()"
        >
          <i class="fa fa-plus-circle" style="font-size: 18px"></i> Add
        </button>
        <button type="button" class="btn btn-danger btn-sm" data-bs-dismiss="modal"><i class="fa fa-ban" ></i> Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AddUpdateForm',
  props: ['varFromHeader'],
  data: () => ({
    isAdd: false,
  }),
  methods: {
    clearModal() {
      if (this.varFromHeader[1]) {
        document.getElementById('modaltitle').innerHTML = '<i class="fa fa-plus-circle" style="font-size: 18px"></i> Add Task';
        document.getElementById('titleinput').style.display = 'block';
        document.getElementById('titleinput').value="";
        document.getElementById('descriptioninput').value="";
        document.getElementById('deadlineinput').value="";
        document.getElementById('low').checked=false;
        document.getElementById('med').checked=false;
        document.getElementById('high').checked=false;
        document.getElementById('submitbutton').innerHTML = '<i class="fa fa-plus-circle" style="font-size: 18px"></i> Add'
      }
      else if(!this.varFromHeader[1]) {
        let entry = this.varFromHeader[2]
        document.getElementById('modaltitle').innerHTML = '<i class="fa fa-edit" style="font-size: 18px"></i> Edit Task';
        document.getElementById('titleinput').style.display = 'none';
        document.getElementById('titleinput').value=entry[0];
        document.getElementById('descriptioninput').value=entry[1];
        document.getElementById('deadlineinput').value=entry[2]
        if(entry[3] == "low") {
          document.getElementById('low').checked=true;
        }
        else if(entry[3] == "med") {
          document.getElementById('med').checked=true;
        }
        else {
          document.getElementById('high').checked=true;
        }
        document.getElementById('submitbutton').innerHTML = '<i class="fa fa-edit" style="font-size: 18px"></i> Edit'
      }
      document.getElementById('restvalidation').classList.remove("was-validated");
      document.getElementById('titleinput').classList.remove("is-valid");
      document.getElementById('titleinput').classList.remove("is-invalid");
    },
    submit() {
      this.isAdd = document.getElementById('modaltitle').innerHTML.indexOf("Add") > -1
      let titleVal = document.getElementById('titleinput').value
      let descriptionVal = document.getElementById('descriptioninput').value
      let dateVal = $("#deadlineinput").val();
      let validTitle = true;
      if (this.isAdd) {
        for (let i = 0; i < this.varFromHeader[0].length; i++) {
          if(this.varFromHeader[0][i][0] == titleVal || titleVal == "") {
            validTitle = false;
            break;
          }
        }
      }
      let radioIds = ["low", "med", "high"]
      let priorityVal = "";
      for (let i = 0; i < radioIds.length; i++) {
          if(document.getElementById(radioIds[i]).checked) {
          priorityVal = radioIds[i];
          break;
        }
      }
      document.getElementById('restvalidation').classList.add("was-validated");
      if(!validTitle) {
        document.getElementById('titleinput').classList.remove("is-valid");
        document.getElementById('titleinput').classList.add("is-invalid");
      }
      else {
        document.getElementById('titleinput').classList.remove("is-invalid");
        document.getElementById('titleinput').classList.add("is-valid");
      }
      if (validTitle && descriptionVal != "" && dateVal != "" && priorityVal != "") {
        var newEntry = [titleVal, descriptionVal, dateVal, priorityVal, false]
        if (this.isAdd) {
          this.varFromHeader[0].push(newEntry)
          toastr.success("Task added sucessfully.")
        }
        else {
          for (let i = 0; i < this.varFromHeader[0].length; i++) {
            if(this.varFromHeader[0][i][0] == titleVal) {
              this.varFromHeader[0][i] = newEntry
              toastr.success("Task editted sucessfully.")
              break;
            }
          } 
        }
        this.$emit('newList', {
          data: this.varFromHeader[0]
        })
        $('.modal').modal('hide');
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.modal {
  margin:auto;
}
.form-control {
  margin-bottom:2%;
}
.invalid-feedback {
  font-size:10px;
  text-align:left;
  padding-top:0;
  margin-bottom:2%;
}

</style>
