<template>
  <div>
    <h1>Create A Post</h1>
    <form @submit.prevent="addPost" enctype="multipart/form-data">
      <div class="row">
        <div class="col-md-6">
          <div class="form-group">
            <label>Post Title:</label>
            <input type="text" class="form-control" v-model="post.title">
          </div>
        </div>
        </div>
        <div class="row">
          <div class="col-md-6">
            <div class="form-group">
              <label>Post Body:</label>
              <textarea class="form-control" v-model.trim="post.body" rows="5"></textarea>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-md-3" v-if="image">
            <img :src="image" class="img-responsive" height="70" width="90">
          </div>
        <div class="col-md-6">
            <div class="form-group">
                <label>Upload Image:</label>
                <input type="file" @change="onImageChange" class="form-control">
            </div>
        </div>
        </div>
        <div class="form-group">
          <button class="btn btn-primary">Create</button>
        </div>
    </form>
  </div>
</template>

<script>
    export default {
        data(){
        return {
          post:{},
          image: ''
        }
    },
    methods: {
      addPost(){
        let uri = 'https://localhost:9146/api/post/create';
        this.axios.post(uri, this.post).then((response) => {
          this.$router.push({name: 'posts'});
        });
      },
      onImageChange(e) {
          let files = e.target.files || e.dataTransfer.files;
          if (!files.length)
            return;
          this.createImage(files[0]);
      },
      createImage(file) {
          let reader = new FileReader();
          let vm = this;
          reader.onload = (e) => {
              vm.image = e.target.result;
          };
          reader.readAsDataURL(file);
      },
      uploadImage(){
          axios.post('/image/upload',{image: this.image}).then(response => {
              console.log(response);
          });
      }
    }
  }
</script>