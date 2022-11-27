<template>
    <div class="container">
        <h1>ENVIAR IMAGEM</h1>
        <form id="form" enctype="multipart/form-data">
            <div class="input-group">
                <label for="files">Select files</label>
                <input id="file" type="file" multiple />
            </div>
            <button class="submit-btn"  type="submit">Upload</button>

<p>IMAGEM ABAIXO: </p>
          <p> <img  :src="dados_img"    alt="Red dot" /></p>
        </form>
    </div>

   
</template>
<script>
export default {
    name: "TelanovaView",
    data() {
      return {
        api_dados: null,
        dados_img: null

      }
    },  
    methods: {
      async getJokes(){
     var myHeaders = new Headers();
      var token = localStorage.getItem("Token");
      console.log(token);
      myHeaders.append("Content-Type", "application/json");
      myHeaders.append("Authorization", `${token}`);
      var requestOptions = {
        method: "GET",
        headers: myHeaders,
        redirect: "follow",
      };
        const req = await fetch("https://subiter.herokuapp.com/requests/download/6", requestOptions);
        const dados = await req.json();
        this.api_dados = dados
        this.dados_img = "data:image/png;base64, "+ dados.img
      },

     sendInfo(){
        const form = document.getElementById("form");
const inputFile = document.getElementById("file");
var myHeaders = new Headers();
        var token = localStorage.getItem("Token");
      console.log(token);
      myHeaders.append("Authorization", `${token}`);

const formData = new FormData();

const handleSubmit = (event) => {
    event.preventDefault();

    for (const file of inputFile.files) {
        formData.append("files", file);
    }

    fetch("https://subiter.herokuapp.com/requests/upload/9", {
        method: "post",
        body: formData,
    }).catch((error) => ("Something went wrong!", error));
};

form.addEventListener("submit", handleSubmit);
      }
      
    },
    mounted(){
      this.getJokes()
    }
  }
  </script>