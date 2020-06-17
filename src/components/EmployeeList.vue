<template>
    <div id="main">
        <div id="employee-list">
            <div class="columns" id="header">
                <div class="column">Name</div>
                <div class="column">Email</div>
                <div class="column">Actions</div>
            </div>
            <p v-if="employees.length < 1" class="empty-table">
                No employees
            </p>
            <ul>
                <li v-for="employee in employees" :key="employee.id" class="columns">
                    <div id="name" class="column">
                        <div v-if="editing === employee.id">
                            <input type="text" v-model="employee.name" />
                        </div>
                        <div v-else>{{employee.name}}</div>
                    </div>
                    <div id="email" class="column">
                        <div v-if="editing === employee.id">
                            <input type="text" v-model="employee.email" />
                        </div>
                        <div v-else>{{employee.email}}</div>
                    </div>
                    <div id="actions" class="column">
                        <div v-if="editing === employee.id">
                            <button @click="editEmployee(employee)">Save</button>
                            <button class="muted-button" @click="cancelEdit(employee)">Cancel</button>
                        </div>
                        <div v-else>
                            <button @click="editMode(employee)">Edit</button>
                            <button @click="$emit('delete:employee', employee.id)">Delete</button>
                        </div>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'employee-list',
        props: {
            employees: Array,
        },
        data() {
            return {
                editing: null,
            }
        },
        methods: {
            editMode(employee) {
                this.cachedEmployee = Object.assign({}, employee);
                this.editing = employee.id
            },
            editEmployee(employee) {
                if (employee.name === '' || employee.email === '') return;
                this.$emit('edit:employee', employee.id, employee);
                this.editing = null
            },
            cancelEdit(employee) {
                Object.assign(employee, this.cachedEmployee);
                this.editing = null;
            }

        }
    }
</script>

<style scoped>
    #header {
        font-weight: bold;
    }

    #employee-list {
        margin-top: 3rem;
    }
</style>