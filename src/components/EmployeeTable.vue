<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="text-center">No employees</p>
    <table v-else>
      <thead>
      <tr>
        <th>Employee name</th>
        <th>Employee email</th>
        <th>Actions</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="employee in employees" :key="employee.id">
        <td id="employee-name">
          <div v-if="editingID === employee.id">
            <input type="text" v-model="employee.name"/>
          </div>
          <div v-else> {{ employee.name }}</div>
        </td>
        <td id="employee-email">
          <div v-if="editingID === employee.id">
            <input type="text" v-model="employee.email"/>
          </div>
          <div v-else>{{ employee.email }}</div>
        </td>
        <td v-if="editingID === employee.id">
          <button @click="editEmployee(employee)">Save</button>
          <button class="muted-button" @click="editingID = null">Cancel</button>
        </td>
        <td v-else>
          <button @click="editMode(employee.id)">Edit</button>
          <button @click="$emit('delete:employee', employee.id)">Delete</button>
        </td>
      </tr>
      </tbody>
    </table>
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
button {
  margin: 0 0.5rem 0 0;
}

tbody tr {
  height: 4.5rem
}

tbody #employee-name {
  width: 13rem
}

tbody #employee-email {
  width: 13rem
}
tbody input{
  width:10rem
}
</style>