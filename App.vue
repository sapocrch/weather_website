<script setup>
import { ref } from 'vue';

// Dati completamente statici, non cambiano.
const currentDate = ref('19/06/2025');
const cityName = ref('Milano');
const currentTempC = ref(20);
const currentTempF = ref(77);
const weatherDescription = ref('Sole e schiarite');
// Le icone sono state rimosse, useremo solo il testo per descrizione

const dailyForecast = ref([
  { day: 20, description: 'Sole' },
  { day: 21, description: 'Sole' },
  { day: 22, description: 'Nuvoloso' },
  { day: 23, description: 'Sole' },
  { day: 24, description: 'Nuvoloso' },
  { day: 25, description: 'Nuvoloso' },
  { day: 26, description: 'Pioggia' },
  { day: 27, description: 'Pioggia' },
  { day: 28, description: 'Nuvoloso' },
]);

const hourlyForecast = ref([
  { time: 'now', temp: '20°', description: 'Sole' },
  { time: '19:00', temp: '20°', description: 'Sole' },
  { time: '20:00', temp: '17°', description: 'Nuvoloso' },
  { time: '21:00', temp: '15°', description: 'Nuvoloso' },
  { time: '22:00', temp: '14°', description: 'Nuvoloso' },
  { time: '23:00', temp: '14°', description: 'Nuvoloso' },
]);

// Funzioni fittizie (non fanno nulla, solo per dimostrazione)
const prevMonth = () => { alert('Mese precedente (funzionalità statica)'); };
const nextMonth = () => { alert('Mese successivo (funzionalità statica)'); };
const searchLocation = () => { alert('Funzionalità di ricerca non implementata (dati statici).'); };
</script>

<template>
  <div class="weather-app-container">
    <div class="weather-details-section">
      <h1 class="main-title">MeteoTrust Italia</h1>
      <p class="current-date">{{ currentDate }}</p>

      <div class="location-search" @click="searchLocation">
        <span>&#128269; clicca qui per il luogo che vuoi vedere</span> </div>

      <div class="current-weather">
        <p class="city-name">{{ cityName }} <span class="weather-emoji">&#9728;</span></p> <p class="temperature">{{ currentTempC }}°C / {{ currentTempF }}°F</p>
        <p class="description">{{ weatherDescription }}</p>
      </div>

      <div class="daily-forecast-container">
        <div class="daily-forecast-month">
          Giugno <button @click="prevMonth">&lt;</button> <button @click="nextMonth">&gt;</button>
        </div>
        <div class="daily-forecast-grid">
          <div v-for="day in dailyForecast" :key="day.day" class="day-forecast-item">
            <span>{{ day.day }}</span>
            <span class="weather-emoji">{{ 
                day.description === 'Sole' ? '&#9728;' : 
                day.description === 'Nuvoloso' ? '&#9729;' : 
                day.description === 'Pioggia' ? '&#127783;' : '' 
            }}</span>
            <span class="day-description">{{ day.description }}</span>
          </div>
        </div>
      </div>

      <div class="hourly-forecast-container">
        <div v-for="(hour, index) in hourlyForecast" :key="index" class="hourly-item">
          <span class="time">{{ hour.time }}</span>
          <span class="weather-emoji">{{ 
                hour.description === 'Sole' ? '&#9728;' : 
                hour.description === 'Nuvoloso' ? '&#9729;' : 
                hour.description === 'Pioggia' ? '&#127783;' : '' 
            }}</span>
          <span class="temp">{{ hour.temp }}</span>
        </div>
      </div>
    </div>

    <div class="map-legend-section">
      <div class="weather-map">
        <div class="map-placeholder">Mappa dell'Italia</div>
      </div>
      <div class="map-legend">
        <div class="legend-item"><span class="legend-color-box red"></span> : caldo</div>
        <div class="legend-item"><span class="legend-color-box blue"></span> : acqua</div>
        <div class="legend-item"><span class="legend-color-box cyan"></span> : neve</div>
      </div>
      <p class="geographic-map-link">Visita la mappa geografica. <a href="#">apri mappa</a></p>
    </div>
  </div>
</template>

<style scoped>
/* ========================================= */
/* Variabili CSS (per facilitare le modifiche) */
/* ========================================= */
:root {
  --background-light-blue: #E0F2F7; /* Un blu chiaro per lo sfondo generale */
  --card-background: #F0F8FF; /* Un colore bianco sporco per la scheda principale */
  --primary-color: #007bff; /* Un blu primario (potrebbe essere il colore del testo per la città) */
  --accent-color: #FFC107; /* Colore giallo per il sole */
  --text-color: #333;
  --light-text-color: #666;
  --border-radius-lg: 20px;
  --border-radius-sm: 8px;
  --shadow-color: rgba(0, 0, 0, 0.1);
  --section-gap: 2rem; /* Dal tuo main.css */
}

/* ========================================= */
/* Stili Globali e Contenitore Principale */
/* ========================================= */
body {
  font-family: 'Arial', sans-serif;
  background-color: var(--background-light-blue);
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  margin: 0;
  padding: 20px;
  box-sizing: border-box;
}

#app {
  max-width: 900px;
  width: 100%;
  margin: 0 auto;
  font-weight: normal;
}

a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
  padding: 3px;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

/* Stile per il contenitore principale della tua app meteo (la "scheda" grande) */
.weather-app-container {
  background-color: var(--card-background);
  border-radius: var(--border-radius-lg);
  box-shadow: 0 10px 30px var(--shadow-color);
  padding: 2.5rem 3rem;
  display: grid;
  grid-template-columns: 2fr 1fr; /* Colonna sinistra (meteo) e destra (mappa) */
  gap: 2.5rem;
  min-height: 580px;
  box-sizing: border-box;
}

