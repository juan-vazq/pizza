<template>
    <div>
        <form @submit.prevent="agregar">
            <h3>Pedido nuevo</h3>
            <input type="date" class="form-control mb-2" placeholder="Fecha" v-model="pedido.fecha">
            <input type="number" class="form-control mb-2" placeholder="Total" v-model="pedido.total">
            <select v-model="pedido.pizzas" class="form-control mb-2">
                <option value="0">SELECCIONA UNA PIZZA</option>
                <option v-for="pizza in pizzas" v-bind:value="pizza.id">{{ pizza.nombre }}</option>
            </select>
            <button class="btn btn-primary" type="submit">Agregar</button>
        </form>
        <hr>
        <h3>Pedidos:</h3>
        <table class="table" v-if="userID == 1">
            <thead class="thead-dark">
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Fecha</th>
                    <th scope="col">Total</th>
                    <th scope="col">Pizzas</th>
                    <th scope="col">Usuario</th>
                    <th scope="col">Creacion</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, index) in pedidos" :key="index" >
                    <th scope="row">{{item.id}}</th>
                    <td>{{item.fecha}}</td>
                    <td>{{item.total}}</td>
                    <td>{{item.nombre}}</td>
                    <td>{{item.name}}</td>
                    <td>{{item.updated_at}}</td>
                </tr>
            </tbody>
        </table>

        <div class="alert alert-danger" role="alert" v-else>
            No tienes permiso para ver esto
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                userID: currentUserId,
                pedidos: [],
                pizzas: [],
                pedido: {fecha: '', total: 0, pizzas: 0}
            }
        },
        created(){
            axios.get('/pedidos').then(res=>{
                this.pedidos = res.data;
            })

            axios.get('/pizzas').then(res=>{
                this.pizzas = res.data;
            })
        },
        methods:{
            agregar(){
                if(this.pedido.fecha.trim() === '' || this.pedido.total === 0 || this.pedido.pizzas === 0){
                    alert('Error, checa los datos');
                    return;
                }
                const le = this.pedido;
                this.pedido = {fecha: '', total: 0, pizzas: 0};    
                axios.post('/pedidos', le)
                .then((res) =>{
                    axios.get('/pedidos').then(res=>{
                        this.pedidos = res.data;
                    })
                    alert("EXITO");
                })
            }
        }
    }
</script>