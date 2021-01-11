<template>
    <v-container
        style="
        font-family: Monaco;
        margin-left: 25%;
        "
    >
        <v-toolbar
            flat
            extended
            style="margin-left: 3.5%"
        >
            <v-toolbar-title>TO DO LIST - SAMUEL WILLIAN DE SOUZA MORAES</v-toolbar-title>

        </v-toolbar>


        <v-form ref="form">
            <v-row>
                <v-col>
                    <v-text-field
                        v-model='todo'
                        placeholder="Insira aqui a sua nova tarefa"
                        :rules="taskRules"
                        v-on:keyup.enter="addEnter"
                    >
                    </v-text-field>
                </v-col>

                <v-col>
                    <v-btn
                        @click="add"
                    >
                        <v-icon>mdi-plus</v-icon>
                    </v-btn>
                </v-col>
            </v-row>
        </v-form>


        <div>
            <p v-if="empty" style="margin-left: 20%;">Insira uma Tarefa</p>

            <v-row
                v-else
                v-for="task in tasks" :key="task.id"
                style="margin-left: 10%"
            >
                <v-col>
                    <v-card
                        class="mx-auto"
                        width="344"
                        elevation="24"
                    >
                        <v-card-title v-if="!task.completed">
                            {{task.tarefa}}
                        </v-card-title>

                        <v-card-title v-else
                            style="
                                font-style: italic;
                                text-decoration: line-through;
                                background-color: green;
                            "
                        >
                            {{task.tarefa}}
                        </v-card-title>

                        <v-card-actions>
                            <v-btn
                                outlined
                                rounded
                                text
                                @click="remove(task)"
                            >
                                Remove
                            </v-btn>

                            <v-spacer></v-spacer>

                            <v-btn
                                outlined
                                rounded
                                text
                                @click="complete(task)"
                            >
                                Completar
                            </v-btn>
                        </v-card-actions>
                    </v-card>   
                </v-col>
            </v-row>
        </div>
    </v-container>
</template>

<script>
const STORAGE_KEY = 'todo-app-storage'

export default {
    data: () => ({
        tasks : [],
        empty : true,
        todo: '',
        taskRules: [
            v => !!v
        ],
    }),

    created (){
        this.tasks = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');

        if(!this.tasks.length)
            this.empty = true;
        else
            this.empty = false;
    },
    methods: {
        add(){
            if( this.$refs.form.validate() ){
                this.tasks.unshift({tarefa: this.todo , id: this.tasks.length, completed: false});
                localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks));
                this.todo = '';
                this.empty = false;
            }
            else
                window.alert("Invalid task");
        },
        remove(task){
            this.tasks.splice(this.tasks.indexOf(task), 1);
            localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks));

            if(! this.tasks.length)
                this.empty = true;
        },
        complete(task){
            if(task.completed)
                task.completed = false;
            else
                task.completed = true;
        }
    }
}
</script>