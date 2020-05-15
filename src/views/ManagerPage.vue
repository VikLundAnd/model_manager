<template>
  <div class="manager">
    <h1>Manager page</h1>

    <p>Create new model</p>
    <input type="text" name="firstName" v-model="input.firstName" placeholder="First name" />
    <input type="text" name="fastName" v-model="input.lastName" placeholder="Last name" />
    <input type="text" name="email" v-model="input.email" placeholder="E-mail" />
    <button type="button" v-on:click="CreateModel()">Create model</button>
  </div>
</template>

<script>
export default {
  data() {
      return {
          input: {
              firstName: "",
              lastName: "",
              email: ""
          },
      }
  },

  methods: {
    CreateModel() {
      if (this.input.firstName != "" && this.input.lastName != "" && this.input.email != "")
      {
        fetch("https://localhost:44368/api/models", {
          method: 'POST',
          credentials: 'include',
           body: JSON.stringify(({
           firstName: this.input.firstName,
           lastName: this.input.lastName,
           email: this.input.email
         })),
         headers: new Headers({
           'Authorization': 'Bearer ' + localStorage.token,
           'Content-Type': 'application/json'
        })
      }).catch(error => console.error('Error:', error))
      }

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
