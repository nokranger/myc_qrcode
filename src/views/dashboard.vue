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
      <div style="border-radius: 5px;border: thin solid #888;margin:5px;width:500px;float: left;" v-for="(post, index) in filteredList" :key="index">
        <b-row>
          <b-col>
            <div style="border-radius: 5px;border: thin solid #888;width: 200px;height: 100px;margin:5px;background-color: #00ccff;color: whitesmoke;">
              <div>
                Author: {{post.title.name}}
              </div>
              <div>
                QR: {{post.title.qr}}
              </div>
            </div>
          </b-col>
          <b-col>
          <div style="border-radius: 5px;border: thin solid #888;width: 200px;height: 100px;margin:5px;background-color: #00ccff;color: whitesmoke;">
            <div>
              Location: {{post.author.name}}
            </div>
            <div>
              QR: {{post.author.qr}}
            </div>
          </div>
          </b-col>
        </b-row>
        <!-- <div>{{filteredList}}</div> -->
      </div>
    </b-container>
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
