<template>
    <div>
         <h1>Tela nova</h1>
    <ul>
            <li v-for="(dado, i) in api_dados" :key="i">{{dado.id}} ->  {{dado.texto}} -> {{dado.usuario.nome}}</li>
        </ul>
    <h2>INSIRA NOVA ANOTACAO</h2>
    <p>texto: <input type="text" id= "texto" v-model="texto"></p>

    <button @click="sendInfo()">Ok</button>
    </div>
   
</template>
<script>
export default {
    name: "TelanovaView",
    data() {
      return {
        api_dados: nulll

      }
    },  
    methods: {
      async getJokes(){
        const req = await fetch("http://localhost:8080/anotacao");
        const dados = await req.json();
        this.api_dados = dados
      },

     sendInfo(){
        var txt = document.getElementById("texto").value;
        var myHeaders = new Headers();
        var token = localStorage.getItem("Token");
      console.log(token);
      myHeaders.append("Authorization", `${token}`);
        myHeaders.append("Content-Type", "application/json");
        var raw = JSON.stringify({
          "texto": txt
        });
        var requestOptions = {
          method: 'POST',
          headers: myHeaders,
          body: raw,
          redirect: 'follow'
        };
        fetch("http://localhost:8080/anotacao", requestOptions)
          .then(response => response.text())
          .then(result => {
            var res = JSON.parse(result)
            console.log(res)
          })
          .catch(error => console.log('error', error));
      }
      
    },
    mounted(){
      this.getJokes()
    }
  }
  </script>