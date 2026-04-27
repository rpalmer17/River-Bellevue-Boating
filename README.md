<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="River Station">
<meta name="theme-color" content="#060b12">
<title>River Station · Bellevue</title>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=IBM+Plex+Mono:wght@400;500;600&family=Manrope:wght@400;500;600;700&display=swap" rel="stylesheet">

<style>
  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  html, body { background: #060b12; color: #f4ede4; }
  body {
    font-family: 'Manrope', system-ui, sans-serif;
    min-height: 100vh;
    min-height: 100dvh;
    background: radial-gradient(ellipse at top, #0f2030 0%, #060b12 70%);
    padding: max(16px, env(safe-area-inset-top)) 16px max(16px, env(safe-area-inset-bottom));
    overflow-x: hidden;
  }

  .container { max-width: 480px; margin: 0 auto; display: flex; flex-direction: column; gap: 16px; padding-top: 8px; }

  .serif { font-family: 'Instrument Serif', serif; font-weight: 400; }
  .mono { font-family: 'IBM Plex Mono', monospace; }
  .label { font-family: 'IBM Plex Mono', monospace; font-size: 11px; letter-spacing: 0.2em; text-transform: uppercase; color: #8ba3b8; }
  .sub { font-family: 'Instrument Serif', serif; font-style: italic; color: #8ba3b8; }

  header { padding-bottom: 4px; }
  header .top { display: flex; justify-content: space-between; align-items: flex-start; gap: 12px; }
  header .locpill { display: flex; align-items: center; gap: 6px; color: #6b8196; font-family: 'IBM Plex Mono', monospace; font-size: 10px; letter-spacing: 0.2em; text-transform: uppercase; margin-bottom: 4px; }
  header h1 { font-family: 'Instrument Serif', serif; font-size: 34px; font-weight: 400; line-height: 1; color: #f4ede4; }
  header .subtitle { font-family: 'Instrument Serif', serif; font-style: italic; font-size: 14px; color: #8ba3b8; margin-top: 2px; }
  header .updated { font-family: 'IBM Plex Mono', monospace; font-size: 10px; color: #6b8196; letter-spacing: 0.12em; margin-top: 10px; }
  #refresh {
    background: rgba(244,237,228,0.05);
    border: 1px solid rgba(244,237,228,0.1);
    color: #d4c7b8;
    border-radius: 999px; padding: 10px; cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    transition: all .2s;
  }
  #refresh:active { transform: scale(.94); }
  #refresh.loading svg { animation: spin 1s linear infinite; }
  @keyframes spin { to { transform: rotate(360deg); } }

  .card {
    background: linear-gradient(180deg, #0f1e2e 0%, #0a1520 100%);
    border: 1px solid rgba(244,237,228,0.08);
    border-radius: 18px;
    padding: 20px;
    position: relative;
  }
  .card.tight { padding: 16px; }

  .section-head { display: flex; align-items: center; gap: 8px; color: #8ba3b8; margin-bottom: 10px; }
  .section-head .label { color: inherit; }

  .safety .score-row { display: flex; justify-content: space-between; align-items: center; margin-top: 8px; }
  .safety .score { font-family: 'Instrument Serif', serif; font-size: 88px; line-height: 1; font-weight: 400; }
  .safety .score-unit { font-family: 'Manrope', sans-serif; font-size: 10px; letter-spacing: 0.15em; color: #6b8196; text-transform: uppercase; margin-top: -4px; }
  .safety .verdict-label { font-family: 'Manrope', sans-serif; font-size: 13px; font-weight: 700; letter-spacing: 0.15em; margin-top: 12px; }
  .safety .verdict-note { font-family: 'Instrument Serif', serif; font-style: italic; font-size: 19px; color: #d4c7b8; margin-top: 4px; line-height: 1.3; }
  .safety .factors { margin-top: 20px; display: flex; flex-direction: column; }
  .factor { display: flex; align-items: flex-start; gap: 10px; padding: 10px 0; border-top: 1px dashed rgba(244,237,228,0.08); font-family: 'Manrope', sans-serif; font-size: 13px; color: #d4c7b8; line-height: 1.4; }
  .factor:first-child { border-top: none; }
  .factor svg { flex-shrink: 0; margin-top: 2px; }

  .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
  .stat {
    background: rgba(244,237,228,0.03);
    border: 1px solid rgba(244,237,228,0.06);
    border-radius: 14px;
    padding: 16px;
  }
  .stat .stat-label { display: flex; align-items: center; gap: 6px; color: #6b8196; margin-bottom: 8px; font-family: 'IBM Plex Mono', monospace; font-size: 10px; letter-spacing: 0.18em; text-transform: uppercase; }
  .stat .stat-value { font-family: 'Instrument Serif', serif; font-size: 36px; line-height: 1; color: #f4ede4; display: flex; align-items: baseline; gap: 4px; }
  .stat .stat-unit { font-family: 'IBM Plex Mono', monospace; font-size: 12px; color: #8ba3b8; }
  .stat .stat-sub { font-family: 'Manrope', sans-serif; font-size: 11px; color: #8ba3b8; margin-top: 4px; }

  .river-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; margin-bottom: 18px; }
  .river-stat .rlabel { font-family: 'IBM Plex Mono', monospace; font-size: 10px; letter-spacing: 0.15em; color: #6b8196; }
  .river-stat .rval { font-family: 'Instrument Serif', serif; font-size: 28px; line-height: 1.1; display: flex; align-items: baseline; gap: 3px; color: #f4ede4; }
  .river-stat .runit { font-family: 'IBM Plex Mono', monospace; font-size: 11px; color: #8ba3b8; }
  .stage-bar { position: relative; height: 10px; background: rgba(244,237,228,0.06); border-radius: 999px; margin-bottom: 6px; }
  .stage-fill { position: absolute; top: 0; left: 0; height: 10px; border-radius: 999px; transition: width .8s ease; }
  .stage-marker { position: absolute; top: -3px; width: 1px; height: 16px; }
  .stage-legend { display: flex; justify-content: space-between; font-family: 'IBM Plex Mono', monospace; font-size: 9px; letter-spacing: 0.08em; color: #6b8196; }
  .river-note { font-family: 'Manrope', sans-serif; font-style: italic; font-size: 11px; color: #6b8196; margin-top: 14px; line-height: 1.4; }

  .hourly-scroll { display: flex; gap: 8px; overflow-x: auto; padding-bottom: 4px; margin: 0 -20px; padding: 0 20px 4px; -webkit-overflow-scrolling: touch; scrollbar-width: thin; }
  .hourly-scroll::-webkit-scrollbar { height: 4px; }
  .hourly-scroll::-webkit-scrollbar-thumb { background: rgba(244,237,228,0.1); border-radius: 2px; }
  .hour {
    flex-shrink: 0; min-width: 64px; padding: 12px 10px; border-radius: 12px; text-align: center;
    background: rgba(244,237,228,0.03);
    border: 1px solid rgba(244,237,228,0.05);
  }
  .hour.stormy { background: rgba(200,90,90,0.12); border-color: rgba(200,90,90,0.4); }
  .hour .h-time { font-family: 'IBM Plex Mono', monospace; font-size: 10px; color: #8ba3b8; }
  .hour .h-icon { margin: 8px auto; color: #d4c7b8; }
  .hour.stormy .h-icon { color: #c85a5a; }
  .hour .h-temp { font-family: 'Instrument Serif', serif; font-size: 20px; color: #f4ede4; line-height: 1; }
  .hour .h-wind { font-family: 'IBM Plex Mono', monospace; font-size: 10px; margin-top: 6px; }
  .hour .h-pop { font-family: 'IBM Plex Mono', monospace; font-size: 9px; color: #6da3c8; margin-top: 2px; }

  .day-row { display: grid; grid-template-columns: 2fr 0.8fr 2.5fr 2fr 1.7fr 1.4fr; align-items: center; gap: 6px; padding: 12px 0; border-top: 1px dashed rgba(244,237,228,0.08); }
  .day-row:first-of-type { border-top: none; }
  .day-name { font-family: 'Manrope', sans-serif; font-size: 13px; font-weight: 600; color: #f4ede4; }
  .day-date { font-family: 'IBM Plex Mono', monospace; font-size: 10px; color: #6b8196; }
  .day-icon { display: flex; justify-content: center; color: #d4c7b8; }
  .day-desc { font-family: 'Manrope', sans-serif; font-size: 11px; color: #8ba3b8; line-height: 1.2; }
  .day-desc .pop { color: #6da3c8; font-size: 10px; font-family: 'IBM Plex Mono', monospace; }
  .day-wind { text-align: right; font-family: 'IBM Plex Mono', monospace; font-size: 11px; }
  .day-wind .gust { color: #6b8196; }
  .day-wind .uv { font-size: 10px; }
  .day-temp { text-align: right; font-family: 'Instrument Serif', serif; line-height: 1; }
  .day-temp .hi { font-size: 18px; color: #f4ede4; }
  .day-temp .lo { font-size: 13px; color: #6b8196; }
  .day-score { display: flex; justify-content: flex-end; align-items: center; }
  .score-badge {
    display: inline-flex; align-items: center; justify-content: center;
    width: 38px; height: 38px; border-radius: 50%;
    border: 1.5px solid currentColor;
    background: rgba(0,0,0,0.25);
    font-family: 'Instrument Serif', serif; font-size: 17px; line-height: 1;
  }

  .alert {
    background: linear-gradient(135deg, #3d1a1a 0%, #2a1515 100%);
    border: 1px solid #c85a5a;
    border-radius: 14px;
    padding: 16px;
    display: flex;
    gap: 12px;
    margin-bottom: 8px;
  }
  .alert svg { flex-shrink: 0; color: #c85a5a; margin-top: 2px; }
  .alert .alabel { font-family: 'Manrope', sans-serif; font-size: 11px; font-weight: 700; letter-spacing: 0.15em; color: #c85a5a; text-transform: uppercase; }
  .alert .aevent { font-family: 'Instrument Serif', serif; font-size: 20px; color: #f4ede4; margin-top: 2px; line-height: 1.2; }
  .alert .ahead { font-family: 'Manrope', sans-serif; font-size: 12px; color: #e9a6a6; margin-top: 6px; line-height: 1.4; }

  .error-card {
    background: rgba(212,160,92,0.08);
    border: 1px solid rgba(212,160,92,0.4);
    border-radius: 14px;
    padding: 16px;
    display: flex;
    gap: 10px;
  }
  .error-card svg { flex-shrink: 0; color: #d4a05c; margin-top: 2px; }
  .error-card .elabel { font-family: 'IBM Plex Mono', monospace; font-size: 10px; color: #d4a05c; letter-spacing: 0.18em; text-transform: uppercase; font-weight: 600; }
  .error-card .edetails { font-family: 'Manrope', sans-serif; font-size: 12px; color: #e8c692; margin-top: 6px; line-height: 1.5; }
  .error-card .ehint { font-family: 'Instrument Serif', serif; font-style: italic; font-size: 12px; color: #a89378; margin-top: 10px; line-height: 1.4; }

  .loader { padding: 40px 20px; text-align: center; }
  .loader svg { color: #8ba3b8; animation: spin 1s linear infinite; }
  .loader .ltext { font-family: 'Instrument Serif', serif; font-style: italic; color: #8ba3b8; margin-top: 10px; font-size: 15px; }

  footer { padding-top: 16px; text-align: center; }
  footer .sources { font-family: 'IBM Plex Mono', monospace; font-size: 9px; color: #4a5a6b; letter-spacing: 0.2em; text-transform: uppercase; line-height: 1.8; }
  footer .motto { font-family: 'Instrument Serif', serif; font-style: italic; font-size: 12px; color: #6b8196; margin-top: 12px; }

  .arc-wrap { flex-shrink: 0; }
  .arc-bg { stroke: rgba(244,237,228,0.08); }

  .grain::before {
    content: ''; position: absolute; inset: 0; pointer-events: none; opacity: 0.04;
    background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='120' height='120'><filter id='n'><feTurbulence baseFrequency='0.9' numOctaves='2'/></filter><rect width='100%25' height='100%25' filter='url(%23n)' opacity='1'/></svg>");
    border-radius: inherit;
  }
</style>
</head>
<body>
<div class="container" id="app"></div>

<script>
'use strict';

const LOCATION = {
  name: 'Bellevue, Iowa',
  subtitle: 'Mississippi River · Pool 12 → Lock & Dam 11',
  lat: 42.2597,
  lon: -90.4246,
};

const RIVER_RATIO_FLOOD  = 2.0;
const RIVER_RATIO_ACTION = 1.4;
const RIVER_RISE_24H_HIGH = 0.20;
const RIVER_RISE_24H_MED  = 0.10;

const ICONS = {
  wind: '<path d="M17.7 7.7a2.5 2.5 0 1 1 1.8 4.3H2"/><path d="M9.6 4.6A2 2 0 1 1 11 8H2"/><path d="M12.6 19.4A2 2 0 1 0 14 16H2"/>',
  sun:  '<circle cx="12" cy="12" r="4"/><path d="M12 2v2"/><path d="M12 20v2"/><path d="m4.93 4.93 1.41 1.41"/><path d="m17.66 17.66 1.41 1.41"/><path d="M2 12h2"/><path d="M20 12h2"/><path d="m6.34 17.66-1.41 1.41"/><path d="m19.07 4.93-1.41 1.41"/>',
  cloud: '<path d="M17.5 19a4.5 4.5 0 1 0-1.4-8.8 7 7 0 1 0-13.4 3.8"/><path d="M17.5 19H9a4 4 0 1 1 .5-7.9"/>',
  rain:  '<path d="M4 14.899A7 7 0 1 1 15.71 8h1.79a4.5 4.5 0 0 1 2.5 8.242"/><path d="M16 14v6"/><path d="M8 14v6"/><path d="M12 16v6"/>',
  storm: '<path d="M17.5 19a4.5 4.5 0 1 0-1.4-8.8 7 7 0 1 0-13.4 3.8"/><path d="m13 12-3 5h4l-3 5"/>',
  snow:  '<path d="M17.5 19a4.5 4.5 0 1 0-1.4-8.8 7 7 0 1 0-13.4 3.8"/><path d="M8 19h.01"/><path d="M8 23h.01"/><path d="M12 21h.01"/><path d="M16 19h.01"/><path d="M16 23h.01"/>',
  fog:   '<path d="M3 8h18"/><path d="M3 12h18"/><path d="M3 16h18"/><path d="M3 20h18"/>',
  anchor: '<path d="M12 22V8"/><path d="M5 12H2a10 10 0 0 0 20 0h-3"/><circle cx="12" cy="5" r="3"/>',
  waves: '<path d="M2 6c.6.5 1.2 1 2.5 1 2.5 0 2.5-2 5-2s2.5 2 5 2 2.5-2 5-2c1.3 0 1.9.5 2.5 1"/><path d="M2 12c.6.5 1.2 1 2.5 1 2.5 0 2.5-2 5-2s2.5 2 5 2 2.5-2 5-2c1.3 0 1.9.5 2.5 1"/><path d="M2 18c.6.5 1.2 1 2.5 1 2.5 0 2.5-2 5-2s2.5 2 5 2 2.5-2 5-2c1.3 0 1.9.5 2.5 1"/>',
  alert: '<path d="m21.73 18-8-14a2 2 0 0 0-3.48 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3Z"/><path d="M12 9v4"/><path d="M12 17h.01"/>',
  refresh: '<path d="M3 12a9 9 0 0 1 9-9 9.75 9.75 0 0 1 6.74 2.74L21 8"/><path d="M21 3v5h-5"/><path d="M21 12a9 9 0 0 1-9 9 9.75 9.75 0 0 1-6.74-2.74L3 16"/><path d="M3 21v-5h5"/>',
  thermo: '<path d="M14 4v10.54a4 4 0 1 1-4 0V4a2 2 0 0 1 4 0Z"/>',
  up: '<path d="M7 17L17 7"/><path d="M7 7h10v10"/>',
  down: '<path d="M7 7l10 10"/><path d="M17 7v10H7"/>',
  minus: '<path d="M5 12h14"/>',
  zap: '<path d="M4 14a1 1 0 0 1-.78-1.63l9.9-10.2a.5.5 0 0 1 .86.46l-1.92 6.02A1 1 0 0 0 13 10h7a1 1 0 0 1 .78 1.63l-9.9 10.2a.5.5 0 0 1-.86-.46l1.92-6.02A1 1 0 0 0 11 14Z"/>',
  compass: '<circle cx="12" cy="12" r="10"/><polygon points="16.24 7.76 14.12 14.12 7.76 16.24 9.88 9.88 16.24 7.76"/>',
  eye: '<path d="M2.062 12.348a1 1 0 0 1 0-.696 10.75 10.75 0 0 1 19.876 0 1 1 0 0 1 0 .696 10.75 10.75 0 0 1-19.876 0"/><circle cx="12" cy="12" r="3"/>',
  pin: '<path d="M20 10c0 4.993-5.539 10.193-7.399 11.799a1 1 0 0 1-1.202 0C9.539 20.193 4 14.993 4 10a8 8 0 0 1 16 0"/><circle cx="12" cy="10" r="3"/>',
};
function svg(name, size = 14, color = 'currentColor') {
  return `<svg width="${size}" height="${size}" viewBox="0 0 24 24" fill="none" stroke="${color}" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">${ICONS[name]||''}</svg>`;
}

const WX = {
  0:  ['Clear', 'sun'], 1: ['Mostly clear', 'sun'], 2: ['Partly cloudy', 'cloud'], 3: ['Overcast', 'cloud'],
  45: ['Fog', 'fog'], 48: ['Rime fog', 'fog'],
  51: ['Light drizzle', 'rain'], 53: ['Drizzle', 'rain'], 55: ['Heavy drizzle', 'rain'],
  61: ['Light rain', 'rain'], 63: ['Rain', 'rain'], 65: ['Heavy rain', 'rain'],
  66: ['Freezing rain', 'rain'], 67: ['Heavy freezing rain', 'rain'],
  71: ['Light snow', 'snow'], 73: ['Snow', 'snow'], 75: ['Heavy snow', 'snow'], 77: ['Snow grains', 'snow'],
  80: ['Rain showers', 'rain'], 81: ['Heavy showers', 'rain'], 82: ['Violent showers', 'rain'],
  85: ['Snow showers', 'snow'], 86: ['Heavy snow showers', 'snow'],
  95: ['Thunderstorm', 'storm'], 96: ['T-storm w/ hail', 'storm'], 99: ['Severe t-storm', 'storm'],
};
const wxText = (c) => (WX[c] || ['Unknown','cloud'])[0];
const wxIcon = (c) => (WX[c] || ['Unknown','cloud'])[1];
const isStorm = (c) => c >= 95 && c <= 99;
const isRain = (c) => (c >= 51 && c <= 67) || (c >= 80 && c <= 82);
const isHeavyRain = (c) => c === 65 || c === 67 || c === 82;

const DIRS = ['N','NNE','NE','ENE','E','ESE','SE','SSE','S','SSW','SW','WSW','W','WNW','NW','NNW'];
const windDir = (d) => DIRS[Math.round(((d%360)/22.5))%16];

function uvBand(uv) {
  if (uv < 3) return ['Low', '#7bb369'];
  if (uv < 6) return ['Moderate', '#d4c05c'];
  if (uv < 8) return ['High', '#e08a3c'];
  if (uv < 11) return ['Very High', '#c85a5a'];
  return ['Extreme', '#a04080'];
}

function fmtTime(iso) {
  return new Date(iso).toLocaleTimeString('en-US', { hour: 'numeric', hour12: true }).replace(' ','');
}
function fmtDay(iso) { return new Date(iso).toLocaleDateString('en-US', { weekday: 'short' }); }
function fmtDate(iso) { return new Date(iso).toLocaleDateString('en-US', { month: 'short', day: 'numeric' }); }

async function fetchWeather() {
  const p = new URLSearchParams({
    latitude: LOCATION.lat, longitude: LOCATION.lon,
    current: 'temperature_2m,apparent_temperature,relative_humidity_2m,precipitation,weather_code,wind_speed_10m,wind_direction_10m,wind_gusts_10m,uv_index',
    hourly: 'temperature_2m,precipitation_probability,precipitation,weather_code,wind_speed_10m,wind_gusts_10m,wind_direction_10m,uv_index',
    daily: 'weather_code,temperature_2m_max,temperature_2m_min,precipitation_sum,precipitation_probability_max,wind_speed_10m_max,wind_gusts_10m_max,uv_index_max,sunrise,sunset',
    temperature_unit: 'fahrenheit', wind_speed_unit: 'mph', precipitation_unit: 'inch',
    timezone: 'America/Chicago', forecast_days: 7,
  });
  const r = await fetch(`https://api.open-meteo.com/v1/forecast?${p}`);
  if (!r.ok) throw new Error(`Open-Meteo ${r.status}`);
  return r.json();
}

async function fetchAlerts() {
  const r = await fetch(`https://api.weather.gov/alerts/active?point=${LOCATION.lat},${LOCATION.lon}`);
  if (!r.ok) throw new Error(`NWS ${r.status}`);
  const j = await r.json();
  return (j.features || []).map(f => ({
    id: f.id,
    event: f.properties.event,
    severity: f.properties.severity,
    headline: f.properties.headline,
  }));
}

async function fetchRiver() {
  const params = new URLSearchParams({
    latitude: LOCATION.lat,
    longitude: LOCATION.lon,
    daily: 'river_discharge',
    past_days: 14,
    forecast_days: 2,
    timezone: 'America/Chicago',
  });
  const r = await fetch(`https://flood-api.open-meteo.com/v1/flood?${params}`);
  if (!r.ok) throw new Error(`Flood API ${r.status}`);
  const j = await r.json();
  const series = (j.daily?.river_discharge || [])
    .map((v, i) => ({ t: j.daily.time[i], v: v == null ? null : parseFloat(v) }))
    .filter(p => p.v != null && !isNaN(p.v));

  if (series.length === 0) throw new Error('No river data returned');

  const latest = series[series.length - 1];
  const yesterday = series.length >= 2 ? series[series.length - 2] : null;
  const baselineSlice = series.slice(0, Math.max(1, series.length - 2));
  const baseline = baselineSlice.reduce((a, p) => a + p.v, 0) / baselineSlice.length;
  const toKcfs = (m3s) => (m3s * 35.3147) / 1000;
  const ratio = baseline > 0 ? latest.v / baseline : null;
  const change24h = (yesterday && yesterday.v > 0) ? (latest.v - yesterday.v) / yesterday.v : null;

  return {
    discharge_kcfs: toKcfs(latest.v),
    baseline_kcfs: toKcfs(baseline),
    ratio,
    change24h,
    history: series.map(p => ({ t: p.t, v: toKcfs(p.v) })),
  };
}

function calcSafety({ weather, alerts, river }) {
  if (!weather) return null;
  const c = weather.current;
  let score = 100;
  const factors = [];

  const w = c.wind_speed_10m;
  const g = c.wind_gusts_10m;
  const dir = windDir(c.wind_direction_10m);

  if (w > 25)      { score -= 55; factors.push(['red','wind',`Sustained ${Math.round(w)} mph ${dir} — unsafe for pontoon`]); }
  else if (w > 20) { score -= 35; factors.push(['red','wind',`Heavy wind ${Math.round(w)} mph ${dir} — rough chop`]); }
  else if (w > 15) { score -= 18; factors.push(['amber','wind',`Brisk ${Math.round(w)} mph ${dir} — expect chop on open pools`]); }
  else if (w > 10) { factors.push(['good','wind',`Light breeze ${Math.round(w)} mph ${dir}`]); }
  else             { factors.push(['good','wind',`Calm — ${Math.round(w)} mph`]); }

  if (g > 30)      { score -= 15; factors.push(['red','zap',`Gusts to ${Math.round(g)} mph`]); }
  else if (g > 25) { score -= 8;  factors.push(['amber','zap',`Gusts to ${Math.round(g)} mph`]); }

  const todayMaxTemp = weather.daily?.temperature_2m_max?.[0] ?? c.temperature_2m;
  if (todayMaxTemp < 65) {
    score -= 50;
    factors.push(['red','thermo',`Today's high only ${Math.round(todayMaxTemp)}°F — below your 65°F minimum`]);
  } else if (c.temperature_2m < 65) {
    score -= 8;
    factors.push(['info','thermo',`Currently ${Math.round(c.temperature_2m)}°F — will warm up today (high ${Math.round(todayMaxTemp)}°F)`]);
  }

  if (isStorm(c.weather_code)) {
    score -= 60;
    factors.push(['red','storm','Thunderstorm now — do not launch']);
  } else if (isHeavyRain(c.weather_code)) {
    score -= 45;
    factors.push(['red','rain','Heavy rain falling now']);
  } else if (isRain(c.weather_code)) {
    score -= 25;
    factors.push(['amber','rain','Rain falling now']);
  }

  const flood = alerts.find(a => /flood/i.test(a.event));
  const severe = alerts.find(a => /tornado|severe thunderstorm|special marine|hurricane/i.test(a.event));
  if (severe) { score -= 55; factors.push(['red','alert',`NWS: ${severe.event}`]); }
  if (flood)  { score -= 35; factors.push(['red','waves',`NWS: ${flood.event} — debris & current risk`]); }

  const hourly = weather.hourly;
  const now = Date.now();
  const upcoming = hourly.time
    .map((t,i) => ({
      t: new Date(t).getTime(),
      code: hourly.weather_code[i],
      pop: hourly.precipitation_probability[i] || 0,
    }))
    .filter(h => h.t >= now - 30*60*1000 && h.t <= now + 6*3600*1000);
  const stormSoon = upcoming.find(h => isStorm(h.code));
  if (stormSoon) {
    score -= 40;
    const hrs = Math.max(0, Math.round((stormSoon.t - now) / 3600000));
    factors.push(['red','storm',`Thunderstorm forecast in ~${hrs}h`]);
  } else {
    const rainSoon = upcoming.find(h => isRain(h.code) || h.pop >= 60);
    if (rainSoon) {
      score -= 18;
      const hrs = Math.max(0, Math.round((rainSoon.t - now) / 3600000));
      factors.push(['amber','rain',`Rain forecast in ~${hrs}h (${Math.round(rainSoon.pop)}% chance)`]);
    }
  }

  if (river && river.ratio != null) {
    if (river.ratio >= RIVER_RATIO_FLOOD) {
      score -= 30;
      factors.push(['red','waves',`Discharge ${(river.ratio*100).toFixed(0)}% of normal — flood-stage water, debris risk high`]);
    } else if (river.ratio >= RIVER_RATIO_ACTION) {
      score -= 12;
      factors.push(['amber','waves',`Discharge ${(river.ratio*100).toFixed(0)}% of normal — elevated, debris likely`]);
    } else {
      factors.push(['good','waves',`River discharge near normal (${river.discharge_kcfs.toFixed(1)} kcfs)`]);
    }
    if (river.change24h != null) {
      if (river.change24h >= RIVER_RISE_24H_HIGH) {
        score -= 10;
        factors.push(['amber','up',`River rising fast +${(river.change24h*100).toFixed(0)}% in 24h — debris incoming`]);
      } else if (river.change24h >= RIVER_RISE_24H_MED) {
        score -= 3;
        factors.push(['amber','up',`River rising +${(river.change24h*100).toFixed(0)}% in 24h`]);
      } else if (river.change24h <= -0.05) {
        factors.push(['good','down',`River falling ${(river.change24h*100).toFixed(0)}% in 24h`]);
      }
    }
  }

  const uv = c.uv_index;
  if (uv >= 8) factors.push(['amber','sun',`UV ${Math.round(uv)} — very high, sunscreen + shade essential`]);
  else if (uv >= 6) factors.push(['info','sun',`UV ${Math.round(uv)} — high, sunscreen advised`]);

  if (c.weather_code === 45 || c.weather_code === 48) {
    score -= 20; factors.push(['amber','eye','Fog — poor visibility for navigation']);
  }

  score = Math.max(0, Math.min(100, score));

  let verdict;
  if (score >= 80)      verdict = { label: 'GOOD TO GO',      tone: 'green',  note: 'Conditions favor a safe day on the water.' };
  else if (score >= 60) verdict = { label: 'USE CAUTION',     tone: 'amber',  note: 'Boating is acceptable — stay alert and monitor changes.' };
  else if (score >= 40) verdict = { label: 'POOR CONDITIONS', tone: 'orange', note: 'Not recommended unless experienced and prepared.' };
  else                  verdict = { label: 'UNSAFE',          tone: 'red',    note: 'Stay ashore — conditions are hazardous.' };

  return { score, verdict, factors };
}

function calcDailyScore(daily, i, river) {
  if (!daily) return null;
  let score = 100;
  const code = daily.weather_code[i];
  const tempMax = daily.temperature_2m_max[i];
  const windMax = daily.wind_speed_10m_max[i];
  const gustMax = daily.wind_gusts_10m_max[i];
  const popMax = daily.precipitation_probability_max[i] ?? 0;

  if (tempMax < 65) score -= 50;
  else if (tempMax < 70) score -= 5;

  if (isStorm(code)) score -= 50;
  else if (isHeavyRain(code)) score -= 35;
  else if (isRain(code)) score -= 22;
  else {
    if (popMax >= 70) score -= 18;
    else if (popMax >= 50) score -= 8;
  }

  if (windMax > 25) score -= 35;
  else if (windMax > 20) score -= 22;
  else if (windMax > 15) score -= 10;

  if (gustMax > 35) score -= 12;
  else if (gustMax > 30) score -= 6;

  if (river && river.ratio != null) {
    if (river.ratio >= RIVER_RATIO_FLOOD) score -= 22;
    else if (river.ratio >= RIVER_RATIO_ACTION) score -= 8;
  }

  return Math.max(0, Math.min(100, score));
}

const TONE_COLOR = { green: '#7bb369', amber: '#d4a05c', orange: '#e08a3c', red: '#c85a5a' };
const FACTOR_COLOR = { red: '#c85a5a', amber: '#d4a05c', good: '#7bb369', info: '#8ba3b8' };

function computeSuggestedWindow(weather) {
  if (!weather?.hourly || !weather?.daily) return null;

  const now = Date.now();
  const todayEnd = new Date();
  todayEnd.setHours(23, 59, 0, 0);
  const todayEndTs = todayEnd.getTime();

  const sunrise = weather.daily.sunrise?.[0] ? new Date(weather.daily.sunrise[0]).getTime() : null;
  const sunset  = weather.daily.sunset?.[0]  ? new Date(weather.daily.sunset[0]).getTime()  : null;

  const h = weather.hourly;
  const hours = h.time
    .map((t, i) => ({
      t: new Date(t).getTime(),
      iso: t,
      temp: h.temperature_2m[i],
      code: h.weather_code[i],
      wind: h.wind_speed_10m[i],
      pop:  h.precipitation_probability[i] ?? 0,
    }))
    .filter(x => x.t >= now - 30 * 60 * 1000 && x.t <= todayEndTs)
    .filter(x => sunrise == null || sunset == null || (x.t >= sunrise && x.t <= sunset));

  if (hours.length === 0) {
    return { available: false, reason: 'No daylight hours remaining today.' };
  }

  const todayMaxTemp = weather.daily.temperature_2m_max?.[0] ?? 0;
  if (todayMaxTemp < 65) {
    return {
      available: false,
      reason: `Today's high is only ${Math.round(todayMaxTemp)}°F — below your 65°F minimum.`,
    };
  }

  const isGood = (x) =>
    x.temp >= 65 &&
    x.pop < 30 &&
    !isRain(x.code) &&
    !isStorm(x.code) &&
    x.wind < 20;

  let bestStart = -1, bestEnd = -1, bestLen = 0;
  let curStart = -1;
  for (let i = 0; i < hours.length; i++) {
    if (isGood(hours[i])) {
      if (curStart === -1) curStart = i;
      const len = i - curStart + 1;
      if (len > bestLen) { bestLen = len; bestStart = curStart; bestEnd = i; }
    } else {
      curStart = -1;
    }
  }

  if (bestStart === -1) {
    const hadStorm = hours.some(x => isStorm(x.code));
    const hadRain  = hours.some(x => isRain(x.code) || x.pop >= 50);
    const hadWarm  = hours.some(x => x.temp >= 65);
    const hadWind  = hours.some(x => x.wind >= 20);
    let reason = 'No window matches your criteria today.';
    if (hadStorm) reason = 'Thunderstorms in forecast.';
    else if (hadRain) reason = 'Rain or high precipitation chance during warm hours.';
    else if (!hadWarm) reason = `Temperature won't reach 65°F during daylight today.`;
    else if (hadWind) reason = 'Winds above 20 mph throughout.';
    return { available: false, reason };
  }

  const startHr = hours[bestStart];
  const endHr   = hours[bestEnd];

  return {
    available: true,
    startISO: startHr.iso,
    endISO:   endHr.iso,
    durationHours: bestLen,
    startsNow: bestStart === 0,
  };
}

function renderHeader(lastUpdated, loading) {
  const time = lastUpdated ? lastUpdated.toLocaleTimeString('en-US', { hour: 'numeric', minute: '2-digit' }) : '—';
  return `
    <header>
      <div class="top">
        <div>
          <div class="locpill">${svg('pin',11)}<span>River Station</span></div>
          <h1>${LOCATION.name}</h1>
          <div class="subtitle">${LOCATION.subtitle}</div>
        </div>
        <button id="refresh" class="${loading?'loading':''}" aria-label="Refresh">${svg('refresh',15)}</button>
      </div>
      <div class="updated">UPDATED ${time}</div>
    </header>
  `;
}

function renderErrors(errors) {
  const keys = Object.keys(errors);
  if (keys.length === 0) return '';
  const list = keys.map(k => `<div><strong style="text-transform:capitalize">${k}:</strong> ${errors[k]}</div>`).join('');
  return `
    <div class="error-card">
      ${svg('alert',16)}
      <div>
        <div class="elabel">Data Connection Issue</div>
        <div class="edetails">${list}</div>
        <div class="ehint">Pull to refresh, or check your internet connection.</div>
      </div>
    </div>
  `;
}

function renderAlerts(alerts) {
  if (!alerts || alerts.length === 0) return '';
  return alerts.map(a => `
    <div class="alert">
      ${svg('alert',18)}
      <div>
        <div class="alabel">NWS Active · ${a.severity || ''}</div>
        <div class="aevent">${a.event}</div>
        ${a.headline ? `<div class="ahint">${a.headline}</div>` : ''}
      </div>
    </div>
  `).join('');
}

function renderSafety(safety) {
  if (!safety) return '';
  const accent = TONE_COLOR[safety.verdict.tone];
  const arcLen = 141;
  const dash = (safety.score / 100) * arcLen;
  const factors = safety.factors.map(([level, iconName, msg]) => `
    <div class="factor">
      <span style="color:${FACTOR_COLOR[level]}">${svg(iconName,14,FACTOR_COLOR[level])}</span>
      <span>${msg}</span>
    </div>
  `).join('');
  return `
    <div class="card safety grain">
      <div class="section-head">${svg('anchor',14)}<span class="label">Boating Conditions</span></div>
      <div class="score-row">
        <div>
          <div class="score" style="color:${accent}">${Math.round(safety.score)}</div>
          <div class="score-unit">out of 100</div>
        </div>
        <svg class="arc-wrap" width="110" height="70" viewBox="0 0 110 70">
          <path class="arc-bg" d="M 10 60 A 45 45 0 0 1 100 60" fill="none" stroke-width="6" stroke-linecap="round"/>
          <path d="M 10 60 A 45 45 0 0 1 100 60" fill="none" stroke="${accent}" stroke-width="6" stroke-linecap="round" stroke-dasharray="${dash} ${arcLen}" style="transition:stroke-dasharray .8s ease"/>
        </svg>
      </div>
      <div class="verdict-label" style="color:${accent}">${safety.verdict.label}</div>
      <div class="verdict-note">${safety.verdict.note}</div>
      <div class="factors">${factors}</div>
    </div>
  `;
}

function renderCurrent(current) {
  if (!current) return '';
  const [uvLabel, uvColor] = uvBand(current.uv_index);
  const windAccent = current.wind_speed_10m > 20 ? '#c85a5a' : current.wind_speed_10m > 15 ? '#d4a05c' : '#f4ede4';
  const wxt = wxText(current.weather_code);
  const wxi = wxIcon(current.weather_code);
  return `
    <div>
      <div class="section-head">${svg('thermo',14)}<span class="label">Current</span></div>
      <div class="stat-grid">
        <div class="stat">
          <div class="stat-label">${svg('thermo',12)}<span>Temp</span></div>
          <div class="stat-value">${Math.round(current.temperature_2m)}<span class="stat-unit">°F</span></div>
          <div class="stat-sub">Feels ${Math.round(current.apparent_temperature)}°</div>
        </div>
        <div class="stat">
          <div class="stat-label">${svg('wind',12)}<span>Wind</span></div>
          <div class="stat-value" style="color:${windAccent}">${Math.round(current.wind_speed_10m)}<span class="stat-unit">mph ${windDir(current.wind_direction_10m)}</span></div>
          <div class="stat-sub">Gusts ${Math.round(current.wind_gusts_10m)} mph</div>
        </div>
        <div class="stat">
          <div class="stat-label">${svg('sun',12)}<span>UV Index</span></div>
          <div class="stat-value" style="color:${uvColor}">${Math.round(current.uv_index)}</div>
          <div class="stat-sub">${uvLabel}</div>
        </div>
        <div class="stat">
          <div class="stat-label">${svg(wxi,12)}<span>Conditions</span></div>
          <div class="stat-value" style="font-size:22px">${wxt}</div>
          <div class="stat-sub">${current.relative_humidity_2m}% humidity</div>
        </div>
      </div>
    </div>
  `;
}

function renderRiverGraph(history) {
  if (!history || history.length < 3) return '';
  const points = history.slice(-9);
  const W = 320, H = 80, P = 14;
  const vals = points.map(p => p.v);
  const minV = Math.min(...vals);
  const maxV = Math.max(...vals);
  const range = Math.max(0.1, maxV - minV);
  const padMin = minV - range * 0.15;
  const padMax = maxV + range * 0.15;
  const xs = points.map((p, i) => P + (i / (points.length - 1)) * (W - 2 * P));
  const ys = points.map(p => H - P - ((p.v - padMin) / (padMax - padMin)) * (H - 2 * P));

  const todayMid = new Date();
  todayMid.setHours(12, 0, 0, 0);
  const todayMidTs = todayMid.getTime();
  let todayIdx = 0;
  for (let i = 0; i < points.length; i++) {
    if (Math.abs(new Date(points[i].t).getTime() - todayMidTs) 
        Math.abs(new Date(points[todayIdx].t).getTime() - todayMidTs)) todayIdx = i;
  }

  let path = `M ${xs[0].toFixed(1)} ${ys[0].toFixed(1)}`;
  for (let i = 1; i < xs.length; i++) path += ` L ${xs[i].toFixed(1)} ${ys[i].toFixed(1)}`;

  let area = path + ` L ${xs[xs.length-1].toFixed(1)} ${(H - P).toFixed(1)} L ${xs[0].toFixed(1)} ${(H - P).toFixed(1)} Z`;

  const dots = points.map((p, i) => {
    const isToday = i === todayIdx;
    const isFuture = new Date(p.t).getTime() > todayMidTs + 12 * 3600 * 1000;
    const r = isToday ? 4 : 2.5;
    const fill = isToday ? '#f4ede4' : isFuture ? '#8ba3b8' : '#7bb369';
    return `<circle cx="${xs[i].toFixed(1)}" cy="${ys[i].toFixed(1)}" r="${r}" fill="${fill}"/>`;
  }).join('');

  const labels = `
    <span>${fmtDate(points[0].t)}</span>
    <span style="color:#d4c7b8">Today</span>
    <span>${fmtDate(points[points.length - 1].t)}</span>
  `;

  return `
    <div style="margin-top:18px">
      <div style="display:flex; align-items:center; gap:6px; margin-bottom:6px;">
        <span style="font-family:'IBM Plex Mono',monospace; font-size:9px; letter-spacing:0.18em; color:#6b8196; text-transform:uppercase;">Trend · ${points.length} days</span>
      </div>
      <svg viewBox="0 0 ${W} ${H}" width="100%" preserveAspectRatio="none" style="display:block; height:80px;">
        <defs>
          <linearGradient id="riverGrad" x1="0" y1="0" x2="0" y2="1">
            <stop offset="0%" stop-color="#7bb369" stop-opacity="0.35"/>
            <stop offset="100%" stop-color="#7bb369" stop-opacity="0"/>
          </linearGradient>
        </defs>
        <path d="${area}" fill="url(#riverGrad)" stroke="none"/>
        <path d="${path}" stroke="#7bb369" stroke-width="2" fill="none" stroke-linejoin="round" stroke-linecap="round"/>
        ${dots}
        <line x1="${xs[todayIdx].toFixed(1)}" y1="${P}" x2="${xs[todayIdx].toFixed(1)}" y2="${H - P}" stroke="#f4ede4" stroke-width="0.5" stroke-dasharray="2 2" opacity="0.4"/>
      </svg>
      <div style="display:flex; justify-content:space-between; font-family:'IBM Plex Mono',monospace; font-size:9px; color:#6b8196; letter-spacing:0.1em; margin-top:4px;">
        ${labels}
      </div>
    </div>
  `;
}

function renderRiver(river) {
  if (!river || river.discharge_kcfs == null) return '';
  const ratio = river.ratio ?? 1;
  const dischargeColor = ratio >= RIVER_RATIO_FLOOD ? '#c85a5a'
                        : ratio >= RIVER_RATIO_ACTION ? '#d4a05c' : '#7bb369';

  const change = river.change24h;
  const riseIcon = change == null ? 'minus' : change > 0.02 ? 'up' : change < -0.02 ? 'down' : 'minus';
  const riseDisplay = change == null ? '—' : (change > 0 ? '+' : '') + (change * 100).toFixed(0) + '%';

  const ratioPct = Math.round(ratio * 100);
  const ratioColor = dischargeColor;

  const maxRef = 250;
  const pct = Math.min(100, (ratioPct / maxRef) * 100);
  const normalPct = (100 / maxRef) * 100;
  const actionPct = (140 / maxRef) * 100;
  const floodPct = (200 / maxRef) * 100;

  return `
    <div class="card">
      <div class="section-head">${svg('waves',14)}<span class="label">River Conditions</span></div>
      <div class="sub" style="font-size:15px;margin-bottom:14px">Mississippi River · GloFAS modeled discharge</div>
      <div class="river-grid">
        <div class="river-stat">
          <div class="rlabel">DISCHARGE</div>
          <div class="rval" style="color:${dischargeColor}">${river.discharge_kcfs.toFixed(1)}<span class="runit">kcfs</span></div>
        </div>
        <div class="river-stat">
          <div class="rlabel">VS NORMAL</div>
          <div class="rval" style="color:${ratioColor}">${ratioPct}<span class="runit">%</span></div>
        </div>
        <div class="river-stat">
          <div class="rlabel">24H CHANGE</div>
          <div class="rval">${svg(riseIcon,16,'#f4ede4')}<span style="margin-left:4px">${riseDisplay}</span></div>
        </div>
      </div>
      <div class="stage-bar">
        <div class="stage-fill" style="width:${pct}%;background:linear-gradient(90deg, ${dischargeColor}88 0%, ${dischargeColor} 100%)"></div>
        <div class="stage-marker" style="left:${normalPct}%;background:#7bb369"></div>
        <div class="stage-marker" style="left:${actionPct}%;background:#d4a05c"></div>
        <div class="stage-marker" style="left:${floodPct}%;background:#c85a5a"></div>
      </div>
      <div class="stage-legend">
        <span style="color:#7bb369">NORMAL 100%</span>
        <span style="color:#d4a05c">ELEVATED</span>
        <span style="color:#c85a5a">FLOOD</span>
      </div>
      ${renderRiverGraph(river.history)}
      <div class="river-note">Modeled river flow vs the past 14-day average. Rising trends are the strongest signal for incoming debris (logs, brush, washed-out docks).</div>
    </div>
  `;
}

function renderSuggestedWindow(win) {
  if (!win) return '';
  if (!win.available) {
    return `
      <div class="card" style="border-color:rgba(212,160,92,0.3); background:linear-gradient(180deg,#1f1810 0%,#15100a 100%);">
        <div class="section-head" style="color:#d4a05c;">${svg('sun',14,'#d4a05c')}<span class="label" style="color:#d4a05c">Today's Window</span></div>
        <div style="font-family:'Instrument Serif',serif; font-style:italic; font-size:20px; color:#d4c7b8; line-height:1.3; margin-top:4px;">
          Not recommended today.
        </div>
        <div style="font-family:'Manrope',sans-serif; font-size:13px; color:#a89378; margin-top:8px; line-height:1.45;">
          ${win.reason}
        </div>
      </div>
    `;
  }
  const startTime = fmtTime(win.startISO);
  const endTime = fmtTime(win.endISO);
  const startsLabel = win.startsNow ? 'Now' : startTime;
  return `
    <div class="card" style="border-color:rgba(123,179,105,0.4); background:linear-gradient(180deg,#172418 0%,#0e1810 100%);">
      <div class="section-head" style="color:#7bb369;">${svg('sun',14,'#7bb369')}<span class="label" style="color:#7bb369">Today's Window</span></div>
      <div style="font-family:'Instrument Serif',serif; font-size:34px; color:#f4ede4; line-height:1.05; margin-top:4px;">
        ${startsLabel} <span style="color:#7bb369;">→</span> ${endTime}
      </div>
      <div style="font-family:'Manrope',sans-serif; font-size:12px; color:#8ba3b8; margin-top:8px; line-height:1.45;">
        ${win.durationHours} hour${win.durationHours === 1 ? '' : 's'} of 65°F+ with no rain and light wind
      </div>
    </div>
  `;
}

function renderHourly(hourly) {
  if (!hourly) return '';
  const now = Date.now();
  const rows = hourly.time
    .map((t, i) => ({
      time: t, ts: new Date(t).getTime(),
      temp: hourly.temperature_2m[i], code: hourly.weather_code[i],
      wind: hourly.wind_speed_10m[i], pop: hourly.precipitation_probability[i],
    }))
    .filter(h => h.ts >= now - 30*60*1000)
    .slice(0, 16);
  const hours = rows.map((h, i) => {
    const windColor = h.wind > 20 ? '#c85a5a' : h.wind > 15 ? '#d4a05c' : '#8ba3b8';
    const stormy = isStorm(h.code);
    return `
      <div class="hour ${stormy?'stormy':''}">
        <div class="h-time">${i === 0 ? 'Now' : fmtTime(h.time)}</div>
        <div class="h-icon">${svg(wxIcon(h.code),18)}</div>
        <div class="h-temp">${Math.round(h.temp)}°</div>
        <div class="h-wind" style="color:${windColor}">${Math.round(h.wind)}mph</div>
        ${h.pop >= 30 ? `<div class="h-pop">${Math.round(h.pop)}%</div>` : ''}
      </div>
    `;
  }).join('');
  return `
    <div class="card">
      <div class="section-head">${svg('compass',14)}<span class="label">Next 16 Hours</span></div>
      <div class="hourly-scroll">${hours}</div>
    </div>
  `;
}

function renderDaily(daily, river) {
  if (!daily) return '';
  const days = daily.time.slice(0, 7).map((d, i) => {
    const windMax = daily.wind_speed_10m_max[i];
    const gustMax = daily.wind_gusts_10m_max[i];
    const uvMax = daily.uv_index_max[i];
    const pop = daily.precipitation_probability_max[i];
    const windTone = windMax > 20 ? '#c85a5a' : windMax > 15 ? '#d4a05c' : '#d4c7b8';
    const uvColor = uvBand(uvMax)[1];
    const score = calcDailyScore(daily, i, river);
    const scoreColor = score >= 80 ? '#7bb369'
                     : score >= 60 ? '#d4a05c'
                     : score >= 40 ? '#e08a3c' : '#c85a5a';
    return `
      <div class="day-row">
        <div>
          <div class="day-name">${i === 0 ? 'Today' : fmtDay(d)}</div>
          <div class="day-date">${fmtDate(d)}</div>
        </div>
        <div class="day-icon">${svg(wxIcon(daily.weather_code[i]),18)}</div>
        <div class="day-desc">
          ${wxText(daily.weather_code[i])}
          ${pop > 20 ? `<div class="pop">${Math.round(pop)}% precip</div>` : ''}
        </div>
        <div class="day-wind" style="color:${windTone}">
          ${svg('wind',10,windTone)} ${Math.round(windMax)}<span class="gust">/${Math.round(gustMax)}</span>
          <div class="uv" style="color:${uvColor}">${svg('sun',9,uvColor)} UV ${Math.round(uvMax)}</div>
        </div>
        <div class="day-temp">
          <div class="hi">${Math.round(daily.temperature_2m_max[i])}°</div>
          <div class="lo">${Math.round(daily.temperature_2m_min[i])}°</div>
        </div>
        <div class="day-score">
          <span class="score-badge" style="color:${scoreColor}" title="Boating score: ${score}/100">${score}</span>
        </div>
      </div>
    `;
  }).join('');
  return `
    <div class="card">
      <div class="section-head">${svg('cloud',14)}<span class="label">7-Day Outlook</span></div>
      <div style="display:grid; grid-template-columns: 2fr 0.8fr 2.5fr 2fr 1.7fr 1.4fr; gap:6px; padding-bottom:6px; margin-bottom:2px; border-bottom:1px solid rgba(244,237,228,0.06); font-family:'IBM Plex Mono',monospace; font-size:9px; letter-spacing:0.15em; color:#6b8196; text-transform:uppercase;">
        <div>Day</div><div></div><div>Sky</div><div style="text-align:right">Wind/UV</div><div style="text-align:right">Temp</div><div style="text-align:right">Score</div>
      </div>
      ${days}
    </div>
  `;
}

function renderFooter() {
  return `
    <footer>
      <div class="sources">
        Weather · Open-Meteo<br>
        Alerts · NWS api.weather.gov<br>
        River · Open-Meteo Flood (GloFAS)
      </div>
      <div class="motto">Always check for hazards. Wear your PFD.</div>
    </footer>
  `;
}

const state = {
  weather: null, alerts: [], river: null,
  errors: {}, loading: true, lastUpdated: null,
};

function render() {
  const loadingFirst = state.loading && !state.weather;
  const safety = calcSafety(state);
  const window_ = state.weather ? computeSuggestedWindow(state.weather) : null;
  const app = document.getElementById('app');
  app.innerHTML = `
    ${renderHeader(state.lastUpdated, state.loading)}
    ${renderErrors(state.errors)}
    ${renderAlerts(state.alerts)}
    ${loadingFirst
      ? `<div class="card loader">${svg('refresh',20)}<div class="ltext">Reading the river…</div></div>`
      : renderSafety(safety)
    }
    ${renderSuggestedWindow(window_)}
    ${state.weather ? renderCurrent(state.weather.current) : ''}
    ${renderRiver(state.river)}
    ${state.weather ? renderHourly(state.weather.hourly) : ''}
    ${state.weather ? renderDaily(state.weather.daily, state.river) : ''}
    ${renderFooter()}
  `;
  const btn = document.getElementById('refresh');
  if (btn) btn.addEventListener('click', () => loadAll());
}

async function loadAll() {
  state.loading = true;
  render();
  const errors = {};
  const [w, a, r] = await Promise.allSettled([fetchWeather(), fetchAlerts(), fetchRiver()]);
  if (w.status === 'fulfilled') state.weather = w.value; else errors.weather = w.reason?.message || 'Failed';
  if (a.status === 'fulfilled') state.alerts = a.value;  else errors.alerts  = a.reason?.message || 'Failed';
  if (r.status === 'fulfilled') state.river = r.value;   else errors.river   = r.reason?.message || 'Failed';
  state.errors = errors;
  state.lastUpdated = new Date();
  state.loading = false;
  render();
}

render();
loadAll();

let lastRefresh = Date.now();
document.addEventListener('visibilitychange', () => {
  if (!document.hidden && Date.now() - lastRefresh > 5 * 60 * 1000) {
    lastRefresh = Date.now();
    loadAll();
  }
});
</script>
</body>
</html>
