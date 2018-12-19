<template>
  <v-card>
    <v-toolbar 
     dense 
     card
     color="info"
     dark
    >
      <v-toolbar-title>My To Do List</v-toolbar-title>
    </v-toolbar>
    <v-container fluid grid-list-xl>
      <v-layout align-center justify-center column>
        <v-flex md6>
          <v-text-field
          v-model="task"
          :disabled="processingTask"
          :append-outer-icon="task ? 'send' : ''"
          clearable
          autofocus
          label="New Task"
          @keyup.enter="submitAddTask"
          @click:append-outer="submitAddTask"
        />
        </v-flex>
        <v-flex md6>
          <v-data-table
          :headers="[
          { text: 'Description', value: 'description', width: '580', align: 'center', sortable: false, },
          { text: 'Actions', value: '', width: '180', align: 'center', sortable: false },

          ]" 
          :items="toDoList"
          hide-actions
          class="elevation-1"
          >
            <tr
              v-if="props.item.status !== 'Done'" 
              slot="items" 
              slot-scope="props"
            >
              <td class="text-xs-center">{{ props.item.description }}</td>
              <td class="text-xs-center">
                <v-btn
                  small
                  fab
                  flat
                  color="success"
                  class="ma-0"
                  @click="submitDoneTask(props.item)"
                >
                  <v-icon>done</v-icon>
                </v-btn>
                <v-btn
                  small
                  fab
                  flat
                  color="error"
                  class="ma-0"
                  @click="submitCancelTask(props.item)"
                >
                  <v-icon>cancel</v-icon>
                </v-btn>
              </td>
            </tr>
          </v-data-table>
        </v-flex>

      </v-layout>
    </v-container>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    processingTask: false,
    task : '',

    toDoList: [
      {
        description: "Mi firts task",
        status: 'pending',
        task_id: 1
      }
    ],

  }),

  methods: {

    submitAddTask (e) {
      e.preventDefault()
      if(this.task) {
        this.toDoList.push({
          description: this.task,
          status: 'pending',
          task_id: this.toDoList.length +1
        })
        this.task = ''
      }
    },

    submitDoneTask (item) {
      let auxList = this.toDoList.map(val => {
        if( val.task_id === item.task_id ) val.status = 'Done'
        return val
      })

      this.toDoList = [ ...auxList ]
    },

    submitCancelTask (item) {
      const index = this.toDoList.findIndex(val => val.task_id === item.task_id)
      this.toDoList.splice(index, 1)
    },


  }
};
</script>
