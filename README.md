# spotify-clone-frontend
A simple frontend Spotify clone using HTML, CSS, and JavaScript


#index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Mini Spotify Clone</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="main-container">
        <div class="sidebar">
            <h2 class="logo"><i class="fab fa-spotify"></i> Spotify</h2>
            <div class="menu">
                <p><i class="fas fa-house"></i> Home</p>
                <p><i class="fas fa-magnifying-glass"></i> Search</p>
            </div>
        </div>

        <div class="content">
            <h1>Your Favorites</h1>
            <div class="song-list">
                <div class="song-item" onclick="playThis('Moon Rise', 'Guru Randhawa', 'https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3', 'https://picsum.photos/seed/guru/50')">
                    <span>1</span>
                    <img src="https://picsum.photos/seed/guru/50" alt="cover">
                    <div class="name"><h5>Moon Rise</h5><p>Guru Randhawa</p></div>
                    <i class="fas fa-play"></i>
                </div>

                <div class="song-item" onclick="playThis('Softly', 'Karan Aujla', 'https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3', 'https://picsum.photos/seed/aujla/50')">
                    <span>2</span>
                    <img src="https://picsum.photos/seed/aujla/50" alt="cover">
                    <div class="name"><h5>Softly</h5><p>Karan Aujla</p></div>
                    <i class="fas fa-play"></i>
                </div>

                <div class="song-item" onclick="playThis('With You', 'AP Dhillon', 'https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3', 'https://picsum.photos/seed/ap/50')">
                    <span>3</span>
                    <img src="https://picsum.photos/seed/ap/50" alt="cover">
                    <div class="name"><h5>With You</h5><p>AP Dhillon</p></div>
                    <i class="fas fa-play"></i>
                </div>

                <div class="song-item" onclick="playThis('Peaches', 'Justin Bieber', 'https://www.soundhelix.com/examples/mp3/SoundHelix-Song-8.mp3', 'https://picsum.photos/seed/bieber/50')">
                    <span>4</span>
                    <img src="https://picsum.photos/seed/bieber/50" alt="cover">
                    <div class="name"><h5>Peaches</h5><p>Justin Bieber</p></div>
                    <i class="fas fa-play"></i>
                </div>
            </div>
        </div>

        <div class="player">
            <div class="info">
                <img id="display-img" src="https://via.placeholder.com/40" style="width:40px; border-radius:3px; display:none;">
                <div style="margin-left: 10px;">
                    <h4 id="display-title">Select a Song</h4>
                    <p id="display-artist" style="font-size: 12px; color: #b3b3b3;">---</p>
                </div>
            </div>
            <div class="controls">
                <i class="fas fa-circle-play" id="masterPlay"></i>
                <input type="range" id="seek" value="0" min="0" max="100">
            </div>
        </div>
    </div>
    <script src="script.js"></script>
</body>
</html>


#style.css

* { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', sans-serif; }
body { background-color: #000; color: white; overflow: hidden; }

.main-container { display: flex; height: 100vh; }

.sidebar { width: 230px; background: #000; padding: 20px; }
.logo { color: #1DB954; margin-bottom: 30px; font-size: 25px; }
.menu p { padding: 12px 0; color: #b3b3b3; cursor: pointer; font-weight: bold; }
.menu p:hover { color: white; }

.content { flex: 1; padding: 30px; background: linear-gradient(#222, #121212); overflow-y: auto; }
.song-list { margin-top: 25px; }

.song-item { 
    display: flex; align-items: center; padding: 10px; 
    border-radius: 5px; cursor: pointer; transition: 0.2s;
}
.song-item:hover { background: rgba(255,255,255,0.1); }
.song-item img { width: 45px; height: 45px; margin: 0 15px; border-radius: 4px; }
.name { flex: 1; }
.name p { font-size: 13px; color: #b3b3b3; }

.player { 
    position: fixed; bottom: 0; width: 100%; height: 90px; 
    background: #000; border-top: 1px solid #282828;
    display: flex; align-items: center; padding: 0 20px; 
}
.info { width: 300px; display: flex; align-items: center; }
.controls { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 5px; }
#masterPlay { font-size: 35px; cursor: pointer; color: white; }
#seek { width: 60%; cursor: pointer; accent-color: #1DB954; }

#script.js

let audio = new Audio();
let masterPlay = document.getElementById('masterPlay');
let seek = document.getElementById('seek');
let displayImg = document.getElementById('display-img');

// Simple Function to change and play song
function playThis(title, artist, url, img) {
    audio.src = url;
    document.getElementById('display-title').innerText = title;
    document.getElementById('display-artist').innerText = artist;
    displayImg.src = img;
    displayImg.style.display = "block";
    
    audio.play();
    masterPlay.classList.replace('fa-circle-play', 'fa-circle-pause');
}

// Master Play/Pause Toggle
masterPlay.addEventListener('click', () => {
    if (audio.src === "") return; // Don't play if no song selected
    
    if (audio.paused) {
        audio.play();
        masterPlay.classList.replace('fa-circle-play', 'fa-circle-pause');
    } else {
        audio.pause();
        masterPlay.classList.replace('fa-circle-pause', 'fa-circle-play');
    }
});

// Update Seekbar
audio.addEventListener('timeupdate', () => {
    let progress = (audio.currentTime / audio.duration) * 100;
    seek.value = progress || 0;
});

// Manual Seeking
seek.addEventListener('input', () => {
    audio.currentTime = (seek.value * audio.duration) / 100;
});
