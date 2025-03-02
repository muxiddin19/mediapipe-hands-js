# mediapipe-hands-js
Implementing Hand Tracking with MediaPipe: A JavaScript Approach
Clone the Repository:

On your computer, open a terminal or command prompt.

Clone your new repository to your local machine using the following command:

bash
git clone https://github.com/your-username/mediapipe-hands-js.git
Replace your-username with your GitHub username and mediapipe-hands-js with the name of your repository.

Add Your Project Files:

Navigate to the directory where you cloned the repository:

bash
cd mediapipe-hands-js
Add your HTML, CSS, and JavaScript files to this directory.

Commit and Push Your Changes:

Stage your changes:

bash
git add .
Commit your changes with a descriptive message:

bash
git commit -m "Initial commit with MediaPipe Hands project"
Push your changes to GitHub:

bash
git push origin main
Example Project Structure
Your project directory might look something like this:

mediapipe-hands-js/
├── index.html
├── script.js
└── style.css
Example index.html File
html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MediaPipe Hands in JavaScript</title>
    <script src="https://cdn.jsdelivr.net/npm/@mediapipe/hands@0.1/hands.js" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/@mediapipe/drawing_utils@0.1/drawing_utils.js" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/@mediapipe/camera_utils@0.1/camera_utils.js" crossorigin="anonymous"></script>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <video class="input_video" style="display:none;"></video>
    <canvas class="output_canvas"></canvas>
    <script src="script.js"></script>
</body>
</html>
Example script.js File
javascript
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
Example style.css File
css
body {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

canvas {
  border: 1px solid black;
}
