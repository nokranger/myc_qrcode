<template>
  <div style="border-radius: 5px;border: thin solid #888;background-color: #f4f5f7;margin: 5px;">
    <br>
    <b-container>
      <b-container fluid>
        <b-row>
          <b-col>
            <div>
              <b-input type="text" v-model="search" v-on:keyup.enter="getQR()" placeholder="Type to Search"></b-input>
            </div>
          </b-col>
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
              <div style="border-radius: 5px;border: thin solid #888;width: 200px;height: 100px;margin:5px;background-color: #29265b;color: whitesmoke;margin-left: auto;margin-right: auto;display: block;">
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
              <div style="border-radius: 5px;border: thin solid #888;width: 200px;height: 100px;margin:5px;background-color: #29265b;color: whitesmoke;margin-left: auto;margin-right: auto;display: block;">
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
    </b-container>
  </div>
</template>
<script>
import axios from 'axios'
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
    axios.get('http://192.168.10.2:9080/fetch_item_list').then(response => {
      localStorage.setItem('qr', JSON.stringify(response.data.item_list))
    })
    this.postss = JSON.parse(localStorage.getItem('qr'))
    console.log('data: ', this.postss)
    this.totalRows = this.postss.length
  },
  mounted () {
  },
  methods: {
    onFiltered (filteredItems) {
      this.totalRows = filteredItems.length
      this.currentPage = 1
    },
    getQR () {
      this.postList = JSON.parse(localStorage.getItem('qr'))
      this.postss = this.postList.filter(record => {
        this.totalRows = this.postList.length
        return record.name.toLowerCase().includes(this.search.toLowerCase())
      })
      this.totalRows = this.postss.length
      this.currentPage = 1
      // console.log('data: ', this.postss)
    }
  },
  computed: {
  }
}
</script>
<style>
</style>
