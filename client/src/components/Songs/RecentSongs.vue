<template>
  <panel title="Recently Viewed">
    <v-data-table
      :headers="headers"
      :pagination.sync="pagination"
      :items="songs">
      <template slot="items" scope="props">
        <td class="text-xs-right">
          {{props.item.title}}
        </td>
        <td class="text-xs-right">
          {{props.item.artist}}
        </td>
      </template>
    </v-data-table>
  </panel>
</template>

<script>
import {mapState} from 'vuex'
import SongHistoryService from '@/services/songHistoryService'
export default {
  data () {
    return {
      headers: [
        {
          text: 'Title',
          value: 'title'
        },
        {
          text: 'Artist',
          value: 'artist'
        }
      ],
      pagination: {
        sortBy: 'updatedAt',
        descending: true
      },
      songs: []
    }
  },
  computed: {
    ...mapState([
      'isUserLoggedIn',
      'user'
    ])
  },
  async mounted () {
    if (this.isUserLoggedIn) {
      try {
        this.songs = (await SongHistoryService.index()).data
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>

<style scoped>
</style>
