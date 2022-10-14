<template>
<!-- :class="{ 'open-modal' : isModalOpen }" -->
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <add-new-employee-modal
      @addNewEmployee="addNewEmployee"
      :isOpen="isModalOpen"
    ></add-new-employee-modal>
    <div>
      <p>Change per view:</p>
      <select v-model="perPage" id="selectPerPage" @change="resetPagination">
        <option :value="5">5</option>
        <option :value="10" selected>10</option>
        <option :value="20">20</option>
        <option :value="50">50</option>
      </select>
      <button @click="openAddNewEmployeeModal">Add New Employee</button>
    </div>
    <table class="employees" style="margin: auto">
      <tr>
        <th>#</th>
        <th @click="sort('name')">Employee Name</th>
        <th @click="sort('position')">Position</th>
        <th @click="sort('office')">Office</th>
        <th @click="sort('age')">Age</th>
        <th @click="sort('startDate')">Start Date</th>
        <th @click="sort('salary')">Salary</th>
        <th>Action</th>
      </tr>
      <tr v-for='(emp, index) in filtered' :key="index">
        <td>{{index+1}}</td>
        <td @click='editInfo($event, index)' data-field="name">{{ emp.name }}</td>
        <td @click='editInfo($event, index)' data-field="position">{{emp.position}}</td>
        <td @click='editInfo($event, index)' data-field="office">{{emp.office}}</td>
        <td @click='editInfo($event, index)' data-field="age">{{emp.age}}</td>
        <td @click='editInfo($event, index)' data-field="startDate">{{emp.startDate}}</td>
        <td @click='editInfo($event, index)' data-field="salary">{{emp.salary}}</td>
        <td @click="deleteEmployee(index)">X</td>
      </tr>
    </table>
    <ul id="nav">
      <li 
        v-if="this.currentPage> 1"
        @click="currentPage--"
      >Prev</li>

      <li v-for="p in totalPages" :key="p" @click="changePage(p)">
        {{ p }}
      </li>

      <li 
        v-if="this.currentPage != totalPages"
        @click="currentPage++"
      >Next</li>
    </ul>
  </div>
</template>

<script>
import AddNewEmployeeModal from './components/addNewEmployeeModal.vue'
import employees from '../employees.json'

export default {
  name: 'App',
  components: {
    AddNewEmployeeModal
  },
  data() {
    return {
      employees: [...employees],
      employeesPerPage: [],
      counter: 0,
      perPage: 10,
      currentPage: 1,
      isModalOpen: false,
    }
  },
  methods: {
    resetPagination() {
      this.currentPage = 1
    },
    sort(col) {
      switch(col) {
        case 'name':
          this.employeesPerPage = this.employeesPerPage.sort((a, b) => {
            if(this.counter % 2 == 0)
              return a.name > b.name ? 1 : -1;
            else 
              return a.name > b.name ? -1 : 1;
          });
        break;

        case 'position': 
          this.employeesPerPage = this.employeesPerPage.sort((a, b) => {
            if(this.counter % 2 == 0)
              return a.position > b.position ? 1 : -1;
            else 
              return a.position > b.position ? -1 : 1;
          });
        break;

        case 'office': 
          this.employeesPerPage = this.employeesPerPage.sort((a, b) => {
            if(this.counter % 2 == 0)
              return a.office > b.office ? 1 : -1;
            else 
              return a.office > b.office ? -1 : 1;
          });
        break;

        case 'salary': 
          this.employeesPerPage = this.employeesPerPage.sort((a, b) => {
            const salaryA = parseInt(a.salary.slice(1))
            const salaryB = parseInt(b.salary.slice(1));
            if(this.counter % 2 == 0)
              return salaryA > salaryB ? 1 : -1;
            else 
              return salaryA > salaryB ? -1 : 1;
          });
        break;

        case 'age': 
          this.employeesPerPage = this.employeesPerPage.sort((a, b) => parseInt(a.age) - parseInt(b.age))
          break;
        /* eslint-disable */
        case 'startDate': 
          this.employeesPerPage = this.employeesPerPage.sort((a, b) => {
            const formatDateA = a.startDate.replaceAll('/', '-');
            const formatDateB = b.startDate.replaceAll('/', '-');
            const dateA = new Date(formatDateA);
            const dateB = new Date(formatDateB);

            if(this.counter % 2 == 0)
              return dateA > dateB ? 1 : -1;
            else 
              return dateA > dateB ? -1 : 1;
          });
        break;
      }

      this.counter++;
    },
    changePage(page) {
      this.currentPage = page
    },
    openAddNewEmployeeModal() {
      this.isModalOpen = !this.isModalOpen;
    },
    addNewEmployee(emp) {
      this.employees.unshift(emp);
      this.isModalOpen = false;
    },
    deleteEmployee(empID) {
      let employeeIndex = empID + this.currentPage * this.perPage - this.perPage;
      this.employees.splice(employeeIndex, 1);

      this.changePage(1);
    },
    editInfo(e, indexOfRow) {
      const indexOfJSON = indexOfRow + this.currentPage * this.perPage - this.perPage
      const objectKey = e.target.getAttribute('data-field')

      const editableInfo = prompt('Change value:');
      this.employees[indexOfJSON][objectKey] = editableInfo; 
    },
  },
  computed: {
    totalPages() {
      return Math.ceil(this.employees.length / this.perPage);
    },
    paginationStart() {
      return (this.currentPage - 1) * this.perPage
    },
    paginationEnd() {
      return this.paginationStart + this.perPage;
    },
    filtered() {
     return this.employees.slice(this.paginationStart, this.paginationEnd)
    }
  }
}
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  position: relative;
}

#app.open-modal {
  opacity: 0.5;
}

#nav {
  list-style: none;
  display: flex;
  justify-content: center;
}

#nav li {
  margin: 0 10px;
}

</style>
