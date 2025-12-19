<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Live Forecast</title>
  <link rel="stylesheet" href="style.css" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css"/>
</head>
<body>
  <!-- Theme Toggle -->
  <button id="themeToggle" class="theme-toggle">
    <i class="fas fa-moon"></i>
  </button>

  <!-- Dynamic Background -->
  <div id="dynamicBg" class="dynamic-bg"></div>

  <div class="container">
    <!-- Live Clock -->
    <div id="liveClock" class="live-clock"></div>

    <!-- Alert Banner -->
    <div id="alertBanner" class="hidden alert-banner">
      <i class="fas fa-exclamation-triangle"></i> <span id="alertText"></span>
    </div>

    <h1>Live Forecast</h1>

    <div class="location-selector">
      <input type="text" id="city" placeholder="City" />
      <input type="text" id="district" placeholder="District" />
      <input type="text" id="state" placeholder="State" />
      <select id="country"><option value="IN">India</option></select>
      <button class="search-btn" onclick="searchFlexibleLocation()">Search</button>
    </div>

    <div class="or-text">— OR —</div>
    <div class="location-btn" onclick="getLocationWeather()">Use My Location</div>

    <div id="loading" class="loading hidden">
      <div class="sun-loader">☀️</div>
      <div>Loading beautiful weather...</div>
    </div>

    <div class="weather-info hidden" id="weatherInfo">
      <div id="currentIcon"></div>
      <div class="temp pulse" id="temp">-</div>
      <div id="minMax"></div>
      <div class="description" id="description">-</div>
      <div class="location" id="location">-</div>
      <div id="uvBadge"></div>
      <div id="aqiBadge" style="margin:10px 0;"></div>

      <div class="details">
        <div><i class="fas fa-tint"></i><br><span id="humidity">-</span>%</div>
        <div><i class="fas fa-wind"></i><br><span id="wind">-</span> m/s</div>
        <div><i class="fas fa-eye"></i><br><span id="visibility">-</span> km</div>
      </div>

      <div id="extraFeatures"></div>
    </div>

    <div class="travel-planner">
      <h2>Planning to go out?</h2>
      <div class="date-time-inputs">
        <input type="date" id="visitDate" />
        <input type="time" id="visitTime" />
      </div>
      <button class="check-btn" onclick="checkIfSafeToGo()">Check If Safe</button>
      <div id="recommendation">Select date & time above</div>
    </div>

    <div class="forecast-section"><h2>24-Hour Forecast</h2><div id="hourlyForecast" class="hourly-forecast"></div></div>
    <div class="forecast-section"><h2>5-Day Forecast</h2><div id="dailyForecast" class="daily-forecast"></div></div>

    <!-- Live Rain Radar -->
    <div class="forecast-section">
      <h2>🌧️ Live Rain Radar</h2>
      <iframe id="windyRadar" width="100%" height="450" style="border-radius:30px;overflow:hidden;box-shadow:0 15px 40px rgba(0,0,0,0.4);border:none;" frameborder="0" allowfullscreen></iframe>
    </div>

    <button onclick="shareWeather()" class="check-btn" style="background:#25D366;margin:30px auto 10px;display:block;">
      WhatsApp Share
    </button>

    <button onclick="speakWeather()" class="check-btn" style="background:#ff6b6b;margin:15px auto 10px;display:block;">
      Voice Summary
    </button>

    <button onclick="saveCurrentLocation()" class="check-btn" style="background:#51cf66;margin:10px auto;display:block;">
      Save as Favorite
    </button>

    <button onclick="showFavorites()" class="check-btn" style="background:#339af0;margin:10px auto 20px;display:block;">
      My Favorite Cities
    </button>

    <div class="footer">
      Made with ❤️ by <strong>You</strong>
    </div>
  </div>
  * {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', 'Segoe UI', sans-serif;
}
body {
  background: linear-gradient(135deg, #a1c4fd, #c2e9fb);
  color: #fff;
  min-height: 100vh;
  padding: 20px;
  transition: background 1s ease;
}
body.dark {
  background: linear-gradient(135deg, #141e30, #243b55);
}
.container {
  max-width: 520px;
  margin: 20px auto;
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(25px);
  -webkit-backdrop-filter: blur(25px);
  border-radius: 35px;
  padding: 40px;
  box-shadow: 0 25px 60px rgba(0,0,0,0.4);
  border: 1px solid rgba(255,255,255,0.15);
}
h1 {
  text-align: center;
  margin-bottom: 30px;
  font-size: 40px;
  font-weight: 600;
  letter-spacing: 2px;
  text-shadow: 0 4px 20px rgba(0,0,0,0.3);
}
.location-selector {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  gap: 15px;
  margin-bottom: 25px;
}
.location-selector input, .location-selector select {
  padding: 18px;
  border: none;
  border-radius: 25px;
  background: rgba(255,255,255,0.85);
  color: #333;
  text-align: center;
  font-size: 16px;
  transition: all 0.4s;
}
.location-selector input:focus {
  outline: none;
  transform: scale(1.08);
  box-shadow: 0 0 20px rgba(255,255,255,0.6);
}
.search-btn {
  grid-column: 1 / -1;
  padding: 20px;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  border: none;
  border-radius: 50px;
  font-weight: bold;
  font-size: 20px;
  cursor: pointer;
  margin-top: 15px;
  box-shadow: 0 10px 30px rgba(102,126,234,0.5);
  transition: all 0.5s;
}
.search-btn:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 40px rgba(102,126,234,0.7);
}
.or-text {
  text-align: center;
  margin: 30px 0;
  font-size: 20px;
  opacity: 0.9;
}
.location-btn {
  text-align: center;
  padding: 20px;
  background: rgba(255,255,255,0.2);
  border-radius: 50px;
  cursor: pointer;
  font-size: 20px;
  font-weight: bold;
  box-shadow: 0 10px 30px rgba(0,0,0,0.4);
  transition: all 0.5s;
}
.location-btn:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 40px rgba(0,0,0,0.6);
}
.weather-info {
  text-align: center;
  margin: 50px 0;
}
.temp {
  font-size: 100px;
  font-weight: 300;
  line-height: 1;
  margin: 25px 0;
  text-shadow: 0 6px 25px rgba(0,0,0,0.4);
}
.description {
  font-size: 30px;
  text-transform: capitalize;
  margin: 20px 0;
  font-weight: 500;
}
.location {
  font-size: 24px;
  margin: 25px 0;
}
.details {
  display: flex;
  justify-content: space-around;
  margin: 40px 0;
  font-size: 18px;
}
.details div {
  background: rgba(255,255,255,0.15);
  padding: 20px;
  border-radius: 25px;
  min-width: 110px;
  backdrop-filter: blur(10px);
}
.extra-feature {
  margin: 30px 0;
  padding: 30px;
  background: rgba(255,255,255,0.12);
  border-radius: 30px;
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255,255,255,0.2);
  box-shadow: 0 15px 40px rgba(0,0,0,0.3);
  text-align: center;
  transition: all 0.6s;
}
.extra-feature:hover {
  transform: translateY(-15px);
  box-shadow: 0 30px 60px rgba(0,0,0,0.5);
}
.travel-planner {
  margin-top: 60px;
  padding: 40px;
  background: rgba(0,0,0,0.35);
  border-radius: 35px;
  text-align: center;
}
.check-btn {
  width: 100%;
  padding: 22px;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  border: none;
  border-radius: 50px;
  font-weight: bold;
  font-size: 20px;
  cursor: pointer;
  box-shadow: 0 12px 35px rgba(102,126,234,0.5);
  transition: all 0.5s;
}
.check-btn:hover {
  transform: translateY(-10px);
  box-shadow: 0 25px 50px rgba(102,126,234,0.7);
}
#recommendation {
  margin-top: 35px;
  padding: 40px;
  border-radius: 30px;
  font-size: 24px;
  font-weight: 600;
  min-height: 160px;
  background: rgba(0,0,0,0.4);
  transition: all 0.7s;
}
.forecast-section h2 {
  text-align: center;
  margin: 50px 0 30px;
  font-size: 30px;
  font-weight: 600;
}
.hourly-forecast, .daily-forecast {
  display: flex;
  overflow-x: auto;
  gap: 25px;
  padding: 25px 0;
}
.hour-card, .day-card {
  min-width: 130px;
  background: rgba(255,255,255,0.18);
  border-radius: 30px;
  padding: 25px;
  text-align: center;
  backdrop-filter: blur(15px);
  box-shadow: 0 15px 40px rgba(0,0,0,0.3);
  transition: all 0.5s;
}
.hour-card:hover, .day-card:hover {
  transform: translateY(-12px);
}
.theme-toggle {
  position: fixed;
  top: 30px;
  right: 30px;
  z-index: 9999;
  background: rgba(255,255,255,0.2);
  border: none;
  width: 70px;
  height: 70px;
  border-radius: 50%;
  color: white;
  font-size: 32px;
  cursor: pointer;
  backdrop-filter: blur(20px);
  box-shadow: 0 15px 40px rgba(0,0,0,0.4);
  transition: all 0.6s;
}
.theme-toggle:hover {
  transform: scale(1.25) rotate(360deg);
}
.hidden { display: none; }
.footer {
  text-align: center;
  margin: 50px 0 20px;
  opacity: 0.8;
  font-size: 18px;
}

