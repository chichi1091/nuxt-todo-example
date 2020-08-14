<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-card v-if="todos">
      <v-card-title>
        TODO一覧
        <v-spacer />
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="検索"
          sigle-line
        />
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="todos"
        :items-per-page="5"
        :search="search"
        sort-by="id"
        :sort-desc="true"
        class="elevation-1"
      >
        <template v-slot:[`item.actions`]="{ item }">
          <v-icon
            small
            @click="edit(item)"
          >
            mdi-pencil
          </v-icon>
          <v-icon
            small
            @click="remove(item)"
          >
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
    </v-card>
  </v-layout>
</template>

<script>
export default {
  data () {
    return {
      search: '',
      headers: [
        { text: 'ID', value: 'id' },
        { text: 'タスク', value: 'task' },
        { text: '操作', value: 'actions' }
      ]
    }
  },
  computed: {
    todos () {
      return this.$store.getters.getTodos
    }
  }
}
</script>
