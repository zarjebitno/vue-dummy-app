<template>
    <div id="newEmployeeModal" v-show="isOpen" @click="close($event)">
      <div class="modal-body">
        <input type="text" placeholder="Employee Name" v-model="employee.name"/>
        <input type="text" placeholder="Position" v-model="employee.position"/>
        <input type="text" placeholder="Office" v-model="employee.office"/>
        <input type="text" placeholder="Age" v-model="employee.age"/>
        <input type="text" placeholder="Start Date" v-model="employee.startDate"/>
        <input type="text" placeholder="Salary" v-model="employee.salary"/>
        <button @click="addNewEmployee">Add Employee</button>
      </div>
    </div>
</template>

<script>
export default {
  props: ['isOpen'],
  data() {
    return {
      employee: {
        name: '',
        position: '',
        office: '',
        age: '',
        startDate: '',
        salary: ''
      }
    }
  },
  methods: {
    addNewEmployee() {
      if(this.employee.name || this.employee.position || this.employee.age) {
        this.$emit('addNewEmployee', this.employee)
      }
    },

    close(e) {
      const target = e.target

      if(target.id =='newEmployeeModal') {
        // 
      }
    }
  }
}
</script>

<style>
#newEmployeeModal {
  position: fixed;
  left: 50%;
  top: 0%;
  transform: translate(-50%, 0%);
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: center;
  align-items: center;
}

#newEmployeeModal .modal-body input {
  display: block;
  margin: 10px 20px;
}

#newEmployeeModal .modal-body {
  display: flex;
  flex-flow: column;
  width: 40em;
  margin: auto;
  background: #42b883;
  padding: 2rem;
  border-radius: 2rem;
  z-index: 20;
}
#newEmployeeModal .modal-body button {
  max-width: 250px;
  margin: .5rem auto;
}
</style>