/* ANIMATIONS */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(50px); }
  to { opacity: 1; transform: translateY(0); }
}
.extra-feature {
  animation: fadeIn 1s ease-out forwards;
  opacity: 0;
}
.extra-feature:nth-child(1) { animation-delay: 0.3s; }
.extra-feature:nth-child(2) { animation-delay: 0.6s; }
.extra-feature:nth-child(3) { animation-delay: 0.9s; }
.extra-feature:nth-child(4) { animation-delay: 1.2s; }
.extra-feature:nth-child(5) { animation-delay: 1.5s; }

@keyframes buttonPulse {
  0% { box-shadow: 0 0 0 0 rgba(102,126,234,0.7); }
  70% { box-shadow: 0 0 0 25px rgba(102,126,234,0); }
  100% { box-shadow: 0 0 0 0 rgba(102,126,234,0); }
}
.check-btn:hover, .search-btn:hover {
  animation: buttonPulse 2s infinite;
}
const API_KEY = "88128aca813781faf99aa10e14783246";

let lat = null, lon = null;
let forecastData = null;

// Live Clock + Date
setInterval(() => {
  const now = new Date();
  const time = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' });
  const date = now.toLocaleDateString([], { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
  document.getElementById('liveClock').innerHTML = `${time}<br><small>${date}</small>`;
}, 1000);

// Theme Toggle
document.getElementById('themeToggle').onclick = () => {
  document.body.classList.toggle('dark');
  const isDark = document.body.classList.contains('dark');
  document.getElementById('themeToggle').innerHTML = isDark
    ? '<i class="fas fa-sun"></i>'
    : '<i class="fas fa-moon"></i>';
};

// Search
function searchFlexibleLocation() {
  let q =
    document.getElementById('city').value.trim() ||
    document.getElementById('district').value.trim() ||
    document.getElementById('state').value.trim() ||
    "Mumbai";
  loadByCity(q);
}

// Location
function getLocationWeather() {
  if (!navigator.geolocation) {
    loadByCity("Mumbai");
    return;
  }
  document.getElementById('loading').classList.remove('hidden');
  navigator.geolocation.getCurrentPosition(
    p => {
      lat = p.coords.latitude;
      lon = p.coords.longitude;
      loadAllData();
    },
    () => loadByCity("Mumbai")
  );
}

function loadByCity(city) {
  document.getElementById('loading').classList.remove('hidden');
  fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city},IN&appid=${API_KEY}&units=metric`)
    .then(r => r.json())
    .then(d => {
      if (d.cod === 200) {
        lat = d.coord.lat;
        lon = d.coord.lon;
        loadAllData();
      } else {
        document.getElementById('loading').innerHTML = "City not found";
      }
    })
    .catch(() => {
      document.getElementById('loading').innerHTML = "Network error";
    });
}

// MAIN
function loadAllData() {
  fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${API_KEY}&units=metric`)
    .then(r => r.json())
    .then(d => {
      document.getElementById('weatherInfo').classList.remove('hidden');
      document.getElementById('currentIcon').innerHTML =
        `<img src="https://openweathermap.org/img/wn/${d.weather[0].icon}@4x.png">`;
      document.getElementById('temp').innerHTML =
        `${Math.round(d.main.temp)}°C<br><small>Feels like ${Math.round(d.main.feels_like)}°C</small>`;
      document.getElementById('minMax').innerHTML =
        `Today: ${Math.round(d.main.temp_min)}° – ${Math.round(d.main.temp_max)}°`;
      document.getElementById('description').textContent = d.weather[0].description;
      document.getElementById('location').innerHTML =
        `${d.name}, ${d.sys.country}<br>
         <small>Sunrise ${new Date(d.sys.sunrise * 1000).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })}
         • Sunset ${new Date(d.sys.sunset * 1000).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })}</small>`;
      document.getElementById('humidity').textContent = d.main.humidity;
      document.getElementById('wind').textContent = d.wind.speed.toFixed(1);
      document.getElementById('visibility').textContent = (d.visibility / 1000).toFixed(1);

      let uvValue = 0;
      let aqiLevel = 1;

      const uvFetch = fetch(`https://api.openweathermap.org/data/2.5/uvi?lat=${lat}&lon=${lon}&appid=${API_KEY}`)
        .then(r => r.json())
        .then(u => {
          uvValue = u.value;
          const c = uvValue < 3 ? "#2ed573" : uvValue < 7 ? "#ffa502" : "#ff4757";
          document.getElementById('uvBadge').innerHTML =
            `<div style="background:${c};">UV Index: ${uvValue.toFixed(1)}</div>`;
        })
        .catch(() => {
          document.getElementById('uvBadge').innerHTML = "UV N/A";
        });

      const aqiFetch = fetch(`https://api.openweathermap.org/data/2.5/air_pollution?lat=${lat}&lon=${lon}&appid=${API_KEY}`)
        .then(r => r.json())
        .then(aqi => {
          aqiLevel = aqi.list[0].main.aqi;
          const quality = ["Good", "Fair", "Moderate", "Poor", "Very Poor"];
          const colors = ["#2ed573", "#7bed9f", "#ffa502", "#ff6348", "#ff4757"];
          document.getElementById('aqiBadge').innerHTML =
            `<div style="background:${colors[aqiLevel - 1]};">
              Air Quality: ${quality[aqiLevel - 1]}
             </div>`;
        })
        .catch(() => {
          document.getElementById('aqiBadge').innerHTML = "AQI N/A";
        });

      // Clear extra features
      document.getElementById('extraFeatures').innerHTML = '';

      // Wind Direction
      const directions = ['N', 'NNE', 'NE', 'ENE', 'E', 'ESE', 'SE', 'SSE', 'S', 'SSW', 'SW', 'WSW', 'W', 'WNW', 'NW', 'NNW'];
      const deg = d.wind.deg || 0;
      const index = Math.round(deg / 22.5) % 16;
      const windDiv = document.createElement('div');
      windDiv.className = 'extra-feature';
      windDiv.innerHTML = `<strong>🧭 Wind Direction</strong><br>
        <div style="font-size:60px;">${directions[index]} ↑</div>
        Wind from ${directions[index]}`;
      document.getElementById('extraFeatures').appendChild(windDiv);

      // Sunrise & Sunset Bar
      const sunriseTime = new Date(d.sys.sunrise * 1000).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
      const sunsetTime = new Date(d.sys.sunset * 1000).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
      const dayLength = Math.floor((d.sys.sunset - d.sys.sunrise) / 3600) + "h " + Math.floor(((d.sys.sunset - d.sys.sunrise) % 3600) / 60) + "m";
      const sunDiv = document.createElement('div');
      sunDiv.className = 'extra-feature';
      sunDiv.innerHTML = `
        <strong>☀️ Sunrise & Sunset</strong><br>
        <div style="display:flex;align-items:center;justify-content:space-between;margin:15px 0;">
          <span>🌅 ${sunriseTime}</span>
          <div style="flex:1;height:10px;background:rgba(255,255,255,0.3);border-radius:10px;margin:0 15px;">
            <div style="width:60%;height:100%;background:linear-gradient(90deg, #ffe66d, #ff9f43);border-radius:10px;"></div>
          </div>
          <span>🌇 ${sunsetTime}</span>
        </div>
        Day length: ${dayLength}
      `;
      document.getElementById('extraFeatures').appendChild(sunDiv);

      // Weather Alerts
      Promise.all([uvFetch, aqiFetch]).then(() => {
        let alerts = [];
        if (d.main.temp > 40) alerts.push("🔥 Heatwave Alert");
        if (d.main.temp < 5) alerts.push("❄️ Cold Wave Alert");
        if (d.weather[0].main.toLowerCase().includes('thunder')) alerts.push("⚡ Thunderstorm Alert");
        if (d.weather[0].main.toLowerCase().includes('rain') && d.wind.speed > 15) alerts.push("🌧️ Heavy Rain & Wind Alert");
        if (uvValue > 8) alerts.push("☀️ Extreme UV Alert");
        if (aqiLevel > 4) alerts.push("😷 Severe Pollution Alert");

        if (alerts.length > 0) {
          document.getElementById('alertBanner').classList.remove('hidden');
          document.getElementById('alertText').textContent = alerts.join(" • ");

          const alertCard = document.createElement('div');
          alertCard.className = 'extra-feature';
          alertCard.style.background = 'linear-gradient(135deg, #ff4757, #c44569)';
          alertCard.style.color = 'white';
          alertCard.style.border = 'none';
          alertCard.innerHTML = `<strong>🚨 Active Weather Alerts</strong><br>${alerts.join("<br>")}`;
          document.getElementById('extraFeatures').insertBefore(alertCard, document.getElementById('extraFeatures').firstChild);
        } else {
          document.getElementById('alertBanner').classList.add('hidden');
        }
      });

      // Health Cautions
      Promise.all([uvFetch, aqiFetch]).then(() => {
        let cautions = [];
        if (d.main.temp > 35) cautions.push("🔥 Extreme heat — stay indoors, drink plenty of water");
        if (d.main.temp < 10) cautions.push("❄️ Cold weather — wear warm layers");
        if (d.main.humidity > 80) cautions.push("💧 High humidity — may cause discomfort");
        if (d.main.humidity < 30) cautions.push("🏜️ Dry air — use moisturizer");
        if (uvValue > 7) cautions.push("☀️ High UV — apply SPF 50+");
        if (aqiLevel > 3) cautions.push("😷 Poor air quality — limit outdoor time");
        if (d.wind.speed > 12) cautions.push("💨 Strong wind — drive carefully");
        if (d.weather[0].main.toLowerCase().includes('rain')) cautions.push("☔ Rain expected — avoid slippery areas");

        if (cautions.length > 0) {
          const cautionDiv = document.createElement('div');
          cautionDiv.className = 'extra-feature';
          cautionDiv.innerHTML = `<strong>⚠️ Health Cautions</strong><br>${cautions.join("<br>")}`;
          document.getElementById('extraFeatures').appendChild(cautionDiv);
        }
      });

      // Live Rain Radar (Windy.com embed — shows real rain data)
      const windyUrl = `https://embed.windy.com/embed2.html?lat=${lat}&lon=${lon}&detailLat=${lat}&detailLon=${lon}&zoom=9&level=radar&overlay=rain&product=ecmwf&menu=&message=true&marker=true&calendar=now&pressure=&type=map&location=coordinates&detail=&metricWind=default&metricTemp=°C&radarRange=-1`;
      document.getElementById('windyRadar').src = windyUrl;

      // Forecast
      fetch(`https://api.openweathermap.org/data/2.5/forecast?lat=${lat}&lon=${lon}&appid=${API_KEY}&units=metric`)
        .then(r => r.json())
        .then(f => {
          forecastData = f.list;

          // 24-Hour Forecast
          let h = document.getElementById('hourlyForecast'); h.innerHTML = "";
          f.list.slice(0,8).forEach(i => {
            h.innerHTML += `<div class="hour-card">
              <div>${new Date(i.dt*1000).toLocaleTimeString([],{hour:'2-digit',minute:'2-digit'})}</div>
              <img src="https://openweathermap.org/img/wn/${i.weather[0].icon}@2x.png">
              <div>${Math.round(i.main.temp)}°</div>
              <div class="rain-chance">${Math.round((i.pop||0)*100)}% rain</div>
            </div>`;
          });

          // 5-Day Forecast
          let days = {}, daily = document.getElementById('dailyForecast'); daily.innerHTML = "";
          f.list.forEach(i => {
            let k = new Date(i.dt*1000).toLocaleDateString('en-US',{weekday:'short',day:'numeric',month:'short'});
            if (!days[k]) days[k] = {temp: [], icon: {}};
            days[k].temp.push(i.main.temp);
            days[k].icon[i.weather[0].icon] = (days[k].icon[i.weather[0].icon] || 0) + 1;
          });
          Object.keys(days).slice(0,5).forEach(day => {
            let high = Math.round(Math.max(...days[day].temp));
            let low = Math.round(Math.min(...days[day].temp));
            let ic = Object.keys(days[day].icon).sort((a,b)=>days[day].icon[b]-days[day].icon[a])[0];
            daily.innerHTML += `<div class="day-card">
              <div class="day">${day}</div>
              <img src="https://openweathermap.org/img/wn/${ic}@2x.png">
              <div class="temps"><span class="high">${high}°</span> / <span class="low">${low}°</span></div>
            </div>`;
          });

          document.getElementById('loading').classList.add('hidden');
        });
    });
}

