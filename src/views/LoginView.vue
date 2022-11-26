<template>
    <div>
        <p>Nome: <input type="text" id="user" v-model="nome"></p>
        <p>Senha: <input type="password" id="password" v-model="senha"></p>
        <button @click="login()">Ok</button>
        {{status}}
    </div>    
</template>

<script>
export default {
    name: 'loginView',
    data() {
        return {
            nome: '',
            senha: '',
            status: ''
        }
    },
    methods: {
      login() {
        var user = document.getElementById("user").value;
        var password = document.getElementById("password").value;
        console.log(user)
        console.log(password)
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");
        var raw = JSON.stringify({
          "nome": user,
          "senha": password,
          "status": null
        });
        var requestOptions = {
          method: 'POST',
          headers: myHeaders,
          body: raw,
          redirect: 'follow'
        };
        fetch("http://localhost:8080/login", requestOptions)
          .then(response => response.text())
          .then(result => {
            var res = JSON.parse(result)
            window.location.replace("/")
            localStorage.setItem("User", res.name)
            localStorage.setItem("Role", res.autorizacao)
            localStorage.setItem("Token", "Bearer "+res.token)
            console.log(res.token)
          })
          .catch(error => console.log('error', error));
      },
    },
}
</script>

  
  
