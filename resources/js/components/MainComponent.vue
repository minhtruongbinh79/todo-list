<template>
    <div class="w-25">
        <transition appear name="form" tag="form">
            <form @submit.prevent="addItem">
                <div class="input-group mb-3 w-100">
                    <input v-focus v-model="newItem.title" type="text" class="form-control form-control-lg">
                    <div class="input-group-append">
                        <button class="btn-add" type="submit" id="basic-addon2">&plus;Add</button>
                    </div>
                </div>
            </form>
        </transition>
        <index-component :items="items" v-on:reloadlist="getItems()"/>
    </div>
</template>

<script>
import IndexComponent from './IndexComponent.vue';
export default {
    components: { IndexComponent },
    data: function () {
        return {
            newItem: {
                title: "",
                errors:[]
            },
            items: []
        }
    },
    methods: {
        addItem: function () {
            // if(this.newItem.title == '') {
            //     console.log('Title required.');
            // } else {
                axios.post('/api/todo',{
                    title: this.newItem.title,
                })
                .then(respone => {
                    this.newItem.title = ''
                    this.getItems()
                })
                .catch(error => {
                    console.log(error);
                });
            // }
        },
        getItems: function () {
            axios.get('api/todo')
            .then(respone => {
                this.items = respone.data
            })
            .catch(error => {
                console.log(error);
            })
        }
    },
    mounted() {
        this.getItems();
    },
    directives: {
        focus: {
            // định nghĩa cho directive
            inserted: function (el) {
                el.focus()
            }
        }
    }
}
</script>

<style>
    .btn-add {
        color: #fff;
        background-color: #38c172;
        border-color: #38c172;
        border: 1px solid transparent;
        border-radius: 0.25rem;
        padding: 0.375rem 0.75rem;
    }
    .btn-add:hover {
        color: #fff;
        background-color: #2fa360;
        border-color: #2d995b;
    }
	.form-enter-active {
		transition: all 1s;
	}
	.form-enter {
		opacity: 0;
		transform: translateY(-30px);
	}
    .todo-enter-active {
        transition: all 1s;
    }
    .todo-enter {
        opacity: 0;
		transform: translateY(-30px);
    }
</style>