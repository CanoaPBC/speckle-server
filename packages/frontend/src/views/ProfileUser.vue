<template>
  <v-container>
    <v-row v-if="$apollo.loading">
      <v-col cols="12">
        <v-skeleton-loader type="card, article"></v-skeleton-loader>
      </v-col>
    </v-row>
    <v-row v-else>
      <v-col cols="12" sm="12" md="4" lg="3" xl="2">
        <user-info-card :user="user"></user-info-card>
      </v-col>
      <v-col cols="12" sm="12" md="8" lg="9" xl="10" class="pt-10">
        <v-card class="mb-3 elevation-0" color="background2">
          <v-card-title>
            {{ user.name }} has {{ user.streams.totalCount }} public streams and
            {{ user.commits.totalCount }} commits.
          </v-card-title>
        </v-card>
        <div v-for="(stream, i) in user.streams.items" :key="i">
          <list-item-stream :stream="stream"></list-item-stream>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import userById from '../graphql/userById.gql'
import UserInfoCard from '../components/UserInfoCard'
import ListItemStream from '../components/ListItemStream'

export default {
  name: 'ProfileUser',
  components: { UserInfoCard, ListItemStream },
  data: () => ({}),
  apollo: {
    user: {
      query: userById,
      variables() {
        return {
          id: this.$route.params.userId
        }
      }
    }
  },
  computed: {},
  created() {
    // Move to self profile
    if (this.$route.params.userId === localStorage.getItem('uuid')) {
      this.$router.replace({ path: '/profile' })
    }
  },
  methods: {}
}
</script>
<style scoped>
.v-item-group {
  float: left;
}

.clear {
  clear: both;
}
</style>
