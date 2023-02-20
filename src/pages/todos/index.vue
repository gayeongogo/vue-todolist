<template>
    <div class="container">
        <h2>TODO-List</h2>
        <button class="btn btnB" @click="moveToCreatePage">Create Todo</button>
        <input type="text" placeholder="검색" class="form-control" v-model="serchText">
        <!-- <TodoSimpleForm  @add-todo="addTodo"/> -->
        <div v-if="!todos.length">
            찾는 문장이 없습니다..
        </div>
        <TodoList :todos="todos" @delete-todo="deleteTodo" @toggle-todo="toggleTodo" />
        <br>
        <nav class="nav">
            <ul class="pagination">
                <li v-if="currentPage !==1" class="page-item"><a href="#" @click="getTodos(currentPage-1)" class="page-link"><i class="fa-solid fa-chevron-left"></i></a></li>

                <li v-for="page in numberOfPages" :key="page" class="page-item" :class="currentPage===page ? 'active' : ''">
                    <a href="#" @click="getTodos(page)" class="page-link" :class="currentPage===page ? 'active' : ''">{{ page }}</a>
                </li> 

                <li v-if="numberOfPages !==currentPage" class="page-item"><a href="#" @click="getTodos(currentPage+1)" class="page-link"><i class="fa-solid fa-chevron-right"></i></a></li>
            </ul>
        </nav>
    </div>
</template>

<script>
import { computed, ref, watch } from 'vue';
/* import TodoSimpleForm from '@/components/TodoSimpleForm.vue'; */
import TodoList from '@/components/TodoList.vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

export default {
    components: {
        /* TodoSimpleForm, */
        TodoList,
    },
    setup(){
        const router = useRouter();
        const todos = ref([]);
        const serchText = ref('');
        const error = ref('');
        const limit = 5;
        const numberOfTodos = ref(0);
        const currentPage=ref(1);
        const numberOfPages=computed(() =>{
            return Math.ceil(numberOfTodos.value/limit)
        });

        const getTodos = async (page = currentPage.value) =>{
            currentPage.value=page;
            try{
                const res=await axios.get(`http://localhost:3000/todos?_sort=id&_order=desc&subject_like=${serchText.value}&_page=${page}&_limit=${limit}`);
                //console.log(res.headers)
                numberOfTodos.value=res.headers['x-total-count'];
                todos.value=res.data;
            }catch (err){
                console.log(err);
                error.value="찾는 문장이 없습니다"
            }
        };
        getTodos();
        const moveToCreatePage = () => {
            router.push({
                name: 'TodoCreate',
            })
        }
        const addTodo = async (todo) => {
            error.value='';
            try{
                await axios.post('http://localhost:3000/todos', {
                    subject:todo.subject,
                    completed:todo.completed, 
                });
                getTodos(1)
                /* todos.value.push(res.data); */
            }catch (err){
                console.log(err);
                error.value="잘못된 입력입니다."
            }
            
        }
        watch(serchText, (/* current, prev */) => {
            //console.log(current, prev);
            getTodos(1);
        })

        /* const addTodo = (todo) => {
           // todos.value.push(todo);
            error.value='';
           axios.post('http://localhost:3000/todos',{
                subject:todo.subject,
                completed:todo.completed,
           }).then((res) =>{
                console.log(res);
                todos.value.push(res.data)
           }).catch((err) =>{
                console.log(err);
                error.value="잘못된 입력입니다."
           });
        } */
       
        const deleteTodo = async (index) => {
            error.value="";
            const id= todos.value[index].id;
            try{
                await axios.delete('http://localhost:3000/todos/'+ id);
                /* todos.value.splice(index, 1); */
                getTodos(1)
            }catch(err){
                console.log(err);
                error.value="찾는 문장이 없습니다"
            }
           
        }
        const toggleTodo= async (index, checked) =>{
           // console.log(index)
           error.value="";
           const id= todos.value[index].id;

           try{
                await axios.patch('http://localhost:3000/todos/'+ id, {
                    completed: checked
                });
                todos.value[index].completed= checked
           }catch(err){
                console.log(err);
                error.value="찾는 문장이 없습니다"
           }
        }
        /* const filteredTodos = computed(() =>{
            if(serchText.value){
                return todos.value.filter(todo =>{
                    return todo.subject.includes(serchText.value);
                })
            }
            return todos.value;
        }) */

        return {
            todos,
           /*  todoStyle, */
           deleteTodo,
           addTodo,
           toggleTodo,
           serchText,
           /* filteredTodos, */
           getTodos,
           numberOfPages,
           currentPage,
           moveToCreatePage,
        }
    }
}
</script>

<style>
    *{margin: 0; padding: 0; box-sizing: border-box;}
    .container{width: 100%; max-width: 1024px; margin: 0 auto; padding: 0 20px;}
    h2{text-align: center; color: #147f8d; margin-bottom: 50px; margin-top: 50px;}
    .d-flex{display: flex;}
    .flex-grow-1{flex-grow: 1;}
    .flex-grow-1 input{width: 98%; padding: 10px 20px;}
    .btn{padding: 12px 30px; border: none; background: #147f8d; color: #fff}
    form{margin-bottom: 50px;}
    .card{margin: 10px 0;}
    .card-body{border: 1px solid #ccc; padding: 10px 20px; display: flex; justify-content: space-between; align-items: center;}
    .form-check-input{margin-right: 10px;}
    .todoStyle{text-decoration: line-through;color: gray}
    .btnR{padding: 5px 20px; background: #e93838; color: #fff; border: none}
    .form-control{width: 100%; border: 1px solid #ddd; margin-bottom: 10px; padding: 10px 20px;}
    .pagination{list-style-type: none; display: flex; justify-content: center;}
    .page-item{ padding: 10px; margin: 0 3px; border: 1px solid #ddd}
    .page-link{color: #666; text-decoration: none;}
    .active{background: #666; color: #fff}
    .btnB{margin-bottom: 10px;}
</style>