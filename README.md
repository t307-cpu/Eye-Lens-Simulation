<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>How the Eye Focuses Light</title>

  <style>
    :root {
      --navy: #17324d;
      --blue: #2f80ed;
      --blue-dark: #1766c2;
      --cyan: #55d6e9;
      --yellow: #ffd166;
      --green: #35b779;
      --red: #ef476f;
      --orange: #f8961e;
      --purple: #8a5cf6;
      --cream: #fffaf1;
      --panel: rgba(255, 255, 255, 0.94);
      --border: #dce8f2;
      --muted: #60758a;
      --shadow: 0 18px 45px rgba(26, 64, 96, 0.13);
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      font-family: Inter, ui-rounded, "Segoe UI", Arial, sans-serif;
      color: var(--navy);
      background:
        radial-gradient(circle at 15% 10%, #dff9ff 0, transparent 30%),
        radial-gradient(circle at 90% 20%, #ece5ff 0, transparent 34%),
        linear-gradient(135deg, #f8fcff, #fffaf0);
    }

    button,
    input {
      font: inherit;
    }

    .page {
      width: min(1200px, calc(100% - 28px));
      margin: 0 auto;
      padding: 28px 0 44px;
    }

    header {
      margin-bottom: 22px;
      text-align: center;
    }

    .eyebrow {
      display: inline-block;
      padding: 7px 13px;
      margin-bottom: 10px;
      border: 1px solid #cce8f1;
      border-radius: 999px;
      color: #16718b;
      background: rgba(255, 255, 255, 0.75);
      font-size: 0.82rem;
      font-weight: 800;
      letter-spacing: 0.08em;
      text-transform: uppercase;
    }

    h1 {
      margin: 0;
      font-size: clamp(2rem, 5vw, 3.5rem);
      line-height: 1.05;
      letter-spacing: -0.04em;
    }

    .subtitle {
      max-width: 770px;
      margin: 12px auto 0;
      color: var(--muted);
      font-size: clamp(1rem, 2vw, 1.14rem);
      line-height: 1.6;
    }

    .main-grid {
      display: grid;
      grid-template-columns: minmax(0, 1fr) 325px;
      gap: 20px;
      align-items: stretch;
    }

    .card {
      border: 1px solid rgba(200, 220, 235, 0.85);
      border-radius: 24px;
      background: var(--panel);
      box-shadow: var(--shadow);
      backdrop-filter: blur(8px);
    }

    .simulation-card {
      overflow: hidden;
    }

    .control-bar {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      justify-content: space-between;
      gap: 16px;
      padding: 18px 20px;
      border-bottom: 1px solid var(--border);
      background: linear-gradient(90deg, #f8fdff, #fffcf5);
    }

    .control-label {
      display: block;
      margin-bottom: 8px;
      color: var(--muted);
      font-size: 0.78rem;
      font-weight: 800;
      letter-spacing: 0.06em;
      text-transform: uppercase;
    }

    .choice-buttons {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    .choice-button,
    .action-button {
      border: 0;
      border-radius: 13px;
      cursor: pointer;
      font-weight: 800;
      transition:
        transform 160ms ease,
        box-shadow 160ms ease,
        background 160ms ease;
    }

    .choice-button {
      padding: 10px 14px;
      color: var(--navy);
      background: #edf4f9;
    }

    .choice-button:hover,
    .action-button:hover {
      transform: translateY(-1px);
    }

    .choice-button:focus-visible,
    .action-button:focus-visible,
    input:focus-visible + .switch-track,
    input[type="range"]:focus-visible {
      outline: 3px solid rgba(47, 128, 237, 0.35);
      outline-offset: 3px;
    }

    .choice-button.active {
      color: white;
      background: linear-gradient(135deg, var(--blue), var(--purple));
      box-shadow: 0 7px 18px rgba(47, 128, 237, 0.25);
    }

    .switch-row {
      display: flex;
      align-items: center;
      gap: 9px;
      font-size: 0.92rem;
      font-weight: 750;
    }

    .switch {
      position: relative;
      width: 48px;
      height: 27px;
      flex: 0 0 auto;
    }

    .switch input {
      width: 0;
      height: 0;
      opacity: 0;
    }

    .switch-track {
      position: absolute;
      inset: 0;
      border-radius: 999px;
      cursor: pointer;
      background: #bdcad5;
      transition: 180ms ease;
    }

    .switch-track::after {
      position: absolute;
      top: 4px;
      left: 4px;
      width: 19px;
      height: 19px;
      border-radius: 50%;
      background: white;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
      content: "";
      transition: 180ms ease;
    }

    .switch input:checked + .switch-track {
      background: var(--blue);
    }

    .switch input:checked + .switch-track::after {
      transform: translateX(21px);
    }

    .diagram-wrap {
      position: relative;
      padding: 8px 12px 0;
      background:
        linear-gradient(rgba(65, 137, 188, 0.04) 1px, transparent 1px),
        linear-gradient(90deg, rgba(65, 137, 188, 0.04) 1px, transparent 1px);
      background-size: 26px 26px;
    }

    svg {
      display: block;
      width: 100%;
      height: auto;
      min-height: 390px;
      overflow: visible;
    }

    .ray {
      fill: none;
      stroke: var(--yellow);
      stroke-width: 4;
      stroke-linecap: round;
      filter: drop-shadow(0 0 4px rgba(255, 193, 7, 0.75));
    }

    .ray-extension {
      fill: none;
      stroke: var(--orange);
      stroke-width: 2.5;
      stroke-dasharray: 7 7;
      opacity: 0.75;
    }

    #raysGroup,
    #outsideRays,
    #extensionGroup {
      transition: opacity 220ms ease;
    }

    .diagram-label {
      fill: var(--navy);
      font-size: 17px;
      font-weight: 800;
      paint-order: stroke;
      stroke: white;
      stroke-width: 4px;
      stroke-linejoin: round;
    }

    .small-label {
      fill: var(--muted);
      font-size: 14px;
      font-weight: 700;
      paint-order: stroke;
      stroke: white;
      stroke-width: 3px;
    }

    .focus-label {
      fill: var(--red);
      font-size: 15px;
      font-weight: 900;
      paint-order: stroke;
      stroke: white;
      stroke-width: 4px;
    }

    #lensShape,
    #lensHighlight,
    #focusPoint,
    #focusGlow {
      transition: 180ms ease;
    }

    .slider-section {
      padding: 18px 22px 22px;
      border-top: 1px solid var(--border);
      background: white;
    }

    .slider-heading {
      display: flex;
      justify-content: space-between;
      gap: 12px;
      align-items: flex-end;
      margin-bottom: 12px;
    }

    .slider-heading h2 {
      margin: 0;
      font-size: 1.08rem;
    }

    .value-badge {
      padding: 6px 11px;
      border-radius: 999px;
      color: #28556f;
      background: #e8f8fc;
      font-size: 0.86rem;
      font-weight: 850;
    }

    .slider-container {
      display: grid;
      grid-template-columns: auto minmax(160px, 1fr) auto;
      gap: 12px;
      align-items: center;
    }

    .slider-end {
      color: var(--muted);
      font-size: 0.82rem;
      font-weight: 800;
    }

    input[type="range"] {
      width: 100%;
      height: 12px;
      border-radius: 999px;
      outline: none;
      appearance: none;
      background: linear-gradient(
        to right,
        #8edce8 0%,
        var(--blue) var(--slider-progress, 50%),
        #dbe5ec var(--slider-progress, 50%),
        #dbe5ec 100%
      );
    }

    input[type="range"]::-webkit-slider-thumb {
      width: 26px;
      height: 26px;
      border: 4px solid white;
      border-radius: 50%;
      appearance: none;
      cursor: grab;
      background: var(--blue);
      box-shadow: 0 3px 10px rgba(30, 80, 130, 0.35);
    }

    input[type="range"]::-moz-range-thumb {
      width: 19px;
      height: 19px;
      border: 4px solid white;
      border-radius: 50%;
      cursor: grab;
      background: var(--blue);
      box-shadow: 0 3px 10px rgba(30, 80, 130, 0.35);
    }

    .side-panel {
      display: flex;
      flex-direction: column;
      gap: 16px;
    }

    .status-card,
    .lesson-card,
    .challenge-card {
      padding: 20px;
    }

    .status-icon {
      display: grid;
      width: 52px;
      height: 52px;
      margin-bottom: 13px;
      place-items: center;
      border-radius: 17px;
      background: #eef4ff;
      font-size: 1.65rem;
    }

    .status-card h2,
    .lesson-card h2,
    .challenge-card h2 {
      margin: 0 0 9px;
      font-size: 1.13rem;
    }

    #statusTitle {
      color: var(--orange);
    }

    #statusText,
    #conceptText,
    #challengeText {
      margin: 0;
      color: var(--muted);
      line-height: 1.52;
    }

    .retina-meter {
      position: relative;
      height: 11px;
      margin: 18px 0 9px;
      border-radius: 999px;
      background: linear-gradient(
        to right,
        #ffd6df 0 43%,
        #a7eccb 43% 57%,
        #ffd6df 57% 100%
      );
    }

    .retina-meter::after {
      position: absolute;
      top: -5px;
      left: 50%;
      width: 3px;
      height: 21px;
      border-radius: 2px;
      background: var(--green);
      content: "";
      transform: translateX(-50%);
    }

    #meterMarker {
      position: absolute;
      top: 50%;
      left: 50%;
      width: 20px;
      height: 20px;
      border: 3px solid white;
      border-radius: 50%;
      background: var(--red);
      box-shadow: 0 3px 8px rgba(40, 70, 100, 0.3);
      transform: translate(-50%, -50%);
      transition:
        left 180ms ease,
        background 180ms ease;
    }

    .meter-labels {
      display: flex;
      justify-content: space-between;
      color: var(--muted);
      font-size: 0.7rem;
      font-weight: 800;
    }

    .key-idea {
      margin-top: 15px;
      padding: 14px;
      border-left: 5px solid var(--purple);
      border-radius: 12px;
      background: #f5f1ff;
      line-height: 1.45;
    }

    .key-idea strong {
      color: #6035c4;
    }

    .action-button {
      width: 100%;
      margin-top: 14px;
      padding: 12px 14px;
      color: white;
      background: linear-gradient(135deg, var(--blue), var(--purple));
      box-shadow: 0 8px 18px rgba(47, 128, 237, 0.2);
    }

    .action-button.secondary {
      color: var(--navy);
      background: #edf4f9;
      box-shadow: none;
    }

    .success-flash {
      animation: successPulse 700ms ease;
    }

    @keyframes successPulse {
      0%,
      100% {
        transform: scale(1);
      }

      45% {
        transform: scale(1.035);
        box-shadow: 0 18px 45px rgba(53, 183, 121, 0.28);
      }
    }

    .summary-grid {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 16px;
      margin-top: 20px;
    }

    .summary-card {
      display: grid;
      grid-template-columns: 62px 1fr;
      gap: 14px;
      align-items: center;
      padding: 18px;
    }

    .summary-icon {
      display: grid;
      width: 62px;
      height: 62px;
      place-items: center;
      border-radius: 19px;
      font-size: 1.8rem;
    }

    .near-summary .summary-icon {
      background: #fff0d5;
    }

    .far-summary .summary-icon {
      background: #e7f4ff;
    }

    .summary-card h3 {
      margin: 0 0 5px;
      font-size: 1rem;
    }

    .summary-card p {
      margin: 0;
      color: var(--muted);
      line-height: 1.45;
      font-size: 0.92rem;
    }

    @media (max-width: 900px) {
      .main-grid {
        grid-template-columns: 1fr;
      }

      .side-panel {
        display: grid;
        grid-template-columns: repeat(2, minmax(0, 1fr));
      }

      .challenge-card {
        grid-column: 1 / -1;
      }
    }

    @media (max-width: 620px) {
      .page {
        width: min(100% - 16px, 1200px);
        padding-top: 18px;
      }

      .control-bar {
        align-items: flex-start;
        flex-direction: column;
      }

      .side-panel,
      .summary-grid {
        grid-template-columns: 1fr;
      }

      .challenge-card {
        grid-column: auto;
      }

      .slider-container {
        grid-template-columns: 1fr;
      }

      .slider-end {
        display: none;
      }

      svg {
        min-height: 300px;
      }
    }
  </style>
