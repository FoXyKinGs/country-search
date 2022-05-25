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
      .display(
        v-if='!countryList.isLoading'

      )
        .not-found(
          v-if='countryList.isError'
        )
          p Data not found
        .recommendation(
          v-else
          v-for='(value, key) in countryList.data'
        )
          .country-name(
            :key='key'
            @click='detailCountry(value.name.common)'
          )
            p {{ value.name.common }}
      .loading(
        v-else-if='countryList.isLoading'
      )
        p Loading
</template>

<script>
import axios from 'axios'
import { ref } from 'vue'
import router from '@/router'
import { watch } from '@vue/runtime-core'

export default {
  setup () {
    const inputValue = ref('')
    const countryList = ref({
      isLoading: true,
      isError: false,
      data: []
    })

    const changeHandle = () => {
      // set default
      countryList.value.data = []
      countryList.value.isLoading = true
      countryList.value.isError = false

      axios.get(`https://restcountries.com/v3.1/name/${inputValue.value}`)
        .then(response => {
          // set to be done
          countryList.value.data = response.data
          countryList.value.isError = false
          countryList.value.isLoading = false
        })
        .catch(() => {
          // set if was error
          countryList.value.isLoading = false
          countryList.value.isError = true
        })
    }

    watch(() => countryList.value.data,
      (newData) => {
        if (newData.length > 5) {
          const data = JSON.parse(JSON.stringify(newData))
          countryList.value.data = data.slice(0, 5)
        } else {
          countryList.value.data = newData
        }
      }
    )

    const detailCountry = (name) => {
      router.push(`/detail/${name}`)
    }

    return {
      countryList,
      inputValue,
      changeHandle,
      detailCountry
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
    box-shadow: -4px -4px 4px rgba(0, 0, 0, 0.02), 4px 4px 4px rgba(0, 0, 0, 0.02);
    border-radius: 5px;
    padding: 20px 0;
  }

  .content #search-bar-recommendation .display .recommendation {
    padding: 10px 30px;
  }

  .content #search-bar-recommendation .display .recommendation:hover {
    background-color: #F4F4F4;
    cursor: pointer;
  }

  .content #search-bar-recommendation .display .not-found {
    color: #FF0000;
    padding: 10px 30px;
  }
</style>
