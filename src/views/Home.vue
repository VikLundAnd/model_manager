<template>
    <div id="login">
        <h1>Login</h1>
        <input type="text" name="username" v-model="input.username" placeholder="Username" />
        <input type="password" name="password" v-model="input.password" placeholder="Password" />
        <button type="button" v-on:click="login()">Login</button>
    </div>
</template>

<script>
    export default {
        name: 'Login',
        data() {
            return {
                input: {
                    username: "",
                    password: ""
                },
                token: null,
                user: null
            }
        },
        methods: {
            login() {
              if (this.input.password != "" && this.input.username != "")
              {
                fetch("https://localhost:44368/api/account/login", {
                  method: 'POST',
                   body: JSON.stringify(({
                   email: this.input.username,
                   password: this.input.password
                 })),
                   headers: new Headers({ 'Content-Type': 'application/json'
                  })
                }).then(res => res.json().then((token) => {
                   localStorage.setItem("token", token.jwt);
                 }))
                .catch(error => console.error('Error:', error))

                this.token = localStorage.token;

                if(this.token != null)
                {
                  this.retrieveAccountInfo(this.token)

                  //this.$router.replace({ name: "Manager" });
                } else
                {
                  console.log("The username and / or password is incorrect");
                }

              } else {
                console.log("A username and password must be present");
              }

            },
            retrieveAccountInfo(token) {

              const response = fetch("https://localhost:44368/api/Managers", {
                method: 'GET',
                credentials: 'include',
                 headers: {
                   'Authorization': 'Bearer ' + token,
                   'Content-Type': 'application/json'}
              })
              .catch(error => console.error('Error:', error))

              console.log(response.json)

            }
        }
    }
</script>

<style scoped>
    #login {
        width: 500px;
        border: 1px solid #CCCCCC;
        background-color: #FFFFFF;
        margin: auto;
        margin-top: 200px;
        padding: 20px;
    }
</style>
