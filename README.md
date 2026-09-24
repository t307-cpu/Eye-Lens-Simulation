<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />

  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  />

  <title>Ciliary Muscle and Accommodation Simulation</title>

  <style>
    :root {
      --page-bg: #eef5fb;
      --panel-bg: #ffffff;
      --text: #183047;
      --muted: #5d7184;

      --blue: #1887c9;
      --blue-dark: #075985;
      --light-blue: #d9f1ff;

      --muscle: #ef476f;
      --muscle-dark: #b42345;

      --ligament: #7c3aed;
      --lens: #f9d976;
      --lens-edge: #a66d00;

      --retina: #e76f51;
      --ray: #f4b400;

      --success: #198754;
      --border: #d5e2ec;
      --shadow: 0 12px 30px rgba(38, 74, 100, 0.12);
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      font-family:
        Inter, system-ui, -apple-system, BlinkMacSystemFont,
        "Segoe UI", sans-serif;
      color: var(--text);
      background:
        radial-gradient(circle at top left, #ffffff 0, transparent 35%),
        var(--page-bg);
    }

    button,
    input {
      font: inherit;
    }

    .page {
      width: min(1180px, calc(100% - 32px));
      margin: 0 auto;
      padding: 32px 0 50px;
    }

    .hero {
      margin-bottom: 22px;
      text-align: center;
    }

    .eyebrow {
      margin: 0 0 8px;
      color: var(--blue-dark);
      font-size: 0.82rem;
      font-weight: 800;
      letter-spacing: 0.11em;
      text-transform: uppercase;
    }

    h1 {
      margin: 0;
      font-size: clamp(1.9rem, 4vw, 3.1rem);
      line-height: 1.08;
    }

    .subtitle {
      max-width: 760px;
      margin: 12px auto 0;
      color: var(--muted);
      font-size: 1.04rem;
      line-height: 1.6;
    }

    .simulation-card {
      overflow: hidden;
      background: var(--panel-bg);
      border: 1px solid var(--border);
      border-radius: 24px;
      box-shadow: var(--shadow);
    }

    .simulation-heading {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 18px;
      padding: 20px 24px;
      border-bottom: 1px solid var(--border);
      background: linear-gradient(90deg, #f7fcff, #ffffff);
    }

    .simulation-heading h2 {
      margin: 0;
      font-size: 1.25rem;
    }

    .mode-badge {
      min-width: 160px;
      padding: 10px 16px;
      color: white;
      background: var(--blue-dark);
      border-radius: 999px;
      font-size: 0.9rem;
      font-weight: 800;
      text-align: center;
      transition:
        background-color 250ms ease,
        transform 250ms ease;
    }

    .visual-wrapper {
      position: relative;
      padding: 12px;
      background:
        linear-gradient(#ffffffdd, #ffffffdd),
        repeating-linear-gradient(
          0deg,
          transparent,
          transparent 24px,
          #e8f2f8 25px
        ),
        repeating-linear-gradient(
          90deg,
          transparent,
          transparent 24px,
          #e8f2f8 25px
        );
    }

    #eyeDiagram {
      display: block;
      width: 100%;
      min-height: 430px;
    }

    .eye-outline {
      fill: #f8fdff;
      stroke: #7ba8c4;
      stroke-width: 5;
    }

    .cornea {
      fill: #d8f5ff;
      fill-opacity: 0.8;
      stroke: var(--blue);
      stroke-width: 5;
    }

    .iris {
      stroke: #1d7c58;
      stroke-width: 15;
      stroke-linecap: round;
    }

    .pupil {
      fill: #15202b;
    }

    .retina {
      fill: none;
      stroke: var(--retina);
      stroke-width: 13;
      stroke-linecap: round;
    }

    .optic-nerve {
      fill: none;
      stroke: #e8a17f;
      stroke-width: 30;
      stroke-linecap: round;
    }

    .lens {
      fill: url(#lensGradient);
      stroke: var(--lens-edge);
      stroke-width: 4;
      filter: drop-shadow(0 5px 5px rgba(139, 93, 12, 0.22));
      transition: d 300ms ease;
    }

    .muscle {
      fill: var(--muscle);
      stroke: var(--muscle-dark);
      stroke-width: 4;
      transition:
        transform 300ms ease,
        opacity 300ms ease;
    }

    .ligament {
      fill: none;
      stroke: var(--ligament);
      stroke-width: 3.5;
      stroke-linecap: round;
      transition: d 300ms ease;
    }

    .light-ray {
      fill: none;
      stroke: var(--ray);
      stroke-width: 4;
      stroke-linecap: round;
      filter: drop-shadow(0 1px 2px rgba(110, 75, 0, 0.2));
    }

    .central-ray {
      stroke-dasharray: 10 8;
      opacity: 0.72;
    }

    .focus-dot {
      fill: #ffcc00;
      stroke: #a66d00;
      stroke-width: 3;
    }

    .label {
      fill: var(--text);
      font-size: 18px;
      font-weight: 800;
    }

    .small-label {
      fill: var(--muted);
      font-size: 15px;
      font-weight: 700;
    }

    .object-arrow {
      fill: var(--blue);
      stroke: var(--blue-dark);
      stroke-width: 3;
    }

    .object-ground {
      stroke: var(--blue-dark);
      stroke-width: 5;
      stroke-linecap: round;
    }

    .control-panel {
      display: grid;
      grid-template-columns: minmax(280px, 1.3fr) minmax(280px, 0.7fr);
      gap: 24px;
      padding: 24px;
      border-top: 1px solid var(--border);
    }

    .slider-area {
      padding: 20px;
      background: #f7fbfe;
      border: 1px solid var(--border);
      border-radius: 18px;
    }

    .slider-heading {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 14px;
      margin-bottom: 16px;
    }

    .slider-heading h3 {
      margin: 0;
      font-size: 1rem;
    }

    .slider-value {
      color: var(--blue-dark);
      font-weight: 900;
    }

    input[type="range"] {
      width: 100%;
      height: 9px;
      appearance: none;
      border-radius: 999px;
      outline: none;
      background:
        linear-gradient(
          to right,
          var(--blue) 0%,
          var(--blue) 50%,
          #cbd9e3 50%,
          #cbd9e3 100%
        );
    }

    input[type="range"]::-webkit-slider-thumb {
      width: 25px;
      height: 25px;
      appearance: none;
      cursor: grab;
      background: white;
      border: 7px solid var(--blue-dark);
      border-radius: 50%;
      box-shadow: 0 3px 9px rgba(0, 0, 0, 0.2);
    }

    input[type="range"]::-moz-range-thumb {
      width: 13px;
      height: 13px;
      cursor: grab;
      background: white;
      border: 7px solid var(--blue-dark);
      border-radius: 50%;
      box-shadow: 0 3px 9px rgba(0, 0, 0, 0.2);
    }

    .range-labels {
      display: flex;
      justify-content: space-between;
      margin-top: 10px;
      color: var(--muted);
      font-size: 0.84rem;
      font-weight: 800;
    }

    .preset-buttons {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 18px;
    }

    .preset-button {
      padding: 10px 15px;
      cursor: pointer;
      color: var(--blue-dark);
      background: white;
      border: 2px solid var(--blue);
      border-radius: 10px;
      font-weight: 800;
      transition:
        color 180ms ease,
        background-color 180ms ease,
        transform 180ms ease;
    }

    .preset-button:hover,
    .preset-button:focus-visible {
      color: white;
      background: var(--blue);
      transform: translateY(-2px);
    }

    .preset-button.active {
      color: white;
      background: var(--blue-dark);
      border-color: var(--blue-dark);
    }

    .status-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
    }

    .status-item {
      min-height: 92px;
      padding: 14px;
      background: white;
      border: 1px solid var(--border);
      border-radius: 14px;
    }

    .status-name {
      display: block;
      margin-bottom: 7px;
      color: var(--muted);
      font-size: 0.75rem;
      font-weight: 900;
      letter-spacing: 0.06em;
      text-transform: uppercase;
    }

    .status-value {
      display: block;
      color: var(--text);
      font-size: 0.96rem;
      font-weight: 900;
      line-height: 1.3;
    }

    .explanation {
      padding: 20px;
      background: #fffaf0;
      border: 1px solid #f2dc9b;
      border-radius: 18px;
    }

    .explanation h3 {
      margin: 0 0 10px;
      color: #805b00;
    }

    .explanation p {
      margin: 0;
      line-height: 1.6;
    }

    .important-note {
      margin-top: 14px !important;
      padding: 12px;
      color: #6a2459;
      background: #fff2fb;
      border-left: 5px solid var(--muscle);
      border-radius: 8px;
      font-size: 0.92rem;
    }

    .story-section {
      margin-top: 24px;
      padding: 24px;
      background: var(--panel-bg);
      border: 1px solid var(--border);
      border-radius: 22px;
      box-shadow: var(--shadow);
    }

    .story-section h2 {
      margin: 0 0 18px;
      text-align: center;
    }

    .story-flow {
      display: grid;
      grid-template-columns:
        minmax(145px, 1fr) 44px
        minmax(145px, 1fr) 44px
        minmax(145px, 1fr) 44px
        minmax(145px, 1fr);
      align-items: stretch;
      gap: 8px;
    }

    .story-step {
      display: flex;
      flex-direction: column;
      justify-content: center;
      min-height: 135px;
      padding: 16px;
      text-align: center;
      background: #f7fbfe;
      border: 2px solid var(--border);
      border-radius: 16px;
      transition:
        border-color 250ms ease,
        background-color 250ms ease,
        transform 250ms ease;
    }

    .story-step.active {
      background: #ebf8ff;
      border-color: var(--blue);
      transform: translateY(-3px);
    }

    .step-number {
      display: grid;
      width: 30px;
      height: 30px;
      margin: 0 auto 9px;
      place-items: center;
      color: white;
      background: var(--blue-dark);
      border-radius: 50%;
      font-size: 0.84rem;
      font-weight: 900;
    }

    .story-step strong {
      margin-bottom: 5px;
    }

    .story-step span:last-child {
      color: var(--muted);
      font-size: 0.88rem;
      line-height: 1.4;
    }

    .story-arrow {
      display: grid;
      place-items: center;
      color: var(--blue);
      font-size: 2rem;
      font-weight: 900;
    }

    .legend {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 12px 22px;
      margin-top: 20px;
      color: var(--muted);
      font-size: 0.86rem;
      font-weight: 700;
    }

    .legend-item {
      display: flex;
      align-items: center;
      gap: 7px;
    }

    .legend-color {
      width: 18px;
      height: 7px;
      border-radius: 999px;
    }

    .footer-note {
      max-width: 850px;
      margin: 20px auto 0;
      color: var(--muted);
      font-size: 0.84rem;
      line-height: 1.5;
      text-align: center;
    }

    @media (max-width: 850px) {
      .control-panel {
        grid-template-columns: 1fr;
      }

      .story-flow {
        grid-template-columns: 1fr;
      }

      .story-arrow {
        height: 34px;
        transform: rotate(90deg);
      }

      #eyeDiagram {
        min-height: 350px;
      }
    }

    @media (max-width: 560px) {
      .page {
        width: min(100% - 18px, 1180px);
        padding-top: 20px;
      }

      .simulation-heading {
        align-items: flex-start;
        flex-direction: column;
      }

      .mode-badge {
        min-width: 0;
      }

      .status-grid {
        grid-template-columns: 1fr;
      }

      .control-panel {
        padding: 14px;
      }

      .visual-wrapper {
        overflow-x: auto;
      }

      #eyeDiagram {
        width: 850px;
      }
    }

    @media (prefers-reduced-motion: reduce) {
      *,
      *::before,
      *::after {
        scroll-behavior: auto !important;
        transition-duration: 0.01ms !important;
      }
    }
  </style>
