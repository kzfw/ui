<template>
    <div>
      <div class="title-banner">
        <h1>Welcome to the ZFW Training Academy</h1>
      </div>
      <div class="button-list">
        <ul>
          <li><button class="btn">S1: Delivery and Ground Control</button></li>
          <li><button class="btn">S2: Local Control</button></li>
          <li><button class="btn">S3: Terminal Radar Control</button></li>
          <li><button class="btn">C1: Enroute Control</button></li>
          <li><button class="btn">Lecture Videos</button></li>
        </ul>
      </div>
      <!-- Other content goes here -->
    </div>
  </template>
  
  <style>
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
    justify-content: flex-end;
  }
  .button-list ul {
    list-style: none;
    margin: 0;
    padding: 0;
    display: flex;
  }
  .button-list li {
    margin-right: 10px;
  }
  .btn {
    background-color: #002868;
    padding: 10px;
    text-align: center;
    color: #f2f2f2;
  }
  </style>