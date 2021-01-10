<template>
  <div id="app">

    <nav>
      <div class="nav-wrapper red darken-1">
        <a href="#" class="brand-logo center">Gestão de Clientes</a>
      </div>
    </nav>

    <div class="container">

      <ul>
        <li v-for="(erro, index) of errors" :key="index">
          campo <b>{{erro.field}}</b> - {{erro.defaultMessage}}
        </li>
      </ul>


      <form @submit.prevent="salvar">

          <label>Nome</label>
          <input type="text" placeholder="Nome" v-model="user.nome" >
          <label>E-mail</label>
          <input type="text" placeholder="E-mail" v-model="user.email" >
          <label>Telefone</label>
          <input type="tel" placeholder="Telefone" v-model="user.tel" >

          <button class="waves-effect waves-light btn-small">Salvar<i class="material-icons left">save</i></button>

      </form>

      <table>

        <thead>

          <tr>
            <th>NOME</th>
            <th>E-MAIL</th>
            <th>TELEFONE</th>
            
          </tr>

        </thead>

        <tbody>

          <tr v-for="user in users" :key="user.id">

            <td>{{ user.nome }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.tel }}</td>
            
            <td>
              <button @click="editar(user)" class="waves-effect btn-small blue darken-1"><i class="material-icons">create</i></button>
              <button @click="remover(user)" class="waves-effect btn-small red darken-1"><i class="material-icons">delete_sweep</i></button>
            </td>

          </tr>

        </tbody>
      
      </table>

    </div>

  </div>
</template>

<script>

import User from './services/users'

export default {
  name: 'app',
  data () {
    return {
      user:{
        id: '',
        nome: '',
        email: '',
        tel: ''
        
      },
      users: [],
      errors: []
    }
  },

  mounted(){
    this.listar()
  },

  methods:{
    
    listar(){
      User.listar().then(resposta => {
        this.users = resposta.data
      }).catch(e => {
        console.log(e)
      })
    },

    salvar(){

      if(!this.user.id){

        User.salvar(this.user).then(resposta => {
          this.user = {}
          alert('Cadastrado com sucesso!')
          this.listar()
          this.errors = {}
        }).catch(e => {
          this.errors = e.response.data.errors
        })

      }else{

        User.atualizar(this.user).then(resposta => {
          this.users = {}
          this.errors = {}
          alert('Atualizado com sucesso!')
          this.listar()
        }).catch(e => {
          this.errors = e.response.data.errors
        })

      }
      

    },

    editar(user){
      this.user = user
    },

    remover(user){
      if(confirm('Deseja excluir o cliente?')){

        User.apagar(user).then(Response => {
          this.listar()
          this.errors = {}
        }).catch(e => {
          this.errors = e.response.data.errors
        })

      }

    }
  }

}
</script>

<style>



</style>
