<script type="text/javascript"  context = "module">
  
</script>

<script type="text/javascript">

let element
let progress
let progressContainer
export let title
export let cover
export let audio
let isPlaying = false
let songIndex = 0;
let mins = 0
let secs = 0
let dmins = 0
let dsecs = 0

// Play song
function playSong() {
  element.play();
}

// Pause song
function pauseSong() {
  element.pause();
}

let playBtn = () => {
    if (isPlaying) {
      pauseSong();
      isPlaying = !isPlaying
    } else {
      playSong();
      isPlaying = !isPlaying
    }
}

// Previous song
function prevSong() {
  songIndex--;

  if (songIndex < 0) {
    songIndex = songs.length - 1;
  }

  loadSong(songs[songIndex]);

  playSong();
}

// Next song
function nextSong() {
  songIndex++;

  if (songIndex > songs.length - 1) {
    songIndex = 0;
  }

  loadSong(songs[songIndex]);

  playSong();
}

// Update progress bar
function updateProgress(e) {
  let duration = (element.duration / 60).toFixed(2).toString()
  dmins = parseInt(duration.split('.')[0])
  dsecs = parseInt(duration.split('.')[1])
  let currentTime = (element.currentTime / 60).toFixed(2).toString()
  mins = parseInt(currentTime.split('.')[0])
  secs = parseInt(currentTime.split('.')[1])
  if (secs > 60) {mins += 1; secs = parseInt(currentTime.split('.')[1] - 60)}
  const progressPercent = (currentTime / duration) * 100;
  progress.style.width = `${progressPercent}%`;
}

// Set progress bar
function setProgress(e) {
  const width = this.clientWidth;
  const clickX = e.offsetX;
  const duration = element.duration;

  element.currentTime = (clickX / width) * duration;
}

</script>

<div class="card" style="border-radius: 0px">
    <div class="card-body">
        <div class="" id="music-container">
          <div class="row">
              <div class="col-sm-2" style="display: contents;">
                  <div class="navigation">
                    <button id="prev" class="action-btn" on:click = {prevSong}>
                      <i class="fas fa-backward"></i>
                    </button>
                    <button id="play" class="action-btn action-btn-big" on:click="{playBtn}">
                      {#if isPlaying}
                      <i class="fas fa-pause"></i>
                      {:else}
                      <i class="fas fa-play"></i>
                      {/if}
                    </button>
                    <button id="next" class="action-btn" on:click = {nextSong}>
                      <i class="fas fa-forward"></i>
                    </button>
                  </div>
              </div>
            <div class="col-sm-8" style="margin-top: -15px;">
                <div class="">
                  <h4 id="title">{title}</h4>
                </div>

                <audio src="{audio}" id="audio" bind:this={element} on:timeupdate="{updateProgress}" on:ended={nextSong}></audio>
                <div style="display: flex;">
              <span>{mins}:{secs}</span>
              <div class="progress-container mx-3" id="progress-container" bind:this={progressContainer} on:click={setProgress}>
                  <div class="progress" id="progress" bind:this={progress}></div>
              </div>
              <span>{dmins}:{dsecs}</span>
              </div>
            </div>

            <div class="col-sm-2" style="display: contents;">
              <div class="navigation">
                <button id="shuffle" class="action-btn action-btn-big">
                    <i class="fas fa-random"></i>
                </button>
              </div>
            </div>
          </div>
  </div>
    </div>
</div>

  <style type="text/css">

.navigation {
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1;
}

.action-btn {
  background-color: #fff;
  border: 0;
  color: #dfdbdf;
  font-size: 20px;
  cursor: pointer;
  padding: 10px;
  margin: 0 20px;
}

.action-btn.action-btn-big {
  color: #cdc2d0;
  font-size: 30px;
}

.action-btn:focus {
  outline: 0;
}

.progress-container {
  background: #eee;
  border-radius: 5px;
  cursor: pointer;
  margin: 10px 0;
  height: 4px;
  width: 100%;
}

.progress {
  background-color: #111e6c;
  border-radius: 5px;
  height: 100%;
  width: 0%;
  transition: width 0.1s linear;
}
  </style>