</head>

<body>
  <main class="page">
    <header>
      <div class="eyebrow">Interactive Grade 8 Science</div>

      <h1>How Does the Eye Focus Light?</h1>

      <p class="subtitle">
        Choose a near or distant object. Then change the thickness of the
        eye's lens until the light focuses exactly on the retina.
      </p>
    </header>

    <section class="main-grid">
      <article class="card simulation-card">
        <div class="control-bar">
          <div>
            <span class="control-label">Choose an object</span>

            <div
              class="choice-buttons"
              role="group"
              aria-label="Object distance"
            >
              <button
                id="nearButton"
                class="choice-button"
                type="button"
                aria-pressed="false"
              >
                📖 Near object
              </button>

              <button
                id="farButton"
                class="choice-button active"
                type="button"
                aria-pressed="true"
              >
                🏔️ Distant object
              </button>
            </div>
          </div>

          <div>
            <span class="control-label">Diagram options</span>

            <label class="switch-row">
              <span class="switch">
                <input id="rayToggle" type="checkbox" checked />
                <span class="switch-track"></span>
              </span>

              Show light rays
            </label>
          </div>
        </div>

        <div class="diagram-wrap">
          <svg
            id="eyeDiagram"
            viewBox="0 0 940 500"
            role="img"
            aria-labelledby="diagramTitle diagramDescription"
          >
            <title id="diagramTitle">
              Simplified eye lens focusing simulation
            </title>

            <desc id="diagramDescription">
              A circular eyeball contains an adjustable lens and a retina.
              Light rays pass through the lens and meet at a focal point.
            </desc>

            <defs>
              <radialGradient id="eyeballFill" cx="38%" cy="32%" r="75%">
                <stop offset="0%" stop-color="#ffffff" />
                <stop offset="65%" stop-color="#edf9ff" />
                <stop offset="100%" stop-color="#d8effb" />
              </radialGradient>

              <linearGradient id="lensFill" x1="0" y1="0" x2="1" y2="0">
                <stop offset="0%" stop-color="#49bad7" stop-opacity="0.92" />
                <stop offset="50%" stop-color="#d9fbff" stop-opacity="0.98" />
                <stop offset="100%" stop-color="#49bad7" stop-opacity="0.92" />
              </linearGradient>

              <radialGradient id="focusGradient">
                <stop offset="0%" stop-color="#ef476f" stop-opacity="0.7" />
                <stop offset="100%" stop-color="#ef476f" stop-opacity="0" />
              </radialGradient>

              <!-- Light inside the eye is clipped to the circular eyeball. -->
              <clipPath id="eyeballClip">
                <circle cx="680" cy="250" r="220" />
              </clipPath>

              <marker
                id="arrowhead"
                markerWidth="9"
                markerHeight="7"
                refX="8"
                refY="3.5"
                orient="auto"
              >
                <polygon points="0 0, 9 3.5, 0 7" fill="#f0b323" />
              </marker>
            </defs>

            <!-- Simplified distant object -->
            <g id="farObject">
              <circle cx="78" cy="105" r="25" fill="#ffd166" />

              <path
                d="M25 355 L115 235 L180 330 L235 270 L325 390 L25 390 Z"
                fill="#8ed3bf"
              />

              <path
                d="M115 235 L93 266 L115 258 L133 274 L147 272 Z"
                fill="white"
              />

              <text x="30" y="425" class="diagram-label">
                Distant object
              </text>

              <text x="30" y="448" class="small-label">
                Rays arrive almost parallel
              </text>
            </g>

            <!-- Simplified near object -->
            <g id="nearObject" style="display: none">
              <rect
                x="90"
                y="170"
                width="125"
                height="170"
                rx="10"
                fill="#ef476f"
              />

              <rect
                x="103"
                y="184"
                width="99"
                height="142"
                rx="5"
                fill="#fff7e8"
              />

              <line
                x1="122"
                y1="245"
                x2="184"
                y2="245"
                stroke="#95a7b7"
                stroke-width="5"
                stroke-linecap="round"
              />

              <line
                x1="122"
                y1="270"
                x2="174"
                y2="270"
                stroke="#95a7b7"
                stroke-width="5"
                stroke-linecap="round"
              />

              <text x="45" y="410" class="diagram-label">
                Near object
              </text>

              <text x="45" y="433" class="small-label">
                Rays spread out
              </text>
            </g>

            <text id="rayTypeLabel" x="260" y="125" class="diagram-label">
              Parallel light rays
            </text>

            <!-- Simple circular eyeball -->
            <circle
              cx="680"
              cy="250"
              r="220"
              fill="url(#eyeballFill)"
              stroke="#5da5c9"
              stroke-width="7"
            />

            <!-- Retina -->
            <path
              d="M840 135 Q900 250 840 365"
              fill="none"
              stroke="#ef476f"
              stroke-width="14"
              stroke-linecap="round"
            />

            <!-- Rays outside the eyeball -->
            <g id="outsideRays">
              <path
                id="outerTop"
                class="ray"
                marker-end="url(#arrowhead)"
              />

              <path
                id="outerMiddle"
                class="ray"
                marker-end="url(#arrowhead)"
              />

              <path
                id="outerBottom"
                class="ray"
                marker-end="url(#arrowhead)"
              />
            </g>

            <!-- Rays inside the eyeball -->
            <g id="raysGroup" clip-path="url(#eyeballClip)">
              <path
                id="incomingTop"
                class="ray"
                marker-end="url(#arrowhead)"
              />

              <path
                id="incomingMiddle"
                class="ray"
                marker-end="url(#arrowhead)"
              />

              <path
                id="incomingBottom"
                class="ray"
                marker-end="url(#arrowhead)"
              />

              <path id="outgoingTop" class="ray" />
              <path id="outgoingMiddle" class="ray" />
              <path id="outgoingBottom" class="ray" />
            </g>

            <!-- Adjustable lens -->
            <path
              id="lensShape"
              d=""
              fill="url(#lensFill)"
              stroke="#188aaa"
              stroke-width="4"
            />

            <path
              id="lensHighlight"
              d=""
              fill="none"
              stroke="white"
              stroke-width="5"
              stroke-linecap="round"
              opacity="0.75"
            />

            <!-- Dashed continuation shows where rays would meet -->
            <g id="extensionGroup">
              <path id="extensionTop" class="ray-extension" />
              <path id="extensionMiddle" class="ray-extension" />
              <path id="extensionBottom" class="ray-extension" />
            </g>

            <!-- Focus point -->
            <circle
              id="focusGlow"
              cx="870"
              cy="250"
              r="30"
              fill="url(#focusGradient)"
            />

            <circle
              id="focusPoint"
              cx="870"
              cy="250"
              r="9"
              fill="#ef476f"
              stroke="white"
              stroke-width="4"
            />

            <!-- Essential labels only -->
            <text x="535" y="115" class="diagram-label">Lens</text>

            <line
              x1="565"
              y1="123"
              x2="565"
              y2="165"
              stroke="#48667d"
              stroke-width="2"
            />

            <text x="817" y="95" class="diagram-label">Retina</text>

            <line
              x1="844"
              y1="103"
              x2="850"
              y2="135"
              stroke="#48667d"
              stroke-width="2"
            />

            <text
              id="focusLabel"
              x="870"
              y="220"
              text-anchor="middle"
              class="focus-label"
            >
              Focus point
            </text>
          </svg>
        </div>

        <div class="slider-section">
          <div class="slider-heading">
            <div>
              <span class="control-label">Adjust the eye</span>
              <h2>Change the lens thickness</h2>
            </div>

            <span id="thicknessBadge" class="value-badge">
              Thin lens
            </span>
          </div>

          <div class="slider-container">
            <span class="slider-end">Thinner</span>

            <input
              id="lensSlider"
              type="range"
              min="0"
              max="100"
              value="30"
              aria-label="Lens thickness"
            />

            <span class="slider-end">Thicker</span>
          </div>
        </div>
      </article>

      <aside class="side-panel">
        <section id="statusCard" class="card status-card" aria-live="polite">
          <div id="statusIcon" class="status-icon">🎯</div>

          <h2 id="statusTitle">Focused on the retina!</h2>

          <p id="statusText">
            The image would appear clear because the light meets on the retina.
          </p>

          <div class="retina-meter">
            <span id="meterMarker"></span>
          </div>

          <div class="meter-labels">
            <span>In front</span>
            <span>On retina</span>
            <span>Behind</span>
          </div>

          <button id="autoFocusButton" class="action-button" type="button">
            Show the correct lens
          </button>
        </section>

        <section class="card lesson-card">
          <h2>💡 What is happening?</h2>

          <p id="conceptText"></p>

          <div class="key-idea">
            <strong>Key idea:</strong>

            <span id="keyIdeaText">
              Far objects need a thinner, flatter lens.
            </span>
          </div>
        </section>

        <section class="card challenge-card">
          <h2>🧪 Focus challenge</h2>

          <p id="challengeText">
            Move the slider away from the correct position. Then try to focus
            the distant object on the retina again.
          </p>

          <button
            id="challengeButton"
            class="action-button secondary"
            type="button"
          >
            Start a challenge
          </button>
        </section>
      </aside>
    </section>

    <section class="summary-grid">
      <article class="card summary-card near-summary">
        <div class="summary-icon">📖</div>

        <div>
          <h3>Near object → thicker lens</h3>

          <p>
            Light from a near object is diverging. A thicker, rounder lens
            bends it more strongly so it focuses on the retina.
          </p>
        </div>
      </article>

      <article class="card summary-card far-summary">
        <div class="summary-icon">🏔️</div>

        <div>
          <h3>Distant object → thinner lens</h3>

          <p>
            Light from a distant object is almost parallel. A thinner, flatter
            lens provides enough bending to focus it on the retina.
          </p>
        </div>
      </article>
    </section>
  </main>

  <script>
    const nearButton = document.getElementById("nearButton");
    const farButton = document.getElementById("farButton");
    const rayToggle = document.getElementById("rayToggle");
    const lensSlider = document.getElementById("lensSlider");

    const nearObject = document.getElementById("nearObject");
    const farObject = document.getElementById("farObject");

    const lensShape = document.getElementById("lensShape");
    const lensHighlight = document.getElementById("lensHighlight");

    const raysGroup = document.getElementById("raysGroup");
    const outsideRays = document.getElementById("outsideRays");
    const extensionGroup = document.getElementById("extensionGroup");

    const outerTop = document.getElementById("outerTop");
    const outerMiddle = document.getElementById("outerMiddle");
    const outerBottom = document.getElementById("outerBottom");

    const incomingTop = document.getElementById("incomingTop");
    const incomingMiddle = document.getElementById("incomingMiddle");
    const incomingBottom = document.getElementById("incomingBottom");

    const outgoingTop = document.getElementById("outgoingTop");
    const outgoingMiddle = document.getElementById("outgoingMiddle");
    const outgoingBottom = document.getElementById("outgoingBottom");

    const extensionTop = document.getElementById("extensionTop");
    const extensionMiddle = document.getElementById("extensionMiddle");
    const extensionBottom = document.getElementById("extensionBottom");

    const focusPoint = document.getElementById("focusPoint");
    const focusGlow = document.getElementById("focusGlow");
    const focusLabel = document.getElementById("focusLabel");

    const rayTypeLabel = document.getElementById("rayTypeLabel");
    const thicknessBadge = document.getElementById("thicknessBadge");

    const statusCard = document.getElementById("statusCard");
    const statusIcon = document.getElementById("statusIcon");
    const statusTitle = document.getElementById("statusTitle");
    const statusText = document.getElementById("statusText");
    const meterMarker = document.getElementById("meterMarker");

    const conceptText = document.getElementById("conceptText");
    const keyIdeaText = document.getElementById("keyIdeaText");
    const challengeText = document.getElementById("challengeText");

    const autoFocusButton = document.getElementById("autoFocusButton");
    const challengeButton = document.getElementById("challengeButton");

    /*
      Important horizontal positions in the simplified diagram.
    */
    const eyeballLeftX = 460;
    const lensX = 565;
    const retinaX = 870;

    /*
      These are simplified teaching values rather than measurements
      of a real human eye.
    */
    const settings = {
      far: {
        idealThickness: 30,
        objectName: "distant object",
        rayType: "Parallel light rays"
      },

      near: {
        idealThickness: 75,
        objectName: "near object",
        rayType: "Diverging light rays"
      }
    };

    let mode = "far";

    function setMode(newMode) {
      mode = newMode;

      const isNear = mode === "near";

      nearButton.classList.toggle("active", isNear);
      farButton.classList.toggle("active", !isNear);

      nearButton.setAttribute("aria-pressed", String(isNear));
      farButton.setAttribute("aria-pressed", String(!isNear));

      nearObject.style.display = isNear ? "block" : "none";
      farObject.style.display = isNear ? "none" : "block";

      rayTypeLabel.textContent = settings[mode].rayType;

      if (isNear) {
        conceptText.textContent =
          "Light from a near object reaches the eye as diverging rays. " +
          "The lens must become thicker and rounder to bend the rays more strongly.";

        keyIdeaText.textContent =
          "Near objects need a thicker lens because their light rays are diverging.";

        challengeText.textContent =
          "Can you make the lens thick enough to focus the near object exactly on the retina?";
      } else {
        conceptText.textContent =
          "Light from a distant object reaches the eye as almost parallel rays. " +
          "The lens becomes thinner and flatter because less bending is needed.";

        keyIdeaText.textContent =
          "Far objects need a thinner lens because their light rays are almost parallel.";

        challengeText.textContent =
          "Can you make the lens thin enough to focus the distant object exactly on the retina?";
      }

      updateSimulation();
    }

    function updateSimulation() {
      const thickness = Number(lensSlider.value);
      const ideal = settings[mode].idealThickness;

      /*
        A lens that is too thin focuses behind the retina.
        A lens that is too thick focuses in front of the retina.
      */
      const focusX = clamp(
        retinaX + (ideal - thickness) * 3.1,
        680,
        935
      );

      updateLens(thickness);
      updateRays(focusX);
      updateFocusPoint(focusX);
      updateStatus(thickness, ideal, focusX);
      updateSliderAppearance(thickness);
    }

    function updateLens(thickness) {
      /*
        Increasing the horizontal half-width makes the lens
        look thicker and rounder.
      */
      const halfWidth = 12 + thickness * 0.25;
      const topY = 170;
      const bottomY = 330;

      const leftX = lensX - halfWidth;
      const rightX = lensX + halfWidth;

      const lensPath = `
        M ${lensX} ${topY}
        C ${leftX} ${topY + 30},
          ${leftX} ${bottomY - 30},
          ${lensX} ${bottomY}
        C ${rightX} ${bottomY - 30},
          ${rightX} ${topY + 30},
          ${lensX} ${topY}
        Z
      `;

      const highlightPath = `
        M ${lensX - halfWidth * 0.4} ${topY + 32}
        C ${lensX - halfWidth * 0.7} 220,
          ${lensX - halfWidth * 0.7} 265,
          ${lensX - halfWidth * 0.45} 296
      `;

      lensShape.setAttribute("d", lensPath);
      lensHighlight.setAttribute("d", highlightPath);

      if (thickness < 42) {
        thicknessBadge.textContent = "Thin lens";
      } else if (thickness < 65) {
        thicknessBadge.textContent = "Medium lens";
      } else {
        thicknessBadge.textContent = "Thick lens";
      }
    }

    function updateRays(focusX) {
      const topLensY = 210;
      const middleLensY = 250;
      const bottomLensY = 290;

      if (mode === "far") {
        /*
          Light from a distant object is represented
          by three parallel rays.
        */
        outerTop.setAttribute(
          "d",
          `M 135 ${topLensY} L ${eyeballLeftX} ${topLensY}`
        );

        outerMiddle.setAttribute(
          "d",
          `M 135 ${middleLensY} L ${eyeballLeftX} ${middleLensY}`
        );

        outerBottom.setAttribute(
          "d",
          `M 135 ${bottomLensY} L ${eyeballLeftX} ${bottomLensY}`
        );

        incomingTop.setAttribute(
          "d",
          `M ${eyeballLeftX} ${topLensY} L ${lensX} ${topLensY}`
        );

        incomingMiddle.setAttribute(
          "d",
          `M ${eyeballLeftX} ${middleLensY} L ${lensX} ${middleLensY}`
        );

        incomingBottom.setAttribute(
          "d",
          `M ${eyeballLeftX} ${bottomLensY} L ${lensX} ${bottomLensY}`
        );
      } else {
        /*
          Light from a near object begins at one point
          and spreads outward.
        */
        const sourceX = 165;
        const sourceY = 240;

        outerTop.setAttribute(
          "d",
          `M ${sourceX} ${sourceY} L ${eyeballLeftX} ${topLensY}`
        );

        outerMiddle.setAttribute(
          "d",
          `M ${sourceX} ${sourceY} L ${eyeballLeftX} ${middleLensY}`
        );

        outerBottom.setAttribute(
          "d",
          `M ${sourceX} ${sourceY} L ${eyeballLeftX} ${bottomLensY}`
        );

        incomingTop.setAttribute(
          "d",
          `M ${eyeballLeftX} ${topLensY} L ${lensX} ${topLensY}`
        );

        incomingMiddle.setAttribute(
          "d",
          `M ${eyeballLeftX} ${middleLensY} L ${lensX} ${middleLensY}`
        );

        incomingBottom.setAttribute(
          "d",
          `M ${eyeballLeftX} ${bottomLensY} L ${lensX} ${bottomLensY}`
        );
      }

      outgoingTop.setAttribute(
        "d",
        makeRayPath(lensX, topLensY, focusX, 250, 930)
      );

      outgoingMiddle.setAttribute(
        "d",
        makeRayPath(lensX, middleLensY, focusX, 250, 930)
      );

      outgoingBottom.setAttribute(
        "d",
        makeRayPath(lensX, bottomLensY, focusX, 250, 930)
      );

      extensionTop.setAttribute(
        "d",
        makeLineAfterRetina(lensX, topLensY, focusX, 250)
      );

      extensionMiddle.setAttribute(
        "d",
        makeLineAfterRetina(lensX, middleLensY, focusX, 250)
      );

      extensionBottom.setAttribute(
        "d",
        makeLineAfterRetina(lensX, bottomLensY, focusX, 250)
      );
    }

    function makeRayPath(x1, y1, focusX, focusY, endX) {
      if (focusX >= endX) {
        const endY = interpolateY(
          x1,
          y1,
          focusX,
          focusY,
          endX
        );

        return `M ${x1} ${y1} L ${endX} ${endY}`;
      }

      const endY = interpolateY(
        x1,
        y1,
        focusX,
        focusY,
        endX
      );

      return `
        M ${x1} ${y1}
        L ${focusX} ${focusY}
        L ${endX} ${endY}
      `;
    }

    function makeLineAfterRetina(x1, y1, focusX, focusY) {
      const startX = retinaX;
      const endX = 930;

      const startY = interpolateY(
        x1,
        y1,
        focusX,
        focusY,
        startX
      );

      const endY = interpolateY(
        x1,
        y1,
        focusX,
        focusY,
        endX
      );

      return `M ${startX} ${startY} L ${endX} ${endY}`;
    }

    function interpolateY(x1, y1, x2, y2, targetX) {
      if (x2 === x1) {
        return y2;
      }

      const ratio = (targetX - x1) / (x2 - x1);

      return y1 + ratio * (y2 - y1);
    }

    function updateFocusPoint(focusX) {
      focusPoint.setAttribute("cx", focusX);
      focusGlow.setAttribute("cx", focusX);

      const labelX = clamp(focusX, 690, 885);

      focusLabel.setAttribute("x", labelX);
    }

    function updateStatus(thickness, ideal, focusX) {
      const difference = thickness - ideal;
      const isFocused = Math.abs(difference) <= 3;

      /*
        Move the marker to show whether the focal point
        is in front of, on, or behind the retina.
      */
      let meterPosition = 50 + (focusX - retinaX) * 0.24;

      meterPosition = clamp(meterPosition, 4, 96);
      meterMarker.style.left = `${meterPosition}%`;

      if (isFocused) {
        statusIcon.textContent = "✅";
        statusTitle.textContent = "Focused on the retina!";
        statusTitle.style.color = "#238a5c";

        statusText.textContent =
          "The light rays meet on the retina, so the image would appear clear.";

        meterMarker.style.background = "#35b779";
        focusPoint.setAttribute("fill", "#35b779");
        focusGlow.style.fill = "rgba(53, 183, 121, 0.35)";
        focusLabel.style.fill = "#238a5c";
      } else if (difference > 0) {
        statusIcon.textContent = "↙️";
        statusTitle.textContent = "Focus is in front of the retina";
        statusTitle.style.color = "#e67e22";

        statusText.textContent =
          "The lens is too thick and bends the light too strongly. " +
          "Make the lens thinner.";

        meterMarker.style.background = "#ef476f";
        focusPoint.setAttribute("fill", "#ef476f");
        focusGlow.style.fill = "rgba(239, 71, 111, 0.35)";
        focusLabel.style.fill = "#ef476f";
      } else {
        statusIcon.textContent = "↘️";
        statusTitle.textContent = "Focus is behind the retina";
        statusTitle.style.color = "#e67e22";

        statusText.textContent =
          "The lens is too thin and does not bend the light enough. " +
          "Make the lens thicker.";

        meterMarker.style.background = "#ef476f";
        focusPoint.setAttribute("fill", "#ef476f");
        focusGlow.style.fill = "rgba(239, 71, 111, 0.35)";
        focusLabel.style.fill = "#ef476f";
      }
    }

    function updateSliderAppearance(thickness) {
      lensSlider.style.setProperty(
        "--slider-progress",
        `${thickness}%`
      );
    }

    function showCorrectLens() {
      lensSlider.value = settings[mode].idealThickness;

      updateSimulation();

      statusCard.classList.remove("success-flash");

      /*
        Reading offsetWidth restarts the animation.
      */
      void statusCard.offsetWidth;

      statusCard.classList.add("success-flash");
    }

    function startChallenge() {
      const ideal = settings[mode].idealThickness;
      let challengeValue;

      if (mode === "near") {
        challengeValue =
          Math.random() > 0.5
            ? randomBetween(5, 45)
            : randomBetween(88, 100);
      } else {
        challengeValue =
          Math.random() > 0.5
            ? randomBetween(55, 100)
            : randomBetween(0, 12);
      }

      if (Math.abs(challengeValue - ideal) < 15) {
        challengeValue = mode === "near" ? 25 : 85;
      }

      lensSlider.value = challengeValue;

      updateSimulation();
    }

    function toggleRays() {
      const opacity = rayToggle.checked ? "1" : "0";

      raysGroup.style.opacity = opacity;
      outsideRays.style.opacity = opacity;
      extensionGroup.style.opacity = opacity;
    }

    function randomBetween(min, max) {
      return Math.round(min + Math.random() * (max - min));
    }

    function clamp(value, min, max) {
      return Math.max(min, Math.min(max, value));
    }

    nearButton.addEventListener("click", () => setMode("near"));
    farButton.addEventListener("click", () => setMode("far"));

    lensSlider.addEventListener("input", updateSimulation);
    rayToggle.addEventListener("change", toggleRays);

    autoFocusButton.addEventListener("click", showCorrectLens);
    challengeButton.addEventListener("click", startChallenge);

    setMode("far");
    showCorrectLens();
  </script>
</body>
</html>
