<template>
  <div class="card bg-light">
    <div class="row text-muted" id="tb-headings">
      <div class="col"><p>Title</p></div>
      <div class="col"><p>Description</p></div>
      <div class="col"><p>Deadline</p></div>
      <div class="col"><p>Priority</p></div>
      <div class="col"><p>Is Complete</p></div>
      <div class="col"><p>Action</p></div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'TableHeadings',
};
</script>

<style>
p {
  font-size: 14px;
  padding-top: 5%;
  padding-bottom: 5%;
  margin: auto;
}
</style>
