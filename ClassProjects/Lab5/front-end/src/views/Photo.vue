<template>
<div class="home">
  <div class="photoStuff">
  <div class='holder'>
    <h3 class="title">{{photo.title}}</h3>
    <img class="thePhoto" :src="photo.path" />
    <div class="userdate">
      <p v-if="photo.user">{{photo.user.username}}</p>
      <p class='date'>{{formatDate(photo.created)}}</p>
    </div>
    <p>{{photo.description}}</p>
    <div class="commentBox">
      <div class="comment" v-for="comment in comments" v-bind:key="comment._id">
        <p>{{comment.user.username}}</p>
        <p>{{comment.comment}}</p>
        <p>{{formatDate(comment.created)}}</p>
      </div>
    </div>
    <div class="commentStuff" v-if="user">
      <textarea v-model="commentText" placeholder="Comment"></textarea>
      <button type="submit" @click.prevent="createComment">Post comment</button>
    </div>
  </div>
  </div>
</div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
export default {
  name: 'Photo',
  data() {
    return {
      photo: {},
      comments: [],
      commentText: '',
    }
  },
  async created() {
    this.getPhoto();
    this.getComments();
    try {
      let response = await axios.get('/api/users');
      this.$root.$data.user = response.data.user;
    } catch (error) {
      this.$root.$data.user = null;
    }
  },
  computed: {
    user() {
      return this.$root.$data.user;
    }
  },
  methods: {
    async getPhoto()
    {
      let id = this.$route.params.id;
      try
      {
        let response = await axios.get('/api/photos/' + id)
        this.photo = response.data;
      } catch (error) {
        console.log(error);this.$root.$data.user
      }

    },
    formatDate(date) {
      if (moment(date).diff(Date.now(), 'days') < 15)
        return moment(date).fromNow();
      else
        return moment(date).format('d MMMM YYYY');
    },

    async getComments()
    {
      let id = this.$route.params.id;
      try
      {
        let response = await axios.get('/api/comments/' + id)
        this.comments = response.data;
      } catch (error) {
        console.log(error);
      }
    },
    async createComment()
    {
      let id = this.$route.params.id;
      try
      {
        await axios.post('/api/comments/' + id, {
          comment: this.commentText,
          user: this.$root.$data.user
        });
        this.getComments();
      } catch (error) {
        console.log(error);
      }
    }
  }


}

</script>

<style scoped>
.home
{
  padding-top: 100px;

}
.thePhoto
{
  height: 400px;

}
.photoStuff
{
width: 100%;

display:flex;
flex-wrap: wrap;
justify-content:center;
}
.title
{
width:100%;
text-align:center;
}
.userdate
{
display:flex;
flex-wrap:nowrap;
}
.userdate p
{
width:50%;
}
.date{
text-align:right;
}
.comment
{
border: 1px solid gray;
}
</style>
