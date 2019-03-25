<template>
  <div>
      <h1>Posts</h1>
        <div class="row">
          <div class="col-md-10"></div>
          <div class="col-md-2">
            <router-link :to="{ name: 'create' }" class="btn btn-sm btn-success">Create Post</router-link>
          </div>
        </div><br/>

          <template>
            <div>
              <b-table :fields="fields" :items="posts">
                <template slot="edit" slot-scope="items">
                    <b-link :to="{name: 'edit', params: { id: items.item.id }}" class="btn btn-sm btn-primary">
                        Edit
                    </b-link>
                </template>
                <template slot="delete" slot-scope="items">
                    <b-button @click.prevent="deletePost(items.item.id)" class="btn btn-sm btn-danger">
                        Delete
                    </b-button>
                </template>
              </b-table>
            </div>
          </template>
  </div>
</template>

<script>
  export default {
      data: function() {
        return {
          fields: ['id', 'title', 'body', 'edit', 'delete'],
          posts: []
        }
      },
      created() {
      let uri = 'https://localhost:9146/api/posts';
      this.axios.get(uri).then(response => {
        this.posts = response.data.data;
      });
    },
    methods: {
      deletePost(id)
      {
        let uri = `https://localhost:9146/api/post/delete/${id}`;
        this.axios.delete(uri).then(response => {
          this.posts.splice(this.posts.indexOf(id), 1);
        });
      }
    }
  }
</script>