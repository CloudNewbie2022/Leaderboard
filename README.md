# Leaderboard
// A way to visualize the leaderboard as it changes over time 

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Angry Dynomites Leaderboard</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      max-width: 1000px;
      margin: 0 auto;
      padding: 20px;
      background-color: #1a1a1a;
      color: #ffffff;
    }
    h1 {
      text-align: center;
      color: #f39c12;
      margin-bottom: 30px;
    }
    #leaderboard {
      list-style: none;
      padding: 0;
    }
    .player {
      display: flex;
      justify-content: space-between;
      background-color: #2c3e50;
      margin-bottom: 10px;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 4px 6px rgba(0,0,0,0.3);
      transition: all 0.3s ease;
      position: relative;
      overflow: hidden;
    }
    .player-info {
      display: flex;
      align-items: center;
      gap: 15px;
    }
    .rank {
      font-weight: bold;
      font-size: 1.4em;
      min-width: 40px;
      text-align: center;
      color: #f1c40f;
    }
    .avatar {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid #3498db;
    }
    .name {
      font-weight: bold;
      font-size: 1.1em;
    }
    .points {
      color: #ecf0f1;
      font-size: 1.1em;
      font-weight: bold;
    }
    .position-change {
      margin-left: 10px;
      font-weight: bold;
      font-size: 0.9em;
    }
    .up {
      color: #2ecc71;
    }
    .down {
      color: #e74c3c;
    }
    .same {
      color: #3498db;
    }
    .new {
      color: #f39c12;
    }
    .podium {
      display: flex;
      justify-content: center;
      margin-bottom: 40px;
      gap: 20px;
      height: 250px;
      align-items: flex-end;
    }
    .podium-step {
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 120px;
    }
    .podium-rank {
      font-size: 24px;
      font-weight: bold;
      margin-bottom: 15px;
      color: #ecf0f1;
    }
    .podium-name {
      font-weight: bold;
      margin-bottom: 10px;
      color: #f1c40f;
      text-align: center;
    }
    .podium-avatar {
      width: 60px;
      height: 60px;
      border-radius: 50%;
      object-fit: cover;
      border: 3px solid;
      margin-bottom: 10px;
    }
    .podium-bar {
      width: 100%;
      border-top-left-radius: 8px;
      border-top-right-radius: 8px;
      transition: height 0.5s ease;
    }
    .first-place {
      z-index: 3;
    }
    .first-place .podium-bar {
      background: linear-gradient(to top, #f1c40f, #f39c12);
      height: 100%;
    }
    .first-place .podium-avatar {
      border-color: #f1c40f;
    }
    .second-place {
      z-index: 2;
    }
    .second-place .podium-bar {
      background: linear-gradient(to top, #95a5a6, #7f8c8d);
      height: 66%;
    }
    .second-place .podium-avatar {
      border-color: #95a5a6;
    }
    .third-place {
      z-index: 1;
    }
    .third-place .podium-bar {
      background: linear-gradient(to top, #cd7f32, #b87333);
      height: 33%;
    }
    .third-place .podium-avatar {
      border-color: #cd7f32;
    }
    .controls {
      display: flex;
      justify-content: center;
      margin: 30px 0;
      gap: 15px;
    }
    button {
      padding: 10px 20px;
      background-color: #3498db;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-weight: bold;
      transition: background-color 0.3s;
    }
    button:hover {
      background-color: #2980b9;
    }
    button:disabled {
      background-color: #7f8c8d;
      cursor: not-allowed;
    }
    #status {
      text-align: center;
      margin: 20px 0;
      font-style: italic;
      color: #bdc3c7;
    }
    @keyframes highlight {
      0% { background-color: rgba(46, 204, 113, 0.5); }
      100% { background-color: #2c3e50; }
    }
    .highlight {
      animation: highlight 2s;
    }
    .loading {
      text-align: center;
      padding: 30px;
      font-style: italic;
      color: #7f8c8d;
      font-size: 1.2em;
    }
    .error {
      color: #e74c3c;
      text-align: center;
      padding: 20px;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h1>Angry Dynomites Leaderboard</h1>
  
  <div class="podium">
    <div class="podium-step first-place">
      <div class="podium-rank">1st</div>
      <img class="podium-avatar" id="first-avatar" src="https://via.placeholder.com/60" alt="1st place">
      <div class="podium-name" id="first-name">-</div>
      <div class="podium-bar"></div>
    </div>
    <div class="podium-step second-place">
      <div class="podium-rank">2nd</div>
      <img class="podium-avatar" id="second-avatar" src="https://via.placeholder.com/60" alt="2nd place">
      <div class="podium-name" id="second-name">-</div>
      <div class="podium-bar"></div>
    </div>
    <div class="podium-step third-place">
      <div class="podium-rank">3rd</div>
      <img class="podium-avatar" id="third-avatar" src="https://via.placeholder.com/60" alt="3rd place">
      <div class="podium-name" id="third-name">-</div>
      <div class="podium-bar"></div>
    </div>
  </div>
  
  <div class="controls">
    <button id="refresh-btn">Refresh Leaderboard</button>
    <button id="auto-refresh-btn">Auto-Refresh (30s)</button>
    <button id="stop-auto-refresh-btn" disabled>Stop Auto-Refresh</button>
  </div>
  
  <div id="status">Loading initial data...</div>
  
  <ul id="leaderboard">
    <li class="loading">Loading leaderboard data...</li>
  </ul>
  
<div id="error-alert" class="error" style="display: none;">
    <p>An error occurred.</p>
    <button id="retry-btn">Retry</button>
  </div>
  
  <script>
  // ===== CONFIGURATION ===== //
  const API_CONFIG = {
    endpoints: {
      local: "http://localhost:8080/graphql",
      github: "https://your-deployed-backend.com/graphql",
      production: "https://api-preview.apps.angrydynomiteslab.com/graphql"
    },
    query: `
      query GetLeaderboard($id: ID!) {
        masterpiece(id: $id) {
          leaderboard {
            position
            masterpiecePoints
            profile {
              displayName
              avatarUrl
            }
          }
        }
      }
    `,
    masterpieceId: 110,
    pollInterval: 30000,
    defaultAvatar: 'https://via.placeholder.com/40'
  };

  // ===== MAIN LEADERBOARD CLASS ===== //
  class Leaderboard {
    constructor() {
      this.state = {
        pollIntervalId: null,
        isFetching: false,
        previousData: null
      };
      
      this.cacheElements();
      this.init();
    }

    cacheElements() {
      this.elements = {
        leaderboardList: document.getElementById('leaderboard'),
        refreshBtn: document.getElementById('refresh-btn'),
        autoRefreshBtn: document.getElementById('auto-refresh-btn'),
        stopRefreshBtn: document.getElementById('stop-auto-refresh-btn'),
        status: document.getElementById('status'),
        errorAlert: document.getElementById('error-alert'),
        retryBtn: document.getElementById('retry-btn'),
        podium: {
          first: document.getElementById('first-name'),
          second: document.getElementById('second-name'),
          third: document.getElementById('third-name'),
          firstAvatar: document.getElementById('first-avatar'),
          secondAvatar: document.getElementById('second-avatar'),
          thirdAvatar: document.getElementById('third-avatar')
        }
      };
    }

    init() {
      this.setupEventListeners();
      this.fetchLeaderboard();
    }

    // ===== API COMMUNICATION ===== //
    async fetchLeaderboard() {
      if (this.state.isFetching) return;
      
      this.setLoading(true);
      this.hideError();
      
      try {
        const response = await fetch(this.getApiUrl(), {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Accept': 'application/json',
            'Cache-Control': 'no-cache'
          },
          body: JSON.stringify({ 
            query: API_CONFIG.query,
            variables: { id: API_CONFIG.masterpieceId }
          })
        });

        if (!response.ok) throw new Error(`HTTP ${response.status}`);
        
        const { data, errors } = await response.json();
        if (errors) throw new Error(errors.map(e => e.message).join(', '));
        
        if (!data || !data.masterpiece || !data.masterpiece.leaderboard) {
          throw new Error("Invalid data structure received");
        }
        
        this.processData(data.masterpiece.leaderboard);
      } catch (error) {
        this.handleError(error);
      } finally {
        this.setLoading(false);
      }
    }

    // ===== DATA PROCESSING ===== //
    processData(leaderboard) {
      if (!leaderboard || !leaderboard.length) {
        throw new Error("No leaderboard data received");
      }
      
      this.updatePodium(leaderboard.slice(0, 3));
      this.renderLeaderboard(leaderboard);
      this.state.previousData = leaderboard;
    }

    // ===== UI RENDERING ===== //
    updatePodium(topThree) {
      const places = ['first', 'second', 'third'];
      topThree.forEach((player, index) => {
        const place = places[index];
        this.elements.podium[place].textContent = player.profile.displayName || 'Anonymous';
        this.elements.podium[`${place}Avatar`].src = 
          player.profile.avatarUrl || API_CONFIG.defaultAvatar;
      });
    }

    renderLeaderboard(leaderboard) {
      const fragment = document.createDocumentFragment();
      
      leaderboard.forEach(player => {
        const playerEl = document.createElement('li');
        playerEl.className = 'player';
        playerEl.innerHTML = `
          <div class="player-info">
            <span class="rank">${player.position}</span>
            <img class="avatar" 
                 src="${player.profile.avatarUrl || API_CONFIG.defaultAvatar}" 
                 alt="${player.profile.displayName || 'Anonymous'}" 
                 loading="lazy">
            <span class="name">${player.profile.displayName || 'Anonymous'}</span>
          </div>
          <span class="points">${player.masterpiecePoints.toLocaleString()}</span>
        `;
        fragment.appendChild(playerEl);
      });
      
      this.elements.leaderboardList.innerHTML = '';
      this.elements.leaderboardList.appendChild(fragment);
    }

    // ===== UTILITIES ===== //
    getApiUrl() {
      const host = window.location.hostname;
      if (/localhost|127\.0\.0\.1|::1/.test(host)) return API_CONFIG.endpoints.local;
      if (host.includes('github.io')) return API_CONFIG.endpoints.github;
      return API_CONFIG.endpoints.production;
    }

    setLoading(isLoading) {
      this.state.isFetching = isLoading;
      this.elements.refreshBtn.disabled = isLoading;
      this.elements.status.textContent = isLoading ? 'Loading...' : '';
    }

    handleError(error) {
      console.error('Leaderboard error:', error);
      this.elements.status.textContent = `Error: ${error.message}`;
      this.elements.status.style.color = '#e74c3c';
      this.showError(error.message);
    }

    showError(message) {
      this.elements.errorAlert.querySelector('p').textContent = `⚠️ ${message}`;
      this.elements.errorAlert.classList.remove('hidden');
    }

    hideError() {
      this.elements.errorAlert.classList.add('hidden');
    }

    // ===== POLLING CONTROL ===== //
    startPolling() {
      this.stopPolling();
      this.state.pollIntervalId = setInterval(
        () => this.fetchLeaderboard(), 
        API_CONFIG.pollInterval
      );
      this.updatePollingButtons(true);
    }

    stopPolling() {
      if (this.state.pollIntervalId) {
        clearInterval(this.state.pollIntervalId);
        this.state.pollIntervalId = null;
      }
      this.updatePollingButtons(false);
    }

    updatePollingButtons(isPolling) {
      this.elements.autoRefreshBtn.disabled = isPolling;
      this.elements.stopRefreshBtn.disabled = !isPolling;
    }

    // ===== EVENT HANDLERS ===== //
    setupEventListeners() {
      this.elements.refreshBtn.addEventListener('click', () => {
        this.fetchLeaderboard();
        if (this.state.pollIntervalId) this.startPolling();
      });
      
      this.elements.autoRefreshBtn.addEventListener('click', () => this.startPolling());
      this.elements.stopRefreshBtn.addEventListener('click', () => this.stopPolling());
      this.elements.retryBtn.addEventListener('click', () => this.fetchLeaderboard());
      
      document.addEventListener('visibilitychange', () => {
        if (document.visibilityState === 'visible') {
          this.fetchLeaderboard();
          if (this.state.pollIntervalId) this.startPolling();
        } else {
          this.stopPolling();
        }
      });
    }
  }

  // Initialize when DOM is ready
  document.addEventListener('DOMContentLoaded', () => new Leaderboard());
  </script>
</body>
</html>
