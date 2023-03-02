<style lang="scss"></style>

<template>
    <TaskItem contentIntroduce='Hi I am the child component' />
    <div class="container">
        <div class="py-5">
            <div class="row align-items-start">
                <div class="col-4">
                    <form>
                        <div class="mb-3">
                            <input type="text" class="form-control" v-model='task.title'>
                        </div>
                        <button type="button" class="btn btn-primary" @click='(event) => createTask(event)'>CREATE</button>
                    </form>
                </div>
                <div class="col-8">
                    <table class="table table-bordered border-primary">
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Title</th>
                                <th>Action</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="({ id, title, created_at }, index) in tasks">
                                <td>{{ id }}</td>
                                <td>{{ title }}</td>
                                <td>
                                    <button type="button" class="btn btn-light me-2" @click='(event) => editTask(event)' data-id="e-{{ id }}">
                                        <img src="./assets/images/v1/icons/edit.png" width="24" height="24" />
                                    </button>
                                    <button type="button" class="btn btn-light" @click='(event) => deleteTask(event)' data-id="d-{{ id }}">
                                        <img src="./assets/images/v1/icons/delete.png" width="24" height="24" />
                                    </button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>


<script type="text/javascript">

import TaskItem from './components/TaskItem.vue';

export default {
    data() {
        return {
            tasks: [],
            task: {
                title: '',
            },
            question: '',
            answer: 'Questions usually contain a question mark. ;-)'
        }
    },
    components: {
        TaskItem
    },
    methods: {
        createTask(event) {
            let newTasks = this.tasks;
            newTasks.push({
                title: this.task.title,
                status: '',
                created_at: '',
                isComplete: false,
            });
            this.tasks = newTasks;
            this.storeTasksInDB(newTasks);
        },
        editTask(event) {
            console.log('editing');
        },
        deleteTask(event) {
            console.log(event);
        },
        storeTasksInDB(tasks) {
            if(tasks) {
                window.localStorage.setItem('tasks', JSON.stringify(tasks));
                return true;
            } else {
                return false;
            }
        },
        getTasksFromDB() {
            let existTasks = window.localStorage.getItem('tasks');
            if(existTasks) {
                return JSON.parse(existTasks);
            }
            return [];
        },
        async getAnswer() {
            this.answer = 'Thinking...'
            try {
                const res = await fetch('https://yesno.wtf/api')
                this.answer = (await res.json()).answer
            } catch (error) {
                this.answer = 'Error! Could not reach the API. ' + error
            }
        }
    },
    watch: {
        // Whenever 'task.title' changes, this function will run
        // The watch option also supports a dot-delimited path as the key
        'task.title'(newTitle, oldTitle) {
            if (newTitle.includes('?')) {
                this.getAnswer();
            }
        }
    },
    mounted() {
        let existTasks = this.getTasksFromDB();
        this.tasks = existTasks;
    }
}
</script>
