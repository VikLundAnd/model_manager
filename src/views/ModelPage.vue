<template>
  <div class="about">
    <h1>Your model jobs: </h1>
    <input type="text" name="Your model ID" v-model="input.modelid" placeholder="ModelID"/>
    <button type="button" v-on:click="getListOfJobs()">Enter</button>
    <ul id="modelJobs"></ul>
  </div>

</template>

<script>
const ul = document.querySelector('#modelJobs');

    export default {
      name: 'ListOfJobs',
      data() {
        return {
          input: {
            modelid:""
          },
          token: null
        }

      },

    methods: {
      getListOfJobs() {
        fetch(`https://localhost:44368/api/Models/${this.input.modelid}`, {
          method: 'GET',
          credentials: 'include',
          headers: {
            'Authorization': 'Bearer ' + localStorage.token,
          'Content-Type': 'application/json'
        }
      })
      //.then(res => res.json())
      .then(data => {
        console.log(data)
        data.map((modelJobs) => {
          let li = createNode('li'),
          span = createNode('span');
        span.innerText = modelJobs.location;
        appendNode(li,span);
        appendNode(ul,li);
      });
    })
      .catch(error => {
         console.error('Error:', error);
    });
  }}}
  const createNode = (elem) => {
    return document.createElement(elem);
  };

  const appendNode = (parent, elem) => {
    parent.appendChild(elem);
  }

</script>

<style scoped>

</style>