</head>

<body>
  <main class="page">
    <header class="hero">
      <p class="eyebrow">Interactive eye simulation</p>

      <h1>How the Eye Focuses</h1>

      <p class="subtitle">
        Move the slider to see how the ciliary muscles,
        suspensory ligaments, and lens work together to focus
        light from near and distant objects.
      </p>
    </header>

    <section class="simulation-card">
      <div class="simulation-heading">
        <h2>Accommodation inside the eye</h2>

        <div
          class="mode-badge"
          id="modeBadge"
          aria-live="polite"
        >
          Intermediate focus
        </div>
      </div>

      <div class="visual-wrapper">
        <svg
          id="eyeDiagram"
          viewBox="0 0 1000 520"
          role="img"
          aria-labelledby="diagramTitle diagramDescription"
        >
          <title id="diagramTitle">
            Eye accommodation simulation
          </title>

          <desc id="diagramDescription">
            An interactive diagram showing how the ciliary
            muscles alter ligament tension and lens shape to
            focus light on the retina.
          </desc>

          <defs>
            <linearGradient
              id="lensGradient"
              x1="0"
              y1="0"
              x2="1"
              y2="1"
            >
              <stop offset="0%" stop-color="#fff8bd" />
              <stop offset="55%" stop-color="#f9d976" />
              <stop offset="100%" stop-color="#e8a920" />
            </linearGradient>

            <marker
              id="rayArrow"
              markerWidth="9"
              markerHeight="9"
              refX="7"
              refY="3"
              orient="auto"
              markerUnits="strokeWidth"
            >
              <path
                d="M0,0 L0,6 L8,3 z"
                fill="#f4b400"
              />
            </marker>

            <filter id="softShadow">
              <feDropShadow
                dx="0"
                dy="4"
                stdDeviation="4"
                flood-opacity="0.18"
              />
            </filter>
          </defs>

          <!-- Object -->
          <g id="objectGroup" filter="url(#softShadow)">
            <line
              id="objectGround"
              class="object-ground"
              x1="55"
              y1="395"
              x2="145"
              y2="395"
            />

            <path
              id="objectArrow"
              class="object-arrow"
              d=""
            />

            <text
              id="objectLabel"
              class="label"
              x="100"
              y="430"
              text-anchor="middle"
            >
              Object
            </text>
          </g>

          <!-- Optic nerve behind the eye -->
          <path
            class="optic-nerve"
            d="M887 284 C927 286, 951 301, 985 330"
          />

          <!-- Eyeball -->
          <path
            class="eye-outline"
            d="
              M305 105
              C390 55, 570 42, 735 90
              C850 123, 918 185, 925 260
              C918 335, 850 397, 735 430
              C570 478, 390 465, 305 415
              C270 365, 252 316, 250 260
              C252 204, 270 155, 305 105
              Z
            "
          />

          <!-- Cornea -->
          <path
            class="cornea"
            d="
              M306 105
              C244 132, 218 194, 220 260
              C218 326, 244 388, 306 415
              C272 365, 255 316, 253 260
              C255 204, 272 155, 306 105
              Z
            "
          />

          <!-- Iris -->
          <path
            class="iris"
            d="M335 158 C320 195, 318 218, 320 236"
          />

          <path
            class="iris"
            d="M335 362 C320 325, 318 302, 320 284"
          />

          <!-- Pupil indicator -->
          <ellipse
            class="pupil"
            cx="321"
            cy="260"
            rx="7"
            ry="24"
          />

          <!-- Retina -->
          <path
            class="retina"
            d="M815 128 C884 166, 907 214, 909 260"
          />

          <path
            class="retina"
            d="M909 260 C907 306, 884 354, 815 392"
          />

          <!-- Light rays: placed behind the lens -->
          <g id="rayGroup">
            <path
              id="upperRay"
              class="light-ray"
              marker-end="url(#rayArrow)"
              d=""
            />

            <path
              id="middleRay"
              class="light-ray central-ray"
              marker-end="url(#rayArrow)"
              d=""
            />

            <path
              id="lowerRay"
              class="light-ray"
              marker-end="url(#rayArrow)"
              d=""
            />
          </g>

          <!-- Ciliary muscles -->
          <g id="muscleGroup">
            <path
              id="upperLeftMuscle"
              class="muscle"
              d=""
            />

            <path
              id="upperRightMuscle"
              class="muscle"
              d=""
            />

            <path
              id="lowerLeftMuscle"
              class="muscle"
              d=""
            />

            <path
              id="lowerRightMuscle"
              class="muscle"
              d=""
            />
          </g>

          <!-- Suspensory ligaments -->
          <g id="ligamentGroup">
            <path
              id="upperLeftLigament"
              class="ligament"
              d=""
            />

            <path
              id="upperRightLigament"
              class="ligament"
              d=""
            />

            <path
              id="lowerLeftLigament"
              class="ligament"
              d=""
            />

            <path
              id="lowerRightLigament"
              class="ligament"
              d=""
            />
          </g>

          <!-- Lens -->
          <path
            id="lens"
            class="lens"
            d=""
          />

          <!-- Focus point -->
          <circle
            id="focusDot"
            class="focus-dot"
            cx="897"
            cy="260"
            r="8"
          />

          <!-- Diagram labels -->
          <text
            class="small-label"
            x="500"
            y="49"
            text-anchor="middle"
          >
            Ciliary muscle
          </text>

          <line
            x1="500"
            y1="58"
            x2="500"
            y2="103"
            stroke="#5d7184"
            stroke-width="2"
          />

          <text
            class="small-label"
            x="500"
            y="492"
            text-anchor="middle"
          >
            Suspensory ligaments
          </text>

          <line
            x1="500"
            y1="475"
            x2="500"
            y2="420"
            stroke="#5d7184"
            stroke-width="2"
          />

          <text
            class="label"
            x="500"
            y="267"
            text-anchor="middle"
          >
            Lens
          </text>

          <text
            class="small-label"
            x="871"
            y="103"
            text-anchor="middle"
          >
            Retina
          </text>

          <line
            x1="871"
            y1="111"
            x2="850"
            y2="143"
            stroke="#5d7184"
            stroke-width="2"
          />

          <text
            class="small-label"
            x="246"
            y="82"
            text-anchor="middle"
          >
            Cornea
          </text>

          <line
            x1="246"
            y1="90"
            x2="251"
            y2="139"
            stroke="#5d7184"
            stroke-width="2"
          />
        </svg>
      </div>

      <div class="control-panel">
        <div>
          <div class="slider-area">
            <div class="slider-heading">
              <h3>Move the object</h3>

              <span
                class="slider-value"
                id="sliderValue"
              >
                50% near
              </span>
            </div>

            <label
              for="distanceSlider"
              class="sr-only"
            >
              Change the viewing distance
            </label>

            <input
              id="distanceSlider"
              type="range"
              min="0"
              max="100"
              value="50"
              step="1"
              aria-valuetext="Intermediate viewing distance"
            />

            <div class="range-labels">
              <span>Far away</span>
              <span>Very near</span>
            </div>

            <div class="preset-buttons">
              <button
                type="button"
                class="preset-button"
                data-value="0"
              >
                Distant object
              </button>

              <button
                type="button"
                class="preset-button active"
                data-value="50"
              >
                Intermediate
              </button>

              <button
                type="button"
                class="preset-button"
                data-value="100"
              >
                Near object
              </button>
            </div>
          </div>

          <div
            class="status-grid"
            style="margin-top: 16px"
          >
            <div class="status-item">
              <span class="status-name">Ciliary muscle</span>
              <span
                class="status-value"
                id="muscleStatus"
              >
                Partly contracted
              </span>
            </div>

            <div class="status-item">
              <span class="status-name">
                Suspensory ligaments
              </span>
              <span
                class="status-value"
                id="ligamentStatus"
              >
                Moderate tension
              </span>
            </div>

            <div class="status-item">
              <span class="status-name">Lens shape</span>
              <span
                class="status-value"
                id="lensStatus"
              >
                Moderately rounded
              </span>
            </div>

            <div class="status-item">
              <span class="status-name">Light bending</span>
              <span
                class="status-value"
                id="lightStatus"
              >
                Moderate
              </span>
            </div>
          </div>
        </div>

        <aside class="explanation">
          <h3 id="explanationTitle">
            Intermediate focus
          </h3>

          <p id="explanationText">
            The ciliary muscles are partly contracted. The
            ligaments have moderate tension, so the lens has an
            intermediate shape.
          </p>

          <p class="important-note">
            <strong>Remember:</strong> ciliary muscle contraction
            does not pull the lens flatter. It reduces tension in
            the ligaments, allowing the elastic lens to become
            rounder.
          </p>
        </aside>
      </div>
    </section>

    <section class="story-section">
      <h2 id="storyHeading">
        The focusing sequence
      </h2>

      <div class="story-flow">
        <div class="story-step active">
          <span class="step-number">1</span>
          <strong id="storyMuscle">
            Muscle partly contracts
          </strong>
          <span>The ciliary muscle starts the change.</span>
        </div>

        <div class="story-arrow" aria-hidden="true">→</div>

        <div class="story-step active">
          <span class="step-number">2</span>
          <strong id="storyLigament">
            Ligament tension decreases
          </strong>
          <span>
            The suspensory ligaments respond to the muscle.
          </span>
        </div>

        <div class="story-arrow" aria-hidden="true">→</div>

        <div class="story-step active">
          <span class="step-number">3</span>
          <strong id="storyLens">
            Lens becomes rounder
          </strong>
          <span>
            The elastic lens changes its curvature.
          </span>
        </div>

        <div class="story-arrow" aria-hidden="true">→</div>

        <div class="story-step active">
          <span class="step-number">4</span>
          <strong id="storyLight">
            Light bends more
          </strong>
          <span>
            The image remains focused on the retina.
          </span>
        </div>
      </div>

      <div class="legend">
        <div class="legend-item">
          <span
            class="legend-color"
            style="background: var(--muscle)"
          ></span>
          Ciliary muscle
        </div>

        <div class="legend-item">
          <span
            class="legend-color"
            style="background: var(--ligament)"
          ></span>
          Suspensory ligaments
        </div>

        <div class="legend-item">
          <span
            class="legend-color"
            style="background: var(--lens)"
          ></span>
          Lens
        </div>

        <div class="legend-item">
          <span
            class="legend-color"
            style="background: var(--ray)"
          ></span>
          Light rays
        </div>

        <div class="legend-item">
          <span
            class="legend-color"
            style="background: var(--retina)"
          ></span>
          Retina
        </div>
      </div>
    </section>

    <p class="footer-note">
      This is a simplified teaching model. It exaggerates some
      movements so that the relationship between the ciliary
      muscle, suspensory ligaments, and lens is easy to see.
    </p>
  </main>

  <style>
    /* Visually hidden but available to screen readers */
    .sr-only {
      position: absolute;
      width: 1px;
      height: 1px;
      padding: 0;
      margin: -1px;
      overflow: hidden;
      clip: rect(0, 0, 0, 0);
      white-space: nowrap;
      border: 0;
    }
  </style>

  <script>
    "use strict";

    const slider = document.getElementById("distanceSlider");
    const sliderValue = document.getElementById("sliderValue");
    const modeBadge = document.getElementById("modeBadge");

    const lens = document.getElementById("lens");
    const focusDot = document.getElementById("focusDot");

    const upperLeftMuscle =
      document.getElementById("upperLeftMuscle");
    const upperRightMuscle =
      document.getElementById("upperRightMuscle");
    const lowerLeftMuscle =
      document.getElementById("lowerLeftMuscle");
    const lowerRightMuscle =
      document.getElementById("lowerRightMuscle");

    const upperLeftLigament =
      document.getElementById("upperLeftLigament");
    const upperRightLigament =
      document.getElementById("upperRightLigament");
    const lowerLeftLigament =
      document.getElementById("lowerLeftLigament");
    const lowerRightLigament =
      document.getElementById("lowerRightLigament");

    const upperRay = document.getElementById("upperRay");
    const middleRay = document.getElementById("middleRay");
    const lowerRay = document.getElementById("lowerRay");

    const objectArrow = document.getElementById("objectArrow");
    const objectGround = document.getElementById("objectGround");
    const objectLabel = document.getElementById("objectLabel");

    const muscleStatus =
      document.getElementById("muscleStatus");
    const ligamentStatus =
      document.getElementById("ligamentStatus");
    const lensStatus =
      document.getElementById("lensStatus");
    const lightStatus =
      document.getElementById("lightStatus");

    const explanationTitle =
      document.getElementById("explanationTitle");
    const explanationText =
      document.getElementById("explanationText");

    const storyMuscle =
      document.getElementById("storyMuscle");
    const storyLigament =
      document.getElementById("storyLigament");
    const storyLens =
      document.getElementById("storyLens");
    const storyLight =
      document.getElementById("storyLight");

    const presetButtons =
      document.querySelectorAll(".preset-button");

    const centerX = 500;
    const centerY = 260;
    const retinaX = 897;

    /**
     * Returns a number between start and end.
     */
    function lerp(start, end, amount) {
      return start + (end - start) * amount;
    }

    /**
     * Rounds a number for cleaner SVG path values.
     */
    function round(value) {
      return Math.round(value * 10) / 10;
    }

    /**
     * Creates the outline of the lens.
     *
     * A wider horizontal radius represents a thicker,
     * more rounded lens.
     */
    function createLensPath(radiusX, radiusY) {
      const cx = centerX;
      const cy = centerY;

      const topY = cy - radiusY;
      const bottomY = cy + radiusY;

      return `
        M ${cx} ${round(topY)}
        C ${round(cx - radiusX)} ${round(cy - radiusY * 0.62)},
          ${round(cx - radiusX)} ${round(cy + radiusY * 0.62)},
          ${cx} ${round(bottomY)}
        C ${round(cx + radiusX)} ${round(cy + radiusY * 0.62)},
          ${round(cx + radiusX)} ${round(cy - radiusY * 0.62)},
          ${cx} ${round(topY)}
        Z
      `;
    }

    /**
     * Creates one of the four pink ciliary-muscle sections.
     */
    function createMusclePath(x1, y1, x2, y2, thickness) {
      const curve = thickness * 0.55;

      return `
        M ${round(x1)} ${round(y1)}
        Q ${round((x1 + x2) / 2)}
          ${round(y1 - curve)}
          ${round(x2)} ${round(y2)}
        Q ${round((x1 + x2) / 2)}
          ${round(y2 + thickness)}
          ${round(x1)} ${round(y1)}
        Z
      `;
    }

    /**
     * Creates a ligament.
     *
     * When slack is high, the path curves visibly.
     * When slack is low, the path is almost straight.
     */
    function createLigamentPath(
      startX,
      startY,
      endX,
      endY,
      slack,
      direction
    ) {
      const midX = (startX + endX) / 2;
      const midY =
        (startY + endY) / 2 +
        slack * 18 * direction;

      return `
        M ${round(startX)} ${round(startY)}
        Q ${round(midX)} ${round(midY)}
          ${round(endX)} ${round(endY)}
      `;
    }

    /**
     * Draws the object arrow.
     */
    function createObjectArrow(x, baseY, height) {
      const topY = baseY - height;
      const shaftWidth = 15;
      const headWidth = 34;
      const headHeight = 40;

      return `
        M ${round(x - shaftWidth / 2)} ${baseY}
        L ${round(x - shaftWidth / 2)}
          ${round(topY + headHeight)}
        L ${round(x - headWidth)} ${round(topY + headHeight)}
        L ${x} ${topY}
        L ${round(x + headWidth)} ${round(topY + headHeight)}
        L ${round(x + shaftWidth / 2)}
          ${round(topY + headHeight)}
        L ${round(x + shaftWidth / 2)} ${baseY}
        Z
      `;
    }

    /**
     * Updates all labels and explanatory text.
     */
    function updateText(value) {
      let mode;

      if (value <= 25) {
        mode = "distant";
      } else if (value >= 75) {
        mode = "near";
      } else {
        mode = "intermediate";
      }

      sliderValue.textContent = `${value}% near`;

      if (mode === "distant") {
        modeBadge.textContent = "Focusing on a distant object";
        modeBadge.style.backgroundColor = "#075985";

        muscleStatus.textContent = "Relaxed";
        ligamentStatus.textContent = "Tight";
        lensStatus.textContent = "Thin and flatter";
        lightStatus.textContent = "Less refraction";

        explanationTitle.textContent = "Distant vision";

        explanationText.textContent =
          "For a distant object, the ciliary muscles relax. " +
          "This increases tension in the suspensory ligaments. " +
          "The ligaments pull the lens into a thinner, flatter " +
          "shape, so the lens bends light less.";

        storyMuscle.textContent = "Ciliary muscle relaxes";
        storyLigament.textContent = "Ligaments become tight";
        storyLens.textContent = "Lens becomes thinner";
        storyLight.textContent = "Light bends less";

        slider.setAttribute(
          "aria-valuetext",
          "Focusing on a distant object"
        );
      } else if (mode === "near") {
        modeBadge.textContent = "Focusing on a near object";
        modeBadge.style.backgroundColor = "#b42345";

        muscleStatus.textContent = "Contracted";
        ligamentStatus.textContent = "Loose";
        lensStatus.textContent = "Thick and rounded";
        lightStatus.textContent = "More refraction";

        explanationTitle.textContent = "Near vision";

        explanationText.textContent =
          "For a near object, the ciliary muscles contract. " +
          "This reduces tension in the suspensory ligaments. " +
          "The elastic lens becomes thicker and rounder, so it " +
          "bends the strongly diverging light rays more.";

        storyMuscle.textContent = "Ciliary muscle contracts";
        storyLigament.textContent = "Ligaments become loose";
        storyLens.textContent = "Lens becomes rounder";
        storyLight.textContent = "Light bends more";

        slider.setAttribute(
          "aria-valuetext",
          "Focusing on a near object"
        );
      } else {
        modeBadge.textContent = "Intermediate focus";
        modeBadge.style.backgroundColor = "#6d4ba0";

        muscleStatus.textContent = "Partly contracted";
        ligamentStatus.textContent = "Moderate tension";
        lensStatus.textContent = "Moderately rounded";
        lightStatus.textContent = "Moderate refraction";

        explanationTitle.textContent = "Intermediate focus";

        explanationText.textContent =
          "The ciliary muscles are partly contracted. The " +
          "suspensory ligaments have moderate tension, so the " +
          "lens has an intermediate curvature and bends light " +
          "by a moderate amount.";

        storyMuscle.textContent =
          "Muscle partly contracts";
        storyLigament.textContent =
          "Ligament tension decreases";
        storyLens.textContent = "Lens becomes rounder";
        storyLight.textContent = "Light bends more";

        slider.setAttribute(
          "aria-valuetext",
          "Intermediate viewing distance"
        );
      }
    }

    /**
     * Updates the active preset button.
     */
    function updateButtons(value) {
      presetButtons.forEach((button) => {
        const buttonValue = Number(button.dataset.value);
        const isActive = Math.abs(buttonValue - value) < 3;

        button.classList.toggle("active", isActive);
      });
    }

    /**
     * Updates the slider's colored track.
     */
    function updateSliderBackground(value) {
      slider.style.background = `
        linear-gradient(
          to right,
          #1887c9 0%,
          #1887c9 ${value}%,
          #cbd9e3 ${value}%,
          #cbd9e3 100%
        )
      `;
    }

    /**
     * Main function that redraws the simulation.
     */
    function updateSimulation() {
      const value = Number(slider.value);

      // 0 = distant vision, 1 = near vision
      const nearAmount = value / 100;

      /*
       * LENS
       *
       * Near vision produces a thicker and more rounded lens.
       */
      const lensRadiusX = lerp(27, 55, nearAmount);
      const lensRadiusY = lerp(82, 98, nearAmount);

      lens.setAttribute(
        "d",
        createLensPath(lensRadiusX, lensRadiusY)
      );

      /*
       * CILIARY MUSCLES
       *
       * They move inward and become visually thicker as
       * contraction increases.
       */
      const inwardMovement = lerp(0, 19, nearAmount);
      const muscleThickness = lerp(15, 28, nearAmount);

      const leftInnerX = 450 + inwardMovement;
      const rightInnerX = 550 - inwardMovement;

      const upperY = lerp(128, 147, nearAmount);
      const lowerY = lerp(392, 373, nearAmount);

      upperLeftMuscle.setAttribute(
        "d",
        createMusclePath(
          370,
          126,
          leftInnerX,
          upperY,
          muscleThickness
        )
      );

      upperRightMuscle.setAttribute(
        "d",
        createMusclePath(
          630,
          126,
          rightInnerX,
          upperY,
          muscleThickness
        )
      );

      lowerLeftMuscle.setAttribute(
        "d",
        createMusclePath(
          370,
          394,
          leftInnerX,
          lowerY,
          -muscleThickness
        )
      );

      lowerRightMuscle.setAttribute(
        "d",
        createMusclePath(
          630,
          394,
          rightInnerX,
          lowerY,
          -muscleThickness
        )
      );

      /*
       * SUSPENSORY LIGAMENTS
       *
       * More contraction means more slack.
       */
      const slack = nearAmount;

      const upperLensY =
        centerY - lensRadiusY * 0.53;
      const lowerLensY =
        centerY + lensRadiusY * 0.53;

      const leftLensX =
        centerX - lensRadiusX * 0.76;
      const rightLensX =
        centerX + lensRadiusX * 0.76;

      upperLeftLigament.setAttribute(
        "d",
        createLigamentPath(
          leftInnerX - 3,
          upperY + 10,
          leftLensX,
          upperLensY,
          slack,
          1
        )
      );

      upperRightLigament.setAttribute(
        "d",
        createLigamentPath(
          rightInnerX + 3,
          upperY + 10,
          rightLensX,
          upperLensY,
          slack,
          1
        )
      );

      lowerLeftLigament.setAttribute(
        "d",
        createLigamentPath(
          leftInnerX - 3,
          lowerY - 10,
          leftLensX,
          lowerLensY,
          slack,
          -1
        )
      );

      lowerRightLigament.setAttribute(
        "d",
        createLigamentPath(
          rightInnerX + 3,
          lowerY - 10,
          rightLensX,
          lowerLensY,
          slack,
          -1
        )
      );

      /*
       * Make loose ligaments look less rigid by changing
       * their dash pattern slightly.
       */
      const dashSize = lerp(0, 7, nearAmount);
      const gapSize = lerp(0, 4, nearAmount);

      if (nearAmount > 0.65) {
        document
          .querySelectorAll(".ligament")
          .forEach((item) => {
            item.style.strokeDasharray =
              `${dashSize} ${gapSize}`;
          });
      } else {
        document
          .querySelectorAll(".ligament")
          .forEach((item) => {
            item.style.strokeDasharray = "none";
          });
      }

      /*
       * OBJECT
       *
       * The object moves closer to the eye as the slider
       * moves toward near vision.
       */
      const objectX = lerp(72, 188, nearAmount);
      const objectHeight = lerp(130, 185, nearAmount);
      const objectBaseY = 395;
      const objectTopY = objectBaseY - objectHeight;

      objectArrow.setAttribute(
        "d",
        createObjectArrow(
          objectX,
          objectBaseY,
          objectHeight
        )
      );

      objectGround.setAttribute("x1", objectX - 44);
      objectGround.setAttribute("x2", objectX + 44);

      objectLabel.setAttribute("x", objectX);

      /*
       * LIGHT RAYS
       *
       * The incoming rays spread more when the object is near.
       * After passing through the lens, all rays meet on the
       * retina.
       */
      const upperLensPointY = lerp(215, 194, nearAmount);
      const lowerLensPointY = lerp(305, 326, nearAmount);

      const lensFrontX = centerX - lensRadiusX * 0.72;
      const lensBackX = centerX + lensRadiusX * 0.72;

      const focusY = centerY;

      upperRay.setAttribute(
        "d",
        `
          M ${round(objectX)} ${round(objectTopY)}
          L ${round(lensFrontX)}
            ${round(upperLensPointY)}
          Q ${centerX} ${round(upperLensPointY + 2)}
            ${round(lensBackX)}
            ${round(upperLensPointY + 7)}
          L ${retinaX} ${focusY}
        `
      );

      middleRay.setAttribute(
        "d",
        `
          M ${round(objectX)} ${round(objectTopY)}
          L ${centerX} ${centerY}
          L ${retinaX} ${focusY}
        `
      );

      lowerRay.setAttribute(
        "d",
        `
          M ${round(objectX)} ${round(objectTopY)}
          L ${round(lensFrontX)}
            ${round(lowerLensPointY)}
          Q ${centerX} ${round(lowerLensPointY - 2)}
            ${round(lensBackX)}
            ${round(lowerLensPointY - 7)}
          L ${retinaX} ${focusY}
        `
      );

      focusDot.setAttribute("cx", retinaX);
      focusDot.setAttribute("cy", focusY);

      updateText(value);
      updateButtons(value);
      updateSliderBackground(value);
    }

    /*
     * Slider interaction
     */
    slider.addEventListener("input", updateSimulation);

    /*
     * Preset button interaction
     */
    presetButtons.forEach((button) => {
      button.addEventListener("click", () => {
        slider.value = button.dataset.value;
        updateSimulation();
      });
    });

    /*
     * Keyboard shortcuts:
     * D = distant
     * I = intermediate
     * N = near
     */
    document.addEventListener("keydown", (event) => {
      const key = event.key.toLowerCase();

      if (
        document.activeElement &&
        document.activeElement.tagName === "INPUT"
      ) {
        return;
      }

      if (key === "d") {
        slider.value = 0;
        updateSimulation();
      }

      if (key === "i") {
        slider.value = 50;
        updateSimulation();
      }

      if (key === "n") {
        slider.value = 100;
        updateSimulation();
      }
    });

    // Draw the initial state.
    updateSimulation();
  </script>
</body>
</html>
