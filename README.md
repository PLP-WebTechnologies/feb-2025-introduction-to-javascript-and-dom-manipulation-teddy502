# Introduction to JavaScript and DOM Manipulation

## Objectives

Write basic JavaScript functions.
Manipulate the DOM dynamically.
Respond to user interactions.

## Instructions

- Create a script.js file and link it to a HTML.
- Structure the document using DOCTYPE, html, head, and body.

>[!NOTE]
>  - Write JavaScript that:
>  - Changes text content dynamically.
>  - Modifies CSS styles via JavaScript.
>  - Adds or removes an element when a button is clicked.


# Tasks
- Create a well-structured HTML5 document.
- Use at least 5 different HTML elements.
- Ensure semantic correctness.

Happy Coding! 💻✨
//Guessinggame.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <script src="guessinggame.js"><script>
  <div id="info">
    <p id="current-time">Current Time: Loading...</p>
    <p id="current-location">Current Date: Loading...</p>
    <p id="current-temp">Current Temperature: Loading...</p>
  </div>
  <input type= "number" max=100 min="1" id="number" placeholder="Enter your guess">
  <p class="message" id="message"></p>
  <button class = "submit" id="submit" type="submit">Submit</button>
  <input type="number" value="10" id="count" class="count" readonly disabled>
  <button class = "restart" id="restart">Restart</button>
  <button class = "next" id="next"  style="display: none;">Next</button>
  <div id="guesses">
    <input type="number" id="guess1" readonly disabled style="display: none;">
    <input type="number" id="guess2" readonly disabled style="display: none;">
    <input type="number" id="guess3" readonly disabled style="display: none;">
    <input type="number" id="guess4" readonly disabled style="display: none;">
    <input type="number" id="guess5" readonly disabled style="display: none;">
    <input type="number" id="guess6" readonly disabled style="display: none;">
    <input type="number" id="guess7" readonly disabled style="display: none;">
    <input type="number" id="guess8" readonly disabled style="display: none;">
    <input type="number" id="guess9" readonly disabled style="display: none;">
    <input type="number" id="guess10" readonly disabled style="display: none;">
    <option>today</option>
  </div>
  </body>
</html>


  guessinggame.js 
   const numberInput = document.getElementById("number");
   const submitNumber = document.getElementById("submit");
   const numberOfAttemptsInput = document.getElementById("count");
   const restartButton = document.getElementById("restart");
   const nextPage = document.getElementById("next");
   const messageFeedback = document.getElementById("message");
   const guessBoxes = Array.from(document.querySelectorAll("#guesses input"));

   let targetNumber = Math.floor(Math.random() *100 )+1;
   let guessIndex = 0; //Index to track where to add new guesses
     
   submitNumber.addEventListener("click",function () {
    const userGuess = Number(numberInput.value);
    let attempts = Number(numberOfAttemptsInput.value);

    if (!numberInput.value || userGuess < 1 || userGuess > 100) {
      messageFeedback.textContent = "Please Insert A valid  number between 1 and 100";
      messageFeedback.style.color = "red";
      numberInput.value = ""; //Clear invalid input
     return;} 
     if(guessIndex < guessBoxes.length) {
      const currentBox = guessBoxes[guessIndex];
      currentBox.value = userGuess;
      currentBox.disabled = false;
      if(userGuess > targetNumber) {
        messageFeedback.textContent = "The number is too high";
        messageFeedback.style.color = "orange";
        currentBox.style.color = "orange";// Number color for "too high"
        currentBox.style.display = "block";
      }else if(userGuess < targetNumber){
        messageFeedback.textContent = "The number is too low";
        messageFeedback.style.color = "blue";
        currentBox.style.color = "blue";//number color for "too low"
        currentBox.style.display= "block";
      }else{
        messageFeedback.textContent = "Success! I can see Mama Raised no fool";
        messageFeedback.style.color = "green"
        currentBox.style.color = "green"; //number color for correct guess
        currentBox.style.display = "block";
        nextPage.style.display = "block";
        submitNumber.disabled = true;
        numberInput.disabled = true;
        return;
      }
      guessIndex++;
     }
   
    if (attempts > 0){
      numberOfAttemptsInput.value = --attempts;
    } else {submitNumber.disabled = true;
      numberInput.disabled = true;
      messageFeedback.textContent =
       "You have failed! I can see the difference between you and a fool is that you are breathing"
      messageFeedback.style.color = "red";
        }
          
    numberInput.value ="";// Clear input field
  });
  restartButton.addEventListener("click", function () {
    targetNumber = Math.floor(Math.random() * 100) + 1;
    numberOfAttemptsInput.value = 10;
    numberInput.value = '';
    messageFeedback.textContent = "Game reset please make a new Guess!";
    nextPage.style.display = "none";
    submitNumber.disabled = false;
    numberInput.disabled = false;
    guessBoxes.forEach((box) => {
      box.value ='';
      box.disabled = true;
      box.style.color ="";
      box.style.display="none";
    });
    guessIndex = 0;
  });
  nextPage.addEventListener('click', function(){
      window.location.href=("http://127.0.0.1:5501/tedjoy.html");
    });
 function displayDateTime() {
  const now = new Date();
  const hours = String(now.getHours()).padStart(2, "0");
  const minutes = String(now.getMinutes()).padStart(2, "0"); 
  const seconds = String(now.getSeconds()).padStart(2, "0");
  const date = now.toLocaleDateString();

  const timeString = `${hours}:${minutes}:${seconds}`;
  document.getElementById("current-time").textContent = `Current Time: ${timeString}`;
  document.getElementById("current-location").textContent = `Current frDate: ${date}`;
    
 } 
//Function to fetch weather data using OpenWeather API
function displayWeather () {
  if(navigator.geolocation) {
    navigator.geolocation.getCurrent((position) => {
      const latitude = position.coords.latitude;
      const longitude = position.coords.longitude;
      //Make a request to openWeather API to get the cu rent temperature
      const apiKey ="YOUR_API_KEY"//replace with your own API key from OpenWeatherMap
      const url = `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&units=metric&appid=${apiKey}`;
      fetch(url)
      .then((response) => response.json())
      .then((data) =>{
        const temp = data.main.temp;
        document.getElementById("current-temp").textContent = `current Temperature: $(temp)C`;
        })
       .catch((error) =>{
        console.error("Error fetching weather data:", error);
        document.getElementById("current-temp").textContent = "Unable to retrieve weather data.";
       });
       (error) => {
        console.error("Geolocation error:", error);
        document.getElementById("current-temp").textContent = "Unable to retrieve location.";
       }

    });
  } else {
    alert("Geolocation is not supported by this browser.");
  }
  inputField.addEventListener('keydown', function(event) {
    if (event.key === 'Enter') {
      event.preventDefault();
      document.getElementById('submit').click();
     }
  });
  form.addEventListener('click', function(event) {
    event.preventDefault();
    const inputValue = inputField.value;
    alert(`submitted: ${inputValue}`);
    inputField.value = '';    
  });
 
}
//Initialize date,time, and weather when the webpage loads
displayDateTime();
displayWeather();
setInterval(displayDateTime, 1000); //update time every second
  



