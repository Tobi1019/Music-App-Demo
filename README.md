# Music App Player

This project is a music app player built using HTML, CSS, and JavaScript. It includes features such as a navigation menu, draggable containers, music player controls, song rotation and information updates, and Swiper for a coverflow effect.

## Table of Contents

- [Features](#features)
  - [Navigation Item Click Handling](#navigation-item-click-handling)
  - [Draggable Container Handling](#draggable-container-handling)
  - [Music Player Controls](#music-player-controls)
  - [Functions for Song Rotation and Info Update](#functions-for-song-rotation-and-info-update)
  - [Event Listeners for Song](#event-listeners-for-song)
  - [Play/Pause and Control Button Event Listeners](#playpause-and-control-button-event-listeners)
  - [Swiper Initialization](#swiper-initialization)
- [Usage](#usage)
- [Installation](#installation)
- [Code Structure](#code-structure)
- [Contributing](#contributing)
- [License](#license)

## Features

### Navigation Item Click Handling

Handles the active state of navigation items.

```js
const navItems = document.querySelectorAll(".nav-item");

navItems.forEach((navItem) => {
  navItem.addEventListener("click", () => {
    navItems.forEach((item) => {
      item.className = "nav-item";
    });
    navItem.className = "nav-item active";
  });
});
```

- **Selection of Navigation Items**: `document.querySelectorAll(".nav-item")` selects all elements with the class `nav-item`.
- **Adding Click Event Listeners**: For each `nav-item`, an event listener is added that:
  - Resets the class of all `nav-item` elements to `"nav-item"`.
  - Sets the clicked `nav-item`'s class to `"nav-item active"`.

### Draggable Container Handling

Enables drag-and-drop functionality for scrolling containers.

```js
const containers = document.querySelectorAll(".containers");

containers.forEach((container) => {
  let isDragging = false;
  let startX;
  let scrollLeft;

  container.addEventListener("mousedown", (e) => {
    isDragging = true;
    startX = e.pageX - container.offsetLeft;
    scrollLeft = container.scrollLeft;
  });

  container.addEventListener("mousemove", (e) => {
    if (!isDragging) return;
    e.preventDefault();

    const x = e.pageX - container.offsetLeft;
    const step = (x - startX) * 0.6;
    container.scrollLeft = scrollLeft - step;
  });

  container.addEventListener("mouseup", () => {
    isDragging = false;
  });

  container.addEventListener("mouseleave", () => {
    isDragging = false;
  });
});
```

- **Selection of Containers**: `document.querySelectorAll(".containers")` selects all elements with the class `containers`.
- **Adding Mouse Event Listeners**:
  - `mousedown`: Initializes dragging, capturing the start position and scroll offset.
  - `mousemove`: Handles the dragging, updating the scroll position based on mouse movement.
  - `mouseup` and `mouseleave`: Ends dragging.

### Music Player Controls

Handles play/pause functionality and song navigation.

```js
const progress = document.getElementById("progress");
const song = document.getElementById("song");
const controlIcon = document.getElementById("controlIcon");
const playPauseButton = document.querySelector(".play-pause-btn");
const forwardButton = document.querySelector(".controls button.forward");
const backwardButton = document.querySelector(".controls button.backward");
const rotatingImage = document.getElementById("rotatingImage");
const songName = document.querySelector(".music-player h2");
const artistName = document.querySelector(".music-player p");
let rotating = false;
let currentRotation = 0;
let rotationInterval;
const songs = [
  // List of songs with title, name, source URL, and cover image URL
];
```

### Functions for Song Rotation and Info Update

Manages image rotation and updates song information.

```js
function startRotation() {
  if (!rotating) {
    rotating = true;
    rotationInterval = setInterval(rotateImage, 50);
  }
}

function pauseRotation() {
  clearInterval(rotationInterval);
  rotating = false;
}

function rotateImage() {
  currentRotation += 1;
  rotatingImage.style.transform = `rotate(${currentRotation}deg)`;
}

function updateSongInfo() {
  songName.textContent = songs[currentSongIndex].title;
  artistName.textContent = songs[currentSongIndex].name;
  song.src = songs[currentSongIndex].source;
  rotatingImage.src = songs[currentSongIndex].cover;
  song.addEventListener("loadeddata", function () {});
}
```

- **startRotation**: Starts rotating the image at regular intervals.
- **pauseRotation**: Stops the image rotation.
- **rotateImage**: Updates the rotation angle of the image.
- **updateSongInfo**: Updates the song and artist information, as well as the song source and cover image.

### Event Listeners for Song

Handles metadata, song end, and time updates.

```js
song.addEventListener("loadedmetadata", function () {
  progress.max = song.duration;
  progress.value = song.currentTime;
});

song.addEventListener("ended", function () {
  currentSongIndex = (currentSongIndex + 1) % songs.length;
  updateSongInfo();
  playPause();
});

song.addEventListener("timeupdate", function () {
  if (!song.paused) {
    progress.value = song.currentTime;
  }
});
```

- **loadedmetadata**: Updates the progress bar's maximum value and current value based on song duration and current time.
- **ended**: Plays the next song when the current song ends.
- **timeupdate**: Continuously updates the progress bar as the song plays.

### Play/Pause and Control Button Event Listeners

Handles play/pause toggle, progress bar interaction, and navigation buttons.

```js
function playPause() {
  if (song.paused) {
    song.play();
    controlIcon.classList.add("fa-pause");
    controlIcon.classList.remove("fa-play");
    startRotation();
  } else {
    song.pause();
    controlIcon.classList.remove("fa-pause");
    controlIcon.classList.add("fa-play");
    pauseRotation();
  }
}

playPauseButton.addEventListener("click", playPause);

progress.addEventListener("input", function () {
  song.currentTime = progress.value;
});

progress.addEventListener("change", function () {
  song.play();
  controlIcon.classList.add("fa-pause");
  controlIcon.classList.remove("fa-play");
  startRotation();
});

forwardButton.addEventListener("click", function () {
  currentSongIndex = (currentSongIndex + 1) % songs.length;
  updateSongInfo();
  playPause();
});

backwardButton.addEventListener("click", function () {
  currentSongIndex = (currentSongIndex - 1 + songs.length) % songs.length;
  updateSongInfo();
  playPause();
});

updateSongInfo();
```

- **playPause**: Toggles play and pause states, updates the control icon, and manages image rotation.
- **Progress Bar Input and Change Events**: Updates song current time and starts playing the song if necessary.
- **Forward and Backward Buttons**: Change the current song and update the UI accordingly.

### Swiper Initialization

Configures a Swiper instance with a coverflow effect.

```js
var swiper = new Swiper(".swiper", {
  effect: "coverflow",
  grabCursor: true,
  centeredSlides: true,
  loop: true,
  speed: 600,
  slidesPerView: "auto",
  coverflowEffect: {
    rotate: 10,
    stretch: 120,
    depth: 200,
    modifier: 1,
    slideShadows: false,
  },
  on: {
    click(event) {
      swiper.slideTo(this.clickedIndex);
    },
  },
  pagination: {
    el: ".swiper-pagination",
  },
});
```

- **Swiper Initialization**: Configures a Swiper instance with coverflow effect and various settings for appearance and behavior.
- **Event Listener for Click**: Allows slides to be navigated by clicking on them.
- **Pagination**: Adds pagination controls to the Swiper.

## Usage

1. Clone the repository.
2. Open `index.html` in your web browser to use the music app player.

## Installation

No special installation steps are required. Ensure you have a modern web browser to view the HTML, CSS, and JavaScript content.

## Code Structure

- **index.html**: The main HTML file containing the structure of the webpage.
- **style.css**: The CSS file for styling the webpage.
- **script.js**: The JavaScript file for functionality and interactions.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request with your changes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
# Music-App-Demo
