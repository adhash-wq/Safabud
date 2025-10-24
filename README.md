<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
<title>Safa Bud v2 — Demo</title>
<style>
  :root{
    --g1: #e6fff2;
    --g2: #e8f8ff;
    --accent-1: #2fb57a;
    --accent-2: #45c0e6;
    --panel: rgba(255,255,255,0.88);
    --muted: #60707a;
    --glass: rgba(255,255,255,0.6);
    --shadow: rgba(12,40,30,0.08);
    --radius: 14px;
  }
  *{box-sizing:border-box}
  html,body{height:100%;margin:0;font-family:Inter,Segoe UI,system-ui,-apple-system,Roboto,'Helvetica Neue',Arial; -webkit-font-smoothing:antialiased;}
  body{
    background: radial-gradient(1200px 400px at 10% 10%, rgba(75,197,153,0.06), transparent),
                linear-gradient(160deg,var(--g1), var(--g2));
    display:flex;align-items:center;justify-content:center;padding:18px;
  }
  .shell{width:1220px; height:720px; border-radius:18px; background:linear-gradient(180deg, rgba(255,255,255,0.9), rgba(255,255,255,0.86)); box-shadow:0 30px 60px rgba(10,30,20,0.08); display:flex; overflow:hidden; border:1px solid rgba(100,200,160,0.06);}
  /* Left large panel (preview / camera / world) */
  .left{
    flex:2.2; padding:22px; display:flex; flex-direction:column; gap:14px;
  }
  .header{display:flex; align-items:center; justify-content:space-between;}
  .brand{display:flex; gap:14px; align-items:center;}
  .logo{
    width:64px; height:64px; border-radius:12px; background:linear-gradient(135deg,var(--accent-1),var(--accent-2)); display:flex;align-items:center;justify-content:center;color:white;font-weight:800;font-size:20px;box-shadow:0 8px 30px rgba(45,180,120,0.12);
  }
  .title{font-weight:800;color:#0b3b2a;font-size:20px}
  .tag{font-size:13px;color:var(--muted)}
  .player-panel{display:flex; gap:12px; align-items:center;}
  .coins{background:var(--panel); padding:10px 14px; border-radius:12px; display:flex; gap:12px; align-items:center; box-shadow:0 8px 18px rgba(80,160,120,0.06);}
  .coins .val{font-weight:800;color:#0b5a35}
  .small{font-size:13px;color:var(--muted)}
  /* main area card */
  .card{flex:1; background:linear-gradient(180deg, rgba(255,255,255,0.98), rgba(250,255,252,0.98)); border-radius:var(--radius); box-shadow:0 6px 20px rgba(50,100,80,0.04); padding:18px; position:relative; overflow:hidden;}
  .camera-view{width:100%; height:100%; border-radius:10px; background:linear-gradient(180deg,#f6fff9,#f1fbff); display:flex; align-items:center; justify-content:center; position:relative; overflow:hidden; border:1px solid rgba(120,200,160,0.05);}
  .camera-placeholder{width:92%; height:86%; border-radius:10px; background:linear-gradient(180deg, rgba(200,245,220,0.6), rgba(215,245,255,0.6)); display:flex; align-items:center; justify-content:center; position:relative; box-shadow:inset 0 6px 40px rgba(0,0,0,0.03);}
  /* robot illustration area */
  .robot-stage{width:720px; height:440px; display:flex; align-items:center; justify-content:center; position:relative;}
  /* right control panel */
  .right{flex:1; padding:20px; background:linear-gradient(180deg, rgba(255,255,255,0.95), rgba(255,255,255,0.9)); border-left:1px solid rgba(120,200,160,0.06); display:flex; flex-direction:column; gap:12px;}
  .menu {display:flex; flex-direction:column; gap:10px; margin-top:6px;}
  .btn{
    background: linear-gradient(180deg, var(--accent-1), var(--accent-2)); color:white; padding:12px 14px; border-radius:12px; font-weight:800; cursor:pointer; border:none; box-shadow:0 12px 26px rgba(30,140,110,0.12);
  }
  .btn.ghost{background:transparent;color:var(--accent-1); border:1px solid rgba(80,170,140,0.08); box-shadow:none; font-weight:700;}
  .panel{background:var(--panel); padding:12px; border-radius:10px; box-shadow:0 10px 26px rgba(20,60,40,0.03);}
  .statrow{display:flex; gap:8px; align-items:center; justify-content:space-between;}
  .controls{display:flex; flex-direction:column; gap:10px;}
  .dpad{width:170px; height:170px; display:grid; grid-template-columns:repeat(3,1fr); grid-template-rows:repeat(3,1fr); gap:8px;}
  .dpad button{background:rgba(255,255,255,0.6); border-radius:10px; border:none; font-weight:800; font-size:18px; cursor:pointer;}
  .dpad .mid{background:transparent; pointer-events:none;}
  .action-row{display:flex; gap:8px; justify-content:center;}
  .badge{padding:6px 10px; border-radius:8px; background:linear-gradient(90deg,#fff,#f8fff8); font-weight:700; color:#0b5a3a;}
  /* page transitions */
  .screen{position:absolute; inset:0; transition: transform 420ms cubic-bezier(.2,.9,.2,1), opacity 220ms; display:flex; flex-direction:column;}
  .screen.hidden{transform:translateX(20px); opacity:0; pointer-events:none;}
  .screen.visible{transform:translateX(0); opacity:1; pointer-events:auto;}
  /* small elements */
  .mini{font-size:12px;color:var(--muted)}
  .row{display:flex; gap:8px; align-items:center;}
  .backlink{background:transparent;border:none;color:var(--accent-1);font-weight:800;cursor:pointer;}
  /* robot SVG style */
  .robot-svg{width:420px;height:260px;}
  .footer{font-size:12px;color:var(--muted); position:absolute; bottom:12px; left:22px;}
  @media (max-width:1250px){
    .shell{width:980px;height:640px}
  }
  @media (max-width:900px){
    body{padding:8px}
    .shell{width:100%;height:100%; flex-direction:column}
    .left{order:2}
    .right{order:1; flex-direction:row; gap:8px; padding:12px}
  }
</style>
</head>
<body>
<div class="shell" role="application" aria-label="Safa Bud v2">
  <div class="left">
    <div class="header">
      <div class="brand">
        <div class="logo">SB</div>
        <div>
          <div class="title">Safa Bud</div>
          <div class="tag">Learn earning · Clean the planet</div>
        </div>
      </div>
      <div class="player-panel">
        <div class="coins panel">
          <div style="display:flex;align-items:center;gap:10px">
            <div style="width:36px;height:36px;border-radius:8px;background:linear-gradient(135deg,#ffd86b,#ffd24d);display:flex;align-items:center;justify-content:center;font-weight:800;color:#144b2f;">¢</div>
            <div>
              <div class="val" id="coins">0</div>
              <div class="mini">Coins</div>
            </div>
          </div>
        </div>
        <div class="panel" style="padding:10px 12px;border-radius:12px">
          <div style="font-weight:800" id="playerName">Player</div>
          <div class="mini">Level <span id="level">1</span> · XP <span id="xp">0</span>/100</div>
        </div>
      </div>
    </div>

    <!-- Main card that will switch screens -->
    <div class="card">
      <!-- HOME SCREEN -->
      <div id="homeScreen" class="screen visible">
        <div style="display:flex; gap:18px; align-items:center; height:100%;">
          <div style="flex:1;">
            <div style="font-size:18px;font-weight:800;color:#0b3b2a">Welcome back, <span id="welcomeName">Player</span>!</div>
            <div class="mini" style="margin-top:6px">Choose an action to begin. Play to control your robot and collect recyclables, or Compete to join events and climb leaderboards.</div>

            <div style="margin-top:22px; display:flex; gap:12px; flex-wrap:wrap;">
              <button class="btn" onclick="openScreen('play')">Play</button>
              <button class="btn ghost" onclick="openScreen('compete')">Compete</button>
              <button class="btn ghost" onclick="openScreen('score')">Score Board</button>
              <button class="btn ghost" onclick="openScreen('controls')">Settings / Controls</button>
              <button class="btn ghost" onclick="openScreen('report')">Report</button>
              <button class="btn ghost" onclick="openScreen('profile')">Player Details</button>
            </div>

            <div style="margin-top:18px; display:flex; gap:12px;">
              <div class="panel" style="padding:12px; flex:1;">
                <div style="font-weight:800">Active Quest</div>
                <div class="mini">Collect 15 plastic bottles · Reward: 15 coins</div>
              </div>
              <div class="panel" style="padding:12px; width:220px;">
                <div style="font-weight:800">Today's Event</div>
                <div class="mini">Quick Clean — top scorer wins bonus</div>
              </div>
            </div>
          </div>

          <div style="width:420px; display:flex; flex-direction:column; gap:12px; align-items:center;">
            <div class="camera-view">
              <div class="camera-placeholder" id="mainPreview">
                <!-- show a stylized robot top-down as preview -->
                <div class="robot-stage" aria-hidden="true">
                  <!-- SVG robot preview -->
                  <svg class="robot-svg" viewBox="0 0 420 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="robot-preview">
                    <!-- wheels -->
                    <g id="wheels" fill="#cfefe0" stroke="#bfe6d0" stroke-width="3">
                      <circle cx="60" cy="220" r="36"/>
                      <circle cx="180" cy="220" r="36"/>
                      <circle cx="300" cy="220" r="36"/>
                      <circle cx="120" cy="220" r="12" fill="#e6fff2" />
                      <circle cx="240" cy="220" r="12" fill="#e6fff2" />
                      <circle cx="360" cy="220" r="12" fill="#e6fff2" />
                    </g>
                    <!-- body -->
                    <rect x="90" y="70" width="240" height="110" rx="12" fill="url(#bodyg)" stroke="#8fd8b1" stroke-width="3"/>
                    <defs>
                      <linearGradient id="bodyg" x1="0" x2="1"><stop offset="0" stop-color="#e9fff6"/><stop offset="1" stop-color="#dff4ff"/></linearGradient>
                    </defs>
                    <!-- solar panel -->
                    <rect x="95" y="50" width="120" height="20" rx="4" fill="#cfeff8" stroke="#9fd8ea" />
                    <rect x="225" y="50" width="80" height="20" rx="4" fill="#cfeff8" stroke="#9fd8ea" />
                    <!-- arm base -->
                    <rect x="270" y="40" width="28" height="28" rx="6" fill="#e8fff2" stroke="#9fd8b0"/>
                    <!-- arm -->
                    <g transform="translate(0,0)" fill="#dff7ec" stroke="#9fd8b0" stroke-width="3">
                      <rect x="290" y="26" width="70" height="12" rx="6"/>
                      <rect x="350" y="22" width="30" height="10" rx="5"/>
                    </g>
                    <!-- claw -->
                    <g transform="translate(0,0)" fill="#fff7ee" stroke="#d6c7a6" stroke-width="3">
                      <path d="M388 20 q4 8 -8 18" />
                      <path d="M394 20 q-4 8 8 18" />
                      <circle cx="386" cy="24" r="8" fill="#fff" stroke="#e0d2b2"/>
                    </g>
                    <!-- camera eye -->
                    <circle cx="150" cy="120" r="16" fill="#083f2a" stroke="#9fd8b0" stroke-width="3"/>
                  </svg>
                </div>
              </div>
            </div>
            <div class="panel" style="width:92%; text-align:center;">
              <div style="font-weight:800">Robot Preview</div>
              <div class="mini">Tap Play to control the robot and view its camera.</div>
            </div>
          </div>
        </div>
      </div>

      <!-- PLAY SCREEN -->
      <div id="playScreen" class="screen hidden" aria-hidden="true">
        <div style="display:flex; height:100%; gap:14px;">
          <div style="flex:1; display:flex; flex-direction:column; gap:12px;">
            <div style="display:flex; align-items:center; justify-content:space-between;">
              <div style="display:flex; gap:12px; align-items:center;">
                <button class="backlink" onclick="openScreen('home')">← Back</button>
                <div style="font-weight:800;color:#0b3b2a">Play — Robot Control</div>
              </div>
              <div style="display:flex; gap:8px; align-items:center;">
                <div class="mini">Battery</div>
                <div class="badge" id="battery">100%</div>
              </div>
            </div>

            <div class="camera-view" style="height:420px;">
              <!-- camera-first-person placeholder -->
              <div class="camera-placeholder" id="cameraView" style="display:flex;flex-direction:column;align-items:center;justify-content:flex-start;padding:12px;">
                <div style="width:100%;display:flex;justify-content:space-between;align-items:center;">
                  <div style="font-weight:800">Robot Camera</div>
                  <div class="mini">Collected: <strong id="collectedCount">0</strong></div>
                </div>

                <div style="flex:1; width:100%; display:flex; align-items:center; justify-content:center;">
                  <!-- simulated camera feed area -->
                  <div style="width:88%; height:78%; border-radius:8px; background:linear-gradient(180deg,#eafff2,#e6f7ff); box-shadow:inset 0 12px 40px rgba(0,0,0,0.04); display:flex; align-items:center; justify-content:center; position:relative;">
                    <div id="cameraFeed" style="width:92%; height:88%; border-radius:6px; background-image:radial-gradient(circle at 20% 20%, rgba(255,255,255,0.6), transparent 10%), linear-gradient(180deg,#f8fff8,#f1fbff); display:flex; align-items:center; justify-content:center; color:var(--muted); font-weight:700;">
                      <div style="text-align:center;">
                        <div style="font-size:22px;color:#0b3b2a">Camera View</div>
                        <div class="mini" style="margin-top:6px">(Simulated first-person feed)</div>
                      </div>
                    </div>

                    <!-- mini radar / minimap -->
                    <div id="miniMap" style="position:absolute; right:12px; bottom:12px; width:130px; height:80px; background:rgba(255,255,255,0.85); border-radius:8px; box-shadow:0 6px 18px rgba(0,0,0,0.06); padding:8px; font-size:12px;">
                      <div style="font-weight:800">Radar</div>
                      <div class="mini">Trash nearby: <span id="radarCount">3</span></div>
                    </div>
                  </div>
                </div>

              </div>
            </div>
          </div>

          <div style="width:320px; display:flex; flex-direction:column; gap:12px;">
            <div class="panel">
              <div style="font-weight:800">Controls</div>
              <div class="mini" style="margin-top:6px">Choose movement or use the joystick arrows below.</div>
              <div style="margin-top:8px;" class="controls">
                <div class="dpad" role="group" aria-label="controls">
                  <button data-dir="up" onclick="move('up')">▲</button>
                  <button class="mid"></button>
                  <button data-dir="right" onclick="move('right')">►</button>
                  <button data-dir="left" onclick="move('left')">◄</button>
                  <button class="mid"></button>
                  <button data-dir="down" onclick="move('down')">▼</button>
                </div>
                <div class="action-row">
                  <button class="btn" onclick="collectAction()">Grab</button>
                  <button class="btn ghost" onclick="toggleCameraMode()">Cam: 1st</button>
                </div>
              </div>
            </div>

            <div class="panel">
              <div style="display:flex; justify-content:space-between; align-items:center;">
                <div>
                  <div style="font-weight:800">Status</div>
                  <div class="mini">XP: <span id="xpDisp">0</span> · Level: <span id="lvlDisp">1</span></div>
                </div>
                <div style="text-align:right">
                  <div style="font-weight:800">Coins</div>
                  <div class="mini" id="coinDisp">0</div>
                </div>
              </div>
            </div>

            <div class="panel">
              <div style="font-weight:800">Quick Actions</div>
              <div style="display:flex; gap:8px; margin-top:8px;">
                <button class="btn ghost" onclick="simulatePickup()">Pick Random</button>
                <button class="btn ghost" onclick="returnHome()">Exit</button>
              </div>
            </div>

            <div class="panel" style="text-align:center;">
              <div class="mini">Tip: Camera mode shows what the robot sees. Use radar to find trash.</div>
            </div>
          </div>
        </div>
      </div>

      <!-- COMPETE SCREEN -->
      <div id="competeScreen" class="screen hidden" aria-hidden="true">
        <div style="display:flex; gap:14px; height:100%;">
          <div style="flex:1; display:flex; flex-direction:column;">
            <div style="display:flex; align-items:center; justify-content:space-between;">
              <div><button class="backlink" onclick="openScreen('home')">← Back</button> <span style="font-weight:800">Compete — Events</span></div>
              <div class="mini">Live events start daily</div>
            </div>
            <div style="margin-top:12px; display:flex; gap:12px;">
              <div class="panel" style="flex:1;">
                <div style="font-weight:800">Quick Clean Challenge</div>
                <div class="mini">Collect the most trash in 30s. Top winner gets bonus coins.</div>
                <div style="margin-top:8px;"><button class="btn" onclick="startEvent()">Join Event</button></div>
              </div>
              <div class="panel" style="width:260px;">
                <div style="font-weight:800">Event Rules</div>
                <ol class="mini">
                  <li>Play fair. No cheating.</li>
                  <li>Reported bad behavior leads to ban.</li>
                  <li>Rewards simulated — parent approval needed for payouts.</li>
                </ol>
              </div>
            </div>

            <div style="margin-top:12px; display:flex; gap:12px;">
              <div class="panel" style="flex:1;">
                <div style="font-weight:800">Top Scores</div>
                <div id="eventScores" class="mini" style="margin-top:8px">Ava — 18 · Kai — 14 · Nova — 12</div>
              </div>
            </div>
          </div>

          <div style="width:320px;">
            <div class="panel">
              <div style="font-weight:800">Join Quick Match</div>
              <div class="mini">Matchmaking simulated in demo.</div>
              <div style="margin-top:8px;"><button class="btn" onclick="openScreen('play')">Play Match</button></div>
            </div>
            <div class="panel" style="margin-top:12px;">
              <div style="font-weight:800">Rewards</div>
              <div class="mini">Bonus for top ranks, daily streaks, and attendance.</div>
            </div>
          </div>
        </div>
      </div>

      <!-- SCOREBOARD SCREEN -->
      <div id="scoreScreen" class="screen hidden" aria-hidden="true">
        <div style="display:flex; flex-direction:column; gap:12px;">
          <div style="display:flex; justify-content:space-between; align-items:center;">
            <div><button class="backlink" onclick="openScreen('home')">← Back</button> <span style="font-weight:800">Score Board</span></div>
            <div>
              <button class="btn ghost" onclick="showLeaderboard('daily')">Daily</button>
              <button class="btn ghost" onclick="showLeaderboard('monthly')">Monthly</button>
            </div>
          </div>
          <div class="panel" style="flex:1;">
            <div id="leaderboardList" style="display:flex; flex-direction:column; gap:8px;"></div>
          </div>
        </div>
      </div>

      <!-- CONTROLS / SETTINGS -->
      <div id="controlsScreen" class="screen hidden" aria-hidden="true">
        <div style="display:flex; gap:12px;">
          <div style="flex:1;">
            <div style="display:flex; align-items:center; justify-content:space-between;">
              <div><button class="backlink" onclick="openScreen('home')">← Back</button> <span style="font-weight:800">Settings / Controls</span></div>
              <div class="mini">Customize controls</div>
            </div>
            <div style="margin-top:12px;" class="panel">
              <div style="font-weight:800">Control Type</div>
              <div class="mini" style="margin-top:8px">
                <label><input type="radio" name="controlType" value="dpad" checked> D-Pad (tap)</label><br>
                <label><input type="radio" name="controlType" value="joystick"> Joystick (touch)</label>
              </div>
            </div>

            <div style="margin-top:12px;" class="panel">
              <div style="font-weight:800">Camera Mode</div>
              <div class="mini" style="margin-top:8px">
                <label><input type="radio" name="camMode" value="first" checked> First-person (robot camera)</label><br>
                <label><input type="radio" name="camMode" value="third"> Third-person (overview)</label>
              </div>
            </div>
          </div>

          <div style="width:320px;">
            <div class="panel">
              <div style="font-weight:800">Audio</div>
              <div class="mini" style="margin-top:8px">
                <label><input type="checkbox" id="sfx"> Enable SFX</label><br>
                <label><input type="checkbox" id="music"> Background Music (demo non-functional)</label>
              </div>
            </div>

            <div class="panel" style="margin-top:12px;">
              <div style="font-weight:800">Privacy & Safety</div>
              <div class="mini">Reporting and ban policies; parental controls required for withdrawals.</div>
              <div style="margin-top:8px;"><button class="btn ghost" onclick="openScreen('report')">Report Player / Bug</button></div>
            </div>
          </div>
        </div>
      </div>

      <!-- REPORT -->
      <div id="reportScreen" class="screen hidden" aria-hidden="true">
        <div style="display:flex; flex-direction:column; gap:12px;">
          <div style="display:flex; justify-content:space-between; align-items:center;">
            <div><button class="backlink" onclick="openScreen('home')">← Back</button> <span style="font-weight:800">Report</span></div>
            <div class="mini">Reports are simulated in demo.</div>
          </div>
          <div class="panel">
            <div style="font-weight:800">Report Player / Bug</div>
            <div class="mini" style="margin-top:8px">Describe the issue below.</div>
            <textarea id="reportText" style="width:100%;height:120px;margin-top:8px;border-radius:8px;border:1px solid #e6f3ea;padding:8px"></textarea>
            <div style="margin-top:8px;text-align:right"><button class="btn" onclick="submitReport()">Submit</button></div>
          </div>
        </div>
      </div>

      <!-- PROFILE -->
      <div id="profileScreen" class="screen hidden" aria-hidden="true">
        <div style="display:flex; gap:12px; height:100%;">
          <div style="flex:1;">
            <div style="display:flex; align-items:center; justify-content:space-between;">
              <div><button class="backlink" onclick="openScreen('home')">← Back</button> <span style="font-weight:800">Player Details</span></div>
              <div class="mini">Achievements and stats</div>
            </div>
            <div style="margin-top:12px;" class="panel">
              <div style="font-weight:800" id="profileName">Player</div>
              <div class="mini">Total Collected: <span id="totalCollected">0</span> · Coins: <span id="profileCoins">0</span></div>
            </div>

            <div style="margin-top:12px;" class="panel">
              <div style="font-weight:800">Badges</div>
              <div class="mini" style="margin-top:8px">Eco Starter · Quick Cleaner · Solar Champion</div>
            </div>
          </div>

          <div style="width:320px;">
            <div class="panel">
              <div style="font-weight:800">Edit Profile</div>
              <div class="mini" style="margin-top:8px">Change display name</div>
              <input id="nameEdit" style="width:100%; padding:10px; border-radius:8px; border:1px solid #e6f3ea;" placeholder="Player">
              <div style="margin-top:8px;text-align:right"><button class="btn" onclick="saveProfile()">Save</button></div>
            </div>
            <div class="panel" style="margin-top:12px;">
              <div style="font-weight:800">Withdraw</div>
              <div class="mini">Convert coins to real money (simulated).</div>
              <div style="margin-top:8px;text-align:right"><button class="btn ghost" onclick="withdraw()">Withdraw</button></div>
            </div>
          </div>
        </div>
      </div>

    </div>

    <div class="footer">Safa Bud — demo · built for iPad preview</div>
  </div>

  <div class="right">
    <div class="panel" style="display:flex;flex-direction:column; gap:8px;">
      <div style="display:flex; justify-content:space-between; align-items:center;">
        <div style="font-weight:800">Quick Actions</div>
        <div class="mini">Demo</div>
      </div>
      <div style="display:flex; gap:8px; align-items:center;">
        <button class="btn" onclick="openScreen('play')">Play</button>
        <button class="btn ghost" onclick="openScreen('compete')">Compete</button>
      </div>
    </div>

    <div class="panel">
      <div style="font-weight:800">Session</div>
      <div class="mini">Collected: <strong id="sessionNow">0</strong></div>
      <div class="mini" style="margin-top:8px">Best Daily: <strong id="bestDaily">0</strong></div>
    </div>

    <div class="panel" style="display:flex; flex-direction:column; gap:8px;">
      <div style="font-weight:800">Controls Shortcut</div>
      <div style="display:flex; gap:6px;">
        <button class="btn ghost" onclick="openScreen('controls')">Controls</button>
        <button class="btn ghost" onclick="openScreen('report')">Report</button>
      </div>
    </div>

    <div class="panel" style="text-align:center;">
      <div style="font-weight:800">Player</div>
      <div class="mini" id="sideName">Player</div>
      <div style="margin-top:8px;"><button class="btn ghost" onclick="openScreen('profile')">View Profile</button></div>
    </div>

  </div>
</div>

<script>
  // state
  const state = {
    coins: parseInt(localStorage.getItem('sb2_coins')||'0',10),
    name: localStorage.getItem('sb2_name')||'Player',
    xp: parseInt(localStorage.getItem('sb2_xp')||'0',10),
    level: parseInt(localStorage.getItem('sb2_level')||'1',10),
    sessionCollected: parseInt(localStorage.getItem('sb2_session')||'0',10),
    battery: 100,
    leaderboards: {
      daily:[{name:'Ava',score:18},{name:'Kai',score:14}],
      monthly:[{name:'Ava',score:320},{name:'Zed',score:280}]
    }
  };

  // init elements
  const coinsEl = document.getElementById('coins');
  const playerName = document.getElementById('playerName');
  const welcomeName = document.getElementById('welcomeName');
  const sideName = document.getElementById('sideName');
  const xpEl = document.getElementById('xp');
  const levelEl = document.getElementById('level');
  const collectedCount = document.getElementById('collectedCount');
  const sessionNow = document.getElementById('sessionNow');
  const batteryEl = document.getElementById('battery');

  function render(){
    coinsEl.textContent = state.coins;
    document.getElementById('playerName').textContent = state.name;
    welcomeName.textContent = state.name;
    sideName.textContent = state.name;
    document.getElementById('welcomeName').textContent = state.name;
    document.getElementById('profileName').textContent = state.name;
    document.getElementById('xp').textContent = state.xp + '/100';
    document.getElementById('level').textContent = state.level;
    document.getElementById('collectedCount').textContent = state.sessionCollected;
    document.getElementById('sessionNow').textContent = state.sessionCollected;
    document.getElementById('totalCollected').textContent = state.sessionCollected;
    document.getElementById('profileCoins').textContent = state.coins;
    document.getElementById('coinDisp').textContent = state.coins;
    document.getElementById('coinEl')?.textContent = state.coins;
    document.getElementById('coins')?.textContent = state.coins;
    batteryEl.textContent = state.battery + '%';
  }
  render();

  function save(){
    localStorage.setItem('sb2_coins', String(state.coins));
    localStorage.setItem('sb2_name', state.name);
    localStorage.setItem('sb2_xp', String(state.xp));
    localStorage.setItem('sb2_level', String(state.level));
    localStorage.setItem('sb2_session', String(state.sessionCollected));
  }

  // screens
  const screens = {
    home: document.getElementById('homeScreen'),
    play: document.getElementById('playScreen'),
    compete: document.getElementById('competeScreen'),
    score: document.getElementById('scoreScreen'),
    controls: document.getElementById('controlsScreen'),
    report: document.getElementById('reportScreen'),
    profile: document.getElementById('profileScreen')
  };

  function openScreen(name){
    Object.keys(screens).forEach(k=>{
      if(k===name){
        screens[k].classList.remove('hidden'); screens[k].classList.add('visible'); screens[k].setAttribute('aria-hidden','false');
      } else {
        screens[k].classList.remove('visible'); screens[k].classList.add('hidden'); screens[k].setAttribute('aria-hidden','true');
      }
    });
    // small tweaks
    if(name === 'play'){
      // focus camera
      document.getElementById('cameraFeed').scrollIntoView({behavior:'smooth', block:'center'});
    }
  }

  // movement & collection simulation
  function move(dir){
    // simulate battery drain slightly
    state.battery = Math.max(0, state.battery - 1);
    batteryEl.textContent = state.battery + '%';
    showToast('Moved ' + dir);
  }

  function collectAction(){
    // collect simulated trash
    state.coins += 1;
    state.sessionCollected += 1;
    state.xp += 6;
    if(state.xp >= 100){ state.level += 1; state.xp -= 100; state.coins += 10; showToast('Level up! +10 coins'); }
    save(); render();
    showToast('Collected 1 item');
  }

  function simulatePickup(){
    collectAction();
  }

  // event
  let eventTimer = null;
  function startEvent(){
    openScreen('play');
    if(eventTimer) return;
    let time = 30;
    showOverlay('Event started — 30s');
    eventTimer = setInterval(()=>{
      time -= 1;
      document.getElementById('radarCount').textContent = Math.max(0, Math.floor(Math.random()*6));
      if(time <= 0){
        clearInterval(eventTimer); eventTimer = null; closeOverlay();
        const gained = Math.floor(Math.random()*6)+3;
        state.coins += gained;
        state.sessionCollected += gained;
        save(); render();
        alert('Event finished! You collected ' + gained + ' items. Bonus awarded.');
      } else {
        document.getElementById('overlayText').textContent = 'Event in progress — ' + time + 's left';
      }
    }, 1000);
  }

  // overlay
  function showOverlay(txt){
    const o = document.getElementById('overlay');
    if(!o){
      const ov = document.createElement('div');
      ov.id = 'overlay';
      ov.style.cssText = 'position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:rgba(0,0,0,0.35);z-index:9999';
      ov.innerHTML = `<div style="background:white;padding:18px;border-radius:12px;min-width:320px;text-align:center"><div id="overlayText" style="font-weight:800;color:#0b3b2a">${txt}</div><div style="margin-top:12px"><button class="btn ghost" onclick="closeOverlay()">Close</button></div></div>`;
      document.body.appendChild(ov);
    } else {
      document.getElementById('overlayText').textContent = txt;
      o.style.display = 'flex';
    }
  }
  function closeOverlay(){ document.getElementById('overlay')?.style.display = 'none'; }

  // leaderboard rendering
  function showLeaderboard(type){
    openScreen('score');
    const list = state.leaderboards[type] || [];
    const el = document.getElementById('leaderboardList');
    el.innerHTML = '';
    list.forEach((p,idx)=>{
      const div = document.createElement('div');
      div.style.display='flex'; div.style.justifyContent='space-between'; div.style.padding='8px'; div.style.borderRadius='8px'; div.style.marginBottom='6px';
      div.innerHTML = `<div><strong>${idx+1}. ${p.name}</strong><div class="mini">Score: ${p.score}</div></div><div style="align-self:center">${p.score}</div>`;
      el.appendChild(div);
    });
  }

  // profile editing
  function saveProfile(){
    const v = document.getElementById('nameEdit').value.trim();
    if(v){ state.name = v; save(); render(); showToast('Name updated'); }
  }

  // report
  function submitReport(){
    const t = document.getElementById('reportText').value.trim();
    if(!t) return alert('Describe the issue first.');
    alert('Report submitted. Thank you!');
    document.getElementById('reportText').value = '';
  }

  // withdraw simulation
  function withdraw(){
    if(state.coins < 100) return alert('Need 100 coins minimum to withdraw (simulated).');
    const amt = Math.floor(state.coins/100);
    if(!confirm('Convert ' + (amt*100) + ' coins to ' + amt + ' unit(s) of money?')) return;
    state.coins -= amt*100; save(); render();
    alert('Withdrawal simulated. Parental approval required for real payouts.');
  }

  // utils
  function showToast(txt){
    const d = document.createElement('div');
    d.textContent = txt;
    d.style.position='fixed'; d.style.right='20px'; d.style.bottom='22px';
    d.style.background='white'; d.style.padding='10px'; d.style.borderRadius='8px'; d.style.boxShadow='0 10px 30px rgba(0,0,0,0.12)';
    document.body.appendChild(d);
    setTimeout(()=>d.style.opacity=0,1200);
    setTimeout(()=>d.remove(),1800);
  }

  // init UI with state
  document.getElementById('nameEdit').value = state.name;
  render();

  // keyboard for demo
  window.addEventListener('keydown', (e)=>{
    if(e.key === 'ArrowUp') move('up');
    if(e.key === 'ArrowDown') move('down');
    if(e.key === 'ArrowLeft') move('left');
    if(e.key === 'ArrowRight') move('right');
    if(e.key === 'c') collectAction();
  });

  // accessibility: escape to home
  window.addEventListener('keydown', (e)=>{
    if(e.key === 'Escape') openScreen('home');
  });

</script>
</body>
</html>
