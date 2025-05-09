<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Web Music Player</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f9f9f9;
      margin: 0;
      padding: 20px;
    }
    .container {
      max-width: 800px;
      margin: auto;
    }
    h1 {
      text-align: center;
      margin-bottom: 20px;
    }
    input[type="text"] {
      width: 100%;
      padding: 10px;
      margin-bottom: 20px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    .track {
      background: white;
      padding: 15px;
      margin-bottom: 10px;
      border-radius: 5px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
      cursor: pointer;
    }
    .track:hover {
      background: #f0f0f0;
    }
    .player {
      background: #eee;
      padding: 15px;
      border-radius: 5px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
      margin-top: 20px;
    }
    .controls {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-top: 10px;
    }
    button {
      padding: 10px 15px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    input[type="range"] {
      flex: 1;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Web Music Player</h1>
    <input type="text" id="search" placeholder="Search by title, artist, or genre" oninput="filterTracks()">
    <div id="track-list"></div>
    <div class="player" id="player" style="display: none;">
      <h2 id="now-playing"></h2>
      <p id="artist"></p>
      <div class="controls">
        <button onclick="togglePlay()">▶️/⏸️</button>
        <button onclick="skipTrack()">⏭️</button>
        <input type="range" id="volume" min="0" max="1" step="0.01" onchange="changeVolume(this.value)">
      </div>
      <audio id="audio" onended="skipTrack()"></audio>
    </div>
  </div>

  <script>
    const musicData = [
      {
        id: 1,
        title: "Dreams",
        artist: "Fleetwood Mac",
        genre: "Rock",
        src: "music/dreams.mp3",
      },
      {
        id: 2,
        title: "Blinding Lights",
        artist: "The Weeknd",
        genre: "Pop",
        src: "music/blinding_lights.mp3",
      },
      {
        id: 3,
        title: "Canon in D",
        artist: "Pachelbel",
        genre: "Classical",
        src: "music/canon_in_d.mp3",
      },
    ];

    let currentTrack = null;
    let isPlaying = false;
    const audio = document.getElementById('audio');
    const player = document.getElementById('player');
    const nowPlaying = document.getElementById('now-playing');
    const artistText = document.getElementById('artist');

    function renderTracks(tracks) {
      const trackList = document.getElementById('track-list');
      trackList.innerHTML = '';
      tracks.forEach(track => {
        const div = document.createElement('div');
        div.className = 'track';
        div.innerHTML = `<strong>${track.title}</strong><br><small>${track.artist} - ${track.genre}</small>`;
        div.onclick = () => playTrack(track);
        trackList.appendChild(div);
      });
    }

    function filterTracks() {
      const query = document.getElementById('search').value.toLowerCase();
      const filtered = musicData.filter(track =>
        track.title.toLowerCase().includes(query) ||
        track.artist.toLowerCase().includes(query) ||
        track.genre.toLowerCase().includes(query)
      );
      renderTracks(filtered);
    }

    function playTrack(track) {
      currentTrack = track;
      audio.src = track.src;
      nowPlaying.textContent = `Now Playing: ${track.title}`;
      artistText.textContent = track.artist;
      player.style.display = 'block';
      audio.play();
      isPlaying = true;
    }

    function togglePlay() {
      if (isPlaying) {
        audio.pause();
      } else {
        audio.play();
      }
      isPlaying = !isPlaying;
    }

    function skipTrack() {
      if (!currentTrack) return;
      const index = musicData.findIndex(t => t.id === currentTrack.id);
      const next = (index + 1) % musicData.length;
      playTrack(musicData[next]);
    }

    function changeVolume(val) {
      audio.volume = parseFloat(val);
    }

    renderTracks(musicData);
  </script>
</body>
</html>
