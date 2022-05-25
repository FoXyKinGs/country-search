<template lang='pug'>
.container
  .content
    h1 Country
    #search-bar
      input(
        type='text'
        placeholder='Type any country name'
        v-model='inputValue'
        @keyup='changeHandle'
      )
      i.fa-solid.fa-magnifying-glass
    #search-bar-recommendation(
      v-if='inputValue !== ""'
    )
      .recommendation(
        v-if='countryList.length > 0'
        v-for='(value, key) in countryList'
      )
        .country-name(
          :key='key'
        )
          p {{ value.name.common }}
      .not-found(
        v-else
      )
        p Data not found
</template>

<script>
import axios from 'axios'
import { ref } from 'vue'

export default {
  setup () {
    const inputValue = ref('')
    const countryList = ref([])

    const changeHandle = () => {
      axios.get(`https://restcountries.com/v3.1/name/${inputValue.value}`)
        .then(response => {
          for (let i = 0; i < 5; i++) {
            countryList.value.push(response.data[i])
          }
        })
        .catch(err => console.log(err))
    }

    return {
      countryList,
      inputValue,
      changeHandle
    }
  }
}
</script>

<style scoped>
  .container {
    height: 100vh;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .content h1 {
    font-size: 72px;
  }

  .content #search-bar {
    position: relative;
  }

  .content #search-bar input {
    margin-top: 20px;
    border-radius: 10px;
    border: 0.5px solid #C8C8C8;
    padding: 0 20px 0 20px;
    width: 700px;
    height: 60px;
    font-size: 18px;
  }

  .content #search-bar input:focus {
    outline: none;
    border: 1px solid rgba(131, 98, 242, 0.5);
  }

  .content #search-bar input::-webkit-input-placeholder {
    color: #C8C8C8;
  }

  .content #search-bar i {
    color: #C8C8C8;
    position: absolute;
    right: 2.5%;
    top: 50%;
  }

  .content #search-bar-recommendation {
    margin-top: 10px;
    width: 700px;
    max-height: 150px;
    overflow: auto;
    background: #FFFFFF;
    box-shadow: -4px -4px 4px rgba(0, 0, 0, 0.5), 4px 4px 4px rgba(0, 0, 0, 0.5);
    border-radius: 5px;
    padding: 20px 0;
  }

  .content #search-bar-recommendation .recommendation {
    padding: 10px 30px;
  }

  .content #search-bar-recommendation .recommendation:hover {
    background-color: #F4F4F4;
    cursor: pointer;
  }

  .content #search-bar-recommendation .not-found {
    color: #FF0000;
    padding: 10px 30px;
  }
</style>
