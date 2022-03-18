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
        <td v-if="editingID === employee.id">
          <input type="text" v-model="employee.name"/>
        </td>
        <td v-else>{{ employee.name }}</td>
        <td v-if="editingID === employee.id">
          <input type="text" v-model="employee.email"/>
        </td>
        <td v-else>{{ employee.email }}</td>
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

</style>