// Check If Safe To Go
function checkIfSafeToGo() {
  if (!forecastData) return alert("Weather data not loaded yet");
  let date = document.getElementById('visitDate').value;
  let time = document.getElementById('visitTime').value || "12:00";
  if (!date) return alert("Select date");
  let target = new Date(`${date}T${time}:00`).getTime() / 1000;
  document.getElementById('recommendation').innerHTML = "Checking...";
  document.getElementById('recommendation').className = "";
  let closest = forecastData.reduce((a, b) => Math.abs(a.dt - target) < Math.abs(b.dt - target) ? a : b);
  let t = Math.round(closest.main.temp);
  let r = Math.round((closest.pop || 0) * 100);
  let desc = closest.weather[0].description.toLowerCase();
  let msg, cls;
  if (r > 70 || desc.includes('storm') || desc.includes('thunder')) {
    msg = "DON'T GO<br>Rain/Storm expected";
    cls = "danger";
  } else if (r > 40 || t > 38 || t < 8) {
    msg = "THINK TWICE<br>Heavy rain or extreme temp";
    cls = "warning";
  } else {
    msg = "GOOD TO GO!<br>Pleasant weather";
    cls = "good";
  }
  document.getElementById('recommendation').innerHTML = "<strong>Recommendation</strong><br><br>" + msg;
  document.getElementById('recommendation').className = cls;
}

