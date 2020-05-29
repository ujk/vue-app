<template>
    <div id="employee-table">

        <p v-if="employees.length < 1" class="empty-table">
            Çalışan kaydı bulunamadı
        </p>
        <table v-else>
            <thead>
                <tr>
                    <th>Çalışanın Adı</th>
                    <th>Çalışanın Emaili</th>
                    <th>İşlemler</th>
                </tr>
            </thead>
            
            <tbody>
                <tr v-for="employee in employees" :key="employee.id">
                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.name">
                    </td>
                    <td v-else>{{ employee.name }}</td>

                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.email">
                    </td>
                    <td v-else>{{ employee.email }}</td>

                    <td v-if="editing === employee.id">
                        <button @click="editEmployee(employee)">Kaydet</button>
                        <button class="muted-button" @click="cancelEdit(employee)">İptal</button>
                    </td>
                    <td v-else>
                        <button @click="editMode(employee)">Düzenle</button>
                        <button @click="$emit('delete:employee', employee.id)">Sil</button>
                    </td>
                </tr>
            </tbody>
        </table>

    </div>
</template>

<script>
    export default {
        name: "employee-table",
        props: {
            employees: Array
        },
        data() {
            return {
                editing: null
            }
        },
        methods: {
            editMode(employee) {
                this.cachedEmployee = Object.assign({}, employee)
                this.editing = employee.id
            },
            cancelEdit(employee) {
                Object.assign(employee, this.cachedEmployee)
                this.editing = null
            },

            editEmployee(employee) {
                if (employee.name === "" || employee.email === "") return
                this.$emit("edit:employee", employee.id, employee)
                this.editing = null
            }
        }
    };
</script>

<style scooped>
    button {
        margin: 0 0.5rem 0 0;
    }
</style>

