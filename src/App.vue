<template>
  <v-app>
    <v-card>
      <v-card-title>
        <h4>SERVER LIST</h4>
        <v-spacer></v-spacer>
        <v-btn
          icon
          @click="get"
        >
          <v-icon>mdi-refresh</v-icon>
        </v-btn>
      </v-card-title>
      <v-card-text>
        <v-data-table
          :headers="headers"
          :items="items"
        >
          <template v-slot:item.ip="{ item }">
            <a
              :href="'http://'+item.ip+':3000'"
              target="_blank"
            >
              {{ item.ip }}
            </a>
          </template>
        </v-data-table>
      </v-card-text>
    </v-card>
  </v-app>
</template>

<script>
const ipcRenderer = window.require('electron').ipcRenderer
export default {
  name: 'App',
  mounted () {
    ipcRenderer.on('recvIp', (event, value) => {
      if (Object.prototype.hasOwnProperty.call(value, 'name')) {
        const result = this.items.every(element => {
          if (element.ip === value.ip) return false
          console.log(value)
        })
        if (result) {
          this.items.push(value)
        }
      }
    })
    this.get()
  },
  methods: {
    get () {
      ipcRenderer.send('get')
    }
  },
  data () {
    return {
      headers: [
        { text: 'Name', value: 'name' },
        { text: 'IpAddr', value: 'ip' }
      ],
      items: []
    }
  }
}
</script>
