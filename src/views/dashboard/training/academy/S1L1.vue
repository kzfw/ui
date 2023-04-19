<template>
    <div>
      <div class="title-banner">
        <h1>Welcome to the ZFW Training Academy</h1>
      </div>
      <div class="academy-content-wrapper">
        <router-link to="/acd/training/academy">Back to Academy Home</router-link>
        <div class="card">
          <div class="card-content">
            <h3>Welcome to the S1 Training Material!</h3>
          </div>
        </div>
        <div class="card">
          <div class="card-content">
            <p>Body</p>
          </div>
        </div>
        <div class="card">
          <div class="card-content">
            <p>Closing</p>
          </div>
        </div>
        <router-link to="/acd/training/academy/S1L1"><button class="btn">Next Lesson</button></router-link>
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
  padding: 8px 16px;
  font-size: 10px;
  text-align: center;
  color: #f2f2f2;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}
</style>