<!--
 * @Descripttion: Leetcode_code
 * @version: 1.0
 * @Author: zhc
 * @Date: 2023-11-28 16:01:03
 * @LastEditors: zhc
 * @LastEditTime: 2023-11-28 19:24:19
-->
<template>
    <div id="app">
      <div class="search-container">
        <input v-model="query" placeholder="输入描述" />
        <button @click="search">查询</button>
      </div>
      <div class="image-container">
        <!-- 显示加载状态 -->
        <div v-if="loading">Loading...</div>
        
        <img v-for="(imageData, index) in images" :key="index" :src=" 'data:image/png;base64,' + imageData " alt="Search Result" />

        
        <!-- 显示错误消息 -->
        <div v-if="error">{{ error }}</div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import { Base64 } from 'js-base64';
  
  export default {
    data() {
      return {
        query: '',
        images: [],
        loading: false,  
        error: null     
      };
    },
    methods: {
      search() {
        // 重置状态
        this.loading = true;
        this.error = null;
        this.images = [];
  
        const API_URL = 'http://localhost:5000/retrieve_images';
        axios.post(API_URL, { query: this.query })
          .then(response => {
            this.images = response.data.results || [];
          })
          .catch(error => {
            console.error('错误信息:', error);
            // 设置错误消息
            this.error = '找不到图片编码.';
          })
          .finally(() => {
            this.loading = false;
          });
      },
      decodeBase64Image(base64String) {
        console.log(base64String);
        const dataUrl = 'data:image/jpeg;base64,' + Base64.decode(base64String);
        console.log(dataUrl);
        return dataUrl;
      },
    },
  };
  </script>
  
  <style>
  #app {
    text-align: center;
    margin-top: 60px;
  }
  
  .search-container {
    margin-bottom: 20px;
  }
  
  .image-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  img {
    max-width: 100%;
    height: auto;
    margin: 10px;
  }
  </style>
  