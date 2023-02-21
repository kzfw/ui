<template>
    <div>
      <div class="title-banner">
        <h1>Welcome to the ZFW Training Academy</h1>
      </div>
      <div class="button-list">
        <ul>
          <li><a href="page1.html"><button class="btn">S1: Delivery and Ground Control</button></a></li>
          <li><a href="page2.html"><button class="btn">S2: Local Control</button></a></li>
          <li><a href="page3.html"><button class="btn">S3: Terminal Radar Control</button></a></li>
          <li><a href="page4.html"><button class="btn">C1: Enroute Control</button></a></li>
          <li><a href="videos.html"><button class="btn">Lecture Videos</button></a></li>
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
