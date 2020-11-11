<template>
    <v-main class="list">
        <h3 class="text-h3 font-weight-medium mb-5">To Do List</h3>

        <v-card>
            <v-card-title>
                <v-text-field
                    v-model="search"
                    append-icon="mdi-magnify"
                    label="Search"
                    single-line
                    hide-details>
                </v-text-field>

                <v-spacer></v-spacer>

                <v-btn color="success" dark @click="dialog = true">
                    Tambah
                </v-btn>
            </v-card-title>

            <v-data-table :headers="headers" 
                        :items="todos" :search="search"
                        :single-expand="singleExpand"
                        :expanded.sync="expanded"
                        item-key="name"
                        show-expand
                        class="elevation-1">
                <template v-slot:top>
                    <v-toolbar flat>
                        <v-toolbar-title>Expandable Table</v-toolbar-title>
                            <v-spacer></v-spacer>
                            <v-switch
                                v-model="singleExpand"
                                label="Single expand"
                                class="mt-2"
                            ></v-switch>
                    </v-toolbar>
                </template>
                <template v-slot:expanded-item="{ headers, item }">
                    <td :colspan="headers.length">
                        {{ item.note }} 
                    </td>
                </template>
                <template v-slot:[`item.actions`]="{ item }">
                    <v-btn small class="mr-2" @click="dialog = true,editItem(item)">
                        edit
                    </v-btn>
                    <v-btn small @click="deleteItem(item)">
                        delete
                    </v-btn>
                </template>
                <template v-slot:[`item.priority`]="{ item }">
                    <v-chip v-if="item.priority == 'Penting'" color="red" outlined>{{item.priority}}</v-chip>
                    <v-chip v-else-if="item.priority == 'Tidak penting'" color="green" outlined>{{item.priority}}</v-chip>
                    <v-chip v-else-if="item.priority == 'Biasa'" color="blue" outlined>{{item.priority}}</v-chip>
                </template>
            </v-data-table>
        </v-card>

        <v-dialog v-model="dialog" persistent max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="headline">Form Todo</span>
                </v-card-title>
                <v-card-text>
                    <v-container>
                        <v-text-field
                            v-model="formTodo.task"
                            label="Task"
                            required>
                        </v-text-field>

                        <v-select
                            v-model="formTodo.priority"
                            :items="['Penting', 'Biasa', 'Tidak penting']"
                            label="Priority"
                            required>
                        </v-select>

                        <v-textarea
                            v-model="formTodo.note"
                            label="Note"
                            required>
                        </v-textarea>
                    </v-container>
                </v-card-text>

                <v-card-actions>
                    <v-spacer></v-spacer>

                    <v-btn color="blue darken-1" text @click="cancel">
                        Cancel
                    </v-btn>
                    <v-btn v-if="isEditing==true" color="blue darken-1" text @click="edit(formTodo)">
                        Update
                    </v-btn>
                    <v-btn v-else color="blue darken-1" text @click="save">
                        Save
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>

        <v-dialog v-model="deleteDialog" persistent max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="headline">Yakin ingin menghapus?</span>
                </v-card-title>
                <v-card-actions>
                    <v-btn color="green darken-1" text @click="cancel">Tidak</v-btn>
                    <v-btn color="red darken-1" text @click="del">Ya</v-btn>                    
                </v-card-actions>
            </v-card>
        </v-dialog>
    </v-main>
</template>


<script>
    export default {
        name: "List",
        data() {
            return {
                expanded: [],
                singleExpand: false,
                isEditing:false,
                deleteDialog:false,
                detailDialog:false,
                search: null,
                dialog: false,
                temp: null,
                headers: [
                    {
                        text: "Task",
                        align: "start",
                        sortable: true,
                        value: "task",
                    },
                    { text: "Priority", value: "priority" },
                    { text: "Actions", value: "actions" },
                ],

                todos: [
                    {
                        task: "bernafas",
                        priority: "Penting",
                        note: "huffttt",
                    },
                    {
                        task: "nongkrong",
                        priority: "Tidak penting",
                        note: "bersama tman2",
                    },
                    {
                        task: "masak",
                        priority: "Biasa",
                        note: "masak air 500ml"
                    },
                ],

                formTodo: {
                    task: null,
                    priority: null,
                    note: null,
                },
            };
        },

        methods: {
            save() {
                this.todos.push(this.formTodo);
                this.resetForm();
                this.dialog = false;
            },
            cancel() {
                this.resetForm();
                this.dialog = false;
                this.deleteDialog = false;
                this.detailDialog = false;
                this.isEditing = false;
                this.temp = null;
            },
            resetForm() {
                this.formTodo = {
                    task: null,
                    priority: null,
                    note: null,
                };
            },
            editItem(item){
                this.isEditing = true;
                this.formTodo = {
                    task : item.task,
                    priority : item.priority,
                    note : item.note,
                };
                this.temp = item;
            },
            edit(formTodo){
                this.temp.task = formTodo.task;
                this.temp.priority=formTodo.priority;
                this.temp.note = formTodo.note;
                this.cancel();
            },
            deleteItem(item) {
                this.deleteDialog=true;
                this.temp=item;
            },
            del(){
                this.todos.splice(this.todos.indexOf(this.temp),1);
                this.cancel();
            },
        },
    };
</script>