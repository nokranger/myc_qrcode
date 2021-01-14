<template>
  <div>
    <br>
    <h1>Search</h1>
    <b-container>
      <b-row>
        <b-col>
          <b-input type="text" placeholder="Search title" v-model="search"></b-input>
        </b-col>
      </b-row>
      <br>
      <div v-for="(post, index) in filteredList" :key="index">
        <div style="border-radius: 5px;border: thin solid #888;width: 200px;height: 100px;margin:5px;">
          <div>name: {{post.title.name}}</div>
          <div>author: {{post.author.name}}</div>
        </div>
        <br>
        <!-- <div>{{filteredList}}</div> -->
      </div>
    </b-container>
    <div>
      {{postLists}}
    </div>
  </div>
</template>
<script>
class Post {
  constructor (title, link, author, img) {
    this.title = title
    this.link = link
    this.author = author
    this.img = img
  }
}
export default {
  data () {
    return {
      search: '',
      postLists: [{
        item: { item: 'nokky', qr: '' },
        location: { location: '32131231231', qr: '' }
      }, {
        item: { item: 'loki', qr: '' },
        location: { location: '3123123123', qr: '' }
      }, {
        item: { item: 'momo', qr: '' },
        location: { location: '312312312', qr: '' }
      }],
      postList: [
        new Post(
          { name: 'anusorn', qr: 'rqwrqwrqwrw' },
          'thavornpon',
          { name: 'nokky', qr: 'rqwrqwrqwrw' },
          'fsaasfasfasfafas'
        ),
        new Post(
          { name: 'anusorn', qr: 'rqwrqwrqwrw' },
          'thavornpon',
          { name: 'loki', qr: 'rqwrqwrqwrw' },
          'fsaasfasfasfafas'
        ),
        new Post(
          { name: 'anusorn', qr: 'rqwrqwrqwrw' },
          'thavornfsfsfpon',
          { name: 'momo', qr: 'rqwrqwrqwrw' },
          'fsaas11111fasfasfafas'
        )
      ]
    }
  },
  computed: {
    filteredList () {
      return this.postList.filter(post => {
        return post.author.name.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  }
}
</script>
<style>
</style>
