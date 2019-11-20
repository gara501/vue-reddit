<template>
  <div class="container">
    <div class="wrapper">
      <div class="left-block">
        hola
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
  post(data) {
    let ObjPost = new Object();
    ObjPost.title = data.title;
    ObjPost.author = data.author;
    ObjPost.entryDate = () => {
      const createdDate = data.created;
    }
    ObjPost.thumb = data.thumbnail;
    ObjPost.comments = data.comments;
    ObjPost.unread = true;
    return ObjPost;
  },
  created: () => {
    let afterParam = '';
    const itemsToShow = [];
    axios
      //.get("https://api.reddit.com/top.json?sort=top&limit=50")
      .get(`https://api.reddit.com/top.json?sort=top&limit=50${afterParam ? '&after=' + afterParam: ''}`)
      .then(res => {
        const items = res.data.data.children;
        afterParam = res.data.after;
        items.forEach(element => {
          console.log(element)
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
    this.elementsToShow = itemsToShow;
    console.log(this.elementsToShow)
  },
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
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
.container {
  margin: 0 auto;
  max-width: 1200px;
}
.wrapper {
  display: flex;
  flex-flow: row;
}
.left-block, .right-block {
  flex: 1;
}
</style>
