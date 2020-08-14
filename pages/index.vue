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
        <template v-slot:top>
          <v-dialog v-model="dialog" max-width="500px">
            <v-card>
              <v-card-title>
                <span class="headline">TODO編集</span>
              </v-card-title>
              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12">
                      <v-text-field v-model="todo.task" label="タスク" />
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>
              <v-card-actions>
                <v-spacer />
                <v-btn @click="close">
                  閉じる
                </v-btn>
                <v-btn class="primary" @click="update">
                  更新する
                </v-btn>
                <v-spacer />
              </v-card-actions>
            </v-card>
          </v-dialog>
        </template>
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
      ],
      todo: {}
    }
  },
  computed: {
    todos () {
      return this.$store.getters.getTodos
    }
  },
  methods: {
    edit (todo) {
      this.todo = Object.assign({}, todo)
      this.dialog = true
    },
    update () {
      const payload = { todo: this.todo }
      this.$store.commit('updateTodo', payload)
      this.close()
    },
    remove (todo) {
      const payload = { todo }
      this.$store.commit('removeTodo', payload)
    },
    close () {
      this.dialog = false
      this.todo = {}
    }
  }
}
</script>
