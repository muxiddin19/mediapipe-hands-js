# MediaPipe Hands in JavaScript

A project implementing hand tracking using MediaPipe in JavaScript. This project demonstrates how to detect and track hand movements directly in the browser.

## Clone the Repository

On your computer, open a terminal or command prompt. Clone your new repository to your local machine using the following command:

```bash
git clone https://github.com/muxiddin19/mediapipe-hands-js.git
```
## Move to the folder
```bash
cd mediapipe-hands-js
```
## Example Project Structure
### Your project directory might look something like this:
```bash
mediapipe-hands-js/
├── index.html
├── script.js
└── style.css
```
## Example index.html File
```bash
const videoElement = document.getElementsByClassName('input_video')[0];
const canvasElement = document.getElementsByClassName('output_canvas')[0];
const canvasCtx = canvasElement.getContext('2d');

const hands = new Hands({
  locateFile: (file) => {
    return `https://cdn.jsdelivr.net/npm/@mediapipe/hands@0.1/${file}`;
  }
});

hands.setOptions({
  maxNumHands: 2,
  minDetectionConfidence: 0.5,
  minTrackingConfidence: 0.5
});

hands.onResults(onResults);

const camera = new Camera(videoElement, {
  onFrame: async () => {
    await hands.send({image: videoElement});
  },
  width: 1280,
  height: 720
});
camera.start();

function onResults(results) {
  canvasCtx.save();
  canvasCtx.clearRect(0, 0, canvasElement.width, canvasElement.height);
  canvasCtx.drawImage(
      results.image, 0, 0, canvasElement.width, canvasElement.height);
  if (results.multiHandLandmarks) {
    for (const landmarks of results.multiHandLandmarks) {
      drawConnectors(canvasCtx, landmarks, HAND_CONNECTIONS,
                     {color: '#00FF00', lineWidth: 5});
      drawLandmarks(canvasCtx, landmarks, {color: '#FF0000', lineWidth: 2});
    }
  }
  canvasCtx.restore();
}

```
