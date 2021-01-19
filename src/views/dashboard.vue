<template>
  <div>
    <br>
    <b-container>
      <b-container fluid>
        <b-row>
          <b-col>
            <div>
              <b-input type="text" v-model="search" v-on:keyup.enter="getQR()" placeholder="Type to Search"></b-input>
            </div>
          </b-col>
          <!-- <b-col>
            <div>
              <b-form-checkbox-group
                v-model="filterOn"
                class="mt-1"
              >
                <b-form-checkbox value="name">Name</b-form-checkbox>
                <b-form-checkbox value="lo">Location</b-form-checkbox>
              </b-form-checkbox-group>
            </div>
          </b-col>
          <b-col></b-col> -->
        </b-row>
        <br>
        <b-row>
          <b-col>
            <div>
              <b-form-select
                id="per-page-select"
                v-model="perPage"
                :options="pageOptions"
                size="sm"
              ></b-form-select>
            </div>
          </b-col>
          <b-col>
            <b-pagination
              v-model="currentPage"
              :total-rows="totalRows"
              :per-page="perPage"
              align="fill"
              size="sm"
              class="my-0"
            ></b-pagination>
          </b-col>
        </b-row>
      </b-container>
      <br>
      <b-table class="text-center" :items="postss" :fields="fields" :current-page="currentPage" :per-page="perPage" :filter="filter" :filter-included-fields="filterOn" @filtered="getQR" borderless fixed>
        <template v-slot:cell(name)="data">
          <b-row>
            <b-col>
              <div style="border-radius: 5px;border: thin solid #888;width: 200px;height: 100px;margin:5px;background-color: #00ccff;color: whitesmoke;margin-left: auto;margin-right: auto;display: block;">
                <div>
                  {{data.item.name}}
                </div>
                <div>
                  <img style="width:70px;height:70px;" :src="data.item.name_qr_url">
                </div>
              </div>
            </b-col>
          </b-row>
        </template>
        <template v-slot:cell(lo)="data">
          <b-row>
            <b-col>
              <div style="border-radius: 5px;border: thin solid #888;width: 200px;height: 100px;margin:5px;background-color: #00ccff;color: whitesmoke;margin-left: auto;margin-right: auto;display: block;">
                <div>
                  {{data.item.lo}}
                </div>
                <div>
                  <img style="width:70px;height:70px;" :src="data.item.lo_qr_url">
                </div>
              </div>
            </b-col>
          </b-row>
        </template>
      </b-table>
      <!-- <div style="border-radius: 5px;border: thin solid #888;margin:5px;width:500px;float: left;" v-for="(post, index) in filteredList" :key="index">
        <b-row>
          <b-col>
            <div style="border-radius: 5px;border: thin solid #888;width: 200px;height: 100px;margin:5px;background-color: #00ccff;color: whitesmoke;">
              <div>
                Name: {{post.name}}
              </div>
              <div>
                <img style="width50px;height:50px;" :src="post.name_qr_url" alt="">
              </div>
            </div>
          </b-col>
          <b-col>
          <div style="border-radius: 5px;border: thin solid #888;width: 200px;height: 100px;margin:5px;background-color: #00ccff;color: whitesmoke;">
            <div>
              Location: {{post.lo}}
            </div>
            <div>
              <img style="width50px;height:50px;" :src="post.lo_qr_url" alt="">
            </div>
          </div>
          </b-col>
        </b-row>
      </div> -->
    </b-container>
  </div>
</template>
<script>
// import axios from 'axios'
export default {
  data () {
    return {
      search: '',
      test: [],
      postList: [],
      postss: [],
      fields: [{ key: 'name', label: '', thStyle: { display: 'none' } }, { key: 'lo', label: '', thStyle: { display: 'none' } }],
      totalRows: 1,
      currentPage: 1,
      perPage: 20,
      pageOptions: [10, 20, 30, 50, 100, { value: 200, text: 'Show a lot' }],
      pages: [],
      filter: null,
      filterOn: []
    }
  },
  beforeCreate () {
  },
  created () {
    this.postss = JSON.parse(localStorage.getItem('qr'))
    console.log('data: ', this.postss)
    this.totalRows = this.postss.length
    // var local = JSON.parse(localStorage.getItem('qr'))
    // if (local === null) {
    //   console.log('test')
    //   // this.getQR()
    //   this.postList = JSON.parse(localStorage.getItem('qr'))
    //   this.totalRows = this.postList.length
    // } else if (local !== null) {
    //   this.postList = JSON.parse(localStorage.getItem('qr'))
    //   this.totalRows = this.postList.length
    // }
  },
  mounted () {
    // var local = JSON.parse(localStorage.getItem('qr'))
    // if (local !== null) {
    //   this.postList = JSON.parse(localStorage.getItem('qr'))
    //   this.totalRows = this.postList.length
    // }
    // var local = JSON.parse(localStorage.getItem('qr'))
    // if (local === null) {
    //   console.log('test')
    //   this.getQR()
    //   this.postList = JSON.parse(localStorage.getItem('qr'))
    //   this.totalRows = this.postList.length
    // } else if (local !== null) {
    //   this.postList = JSON.parse(localStorage.getItem('qr'))
    //   this.totalRows = this.postList.length
    // }
  },
  methods: {
    // getQR () {
    //   axios.get('http://192.168.1.41:9080/fetch_item_list').then(response => {
    //     // this.postList = response.data.item_list
    //     localStorage.setItem('qr', JSON.stringify(response.data.item_list))
    //     this.postList = JSON.parse(localStorage.getItem('qr'))
    //     // console.log('User', user)
    //     // CacheStorage.setItem('qr', JSON.stringify(response.data.item_list))
    //     // this.totalRows = this.postList.length
    //   })
    // },
    onFiltered (filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length
      this.currentPage = 1
    },
    getQR () {
      // const data = {
      //   list: [
      //     {
      //       name: 'myname', id: 12, type: 'car owner'
      //     },
      //     {
      //       name: 'myname2', id: 13, type: 'car owner2'
      //     },
      //     {
      //       name: 'myname3', id: 14, type: 'car owner3'
      //     },
      //     {
      //       name: 'myname4', id: 15, type: 'car owner4'
      //     }
      //   ]
      // }
      // var ss = []
      this.postList = JSON.parse(localStorage.getItem('qr'))
      // this.totalRows = this.postList.length
      // console.log(this.postList)
      this.postss = this.postList.filter(record => {
        this.totalRows = this.postList.length
        return record.name.toLowerCase().includes(this.search.toLowerCase())
      })
      this.totalRows = this.postss.length
      this.currentPage = 1
      console.log('data: ', this.postss)
    }
  },
  computed: {
    filteredList () {
      return this.postList.filter(post => {
        return post.name.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  }
}
</script>
<style>
</style>