// Voice Summary
function speakWeather() {
  const text = `Current temperature is ${document.getElementById('temp').innerText}. ${document.getElementById('description').textContent}. In ${document.getElementById('location').innerText}.`;
  const utter = new SpeechSynthesisUtterance(text);
  utter.lang = 'en-IN';
  speechSynthesis.speak(utter);
}

// Save Favorite
function saveCurrentLocation() {
  const name = document.getElementById('location').innerText.split(',')[0].trim();
  let favs = JSON.parse(localStorage.getItem('weatherFavs') || '[]');
  if (!favs.includes(name)) {
    favs.push(name);
    localStorage.setItem('weatherFavs', JSON.stringify(favs));
    alert(name + " saved as favorite!");
  } else alert("Already saved");
}

// Show Favorites
function showFavorites() {
  const favs = JSON.parse(localStorage.getItem('weatherFavs') || '[]');
  if (favs.length === 0) return alert("No favorites yet");
  const choice = prompt("Favorites:\n" + favs.join('\n') + "\n\nType city name to load:");
  if (choice && favs.includes(choice.trim())) {
    document.getElementById('city').value = choice;
    searchFlexibleLocation();
  }
}

// Share
function shareWeather() {
  const text = `Kalyan Live Weather\n${document.getElementById('location').innerText}\n${document.getElementById('temp').innerText}\n${document.getElementById('description').textContent}\n\nCheck: ${location.href}`;
  window.open(`https://wa.me/?text=${encodeURIComponent(text)}`);
}

// Auto load
window.onload = () => getLocationWeather();

  <script src="script.js"></script>
</body>
</html>
