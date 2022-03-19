<template>
  <v-app id="app">
    <v-app-bar max-height="4rem">
      <v-container>
        <p class="text-sm-h5 my-5">Employees list </p>
      </v-container>
    </v-app-bar>
    <v-container>

      <employee-form @add:employee="addEmployee"/> <!--@name-of-event="メソッド"-->
      <employee-table
          :employees="employees"
          @delete:employee="deleteEmployee"
          @edit:employee="editEmployee"
      />
    </v-container>
  </v-app>
</template>

<script>
import EmployeeTable from '@/components/EmployeeTable.vue'
import EmployeeForm from "@/components/EmployeeForm";

export default {
  name: 'app',
  components: {
    EmployeeForm,
    EmployeeTable,
  },
  data() {
    return {
      employees: [
        {
          id: 1,
          name: 'Richard Hendricks',
          email: 'richard@piedpiper.com',
        },
        {
          id: 2,
          name: 'Bertram Gilfoyle',
          email: 'gilfoyle@piedpiper.com',
        },
        {
          id: 3,
          name: 'Dinesh Chugtai',
          email: 'dinesh@piedpiper.com',
        },
      ],
    }
  },
  methods: {
    addEmployee(employee) {
      const lastId =
          this.employees.length > 0
              ? this.employees[this.employees.length - 1].id
              : 0;
      const id = lastId + 1;
      const newEmployee = {...employee, id};

      this.employees = [...this.employees, newEmployee];
    },
    deleteEmployee(id) {
      this.employees = this.employees.filter(
          employee => employee.id !== id
      )
    },
    editEmployee(id, updatedEmployee) {
      this.employees = this.employees.map(employee =>
          employee.id === id ? updatedEmployee : employee
      )
    }

  }
}
</script>

<style>
</style>