/* ========================================= */
/* Sezione Sinistra (Meteo Dettagliato) */
/* ========================================= */
.weather-details-section {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.main-title {
  font-size: 2.8rem;
  font-weight: bold;
  color: var(--text-color);
  text-align: center;
  margin-bottom: 0.5rem;
}

.current-date {
  text-align: center;
  color: var(--light-text-color);
  font-size: 1rem;
  margin-bottom: 1rem;
}

.location-search {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  color: var(--light-text-color);
  font-size: 0.9rem;
  cursor: pointer;
}
/* Stile per emoji della lente */
.location-search span:first-child {
  font-size: 1.2rem;
  line-height: 1;
}

/* Sezione Meteo Attuale (Milano) */
.current-weather {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  text-align: center;
  gap: 0.5rem;
  padding: 1.5rem 0;
}
.current-weather .city-name {
  font-size: 2.5rem;
  font-weight: bold;
  color: var(--text-color);
  display: flex;
  align-items: center;
  gap: 0.8rem;
}
.weather-emoji { /* Stile generale per le emoji meteo */
  font-size: 1.8rem; /* Dimensione base per le emoji */
  line-height: 1;
}
.current-weather .weather-emoji {
  font-size: 2.5rem; /* Emoji più grande per il meteo attuale */
}

.current-weather .temperature {
  font-size: 1.8rem;
  color: var(--light-text-color);
}
.current-weather .description {
  font-size: 1.1rem;
  color: var(--light-text-color);
}


/* Calendario previsioni (Giorni) */
.daily-forecast-container {
  display: grid;
  grid-template-columns: auto 1fr;
  align-items: start;
  gap: 1.5rem;
  padding-top: 1.5rem;
  border-top: 1px solid rgba(0,0,0,0.08);
}

.daily-forecast-month {
  font-weight: bold;
  color: var(--text-color);
  font-size: 1.1rem;
  padding-left: 0.5rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
.daily-forecast-month button {
  background: none;
  border: none;
  font-size: 1.5rem;
  color: var(--light-text-color);
  cursor: pointer;
  padding: 0 5px;
}

.daily-forecast-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem 2rem;
}

.day-forecast-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  color: var(--text-color);
  font-size: 0.95rem;
}
.day-forecast-item span {
  font-weight: bold;
  margin-bottom: 0.4rem;
}
.day-forecast-item .weather-emoji {
  font-size: 1.8rem; /* Dimensione emoji per giorni */
  margin-bottom: 0.4rem;
}
.day-forecast-item .day-description {
    font-size: 0.8rem; /* Testo descrizione più piccolo sotto l'emoji */
    color: var(--light-text-color);
}


/* Previsioni orarie */
.hourly-forecast-container {
  display: flex;
  justify-content: center;
  gap: 1.5rem;
  margin-top: 2rem;
  padding: 1.2rem;
  background-color: rgba(0, 0, 0, 0.03);
  border-radius: var(--border-radius-sm);
}

.hourly-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: var(--light-text-color);
  font-size: 0.85rem;
  gap: 0.3rem;
}
.hourly-item .time {
  font-weight: bold;
  color: var(--text-color);
}
.hourly-item .weather-emoji {
  font-size: 1.2rem; /* Dimensione emoji per orari */
}
.hourly-item .temp {
  font-weight: bold;
  color: var(--text-color);
}

/* ========================================= */
/* Sezione Destra (Mappa e Legenda) */
/* ========================================= */
.map-legend-section {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}

.weather-map .map-placeholder {
  width: 100%;
  max-width: 280px;
  height: 200px; /* Altezza fissa per il placeholder */
  background-color: #e0e0e0; /* Colore grigio per il placeholder */
  border-radius: var(--border-radius-sm);
  box-shadow: 0 5px 15px rgba(0,0,0,0.08);
  display: flex;
  justify-content: center;
  align-items: center;
  color: #777;
  font-size: 1.1rem;
  font-weight: bold;
  text-align: center;
}

.map-legend {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
  font-size: 0.9rem;
  color: var(--text-color);
  padding-top: 2rem;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 0.7rem;
}
.legend-color-box {
  width: 18px;
  height: 18px;
  border-radius: 3px;
}
.legend-color-box.red { background-color: red; }
.legend-color-box.blue { background-color: blue; }
.legend-color-box.cyan { background-color: cyan; }


.geographic-map-link {
  text-align: center;
  margin-top: auto;
  color: var(--light-text-color);
  font-size: 0.8rem;
}
.geographic-map-link a {
  color: var(--primary-color);
  text-decoration: underline;
}

/* ========================================= */
/* Media Queries per la Responsività */
/* ========================================= */

/* Per schermi più piccoli (tablet, mobile) */
@media (max-width: 850px) {
  .weather-app-container {
    grid-template-columns: 1fr;
    padding: 2rem;
    gap: 2rem;
  }

  .main-title {
    font-size: 2.2rem;
  }

  .current-weather .city-name {
    font-size: 2rem;
  }

  .daily-forecast-container {
    grid-template-columns: 1fr;
    text-align: center;
  }
  .daily-forecast-month {
    justify-content: center;
  }

  .map-legend-section {
    flex-direction: column;
    gap: 2rem;
  }

  .hourly-forecast-container {
    flex-wrap: wrap;
    justify-content: center;
  }
}

/* Per schermi molto piccoli (smartphone) */
@media (max-width: 500px) {
  .weather-app-container {
    padding: 1.5rem;
  }
  .main-title {
    font-size: 1.8rem;
  }
  .daily-forecast-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>