<template lang='pug'>
.container
  router-link.btn-back(
    to='/'
  )
    i.fa-solid.fa-arrow-left
    |  Back to Homepage
  .content(
    v-if='!dataDetail.isLoading'
  )
    #country-name
      .title
        h1 {{dataDetail.data.name.common}}
        img(
          :src='dataDetail.data.flags.png'
        )
      .another-name
        p(
          v-for='(value, key) in dataDetail.data.altSpellings'
          :key='key'
        ) {{value}}
    #location
      .left-content
        .display-box
          .left-box
            p.title LatLong
            p.location {{ latlngHelper(dataDetail.data.latlng[0]) }}, {{ latlngHelper(dataDetail.data.latlng[1]) }}
          .right-box
            img(
              src='@/assets/globe.svg'
            )
      .right-content
        .display-box
          p.capital Capital: {{ dataDetail.data.capital[0] }}
          p.region Region: {{ dataDetail.data.region }}
          p.subregion Subregion: {{ dataDetail.data.subregion }}
    #more-information
      .left-content
        .call-code
          p.title Calling Code
          p.number {{ numberTelHelper(dataDetail.data.idd.root) + dataDetail.data.idd.suffixes[0] }}
          p.more-information
            span(
              :title='sameNumTel(callingCode)'
            ) {{ callingCode.length }} country
            |  with this calling code
      .right-content
        .currency
          p.title Currency
          p.currency-type {{ Object.keys(dataDetail.data.currencies).toString() }}
          p.more-information
            span(
              :title='filterCurrency(sameCurrency)'
            ) {{ sameCurrency.length }} country
            |  with this currency
  .loading(
    v-else
  )
    p Loading
</template>

<script>
import router from '@/router'
import axios from 'axios'
import { ref } from '@vue/reactivity'
import { watch } from '@vue/runtime-core'
export default {
  setup () {
    const params = router.currentRoute.value.params.name
    const dataDetail = ref({
      isLoading: true,
      isError: false,
      data: {}
    })
    const callingCode = ref([])
    const sameCurrency = ref([])

    dataDetail.value.isLoading = true
    dataDetail.value.isError = false
    dataDetail.value.data = {}

    axios.get(`https://restcountries.com/v3.1/name/${params}`)
      .then(response => {
        dataDetail.value.isLoading = false
        dataDetail.value.data = { ...response.data[0] }
      })
      .catch(() => {
        dataDetail.value.isLoading = false
        dataDetail.value.isError = true
      })

    const latlngHelper = (val) => {
      return val + '.0'
    }

    const numberTelHelper = (val) => {
      return val.replace(/\+/g, '')
    }

    const sameNumTel = (val) => {
      let holder = ''
      val.forEach(item => {
        holder += `${item.name}\n`
      })
      return holder
    }

    const filterCurrency = (val) => {
      let holder = ''
      val.forEach(item => {
        holder += `${item.name}\n`
      })
      return holder
    }

    watch(() => dataDetail.value.data,
      (newData) => {
        const data = JSON.parse(JSON.stringify(newData))
        const number = `${numberTelHelper(data.idd.root)}${data.idd.suffixes[0]}`
        axios.get(`https://restcountries.com/v2/callingcode/${number}`)
          .then(response => {
            callingCode.value = response.data.map(val => {
              return { name: val.name }
            })
          })
          .catch(err => console.log(err))
      }
    )

    watch(() => dataDetail.value.data,
      (newData) => {
        const data = JSON.parse(JSON.stringify(newData))
        const currency = Object.keys(data.currencies).toString()
        axios.get(`https://restcountries.com/v3.1/currency/${currency}`)
          .then(response => {
            sameCurrency.value = response.data.map(val => {
              return { name: val.name.common }
            })
          })
          .catch(err => console.log(err))
      }
    )

    return {
      dataDetail,
      latlngHelper,
      numberTelHelper,
      callingCode,
      sameNumTel,
      sameCurrency,
      filterCurrency
    }
  }
}
</script>

<style scoped>
  .container {
    padding: 50px;
  }

  .btn-back {
    color: #fff;
    border: none;
    width: 230px;
    height: 50px;
    border-radius: 10px;
    background-color: #8362F2;
    font-size: 18px;
    padding: 10px;
    text-decoration: none;
  }

  .content #country-name {
    margin-top: 50px;
  }

  .content #country-name .title {
    display: flex;
    align-items: center;
  }

  .content #country-name .title h1 {
    font-size: 48px;
  }

  .content #country-name .title img {
    width: 46px;
    height: 30px;
    margin-left: 20px;
  }

  .content #country-name .another-name {
    font-size: 12px;
    display: flex;
    color: #fff;
  }

  .content #country-name .another-name p {
    background: #8DD4CC;
    margin: 5px 5px 5px 0;
    padding: 5px 10px;
    border-radius: 50px;
  }

  .content #location {
    display: flex;
  }

  .content #location .left-content {
    padding: 10px;
    width: 50%;
    height: 145px;
  }

  .content #location .left-content  .display-box {
    display: flex;
    overflow: hidden;
  }

  .content #location .left-content  .display-box .left-box {
    width: 60%;
    height: 100%;
    padding: 20px;
    overflow: auto;
  }

  .content #location .left-content  .display-box .left-box .title {
    font-size: 18px;
  }

  .content #location .left-content  .display-box .left-box .location {
    font-size: 48px;
    margin: 10px 0;
    color: #8362F2;
    font-weight: 700;
  }

  .content #location .left-content  .display-box .right-box {
    width: 40%;
    height: 100%;
    display: flex;
    justify-content: flex-end;
  }

  .content #location .left-content  .display-box .right-box img {
    margin-top: 15px;
    width: 204px;
    height: 204px;
  }

  .content #location .right-content {
    padding: 10px;
    width: 50%;
    height: 145px;
  }

  .content #location .right-content .display-box {
    font-size: 18px;
    padding: 20px;
    line-height: 25px;
  }

  .content #location .left-content .display-box,
  .content #location .right-content .display-box {
    height: 100%;
    box-shadow: -4px -4px 4px rgba(0, 0, 0, 0.02), 4px 4px 4px rgba(0, 0, 0, 0.02);
  }

  .content #more-information {
    display: flex;
  }

  .content #more-information .left-content {
    padding: 10px;
    width: 50%;
    height: 145px;
  }

  .content #more-information .left-content .call-code {
    padding: 10px
  }

  .content #more-information .left-content .call-code .title {
    font-size: 18px;
  }

  .content #more-information .left-content .call-code .number {
    margin: 10px 0;
    color: #8362F2;
    font-size: 48px;
    font-weight: 700;
  }

  .content #more-information .left-content .call-code .more-information {
    font-size: 14px;
  }

  .content #more-information .left-content .call-code .more-information>span {
    color: #8362F2;
    text-decoration: underline;
    cursor: pointer;
  }

  .content #more-information .right-content {
    padding: 10px;
    width: 50%;
    height: 145px;
  }

  .content #more-information .right-content .currency {
    padding: 10px
  }

  .content #more-information .right-content .currency .title {
    font-size: 18px;
  }

  .content #more-information .right-content .currency .currency-type {
    margin: 10px 0;
    color: #8362F2;
    font-size: 48px;
    font-weight: 700;
  }

  .content #more-information .right-content .currency .more-information {
    font-size: 14px;
  }

  .content #more-information .right-content .currency .more-information>span {
    color: #8362F2;
    text-decoration: underline;
    cursor: pointer;
  }

  .loading {
    display: flex;
    justify-content: center;
  }
</style>
