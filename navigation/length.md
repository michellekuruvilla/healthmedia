---
layout: post
title: Video Length and Engagement Insights!
permalink: /lengths
menu: nav/home.html
search_exclude: true
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600&display=swap');

  body {
    background: #f0fff8;
    font-family: 'Quicksand', sans-serif;
    color: #003d33;
    margin: 0;
    padding: 2rem 1rem;
  }

  .container {
    max-width: 480px;
    margin: 0 auto;
    background: #e6fff5;
    border-radius: 18px;
    box-shadow: 0 12px 28px rgba(0, 153, 153, 0.2);
    padding: 2.5rem 2rem;
    text-align: center;
    border: 3px solid #99ffcc;
    transition: transform 0.3s ease;
  }

  .container:hover {
    transform: translateY(-5px);
    box-shadow: 0 16px 36px rgba(0, 204, 204, 0.4);
  }

  .video-icon {
    width: 72px;
    height: 72px;
    margin: 0 auto 1rem;
    background: #33cccc;
    border-radius: 50%;
    position: relative;
    box-shadow: 0 4px 8px rgba(0, 153, 153, 0.3);
  }

  .video-icon::before {
    content: '';
    position: absolute;
    top: 22px;
    left: 26px;
    border-style: solid;
    border-width: 14px 0 14px 24px;
    border-color: transparent transparent transparent #e6fff5;
    filter: drop-shadow(1px 1px 1px rgba(0,0,0,0.1));
  }

  h1 {
    font-weight: 700;
    font-size: 2.2rem;
    margin-bottom: 1rem;
    color: #00796b;
    text-shadow: 1px 1px 2px #b2dfdb;
  }

  .form-container h2 {
    font-weight: 600;
    font-size: 1.4rem;
    margin-bottom: 1rem;
    color: #009688;
  }

  form {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
  }

  label {
    font-weight: 600;
    font-size: 1.1rem;
    color: #00695c;
    align-self: flex-start;
  }

  input[type="number"] {
    width: 100%;
    max-width: 260px;
    padding: 0.5rem 0.75rem;
    border: 2px solid #99ffcc;
    border-radius: 12px;
    font-size: 1rem;
    font-weight: 500;
    transition: border-color 0.3s ease;
  }

  input[type="number"]:focus {
    outline: none;
    border-color: #33cccc;
    box-shadow: 0 0 8px #33cccc88;
  }

  button {
    background: linear-gradient(135deg, #66ffcc, #33cccc);
    border: none;
    color: white;
    font-weight: 700;
    font-size: 1.1rem;
    padding: 0.6rem 1.8rem;
    border-radius: 20px;
    cursor: pointer;
    box-shadow: 0 4px 12px #33cccc88;
    transition: background 0.3s ease, transform 0.2s ease;
  }

  button:hover {
    background: linear-gradient(135deg, #33cccc, #00b3b3);
    transform: scale(1.05);
  }

  #result {
    margin-top: 1.5rem;
    font-size: 1.2rem;
    font-weight: 600;
    min-height: 4rem;
    transition: color 0.3s ease;
    color: #004d40;
  }

  #result p {
    color: #004d40 !important;
    margin: 0.5rem 0;
  }

  #result.error {
    color: #00695c !important;
  }
</style>

<div class="container">
  <div class="video-icon"></div>
  <h1>Explore Video Length and Engagement Insights!</h1>

  <div class="form-container">
    <h2>Predict Engagement for a Video Length</h2>
    <form id="predictForm">
      <label for="video_length">Video Length (second):</label>
      <input type="number" id="video_length" name="video_length" required min="1" max="3600" placeholder="e.g., 60">
      <button type="submit">Predict</button>
    </form>
    <div id="result"></div>
  </div>
</div>

<script type="module">
  const URL = "http://127.0.0.1:8887/api/lengths";

  document.getElementById('predictForm').addEventListener('submit', async function(event) {
    event.preventDefault();
    const videoLength = parseInt(document.getElementById('video_length').value);
    const resultDiv = document.getElementById('result');
    resultDiv.textContent = 'Loading...';

    try {
      const response = await fetch(`${URL}/predict?video_length_seconds=${videoLength}`);
      if (!response.ok) throw new Error('Failed to get prediction');

      const data = await response.json();

      resultDiv.innerHTML = `
        <p><strong>Input Length:</strong> ${data.input_length} seconds</p>
        <p><strong>Closest Match:</strong> ${data.closest_match_length} seconds</p>
        <p><strong>Predicted Engagement:</strong> ${data.predicted_engagement}</p>
      `;
      resultDiv.classList.remove('error');
    } catch (error) {
      console.error('Error:', error);
      resultDiv.textContent = 'Error fetching prediction. Try again!';
      resultDiv.classList.add('error');
    }
  });
</script>
