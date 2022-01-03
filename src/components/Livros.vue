<template>
  <v-container>
    <v-layout>
      <v-flex xs6 offset-xs3>
        <v-layout row><h1 class="display-1">Novo Livro</h1></v-layout>
        <form @submit.prevent="submit">
          <v-text-field v-model="livro.nome"
            label="Nome"
          ></v-text-field>
          <v-text-field v-model="livro.dataDeLancamento"
            label="Data de Lançamento"
            required
          ></v-text-field>
          <v-text-field v-model="livro.numPaginas"
            label="Numero de Paginas"
            required
          ></v-text-field>
          <v-text-field v-model="livro.autorId"
            label="Autor"
            required
          ></v-text-field>
          <v-btn
            class="mr-4"
            type="submit"
            @click="salvar"
          >Cadastrar</v-btn>
          <v-btn @click="limpar">Limpar</v-btn>
        </form>
      </v-flex>
    </v-layout>
    <v-layout row wrap>
      <h1 class="display-1">Livros Cadastrados</h1>
    </v-layout>
    <v-layout row wrap>
      <v-flex xs10 offset-xs1>
        <v-data-table
        :headers="headers"
        :items="livros"
        :items-per-page="5"
        class="elevation-3"
      >
        <template slot="items" slot-scope="livro">
          <td>{{livro.nome}}</td>
          <td>{{livro.dataDeLancamento}}</td>
          <td>{{livro.numPaginas}}</td>
          <td>{{livro.autorId}}</td>
        </template>
      </v-data-table>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      livro: {},
      autores: [],
      livros: [],
      headers: [
        {
          text: 'Nome',
          align: 'start',
          sortable: true,
          value: 'nome'
        },
        { text: 'Data de Lançamento', value: 'dataDeLancamento', sortable: true },
        { text: 'Páginas', value: 'numPaginas', sortable: true },
        { text: 'Autor', value: /* JSON.stringify('this.livro.autor') */'autorId', sortable: true }
      ]
    }
  },

  methods: {
    async buscarTodos () {
      //  HTTP Request: GET (http://localhost:8080/livros)
      var resposta = await axios.get('http://localhost:8080/livros')
      this.livros = resposta.data
    },

    async salvar () {
      //  HTTP Request: POST (http://localhost:8080/livros)
      await axios.post('http://localhost:8080/livros', this.livro)
      alert("Salvo com Sucesso!")
      this.buscarTodos()
      this.limpar()
    },
    //limpar caixa de inputs após cadastrar
    limpar () {
      this.livro.nome = ''
      this.livro.dataDeLancamento = ''
      this.livro.numPaginas = ''
      this.livro.autor = ''
    },
    asyc atualizar () {
        
    }
  },
  mounted () {
    this.buscarTodos()
  }

}
</script>
