<style lang="scss">

</style>

<template>
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
                    <ul class="list-group">
                        <li class="list-group-item" v-for="({ title, created_at }, index) in tasks">
                            {{ title }} 
                        </li>
                    </ul>
                </div>
        </div>
        </div>
    </div>
</template>



<script type="text/javascript">
export default {
    data() {
        return {
            tasks: [
                {
                    title: 'Learning English',
                    status: '',
                    created_at: '',
                    isComplete: false,
                }
            ],
            task: {
                title: '',
            }
        }
    },
    methods: {
        createTask(event) {
            let newTasks = this.tasks;
            newTasks.push( {
                title: this.task.title,
                status: '',
                created_at: '',
                isComplete: false,
            });
            this.tasks = newTasks;
        }
    },
    mounted() {

    }
}
</script>
