<template>
  <div class="container">
    <div class="wrapper">
      <div class="left-block">
        <div class="posts-block">
          <div class="posts-block--header">
            <h4>Reddit Posts</h4>
          </div>
          <div class="posts-block--content" :key="post.title" v-for="post in posts">

            <div class="post">
              <div class="unseen"></div>
              <div class="post-author">
                <h5>By: {{post.author}}</h5> <h6>{{post.entryDate()}}</h6>
              </div>
              <div class="post-content">
                <img v-bind:src="post.thumbnail" v-if="post.thumbnail.substring(0,4) === 'http'" alt="thumbnail" />
                <h4 class="post-title">{{post.title}}</h4>
              </div>
              <div class="post-footer">
                <a href="">Dismiss post</a>
                <h4>{{post.comments}} comments</h4>
              </div>
            </div>
          </div>
          <div class="posts-block--footer">
            <a href="">Dismiss All</a>
          </div>
        </div>
      </div>
      <div class="right-block">
        hosd
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
  elementsToShow: '',
  methods:{
    getPosts() {
      let afterParam = '';
      const itemsToShow = [];
      axios
        //.get("https://api.reddit.com/top.json?sort=top&limit=50")
        .get(`https://api.reddit.com/top.json?sort=top&limit=50${afterParam ? '&after=' + afterParam: ''}`)
        .then(res => {
          const items = res.data.data.children;
          afterParam = res.data.after;
          items.forEach(element => {
            console.log('ELEMENT', element.data.thumbnail);
            const objPost = {
              title: element.data.title,
              author: element.data.author,
              entryDate: () => {
                const diff = Math.floor(new Date().getTime()/1000) - element.data.created_utc;
                const formatted = Math.round(diff / 3600);
                return `${formatted} hours ago`;
              },
              thumbnail: element.data.thumbnail,
              comments: element.data.num_comments,
              unread: true
            }
            console.log(objPost.entryDate())
            itemsToShow.push(objPost);
          });
        })
      return itemsToShow;
    }
  },
  mounted () {
    this.posts = this.getPosts();
  },
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
      posts: []
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.posts-block {
  display: flex;
  flex-flow: column;
}
.posts-block--header,
.posts-block--footer {
  align-items: center;
  background: var(--color-dark-gray);
  color: var(--color-white);
  display: flex;
  height: 50px;
  justify-content: center;
}
.posts-block--content .post{
  border-bottom: 1px solid var(--color-gray);
}
.post {
  position: relative;
}
.post-author {
  align-items: center;
  display: flex;
  flex-flow: row;
  justify-content: flex-end;
}
.post-author h5 {
  font-style: italic;
}
.post-author h6 {
  font-size: .7rem;
  font-style: italic;
  margin-left: 10px;
}
.post-content {
  display: flex;
  flex-flow: row;
  margin-bottom: 20px;
}
.post-content .post-title {
  margin: 0;
  padding-left: 30px;
  text-align: left;
  width: 100%;
}
.post-content img {
  height: 100%;
  width: 100%;
}
.post-footer {
  align-items: center;
  display: flex;
  justify-content: space-between;
}
.unseen {
  background: blue;
  border-radius: 50%;
  height: 15px;
  left: 10px;
  position: absolute;
  top: 10px;
  width: 15px;
}
</style>
