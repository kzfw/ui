<template>
    <div class="container">
      <div class="title-banner">
        <h1>Welcome to the ZFW Training Academy</h1>
      </div>
      <div class="button-list">
        <ul>
          <li><a href="page1.html" class="btn">S1: Delivery and Ground Control</a></li>
          <li><a href="page2.html" class="btn">S2: Local Control</a></li>
          <li><a href="page3.html" class="btn">S3: Terminal Radar Control</a></li>
          <li><a href="page4.html" class="btn">C1: Enroute Control</a></li>
          <li><a href="videos.html" class="btn">Lecture Videos</a></li>
        </ul>
      </div>
      <!-- Other content goes here -->
    </div>
  </template>
  
  <style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100vh;
  }
  .title-banner {
    background-color: #002868;
    padding: 20px;
    margin-bottom: 20px;
    text-align: center;
  }
  .title-banner h1 {
    font-size: 36px;
    margin: 0;
    color: #f2f2f2;
  }
  .button-list {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .button-list ul {
    list-style: none;
    margin: 0;
    padding: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .button-list li {
    margin-bottom: 10px;
  }
  .btn {
    background-color: #002868;
    padding: 16px 32px;
    font-size: 16px;
    text-align: center;
    color: #f2f2f2;
    text-decoration: none;
    display: block;
  }
  </style>
  