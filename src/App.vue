<template>
  <div id="app">

    <table>
      <thead>
        <tr>
          <th>Azonsító</th>
          <th>Cím</th>
          <th>Év</th>
          <th>Kiállítva(piros lap)</th>
          <th>Műveletek</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="painting in paintings" v-bind:key="painting.id">
          <td>{{ painting.id }}</td>
          <td>{{ painting.title }}</td>
          <td>{{ painting.year }}</td>
          <td>{{ painting.on_display }}</td>
          <td>
            <button @click="deletePainting(painting.id)">Törlés</button>
            <button @click="editPainting(painting.id)">Szerkesztés</button>
          </td>
        </tr>
        <tr>
          <td>
            <input type="hidden" v-model="painting.id">
          </td>
          <td>
            <input type="text" v-model="painting.title">
          </td>
          <td>
            <input type="number" v-model="painting.year">
          </td>
          <td>
            <input type="checkbox" v-model="painting.on_display">
          </td>
          <td>
            <button v-if="mod_new" @click="newPainting" :disabled="saving">Létrehoz</button>
            <button v-if="!mod_new" @click="savePainting" :disabled="saving">Mentés</button>
            <button v-if="!mod_new" @click="cancelEdit" :disabled="saving">Mégse</button>
          </td>
        </tr>
      </tbody>
    </table>
    <table>
      <thead>
        <tr>
          <th>Azonsító</th>
          <th>Név</th>
          <th>Magasság</th>
          <th>Ár</th>
          <th>Műveletek</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="statue in statues" v-bind:key="statue.id">
          <td>{{ statue.id }}</td>
          <td>{{ statue.person }}</td>
          <td>{{ statue.height }}</td>
          <td>{{ statue.price }}</td>
          <td>
            <button @click="deleteStatues(statue.id)">Törlés</button>
            <button @click="editStatues(statue.id)">Szerkesztés</button>
          </td>

        </tr>
        <tr>
          <td>
            <input type="hidden" v-model="statue.id">
          </td>
          <td>
            <input type="text" v-model="statue.person">
          </td>
          <td>
            <input type="number" v-model="statue.height">
          </td>
          <td>
            <input type="number" v-model="statue.price">
          </td>
          <td>
            <button v-if="mod_new" @click="newStatues" :disabled="StatueSaving">Létrehoz</button>
            <button v-if="!mod_new" @click="saveStatues" :disabled="StatueSaving">Mentés</button>
            <button v-if="!mod_new" @click="cancelEdit" :disabled="StatueSaving">Mégse</button>
          </td>
        </tr>
      </tbody>
    </table>
    <button @click="loadData">Adatok betöltése</button>
    <button @click="loadSzobrokData">Szobrok betöltése</button>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      mod_new: true, 
      saving: false,
      painting: {
        id: null,
        title: '',
        year: '',
        on_display: false
      },
      statue: {
        id: null,
        person: '',
        height: '',
        price: '',
      },
      paintings: [],
      statues:[]
      
    }
  },
  methods: {
    async loadData () {
     let Response = await fetch('http://127.0.0.1:8000/api/paintings')
     let data = await Response.json()
     this.paintings = data
    },
    async loadSzobrokData () {
     let Response = await fetch('http://127.0.0.1:8000/api/statues')
     let data = await Response.json()
     this.statues = data
    },
    async deletePainting(id) {
      let Response = await fetch(`http://127.0.0.1:8000/api/paintings/${id}`, {
        method: 'DELETE'
      })
      console.log(Response)
      await this.loadData()
    },
    async deleteStatues(id) {
      let Response = await fetch(`http://127.0.0.1:8000/api/statues/${id}`, {
        method: 'DELETE'
      })
      console.log(Response)
      await this.loadSzobrokData()
    },
    
    async newPainting() {
      this.saving='disabled'
     await fetch('http://127.0.0.1:8000/api/paintings', {
       method: 'POST',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.painting) 
     })
     await this.loadData()
     this.saving=false
     this.resetForm()
    },
    async newStatues() {
      this.StatueSaving='disabled'
     await fetch('http://127.0.0.1:8000/api/statues', {
       method: 'POST',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.statue) 
     })
     await this.loadSzobrokData()
     this.StatueSaving=false
     this.resetFormStatues()
    },
    async savePainting() {
      this.saving='disabled'
     await fetch(`http://127.0.0.1:8000/api/paintings/${this.painting.id}`, {
       method: 'PATCH',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.painting) 
     })
     await this.loadData()
     this.saving=false
     this.resetForm()
    },
    async saveStatues() {
      this.StatueSaving='disabled'
    await fetch(`http://127.0.0.1:8000/api/statues/${this.statue.id}`, {
       method: 'PATCH',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.statue) 
     })
     await this.loadSzobrokData()
     this.StatueSaving=false
     this.resetFormStatues()
    },
    async editPainting(id) {
      let Response = await fetch(`http://127.0.0.1:8000/api/paintings/${id}`)
      let data = await Response.json()
      this.painting = {...data};
      this.mod_new = false
    },
     async editStatues(id) {
      let Response = await fetch(`http://127.0.0.1:8000/api/statues/${id}`)
      let data = await Response.json()
      this.statue = {...data};
      this.mod_new = false
    },
    cancelEdit () {
      this.resetForm()
      this.resetFormStatues()
    },
    resetForm() {
      this.painting = {
        id: null,
        title: '',
        year: '',
        on_display: false
      }
      this.mod_new = true
    },
    resetFormStatues() {
      this.statue = {
        id: null,
        person: '',
        height: '',
        price: '',
      }
      this.mod_new = true
    }
  },
  mounted() {
    this.loadData()
    this.loadSzobrokData()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
