<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="text-center">No employees</p>
    <v-data-table
        v-else
        :headers="headers"
        :items="employees"
    >
      <template v-slot:[`item.name`]="{ item }">
        <div id="employee-name">
          <v-text-field v-model="item.name" :hide-details="true" dense single-line :autofocus="true"
                        v-if="item.id === editingID"></v-text-field>
          <span v-else>{{ item.name }}</span>
        </div>
      </template>
      <template v-slot:[`item.email`]="{ item }">
        <v-text-field v-model="item.email" :hide-details="true" dense single-line :autofocus="true"
                      v-if="item.id === editingID"></v-text-field>
        <span v-else>{{ item.email }}</span>
      </template>
      <template v-slot:[`item.actions`]="{ item }">
        <div v-if="item.id === editingID">
          <v-icon color="red" class="mr-3" @click="editingID = null">
            mdi-window-close
          </v-icon>
          <v-icon color="green" @click="editEmployee(item)">
            mdi-content-save
          </v-icon>
        </div>
        <div v-else>
          <v-icon color="green" class="mr-3" @click="editMode(item.id)">
            mdi-pencil
          </v-icon>
          <v-icon color="red" @click="$emit('delete:employee', item.id)">
            mdi-delete
          </v-icon>
        </div>
      </template>
    </v-data-table>

    <!--      <thead>-->
    <!--      <tr>-->
    <!--        <th>Employee name</th>-->
    <!--        <th>Employee email</th>-->
    <!--        <th>Actions</th>-->
    <!--      </tr>-->
    <!--      </thead>-->
    <!--      <tbody>-->
    <!--      <tr v-for="employee in employees" :key="employee.id">-->
    <!--        <v-form>-->
    <!--          <td id="employee-name">-->
    <!--            <div v-if="editingID === employee.id">-->
    <!--              <input type="text" v-model="employee.name"/>-->
    <!--            </div>-->
    <!--            <div v-else> {{ employee.name }}</div>-->
    <!--          </td>-->
    <!--          <td id="employee-email">-->
    <!--            <div v-if="editingID === employee.id">-->
    <!--&lt;!&ndash;              <v-text-field&ndash;&gt;-->
    <!--&lt;!&ndash;                  v-model="employee.email"&ndash;&gt;-->
    <!--&lt;!&ndash;                  label="Employee name"&ndash;&gt;-->
    <!--&lt;!&ndash;                  required&ndash;&gt;-->
    <!--&lt;!&ndash;              ></v-text-field>&ndash;&gt;-->
    <!--              <input type="text" v-model="employee.email"/>-->

    <!--            </div>-->
    <!--            <div v-else>{{ employee.email }}</div>-->
    <!--          </td>-->
    <!--          <td v-if="editingID === employee.id">-->
    <!--            <v-btn color="primary" @click="editEmployee(employee)">Save</v-btn>-->
    <!--            <v-btn @click="editingID = null">Cancel</v-btn>-->
    <!--          </td>-->
    <!--          <td v-else>-->
    <!--            <v-btn @click="editMode(employee.id)">Edit</v-btn>-->
    <!--            <v-btn color="error" @click="$emit('delete:employee', employee.id)">Delete</v-btn>-->
    <!--          </td>-->
    <!--          </v-form>-->
    <!--      </tr>-->
    <!--      </tbody>-->
  </div>
</template>


<script>
export default {
  name: 'employee-table',
  props: {// 親コンポーネントより渡されるデータ
    employees: Array,
  },
  data() {
    return {
      editingID: null,
      headers: [
        {
          text: 'name',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        {text: 'email', value: 'email'},
        {text: 'Actions', value: 'actions', sortable: false},

      ],
    }
  },
  methods: {
    editMode(id) {
      this.editingID = id
    },
    editEmployee(employee) {
      if (employee.name === '' || employee.email === '') return
      this.$emit('edit:employee', employee.id, employee)
      this.editingID = null
    }
  }
}
</script>

<style scoped>
</style>