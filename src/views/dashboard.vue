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
                Author: {{post.name.name}}
              </div>
              <div>
                <!-- QR: {{post.title.qr}} -->
              </div>
            </div>
          </b-col>
          <b-col>
          <div style="border-radius: 5px;border: thin solid #888;width: 200px;height: 100px;margin:5px;background-color: #00ccff;color: whitesmoke;">
            <div>
              <!-- Location: {{post.author.name}} -->
            </div>
            <div>
              <!-- QR: {{post.author.qr}} -->
            </div>
          </div>
          </b-col>
        </b-row>
      </div>
    </b-container>
  </div>
</template>
<script>
import axios from 'axios'
// class Post {
//   constructor (title, link, author, img) {
//     this.title = title
//     this.link = link
//     this.author = author
//     this.img = img
//   }
class Post {
  constructor (name, lo, nameQrUrl, loQrUrl) {
    this.name = name
    this.lo = lo
    this.name_qr_url = nameQrUrl
    this.lo_qr_url = loQrUrl
  }
}
export default {
  data () {
    return {
      search: '',
      postList: []
      // ,
      // postList: [
      //   new Post(
      //     { name: 'anusorn', qr: 'rqwrqwrqwrw' },
      //     'thavornpon',
      //     { name: 'nokky', qr: 'rqwrqwrqwrw' },
      //     'fsaasfasfasfafas'
      //   ),
      //   new Post(
      //     { name: 'anusorn', qr: 'rqwrqwrqwrw' },
      //     'thavornpon',
      //     { name: 'loki', qr: 'rqwrqwrqwrw' },
      //     'fsaasfasfasfafas'
      //   ),
      //   new Post(
      //     { name: 'anusorn', qr: 'rqwrqwrqwrw' },
      //     'thavornfsfsfpon',
      //     { name: 'momo', qr: 'rqwrqwrqwrw' },
      //     'fsaas11111fasfasfafas'
      //   )
      // ]
    }
  },
  async created () {
    await this.getQR()
  },
  mounted () {
    // axios.get('http://192.168.1.34:9080/fetch_item_list').then(response => {
    //   console.log('APIQRCODE', response)
    //   console.log(typeof (response))
    //   this.postLists = new Post(
    //     {
    //       name: response.data.item_list.map((data, i) => {
    //         return {
    //           name: data.name
    //         }
    //       }),
    //       name_qr_url: response.data.item_list.map((data, i) => {
    //         return {
    //           name_qr_url: data.name_qr_url
    //         }
    //       })
    //     },
    //     {
    //       lo: response.data.item_list.map((data, i) => {
    //         return {
    //           lo: data.lo
    //         }
    //       }),
    //       lo_qr_url: response.data.item_list.map((data, i) => {
    //         return {
    //           lo_qr_url: data.lo_qr_url
    //         }
    //       })
    //     }
    //   )
    //   // response.data.item_list
    //   console.log('NEWAPIQRCODE', this.postLists)
    // })
  },
  methods: {
    getQR () {
      axios.get('http://192.168.1.34:9080/fetch_item_list').then(response => {
        console.log('APIQRCODE', response)
        console.log(typeof (response))
        this.postList = new Post(
          {
            name: response.data.item_list.map((data, i) => {
              return {
                name: data.name
              }
            })
          },
          {
            name_qr_url: response.data.item_list.map((data, i) => {
              return {
                name_qr_url: data.name_qr_url
              }
            })
          },
          {
            lo: response.data.item_list.map((data, i) => {
              return {
                lo: data.lo
              }
            })
          },
          {
            lo_qr_url: response.data.item_list.map((data, i) => {
              return {
                lo_qr_url: data.lo_qr_url
              }
            })
          }
        )
        // response.data.item_list
        console.log('NEWAPIQRCODE', this.postList)
      })
    }
  },
  computed: {
    // filteredList () {
    //   return this.postList.filter(post => {
    //     return post.author.name.toLowerCase().includes(this.search.toLowerCase())
    //   })
    // }
    filteredList () {
      return this.postList.filter(post => {
        return post.name.name.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  }
}
</script>
<style>
</style>
