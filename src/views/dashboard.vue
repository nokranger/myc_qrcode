<template>
  <div>
    <br>
    <div>
    </div>
    <b-container>
      <b-container fluid>
        <b-row>
          <b-col>
            <div>
              <b-form-group
                label="Filter"
                label-for="filter-input"
                label-cols-sm="3"
                label-align-sm="right"
                label-size="sm"
                class="mb-0"
              >
                <b-input-group size="sm">
                  <b-form-input
                    id="filter-input"
                    v-model="filter"
                    type="search"
                    placeholder="Type to Search"
                  ></b-form-input>
                  <b-input-group-append>
                    <b-button :disabled="!filter" @click="filter = ''">Clear</b-button>
                  </b-input-group-append>
                </b-input-group>
              </b-form-group>
            </div>
          </b-col>
          <b-col>
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
          <b-col></b-col>
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
      <b-table :items="postList" :fields="fields" :current-page="currentPage" :per-page="perPage" :filter="filter" :filter-included-fields="filterOn" @filtered="onFiltered">
        <template v-slot:cell(name)="data">
          <div style="border-radius: 5px;border: thin solid #888;width: 200px;height: 100px;margin:5px;background-color: #00ccff;color: whitesmoke;">
            <div>
              {{data.item.name}}
            </div>
            <div>
              <img style="width:70px;height:70px;" :src="data.item.name_qr_url">
            </div>
          </div>
        </template>
        <template v-slot:cell(lo)="data">
          <div style="border-radius: 5px;border: thin solid #888;width: 200px;height: 100px;margin:5px;background-color: #00ccff;color: whitesmoke;">
            <div>
              {{data.item.lo}}
            </div>
            <div>
              <img style="width:70px;height:70px;" :src="data.item.lo_qr_url">
            </div>
          </div>
        </template>
      </b-table>
      <!-- <div style="border-radius: 5px;border: thin solid #888;margin:5px;width:500px;float: left;" v-for="(post, index) in filteredList" :key="index" :current-page="currentPage" >
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
import axios from 'axios'
export default {
  data () {
    return {
      search: '',
      postList: [],
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
  },
  mounted () {
    this.getQR()
  },
  methods: {
    getQR () {
      axios.get('http://192.168.1.41:9080/fetch_item_list').then(response => {
        this.postList = response.data.item_list
        this.totalRows = this.postList.length
      })
    },
    onFiltered (filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length
      this.currentPage = 1
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
