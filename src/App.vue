<template>
    <div id="app" class="container">
        <img alt="Vue logo" src="./assets/logo.png">

        <EmployeeForm @add:employee="addEmployee"/>
        <EmployeeList :employees="employees"
                      @delete:employee="deleteEmployee"
                      @edit:employee="editEmployee"
        />

    </div>
</template>

<script>
    import EmployeeList from './components/EmployeeList.vue'
    import EmployeeForm from './components/EmployeeForm';

    export default {
        name: 'App',
        components: {
            EmployeeForm,
            EmployeeList
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

                this.employees = [...this.employees, newEmployee]
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
            },
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
        width: 25%;
    }
</style>
