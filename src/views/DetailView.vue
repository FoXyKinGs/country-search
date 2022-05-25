<template lang='pug'>
.container
  button.btn-back
    i.fa-solid.fa-arrow-left
    |  Back to Homepage
  #country-name
    .title
      h1 Indonesia
      img(
        src='https://flagcdn.com/w320/id.png'
      )
    .another-name
      p ID
      p Republic of Indonesia
      p Republik Indonesia
  #location
    .left-content
      .display-box
        .left-box
          p.title LatLong
          p.location -5.0, 120.0
        .right-box
          img(
            src='@/assets/globe.svg'
          )
    .right-content
      .display-box
        p.capital Capital: Jakarta
        p.region Region: Asia
        p.subregion Subregion: South-Eastern Asia
  #more-information
    .left-content
      .call-code
        p.title Calling Code
        p.number 62
        p.more-information
          span 1 country
          |  with this calling code
    .right-content
      .currency
        p.title Currency
        p.currency-type IDR
        p.more-information
          span 1 country
          |  with this currency
</template>

<script>
export default {
  setup () {
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
  }

  #country-name {
    margin-top: 50px;
  }

  #country-name .title {
    display: flex;
    align-items: center;
  }

  #country-name .title h1 {
    font-size: 48px;
  }

  #country-name .title img {
    width: 46px;
    height: 30px;
    margin-left: 20px;
  }

  #country-name .another-name {
    font-size: 12px;
    display: flex;
    color: #fff;
  }

  #country-name .another-name p {
    background: #8DD4CC;
    margin: 5px 5px 5px 0;
    padding: 5px 10px;
    border-radius: 50px;
  }

  #location {
    display: flex;
  }

  #location .left-content {
    padding: 10px;
    width: 50%;
    height: 145px;
  }

  #location .left-content  .display-box {
    display: flex;
    overflow: hidden;
  }

  #location .left-content  .display-box .left-box {
    width: 60%;
    height: 100%;
    padding: 20px;
  }

  #location .left-content  .display-box .left-box .title {
    font-size: 18px;
  }

  #location .left-content  .display-box .left-box .location {
    font-size: 48px;
    margin: 10px 0;
    color: #8362F2;
    font-weight: 700;
  }

  #location .left-content  .display-box .right-box {
    width: 40%;
    height: 100%;
    display: flex;
    justify-content: flex-end;
  }

  #location .left-content  .display-box .right-box img {
    margin-top: 15px;
    width: 204px;
    height: 204px;
  }

  #location .right-content {
    padding: 10px;
    width: 50%;
    height: 145px;
  }

  #location .right-content .display-box {
    font-size: 18px;
    padding: 20px;
    line-height: 25px;
  }

  #location .left-content .display-box,
  #location .right-content .display-box {
    height: 100%;
    box-shadow: -4px -4px 4px rgba(0, 0, 0, 0.02), 4px 4px 4px rgba(0, 0, 0, 0.02);
  }

  #more-information {
    display: flex;
  }

  #more-information .left-content {
    padding: 10px;
    width: 50%;
    height: 145px;
  }

  #more-information .left-content .call-code {
    padding: 10px
  }

  #more-information .left-content .call-code .title {
    font-size: 18px;
  }

  #more-information .left-content .call-code .number {
    margin: 10px 0;
    color: #8362F2;
    font-size: 48px;
    font-weight: 700;
  }

  #more-information .left-content .call-code .more-information {
    font-size: 14px;
  }

  #more-information .left-content .call-code .more-information>span {
    color: #8362F2;
    text-decoration: underline;
    cursor: pointer;
  }

  #more-information .right-content {
    padding: 10px;
    width: 50%;
    height: 145px;
  }

  #more-information .right-content .currency {
    padding: 10px
  }

  #more-information .right-content .currency .title {
    font-size: 18px;
  }

  #more-information .right-content .currency .currency-type {
    margin: 10px 0;
    color: #8362F2;
    font-size: 48px;
    font-weight: 700;
  }

  #more-information .right-content .currency .more-information {
    font-size: 14px;
  }

  #more-information .right-content .currency .more-information>span {
    color: #8362F2;
    text-decoration: underline;
    cursor: pointer;
  }
</style>
