---
layout: post
title: Photo Analysis
permalink: /photoanalysis/
---


#### Upload a photo you would like to post, and receive a prediction of how many likes it will get (media performance)!


<div markdown="0">
<head>
  <title>Photo Upload - Predict Likes</title>
  <style>
    .container {
      width: 350px;
      margin: 50px auto;
      text-align: center;
      border: 2px dashed #aaa;
      padding: 20px;
      border-radius: 10px;
      font-family: Arial, sans-serif;
    }
    img {
      max-width: 100%;
      margin-top: 10px;
      display: none;
      border-radius: 6px;
    }
    button {
      margin-top: 15px;
      padding: 10px 15px;
      font-weight: bold;
      cursor: pointer;
    }
    .results {
      margin-top: 15px;
      font-size: 14px;
      color: #333;
    }
    #logList {
      list-style-type: none;
      padding-left: 0;
    }
    #logList li {
      margin-bottom: 8px;
      padding: 6px;
      border-bottom: 1px solid #ddd;
    }
  </style>
</head>


<body>
  <div class="container">
    <h2>Upload a Photo to Predict Likes</h2>
    <input type="file" accept="image/*" id="photoInput" />
    <img id="preview" />
    <button id="predictButton">Predict</button>
    <div class="results" id="results"></div>
    <hr />
    <div class="results" id="logs">
      <h4>Prediction Log:</h4>
      <ul id="logList"></ul>
    </div>
  </div>


  <script>
    const preview = document.getElementById('preview');
    const input = document.getElementById('photoInput');
    const results = document.getElementById('results');
    const logList = document.getElementById('logList');
    const predictButton = document.getElementById('predictButton');


    // Preview uploaded photo
    input.addEventListener('change', function () {
      const file = this.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function (e) {
          preview.src = e.target.result;
          preview.style.display = 'block';
          results.textContent = '';
        };
        reader.readAsDataURL(file);
      }
    });


    // Add event listener to the button
    predictButton.addEventListener('click', predictLikes);


    // Send photo to backend and display predicted likes
    async function predictLikes() {
      const file = input.files[0];
      if (!file) {
        alert('Please upload a photo first.');
        return;
      }


      const formData = new FormData();
      formData.append('image', file);


      try {
        const res = await fetch('http://localhost:8887/api/predict-likes', {
          method: 'POST',
          body: formData
        });


        const data = await res.json();


        if (data.error) {
          results.innerHTML = `<span style="color:red;">Error: ${data.error}</span>`;
          return;
        }


        // Show results
        results.innerHTML = `
          <strong>Predicted Likes:</strong> ${Math.round(data.predicted_likes)}<br>
          <strong>Rating Score:</strong> ${data.rating_score.toFixed(2)}%<br>
          <strong>Performance:</strong> <span style="font-weight:bold; color:${
            data.rating_label === 'Excellent' ? '#2e7d32' :
            data.rating_label === 'Good' ? '#1565c0' :
            data.rating_label === 'Moderate' ? '#ef6c00' : '#757575'
          }">${data.rating_label}</span>
        `;


        // Add to log
        const logItem = document.createElement('li');
        logItem.innerHTML = `
          The average number of likes on your previous post was 1122 likes. Based on this, we expect your current photo to receive ${Math.round(data.predicted_likes)} likes. This would achieve a score of ${data.rating_score.toFixed(2)}% which we rate ${data.rating_label}.  
        `;
        logList.prepend(logItem); // Add to top


      } catch (err) {
        console.error('Error contacting backend:', err);
        results.innerHTML = `<span style="color:red;">Unable to contact prediction server.</span>`;
      }
    }
  </script>
</body>
</div>





