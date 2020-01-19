<template>
    <div class="uploader"
      @dragenter="onDragEnter"
      @dragleave="onDragLeave"
      @dragover.prevent
      @drop="onDrop"
      :class="{dragging: isDragging}">
      <div class="upload-control" v-show="images.length">
        <label for="file">Select a File</label>
        <button @click="upload">Upload</button>
      </div>
      <div v-show="!images.length">
        <i class="fas fa-cloud-upload-alt fa-3x"></i>
        <p>Drag your images here</p>
        <p>OR</p>
        <div class="file-input">
          <label for="file">select a file</label>
          <input type="file" id="file" @change="onInputChange" multiple>
        </div>
      </div>
      <div class="images-preview" v-show="images.length">
        <div class="img-wrapper" v-for="(image, index) in images" :key="index">
          <img :src="image" :alt="'image uploader' + index">
          <div class="overlay">
            <i @click="removeImage(index)" class="fas fa-times"></i>
            <i @click="showSingle" class="fas fa-search"></i>
          </div>
          <div class="details">
            <span class="name" v-text="files[index].name"></span>
            <span class="size" v-text="files[index].size"></span> 
          </div>
        </div>
      </div>
      <vue-easy-lightbox
        :visible="visible"
        :imgs="images"
        @hide="handleHide"
      ></vue-easy-lightbox>
    </div>
</template>

<script>

export default {
    name: 'uploadFile',
    data: function() {
        return {
        isDragging: false,
        dragCount: 0,
        files: [],
        images: [],
        visible: false,
        index: 0
      }
    },
    methods: {
      onDragEnter: function(e) {
        e.preventDefault();
        this.dragCount++;
        this.isDragging = true
      },
      onDragLeave: function(e) {
        e.preventDefault();
        this.dragCount--;

        if (this.dragCount <= 0) {
          this.isDragging = false
        }
         
      },
      onInputChange: function(e) {
        const files = e.target.files;
        
        Array.from(files).forEach(file => this.addImage(file));
      },
      onDrop: function(e) {
        e.preventDefault();
        e.stopPropagation();

        this.isDragging = false;

        const files = e.dataTransfer.files;
        
        Array.from(files).forEach(file => this.addImage(file));
      },
      addImage: function(file) {
        if (!file.type.match('image.*')) {
          alert(file.name + ' is not an image');
          return;
        }
        this.files.push(file);
         //img = new Image(),
             const reader = new FileReader();

              reader.onload = (e) => this.images.push(e.target.result);

              reader.readAsDataURL(file)
      },
      removeImage: function (index) {

        this.images.splice(index, 1);
      },
      upload: function() {

      },
      showSingle: function (index) {
        this.index = index;
        this.show();
        this.index = 1
      },
      show: function() {
        this.visible = true
      },
      handleHide: function () {
         this.visible = false
      }
    }
}
</script>

<style>
    
  .uploader {
    width:100%;
    background: #00f;
    color: #fff;
    padding: 72px 15px 40px 15px;
    text-align: center;
    border-radius: 10px;
    border: 3px dashed #fff;
    font-size: 20px;
    position: relative;
  }

  .dragging {
    background: #fff;
    color: #00f;
    border: 3px dashed #00f;
  }

  .file-input {
    width: 200px;
    margin: auto;
    height: 68px;
    position: relative;
  }

  .file-input label, .file-input input {
    background: #fff;
    color: #2196f3;
    width: 100%;
    position: absolute;
    top:0;
    left:0;
    padding: 10px;
    border-radius: 10px;
    margin-top: 7px;
    cursor: pointer
  }

  .file-input input {
    z-index: -3;
    opacity: 0;
  }

  .dragging .file-input label {
    background: #2196f3;
    color: #fff;
  }
  .images-preview {
    display: flex;
    flex-wrap: wrap;
    margin-top: 10px;
  }

  .images-preview .img-wrapper {
    background-color: #fff;
    width:160px;
    display: flex;
    flex-direction: column;
    margin: 10px;
    height: 158px;
    justify-content: space-between;
    padding: 3px;
    box-shadow: 1px 1px 7px 2px #000
  }

  .images-preview .img-wrapper img {
    height: 77%
  }
  .images-preview .details {
    background-color: #fff;
    font-size: 12px;
    color: #000;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .images-preview .details .name {
    overflow: hidden;
    height: 18px;
  }

  .upload-control {
    position: absolute;
    top:0;
    left:0;
    width:100%;
    background: #fff;
    border-top-left-radius: 7px;
    border-top-right-radius: 7px;
    padding: 18px;
    padding-bottom: 4px;
  }

  .upload-control label, .upload-control button {
    background: #2196f3;
    border: 2px solid #084c82;
    border-radius: 3px;
    color: #fff;
    font-size: 15px;
    padding: 8px 15px;
    cursor: pointer;
  }

  .upload-control label {
    margin-right:10px;
  }

  .images-preview .img-wrapper {
    position: relative;
  }

  .images-preview .img-wrapper .overlay {
    position:absolute;
    top:0;
    left:0;
    width: 100%;
    height:100%;
    background: rgba(0, 0, 0, .3);
    opacity: 0;
    transition: opacity .3s ease-in-out
  }

  .images-preview .img-wrapper:hover .overlay {
    opacity: 1;
  }

  .images-preview .img-wrapper .overlay .fa-times{
    float: right;
    margin: 10px;
    cursor: pointer;
    
  }

   .images-preview .img-wrapper .overlay .fa-search {
    position: absolute;
    left:50%;
    top:50%;
    transform: translate(-50%, -50%);
    z-index: 1;
    cursor: pointer;
   }

   /* Start slider style */
   .vel-img-wrapper[data-v-4de35050] {
     padding: 10px;
    background: #fff;
   }
    
    
</style>