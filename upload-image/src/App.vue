<template>
  <div id="app">
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-md-9">
        <div class="card card-default">
          <div class="card-header">Upload image component</div>
          <div class="card-body">
            <upload-file></upload-file>
          </div>
        </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      
    }
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
body {
      background-color: #ddd;
    }
</style>
