<template>
    <div class="container containers ">
        <h1 class="text-center m-3">Todo App</h1>
        <div class="mb-3 main">
            <input type="text" class="form-control " placeholder="Add your new todo" v-model="task">&nbsp;
            <button class="btn" @click="addTask" style="background-color:#9C27B0; color:white;" ><strong>+</strong></button>&nbsp;
            <button class="btn btn-success" @click="submitTask"><strong>0</strong></button>
        </div>
        <div v-show="show">
            <ul class="list-group list-group-numbered">
                <li v-for="i, index in data " :key="index"
                    class="list-group-item d-flex justify-content-between align-items-start">
                    <div class="ms-2 me-auto">
                        {{ i.m }}
                    </div>
                    
                    <button class="badge bg-warning  pill" @click="DeleteTask(i.id)"><i
                            class="fa fa-trash"></i></button>&nbsp;
                    <button class="badge bg-success  pill" style="background-color: white;" @click="EditTask(i.id)"><i
                            class="fa fa-pencil-square-o" aria-hidden="true"></i></button>
                </li>
            </ul>
        </div>
    </div>
</template>
<script>
export default {
    mounted() {
        this.show = true;
        this.showData()
    },
    data() {
        return {
            data: [],
            task: '',
            show: false,
            edit_id:'',
            Save_edit_btn:true
        }
    },
    methods: {
        showData() {
            let index = 0;
            for (let o in localStorage) {
                if (o.includes('task')) {
                    if (index <= o.split('_').slice(-1)) {
                        index = o.split('_').slice(-1)
                    }
                }
            }
            //key in localstorage
            this.data = []
            for (let i = 1; i <= index; i++) {
                if (localStorage.getItem('task_detail_' + i) != null) {
                    this.data.unshift({
                        id: i,
                        m: localStorage.getItem('task_detail_' + i)
                    })
                }
            }
            console.log(">>>>", this.data)
        },
        addTask() {
            if (this.task == '') {
                alert("Please Add Your Task")
            } else {
                this.task == this.task

                let index = 0;
                for (let o in localStorage) {
                    if (o.includes('task')) {
                        if (index <= o.split('_').slice(-1)) {
                            index = o.split('_').slice(-1)
                        }
                    }
                }
                this.show = true,
                console.log(">>>>>>", index)
                index = parseInt(index) + 1
                localStorage.setItem("task_detail_" + index, this.task);
                this.showData()
                console.log(this.data)
                 this.task = null
            }
        },
        DeleteTask(id) {
            console.log(">>>>", id)
            let delete_todo = localStorage.removeItem('task_detail_' + id);
            this.showData()

        },
        EditTask(id) {
            this.task = localStorage.getItem('task_detail_' + id);
            console.log("???", this.task)
            this.edit_id=id
           
        },
        submitTask(){
            localStorage.setItem('task_detail_' + this.edit_id, this.task)
            this.showData()
        }

    }
}

</script>
<style>
.main {
    display: flex;
    padding: 2%;

}

.containers {
    margin: 8% auto;
    border: 1px solid white;
    border-radius: 2%;
    max-width: 30% !important;
    background-color: white;
    height: 100%;

}

.table {
    margin: 2%;
    height: 40px;
    width: 97% !important;
}

.alert {
    color: red;
}

.list-group {
    height: 300px !important;
    overflow-y: scroll;
}
</style>