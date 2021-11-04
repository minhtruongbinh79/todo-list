<template>
    <div class="item">
        <input class="mr-2"
            id="completed"
            type="checkbox"
            @change="updateItem(); completed = !completed"
            v-model="item.completed"
        />
        <input type="text" v-model="item.title" class="w-75 mr-2"
            :class="[isEditing ? 'active' : 'inactive']"
            :disabled="!isEditing"            
        />     
        <button @click="editing=false; updateItem()"
            :hidden="!isEditing"
            class="btn-save">&#10004;</button>
        <button :disabled="completed"
            @click="editing=true"
            :hidden="isEditing"
            class="btn-edit">&#9998;</button>
        <button @click="deleteItem()" class="btn-delete">&otimes;</button>
    </div>
</template>

<script>
export default {
    props: ['item'],
    data: function () {
        return {        
            editing: false,
            completed: Boolean(this.item.completed)
        }
    },
    methods: {
        deleteItem: function () {
            axios.delete('api/todo/' + this.item.id)
            .then(response => {
                if(response.status == 200) {
                    this.$emit('itemchanged');
                }
            })
            .catch(error => {
                console.log(error);
            })
        },
        updateItem: function () {
            axios.put('api/todo/' + this.item.id, {
                title: this.item.title,
                completed: this.item.completed
            })
            .then(response => {
                if(response.status == 200) {
                    this.$emit('itemchanged');
                }
            })
            .catch(error => {
                console.log(error);
            })
        },
    },
    computed: {
        isCompleted: function () {
            return this.completed;
        },
        isEditing: function () {
            return this.editing;
        }
    }
}
</script>

<style>
    .btn-delete {
		color: #fff;
		background-color: #e3342f;
		border-color: #e3342f;
        border: 1px solid transparent;
        border-radius: 0.25rem;
        padding-left: 0.3rem;
        padding-right: 0.3rem;
	}
	.btn-delete:hover {
		color: #fff;
		background-color: #d0211c;
		border-color: #c51f1a;
	}
    .btn-edit {
        color: #fff;
        background-color: #6cb2eb;
        border-color: #6cb2eb;
        border: 1px solid transparent;
        border-radius: 0.25rem;
        padding-left: 0.3rem;
        padding-right: 0.3rem;
        margin-right: 0.75rem;

    }
    .btn-edit:hover {
        background-color: #4aa0e6;
        border-color: #3f9ae5;
    }    
    .btn-save {
        color: #fff;
        background-color: #6cb2eb;
        border-color: #6cb2eb;
        border: 1px solid transparent;
        border-radius: 0.25rem;
        padding-left: 0.4rem;
        padding-right: 0.35rem;
        margin-right: 0.75rem;

    }
    .btn-save:hover {
        background-color: #4aa0e6;
        border-color: #3f9ae5;
    }
    .active {
        border: none;
        background-color: #c4c0bd;
    }
    .inactive {
        border: none;
    }
    .inp-text {
        border-color: transparent;
        background-color: initial;
        color: initial
    }
    .on {
        color: #212529
    }
    .off {
        color: #ffffff
    }
    .item {
        display: flex;
        justify-content: center;
        align-items: center;
    }
</style>