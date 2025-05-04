<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Simulador Quirúrgico - Agujero Macular</title>
  <link rel="stylesheet" href="SimuladorOE.css">
  <link rel="stylesheet" href="alerts.css">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/3.2.1/anime.min.js"></script>
  <style>
    
/* Estilos base */
body {
    margin: 0;
    padding: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #0f172a;
    color: white;
    overflow: hidden;
    touch-action: none;
    height: 100vh;
    width: 100vw;
  }
  
  #container {
    position: relative;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
  }
  
  /* Caso clínico */
  #clinical-case {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.9);
    z-index: 100;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: opacity 0.5s;
  }
  
  #clinical-case-content {
    max-width: 800px;
    padding: 20px;
    background-color: #1e293b;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
  }
  
  #clinical-case h2 {
    color: #38bdf8;
    margin-top: 0;
  }
  
  #clinical-case button {
    background-color: #38bdf8;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    font-size: 1rem;
    cursor: pointer;
    margin-top: 20px;
    transition: background-color 0.3s;
  }
  
  #clinical-case button:hover {
    background-color: #0ea5e9;
  }
  
  /* Sistema de alertas */
  #alert-system {
    position: absolute;
    top: 20px;
    right: 20px;
    z-index: 50;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  
  .alert-container {
    background-color: #1e293b;
    border-left: 5px solid;
    padding: 15px;
    border-radius: 5px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    display: none;
    max-width: 350px;
    animation: slideIn 0.3s ease-out;
    transform: translateY(0);
    opacity: 1;
  }
  
  .alert-icon {
    font-size: 1.5rem;
    margin-right: 10px;
  }
  
  .alert-content {
    flex: 1;
  }
  
  .alert-content h3 {
    margin: 0 0 5px 0;
    font-size: 1rem;
  }
  
  .alert-content p {
    margin: 0;
    font-size: 0.9rem;
    color: #e2e8f0;
  }
  
  .alert-timer {
    height: 3px;
    background-color: rgba(255, 255, 255, 0.3);
    margin-top: 10px;
    border-radius: 3px;
    overflow: hidden;
  }
  
  .alert-dismiss {
    background: none;
    border: none;
    color: white;
    font-size: 1.2rem;
    cursor: pointer;
    padding: 0;
    margin-left: 10px;
  }
  
  /* Mini mapa */
  #miniMapContainer {
    position: absolute;
    top: 20px;
    left: 20px;
    width: 200px;
    height: 150px;
    background-color: #1e293b;
    border-radius: 10px;
    overflow: hidden;
    z-index: 20;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
  }
  
  /* Panel de instrumentos */
  .instrument-panel {
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    gap: 10px;
    background-color: #1e293b;
    padding: 10px;
    border-radius: 10px;
    z-index: 20;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
  }
  
  .toggle-btn {
    background-color: #334155;
    color: white;
    border: none;
    padding: 8px 15px;
    border-radius: 5px;
    font-size: 0.9rem;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .toggle-btn:hover {
    background-color: #475569;
  }
  
  .toggle-btn.active {
    background-color: #38bdf8;
    color: white;
  }
  
  /* Panel de parámetros */
  .control-panel {
    position: absolute;
    top: 20px;
    right: 20px;
    background-color: #1e293b;
    padding: 15px;
    border-radius: 10px;
    z-index: 10;
    width: 200px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
  }
  
  .vital-sign {
    display: flex;
    justify-content: space-between;
    margin-bottom: 10px;
    font-size: 0.9rem;
  }
  
  .vital-label {
    color: #94a3b8;
  }
  
  .vital-value {
    font-weight: bold;
  }
  
  .vital-value.normal {
    color: #4ade80;
  }
  
  .vital-value.warning {
    color: #fbbf24;
  }
  
  .vital-value.danger {
    color: #f87171;
  }
  
  .gauge-container {
    height: 6px;
    background-color: #334155;
    border-radius: 3px;
    margin-bottom: 15px;
    overflow: hidden;
  }
  
  .gauge-level {
    height: 100%;
    background-color: #38bdf8;
    width: 50%;
    transition: width 0.5s ease;
  }
  
  /* Joysticks */
  .joystick-container {
    position: absolute;
    bottom: 100px;
    display: flex;
    flex-direction: column;
    align-items: center;
    z-index: 20;
  }
  
  #joystick-light-container {
    left: 30px;
  }
  
  #joystick-vitrectomo-container {
    right: 30px;
  }
  
  .joystick {
    width: 80px;
    height: 80px;
    background-color: #334155;
    border-radius: 50%;
    position: relative;
    margin-bottom: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
  }
  
  .joystick-handle {
    width: 30px;
    height: 30px;
    background-color: #38bdf8;
    border-radius: 50%;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    transition: transform 0.1s;
  }
  
  .slider-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
  }
  
  .slider-container label {
    margin-bottom: 5px;
    font-size: 0.8rem;
    color: #94a3b8;
  }
  
  .slider-container input[type="range"] {
    width: 100%;
    margin-bottom: 10px;
  }
  
  .slider-container button {
    background-color: #38bdf8;
    color: white;
    border: none;
    padding: 8px 15px;
    border-radius: 5px;
    font-size: 0.9rem;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .slider-container button:hover {
    background-color: #0ea5e9;
  }
  
  /* Cámara de retina */
  #eye-chamber {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 5;
  }
  
  .retina-container {
    position: relative;
    width: 80vmin;
    height: 80vmin;
    max-width: 500px;
    max-height: 500px;
  }
  
  .retina-sphere {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: radial-gradient(circle at center, #570000 0%, #400000 20%, #300000 40%, #200000 70%, #100000 100%);
    box-shadow: inset 0 0 50px rgba(0, 0, 0, 0.8), 0 0 60px rgba(0, 0, 0, 0.6);
    transform-style: preserve-3d;
    overflow: hidden;
  }
  
  .retina-texture {
    position: absolute;
    width: 100%;
    height: 100%;
    background: 
      radial-gradient(circle at 30% 30%, transparent 60%, rgba(0, 0, 0, 0.3) 100%),
      repeating-linear-gradient(45deg, rgba(0, 0, 0, 0.1) 0px, rgba(0, 0, 0, 0.1) 1px, transparent 1px, transparent 20px);
    border-radius: 50%;
  }
  
  .retina-nerve {
    position: absolute;
    bottom: 15%;
    left: 50%;
    transform: translateX(-50%);
    width: 15%;
    height: 8%;
    background: radial-gradient(circle, rgba(209, 85, 85, 0.8) 0%, rgba(180, 40, 40, 0.6) 100%);
    border-radius: 50% 50% 0 0;
  }
  
  .optic-disc {
    position: absolute;
    bottom: 15%;
    left: 50%;
    transform: translateX(-50%);
    width: 10%;
    height: 5%;
    background: radial-gradient(circle, rgba(206, 88, 25, 0.8) 0%, rgba(180, 40, 40, 0.6) 100%);
    border-radius: 50%;
    box-shadow: inset 0 0 20px rgba(191, 78, 78, 0.6), 0 0 25px rgba(220, 80, 80, 0.5);
  }
  
  .optic-cup {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 50%;
    height: 50%;
    background: rgba(0, 0, 0, 0.5);
    border-radius: 50%;
  }
  
  .macula {
    position: absolute;
    top: 40%;
    left: 50%;
    transform: translateX(-50%);
    width: 8%;
    height: 8%;
    background: radial-gradient(circle, rgba(255, 255, 200, 0.3) 0%, rgba(255, 255, 150, 0.2) 50%, transparent 100%);
    border-radius: 50%;
  }
  
  .blood-vessels {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
  }
  
  #light-mask {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: radial-gradient(circle at var(--light-x, 50%) var(--light-y, 50%), 
      transparent 0%, 
      rgba(0, 0, 0, 0.8) calc(var(--light-size, 100px) * 1.5), 
      rgba(0, 0, 0, 0.9) 100%);
    pointer-events: none;
    z-index: 6;
  }
  
  #light-reflection {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: radial-gradient(circle at var(--light-x, 50%) var(--light-y, 50%), 
      rgba(255, 255, 255, 0.3) 0%, 
      rgba(255, 255, 255, 0.1) var(--light-size, 100px), 
      transparent calc(var(--light-size, 100px) * 1.5));
    pointer-events: none;
    z-index: 7;
    opacity: 0;
    transition: opacity 0.3s;
  }
  
  .light-scatter {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: radial-gradient(circle at var(--light-x, 50%) var(--light-y, 50%), 
      rgba(255, 255, 255, 0.05) 0%, 
      transparent 70%);
    pointer-events: none;
    z-index: 4;
    opacity: 0;
    transition: opacity 0.3s;
  }
  
  .light-scatter.active {
    opacity: 1;
  }
  
  .specular-highlight {
    position: absolute;
    width: 30px;
    height: 30px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(255, 255, 255, 0.8) 0%, transparent 70%);
    top: calc(var(--light-y, 50%) - 15px);
    left: calc(var(--light-x, 50%) - 15px);
    pointer-events: none;
    z-index: 8;
    opacity: 0;
    transition: opacity 0.3s;
  }
  
  .specular-highlight.active {
    opacity: 0.6;
  }
  
  /* Instrumentos */
  .instrument {
    position: absolute;
    width: 40px;
    height: 120px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    transform-style: preserve-3d;
    display: none;
    z-index: 10;
  }
  
  .instrument-body {
    position: relative;
    width: 100%;
    height: 100%;
    background: linear-gradient(to right, #555 0%, #777 50%, #555 100%);
    border-radius: 5px;
    transform: rotateX(75deg);
    transform-origin: bottom center;
  }
  
  .instrument-tip {
    width: 10px;
    height: 30px;
    background: linear-gradient(to right, #333 0%, #666 50%, #333 100%);
    border-radius: 0 0 5px 5px;
  }
  
  .instrument-side {
    position: absolute;
    width: 100%;
    height: 20px;
    background: linear-gradient(to right, #444 0%, #666 50%, #444 100%);
    top: 50%;
    transform: translateY(-50%) rotateX(90deg);
    transform-origin: top center;
  }
  
  .instrument-light-reflection {
    position: absolute;
    width: 100%;
    height: 100%;
    background: linear-gradient(
      135deg,
      rgba(255,255,255,0.3) 0%,
      rgba(255,255,255,0.1) 50%,
      transparent 100%
    );
    border-radius: inherit;
    pointer-events: none;
    z-index: 3;
  }
  
  .instrument-shadow {
    position: absolute;
    width: 100%;
    height: 20px;
    background: rgba(0, 0, 0, 0.5);
    bottom: -10px;
    left: 0;
    border-radius: 50%;
    filter: blur(5px);
    transform: rotateX(75deg) translateZ(-10px);
    z-index: -1;
  }
  
  /* Efectos */
  .vitreous-particle {
    position: absolute;
    width: 5px;
    height: 5px;
    background-color: rgba(200, 200, 255, 0.7);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9;
    animation: float 1.5s ease-out forwards;
  }
  
  @keyframes float {
    to {
      transform: translate(var(--tx, 0), var(--ty, 0));
      opacity: 0;
    }
  }
  
  .pfc-bubble {
    position: absolute;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(100, 255, 255, 0.8) 0%, rgba(50, 200, 200, 0.6) 70%, transparent 100%);
    pointer-events: none;
    z-index: 11;
    animation: float-up 2s ease-out forwards;
  }
  
  @keyframes float-up {
    to {
      transform: translate(var(--tx, 0), var(--ty, -100px));
      opacity: 0;
    }
  }
  
  .hemorrhage-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle, rgba(180, 0, 0, 0.7) 0%, rgba(120, 0, 0, 0.5) 100%);
    pointer-events: none;
    z-index: 12;
    opacity: 0;
    display: none;
  }
  
  /* Indicador de profundidad */
  .depth-indicator {
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    background-color: #1e293b;
    padding: 8px 15px;
    border-radius: 20px;
    font-size: 0.9rem;
    z-index: 20;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
    color: var(--depth-color, #10b981);
  }
  
  /* Animaciones */
  @keyframes slideIn {
    from {
      transform: translateY(-20px);
      opacity: 0;
    }
    to {
      transform: translateY(0);
      opacity: 1;
    }
  }
  /* ================== SISTEMA DE ALERTAS PROFESIONAL ================== */
  #alert-system {
      position: fixed;
      top: 20px;
      left: 50%;
      transform: translateX(-50%);
      z-index: 10000;
      width: 90%;
      max-width: 600px;
      display: flex;
      flex-direction: column;
      gap: 15px;
    }
    
    .alert-container {
      display: none;
      background: rgba(20, 20, 30, 0.95);
      border-left: 5px solid;
      border-radius: 8px;
      padding: 15px 20px;
      align-items: center;
      box-shadow: 0 10px 25px rgba(0,0,0,0.5);
      backdrop-filter: blur(8px);
      animation: alertSlideIn 0.3s ease-out forwards;
      transform-origin: top center;
      border-left-color: #ff5555;
    }
    
    @keyframes alertSlideIn {
      0% { opacity: 0; transform: translateY(-20px); }
      100% { opacity: 1; transform: translateY(0); }
    }
    
    @keyframes alertSlideOut {
      0% { opacity: 1; transform: translateY(0); }
      100% { opacity: 0; transform: translateY(-20px); }
    }
    
    .alert-container#wall-collision-alert {
      border-left-color: #ff9933;
      background: rgba(40, 30, 10, 0.95);
      animation: alertShake 0.5s ease-out forwards;
    }
    
    @keyframes alertShake {
      0%, 100% { transform: translateX(0) translateY(0); }
      20% { transform: translateX(-10px) translateY(0); }
      40% { transform: translateX(10px) translateY(0); }
      60% { transform: translateX(-10px) translateY(0); }
      80% { transform: translateX(10px) translateY(0); }
    }
    
    .alert-container#vessel-damage-alert {
      border-left-color: #ff3333;
      background: rgba(40, 10, 10, 0.95);
    }
    
    .alert-container#retina-detachment-alert {
      border-left-color: #ff5555;
      background: rgba(30, 10, 20, 0.95);
    }
    
    .alert-container#lens-contact-alert {
      border-left-color: #55aaff;
      background: rgba(10, 20, 40, 0.95);
    }
    
    .alert-container#high-iop-alert {
      border-left-color: #ffcc00;
      background: rgba(40, 30, 10, 0.95);
    }
    
    .alert-container#retina-fixed-alert {
      border-left-color: #00cc66;
      background: rgba(10, 40, 20, 0.95);
    }
    
    .alert-container#vitreous-removed-alert {
      border-left-color: #55ff55;
      background: rgba(10, 40, 10, 0.95);
    }
    
    .alert-container#hole-located-alert {
      border-left-color: #ffff55;
      background: rgba(40, 40, 10, 0.95);
    }
    
    .alert-container#gas-injected-alert {
      border-left-color: #55aaff;
      background: rgba(10, 20, 40, 0.95);
    }
    
    .alert-icon {
      font-size: 1.8rem;
      margin-right: 15px;
      flex-shrink: 0;
    }
    
    .alert-content {
      flex-grow: 1;
    }
    
    .alert-content h3 {
      margin: 0 0 5px 0;
      color: white;
      font-size: 1.1rem;
      font-weight: 600;
    }
    
    .alert-content p {
      margin: 0;
      color: #ddd;
      font-size: 0.9rem;
      line-height: 1.4;
    }
    
    .alert-timer {
      height: 3px;
      background: rgba(255,255,255,0.3);
      margin-top: 8px;
      border-radius: 2px;
      overflow: hidden;
    }
    
    .alert-timer::after {
      content: '';
      display: block;
      height: 100%;
      width: 100%;
      background: white;
    }
    
    .alert-dismiss {
      background: none;
      border: none;
      color: white;
      font-size: 1.5rem;
      cursor: pointer;
      padding: 0 0 0 15px;
      opacity: 0.7;
      transition: opacity 0.2s;
      flex-shrink: 0;
    }
    
    .alert-dismiss:hover {
      opacity: 1;
    }
    
    /* Efecto de hemorragia */
    .hemorrhage-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(200, 0, 0, 0.5);
      z-index: 9998;
      pointer-events: none;
      display: none;
      opacity: 0;
      transition: opacity 1s ease;
    }
    
    /* Coágulos de sangre */
    .blood-clot {
      position: absolute;
      background: radial-gradient(circle, rgba(180,0,0,0.9), rgba(120,0,0,0.7));
      border-radius: 50%;
      transform: translate(-50%, -50%);
      z-index: 15;
      box-shadow: 
          0 0 10px rgba(180,0,0,0.8),
          inset 0 0 5px rgba(255,255,255,0.3);
      filter: blur(0.5px);
      animation: clotPulse 2s infinite alternate;
    }
    
    @keyframes clotPulse {
      0% { transform: translate(-50%, -50%) scale(1); }
      100% { transform: translate(-50%, -50%) scale(1.05); }
    }
    
    /* Burbujas subretinianas */
    .subretinal-bubble {
      position: absolute;
      background: rgba(200, 50, 50, 0.6);
      border-radius: 50%;
      pointer-events: none;
      z-index: 14;
      filter: blur(1px);
      box-shadow: 
        0 0 8px rgba(200, 50, 50, 0.7),
        inset 0 0 3px rgba(255, 255, 255, 0.4);
    }
    
    /* Marcas de cauterio */
    .cautery-mark {
      position: absolute;
      width: 8px;
      height: 8px;
      background: radial-gradient(circle, 
        white 0%, 
        rgba(255,255,255,0.9) 60%, 
        transparent 80%);
      border-radius: 50%;
      pointer-events: none;
      z-index: 16;
      box-shadow: 0 0 15px rgba(255,180,180,0.6);
      animation: burnPulse 3s infinite alternate;
    }
    
    @keyframes burnPulse {
      0%, 100% { opacity: 0.8; transform: scale(1); }
      50% { opacity: 1; transform: scale(1.1); }
      100% { opacity: 0.8; transform: scale(1); }
    }
    
    /* ================== ESTILOS BASE MEJORADOS ================== */
    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
      touch-action: manipulation;
    }
    
    body {
      margin: 0;
      padding: 0;
      background: #0a0a12;
      overflow: hidden;
      font-family: 'Roboto', 'Segoe UI', system-ui, sans-serif;
      color: #e0e0e0;
    }
    
    #container {
      position: relative;
      width: 100vw;
      height: 100vh;
      perspective: 1200px;
      background: radial-gradient(circle at center, #1a1a2a 0%, #0a0a12 100%);
    }
    
    /* ================== RETINA CENTRAL - MEJORADA CON TEXTURAS REALISTAS ================== */
    #eye-chamber {
      position: absolute;
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      touch-action: none;
      background: radial-gradient(circle at center, rgba(0,0,0,0.3) 0%, transparent 70%);
    }
    
    .retina-container {
      position: relative;
      width: 80vmin;
      height: 80vmin;
      max-width: 600px;
      max-height: 600px;
      transform-style: preserve-3d;
      perspective: 1200px;
      border-radius: 50%;
      overflow: hidden;
      transition: transform 0.3s ease;
      filter: contrast(1.2) brightness(0.9) saturate(1.1);
      will-change: transform, filter;
      box-shadow: 
        inset 0 0 100px rgba(100, 20, 20, 0.3),
        0 0 80px rgba(0, 0, 0, 0.8);
    }
    
    .retina-sphere {
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background: radial-gradient(circle at center, 
        #500000 0%, 
        #400000 20%, 
        #300000 40%,
        #200000 70%,
        #100000 100%);
      transform: rotateX(15deg) translateZ(50px);
      box-shadow: 
        inset 0 0 200px rgba(220, 80, 80, 0.3),
        inset 0 0 80px rgba(255, 120, 120, 0.2),
        0 0 120px rgba(0, 0, 0, 0.9);
      overflow: hidden;
    }
    
    .retina-sphere::after {
      content: '';
      position: absolute;
      width: 200%;
      height: 200%;
      left: -50%;
      top: -50%;
      background-image: 
        radial-gradient(circle at 50% 50%, 
          rgba(255, 180, 180, 0.08) 0.5%, 
          transparent 1.2%),
        repeating-linear-gradient(
          45deg,
          transparent 0px,
          transparent 2px,
          rgba(255, 255, 255, 0.03) 2px,
          rgba(255, 255, 255, 0.03) 3px
        ),
        repeating-linear-gradient(
          -45deg,
          transparent 0px,
          transparent 3px,
          rgba(150, 50, 50, 0.05) 3px,
          rgba(150, 50, 50, 0.05) 5px
        );
      background-size: 
        60px 60px,
        100% 100%,
        100% 100%;
      transform: rotate(15deg);
      animation: textureMove 120s linear infinite;
      pointer-events: none;
    }
    
    @keyframes textureMove {
      0% { transform: rotate(15deg) translate(0,0); }
      100% { transform: rotate(15deg) translate(200px,200px); }
    }
    
    .retina-texture {
      position: absolute;
      width: 100%;
      height: 100%;
      background: 
        radial-gradient(circle at 50% 50%, rgba(255,200,200,0.2) 0%, rgba(255,150,150,0.15) 80%),
        url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100"><path d="M${Array.from({length:50},(_,i)=>`${Math.random()*100},${Math.random()*100}`).join(" ")}" stroke="rgba(180,80,80,0.08)" fill="none" stroke-width="0.5"/></svg>');
      background-size: 100%, 15px 15px;
      opacity: 0.8;
      border-radius: 50%;
      mix-blend-mode: overlay;
      filter: contrast(1.3);
      will-change: transform;
    }
    
    .macula {
      position: absolute;
      width: 100px;
      height: 100px;
      background: radial-gradient(circle at center, 
        rgba(255,230,200,0.7) 0%, 
        rgba(255,210,170,0.5) 60%, 
        transparent 100%);
      border-radius: 50%;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      z-index: 2;
      box-shadow: 
        0 0 40px rgba(255, 220, 150, 0.4),
        inset 0 0 15px rgba(255, 200, 100, 0.5);
      animation: pulse 4s ease-in-out infinite;
    }
    
    @keyframes pulse {
      0%, 100% { opacity: 0.9; transform: translate(-50%, -50%) scale(1); }
      50% { opacity: 1; transform: translate(-50%, -50%) scale(1.05); }
    }
    
    .blood-vessels {
      position: absolute;
      width: 100%;
      height: 100%;
      z-index: 3;
      pointer-events: none;
      filter: drop-shadow(0 0 3px rgba(80, 0, 0, 0.7));
      will-change: transform;
    }
    
    .blood-vessels-animated {
      animation: vesselPulse 3s ease-in-out infinite;
    }
    
    @keyframes vesselPulse {
      0%, 100% { opacity: 0.7; transform: scale(1); }
      50% { opacity: 0.9; transform: scale(1.01); }
    }
    
    .retina-nerve {
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="300" height="300"><path d="M0,0 Q50,50 100,0 T200,0" stroke="rgba(255,255,255,0.06)" stroke-width="1.5" fill="none"/></svg>') center/cover;
      pointer-events: none;
      opacity: 0.6;
      z-index: 4;
    }
    
    .optic-disc {
      position: absolute;
      width: 80px;
      height: 80px;
      background: radial-gradient(circle at center, 
        rgba(220,120,120,0.5) 0%, 
        rgba(200,100,100,0.4) 100%);
      border-radius: 50%;
      top: 50%;
      left: 25%;
      transform: translate(-50%, -50%);
      box-shadow: 
        inset 0 0 20px rgba(220,80,80,0.6), 
        0 0 25px rgba(220,80,80,0.5);
      z-index: 5;
      animation: discPulse 5s ease-in-out infinite;
    }
    
    @keyframes discPulse {
      0%, 100% { transform: translate(-50%, -50%) scale(1); }
      50% { transform: translate(-50%, -50%) scale(1.03); }
    }
    
    .optic-cup {
      position: absolute;
      width: 35px;
      height: 35px;
      background: radial-gradient(circle at center, 
        rgba(220,80,80,0.6) 0%, 
        rgba(200,60,60,0.5) 100%);
      border-radius: 50%;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      box-shadow: inset 0 0 15px rgba(180,50,50,0.7);
      z-index: 6;
    }
    
    /* ================== LUZ ENDOILUMINADOR - EFECTOS DINÁMICOS MEJORADOS ================== */
    #light-mask {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0,0,0,0.95);
      mask-image: radial-gradient(
        circle at var(--light-x, 50%) var(--light-y, 50%), 
        transparent var(--light-size, 80px), 
        black calc(var(--light-size, 80px) + 80px)
      );
      -webkit-mask-image: radial-gradient(
        circle at var(--light-x, 50%) var(--light-y, 50%), 
        transparent var(--light-size, 80px), 
        black calc(var(--light-size, 80px) + 80px)
      );
      transition: all 0.15s ease-out;
      pointer-events: none;
      z-index: 7;
      will-change: mask-image, -webkit-mask-image;
    }
    
    #light-reflection {
      position: absolute;
      width: 100%;
      height: 100%;
      background: radial-gradient(
        circle at var(--light-x, 50%) var(--light-y, 50%), 
        rgba(255,255,240,0.95) calc(var(--light-size, 80px)*0.1), 
        rgba(255,220,180,0.6) calc(var(--light-size, 80px)*0.3), 
        transparent var(--light-size, 80px)
      );
      opacity: 0;
      transition: opacity 0.25s ease, transform 0.25s ease;
      border-radius: 50%;
      pointer-events: none;
      z-index: 8;
      mix-blend-mode: soft-light;
      filter: blur(1.5px);
      will-change: opacity, transform;
    }
    
    .light-scatter {
      position: absolute;
      width: 300%;
      height: 300%;
      left: -100%;
      top: -100%;
      background: radial-gradient(
        circle at var(--light-x, 50%) var(--light-y, 50%),
        rgba(255, 230, 200, 0.15) 0%,
        transparent 80%
      );
      pointer-events: none;
      z-index: 9;
      opacity: 0;
      transition: opacity 0.3s ease;
      will-change: background, opacity;
    }
    
    .light-scatter.active {
      opacity: 0.7;
    }
    
    .specular-highlight {
      position: absolute;
      width: 100%;
      height: 100%;
      background: radial-gradient(
        circle at var(--light-x, 50%) var(--light-y, 50%),
        rgba(255, 255, 255, 0.5) 0%,
        transparent 70%
      );
      pointer-events: none;
      z-index: 10;
      mix-blend-mode: overlay;
      opacity: 0;
      transition: opacity 0.3s ease;
    }
    
    .specular-highlight.active {
      opacity: 0.4;
    }
    
    /* ================== INSTRUMENTOS QUIRÚRGICOS 3D MEJORADOS ================== */
    .instrument {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%) translateZ(0);
      display: none;
      z-index: 10;
      transition: transform 0.1s ease-out;
      will-change: transform;
      transform-style: preserve-3d;
      perspective: 1000px;
    }
    
    .instrument-body {
      position: relative;
      width: 100%;
      height: 100%;
      transform-style: preserve-3d;
      transition: all 0.2s ease;
    }
    
    .instrument-shadow {
      position: absolute;
      width: 120%;
      height: 120%;
      left: -10%;
      top: -10%;
      background: radial-gradient(
        ellipse at center,
        rgba(0,0,0,0.4) 0%,
        transparent 70%
      );
      transform: rotateX(75deg) translateZ(-10px);
      pointer-events: none;
      z-index: -1;
      filter: blur(5px);
      opacity: 0.7;
      transition: all 0.3s ease;
    }
    
    /* Vitrectomo 3D mejorado */
    #vitrectome {
      width: 2.5mm;
      height: 15mm;
      transform-style: preserve-3d;
    }
    
    #vitrectome .instrument-body {
      background: linear-gradient(to bottom, 
        #999 0%, 
        #ccc 10%, 
        #eee 20%, 
        #fff 40%, 
        #ddd 60%, 
        #aaa 80%,
        #888 100%);
      border: 1px solid #777;
      border-radius: 1.2mm;
      box-shadow: 
        0 0 1.5mm rgba(0,0,0,0.7),
        0 0 3mm rgba(255,255,255,0.15),
        inset 0 0 5px rgba(255,255,255,0.3);
      transform: rotateX(10deg);
    }
    
    #vitrectome::before {
      content: '';
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateX(-50%) translateZ(1px);
      width: 3mm;
      height: 1mm;
      background: linear-gradient(to right, #aaa, #ddd, #aaa);
      border-radius: 0.5mm;
      box-shadow: 0 1px 2px rgba(0,0,0,0.5);
      z-index: 2;
    }
    
    #vitrectome::after {
      content: '';
      position: absolute;
      bottom: -1mm;
      left: 50%;
      transform: translateX(-50%) translateZ(-1px);
      width: 1.8mm;
      height: 1.8mm;
      background: radial-gradient(circle, 
        #fff 0%, 
        #bbb 50%, 
        #888 100%);
      border-radius: 50%;
      box-shadow: 
        0 0 0.8mm rgba(0,0,0,0.7),
        0 0 1.5mm rgba(255,255,255,0.3),
        inset 0 0 5px rgba(255,255,255,0.4);
      z-index: 1;
    }
    
    #vitrectome .instrument-tip {
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%) translateZ(0);
      width: 1.2mm;
      height: 3mm;
      background: linear-gradient(to bottom, #ccc, #999);
      border-radius: 0.3mm;
      box-shadow: 
        inset 0 -1px 2px rgba(0,0,0,0.5),
        0 0 5px rgba(255,255,255,0.2);
    }
    
    #vitrectome .instrument-side {
      position: absolute;
      width: 100%;
      height: 100%;
      background: linear-gradient(to bottom, #888, #aaa, #888);
      transform: rotateY(90deg) translateZ(1.25mm);
      border-radius: 1.2mm;
      box-shadow: inset 0 0 5px rgba(0,0,0,0.5);
    }
    
    /* Sonda de Láser 3D mejorada */
    #laser-probe {
      width: 0.5mm;
      height: 12mm;
      transform-style: preserve-3d;
    }
    
    #laser-probe .instrument-body {
      background: linear-gradient(to bottom, 
        #f00, 
        #f90 20%, 
        #ff0 30%, 
        #f90 70%, 
        #f00);
      border-radius: 0.3mm;
      box-shadow: 
        0 0 1.5mm rgba(255,0,0,0.9),
        0 0 3mm rgba(255,100,100,0.6),
        inset 0 0 5px rgba(255,255,255,0.4);
      transform: rotateX(10deg);
    }
    
    #laser-probe::before {
      content: '';
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateX(-50%) translateZ(1px);
      width: 1.5mm;
      height: 1mm;
      background: linear-gradient(to right, #aaa, #ddd, #aaa);
      border-radius: 0.5mm;
      box-shadow: 0 1px 2px rgba(0,0,0,0.5);
      z-index: 2;
    }
    
    #laser-probe .instrument-tip {
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%) translateZ(0);
      width: 0.8mm;
      height: 1.5mm;
      background: radial-gradient(circle, #fff, #f99);
      border-radius: 50%;
      box-shadow: 
        0 0 5px rgba(255,100,100,0.8),
        inset 0 0 3px rgba(255,255,255,0.8);
    }
    
    /* Sonda de PFC */
    #pfc-probe {
      width: 0.8mm;
      height: 13mm;
      transform-style: preserve-3d;
    }
    
    #pfc-probe .instrument-body {
      background: linear-gradient(to bottom, 
        #0af 0%, 
        #5bf 30%, 
        #8df 70%, 
        #0af 100%);
      border: 1px solid #07a;
      border-radius: 0.5mm;
      box-shadow: 
        0 0 1.5mm rgba(0,100,255,0.7),
        0 0 3mm rgba(100,200,255,0.4),
        inset 0 0 5px rgba(255,255,255,0.6);
      transform: rotateX(10deg);
    }
    
    #pfc-probe::before {
      content: '';
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateX(-50%) translateZ(1px);
      width: 2mm;
      height: 1mm;
      background: linear-gradient(to right, #aaa, #ddd, #aaa);
      border-radius: 0.5mm;
      box-shadow: 0 1px 2px rgba(0,0,0,0.5);
      z-index: 2;
    }
    
    #pfc-probe .instrument-tip {
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%) translateZ(0);
      width: 1mm;
      height: 1.5mm;
      background: radial-gradient(circle, #fff, #5bf);
      border-radius: 50%;
      box-shadow: 
        0 0 5px rgba(0,150,255,0.8),
        inset 0 0 3px rgba(255,255,255,0.8);
    }
    
    /* Cauterio */
    #cautery-probe {
      width: 1.5mm;
      height: 12mm;
      transform-style: preserve-3d;
    }
    
    #cautery-probe .instrument-body {
      background: linear-gradient(to bottom, 
        #ff3333 0%, 
        #ff6666 20%, 
        #ff9999 40%, 
        #ff6666 70%, 
        #ff3333 100%);
      border: 1px solid #cc0000;
      border-radius: 0.8mm;
      box-shadow: 
        0 0 1.5mm rgba(255,0,0,0.7),
        0 0 3mm rgba(255,100,100,0.4),
        inset 0 0 5px rgba(255,255,255,0.6);
      transform: rotateX(10deg);
    }
    
    #cautery-probe::before {
      content: '';
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateX(-50%) translateZ(1px);
      width: 2mm;
      height: 1mm;
      background: linear-gradient(to right, #aaa, #ddd, #aaa);
      border-radius: 0.5mm;
      box-shadow: 0 1px 2px rgba(0,0,0,0.5);
      z-index: 2;
    }
    
    #cautery-probe .instrument-tip {
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%) translateZ(0);
      width: 1.2mm;
      height: 2mm;
      background: radial-gradient(circle, #ff9999, #ff3333);
      border-radius: 0.3mm;
      box-shadow: 
        0 0 5px rgba(255,100,100,0.8),
        inset 0 0 3px rgba(255,255,255,0.8);
      z-index: 1;
    }
    
    /* Efecto de cauterio */
    .cautery-effect {
      position: absolute;
      width: 30px;
      height: 30px;
      background: radial-gradient(circle, 
        rgba(255,200,200,0.9) 0%, 
        rgba(255,100,100,0.7) 70%, 
        transparent 90%);
      border-radius: 50%;
      pointer-events: none;
      z-index: 18;
      animation: cauteryFade 1s ease-out forwards;
      filter: blur(1.5px);
      box-shadow: 0 0 20px rgba(255,100,100,0.8);
    }
    
    @keyframes cauteryFade {
      0% { transform: scale(0.8); opacity: 1; }
      50% { transform: scale(1.5); opacity: 0.9; }
      100% { transform: scale(1.5); opacity: 0; }
    }
    
    /* Burbujas de PFC */
    .pfc-bubble {
      position: absolute;
      background: rgba(100, 255, 255, 0.7);
      border-radius: 50%;
      pointer-events: none;
      z-index: 5;
      filter: blur(1px);
      box-shadow: 
        0 0 10px rgba(100, 255, 255, 0.8),
        inset 0 0 5px rgba(255, 255, 255, 0.8);
      transform: translate(-50%, -50%);
      will-change: transform, opacity;
    }
    
    .instrument-active {
      animation: instrumentVibrate 0.1s linear infinite;
    }
    
    @keyframes instrumentVibrate {
      0% { transform: translate(-50%, -50%) rotate(0.5deg) translateZ(0); }
      50% { transform: translate(-50%, -50%) rotate(-0.5deg) translateZ(0); }
      100% { transform: translate(-50%, -50%) rotate(0.5deg) translateZ(0); }
    }
    
    /* ================== INTERFAZ DE USUARIO PROFESIONAL ================== */
    .instrument-panel {
      position: absolute;
      top: 50%;
      left: 20px;
      transform: translateY(-50%);
      background: rgba(0,0,0,0.85);
      padding: 15px 10px;
      border-radius: 12px;
      z-index: 5000;
      display: flex;
      flex-direction: column;
      box-shadow: 
        0 6px 20px rgba(0,0,0,0.6),
        inset 0 0 15px rgba(255,255,255,0.15);
      backdrop-filter: blur(8px);
      border: 1px solid rgba(255,255,255,0.15);
    }
    
    .toggle-btn {
      background: #1a3a8a;
      color: white;
      padding: 12px 18px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      font-size: clamp(0.85rem, 2.5vw, 1rem);
      margin: 8px 5px;
      min-width: 100px;
      transition: all 0.2s;
      font-weight: 500;
      box-shadow: 
        0 3px 8px rgba(0,0,0,0.4),
        inset 0 0 8px rgba(255,255,255,0.15);
      position: relative;
      overflow: hidden;
      text-transform: uppercase;
      letter-spacing: 0.5px;
    }
    
    .toggle-btn::after {
      content: '';
      position: absolute;
      top: 50%;
      left: 50%;
      width: 5px;
      height: 5px;
      background: rgba(255, 255, 255, 0.6);
      opacity: 0;
      border-radius: 100%;
      transform: scale(1, 1) translate(-50%, -50%);
      transform-origin: 50% 50%;
    }
    
    .toggle-btn:active::after {
      animation: ripple 0.6s ease-out;
    }
    
    @keyframes ripple {
      0% {
        transform: scale(0, 0);
        opacity: 1;
      }
      20% {
        transform: scale(25, 25);
        opacity: 1;
      }
      100% {
        opacity: 0;
        transform: scale(40, 40);
      }
    }
    
    .toggle-btn.active {
      background: #3b82f6;
      box-shadow: 
        0 0 20px #3b82f6,
        0 0 40px rgba(59, 130, 246, 0.4);
    }
    
    /* Panel de controles mejorado */
    .control-panel {
      position: absolute;
      right: 20px;
      top: 190px;
      background: rgba(10,10,20,0.95);
      padding: 20px;
      border-radius: 50px;
      z-index: 5000;
      font-size: clamp(0.75rem, 2vw, 0.9rem);
      width: 180px;
      max-height: 80vh;
      overflow-y: auto;
      -webkit-overflow-scrolling: touch;
      box-shadow: 
        0 6px 20px rgba(18, 20, 41, 0.6),
        inset 0 0 15px rgba(255,255,255,0.15);
      border: 1px solid rgba(255,255,255,0.15);
      backdrop-filter: blur(8px);
    }
    
    .control-panel h3 {
      margin: 0 0 15px 0;
      color: #2990aa;
      font-size: 1.1rem;
      text-align: center;
      border-bottom: 1px solid #444;
      padding-bottom: 8px;
    }
    
    .vital-sign {
      display: flex;
      justify-content: space-between;
      margin: 15px 0;
      flex-wrap: wrap;
    }
    
    .vital-label {
      color: #aaa;
      font-size: 0.85em;
      width: 60%;
    }
    
    .vital-value {
      font-family: 'Courier New', monospace;
      font-weight: bold;
      font-size: 0.95em;
      width: 40%;
      text-align: right;
      transition: color 0.3s;
    }
    
    .normal { color: #2ecc71; text-shadow: 0 0 8px rgba(46, 204, 113, 0.4); }
    .warning { color: #f39c12; text-shadow: 0 0 8px rgba(243, 156, 18, 0.4); }
    .danger { color: #e74c3c; text-shadow: 0 0 8px rgba(231, 76, 60, 0.4); }
    
    .gauge-container {
      width: 100%;
      height: 10px;
      background: #222;
      border-radius: 5px;
      margin: 8px 0 15px 0;
      overflow: hidden;
      box-shadow: inset 0 0 8px rgba(0,0,0,0.7);
    }
    
    .gauge-level {
      height: 100%;
      border-radius: 5px;
      transition: width 0.5s ease;
      box-shadow: inset 0 0 8px rgba(255,255,255,0.3);
    }
    
    #iop-gauge .gauge-level {
      background: linear-gradient(to right, #2ecc71, #f39c12, #e74c3c);
    }
    
    #perfusion-gauge .gauge-level {
      background: linear-gradient(to right, #3498db, #9b59b6);
    }
    
    #vitreous-gauge .gauge-level {
      background: linear-gradient(to right, #1abc9c, #f1c40f);
    }
    
    #pfc-gauge .gauge-level {
      background: linear-gradient(to right, #0af, #5bf);
    }
    
    /* Joysticks mejorados */
    .joystick-container {
      position: absolute;
      bottom: 1px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 20px;
      z-index: 3000;
    }
    
    #joystick-light-container {
      left: 200px;
    }
    
    #joystick-vitrectomo-container {
      right: 180px;
    }
    
    .joystick {
      width: 100px;
      height: 100px;
      background: rgba(255,255,255,0.1);
      border: 2px solid rgba(255,255,255,0.4);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      touch-action: none;
      position: relative;
      box-shadow: 
        0 6px 15px rgba(0,0,0,0.4),
        inset 0 0 25px rgba(255,255,255,0.15);
      backdrop-filter: blur(8px);
      will-change: transform;
    }
    
    .joystick .joystick-handle {
      width: 40px;
      height: 40px;
      background: rgba(255,255,255,0.8);
      border-radius: 50%;
      position: absolute;
      transition: transform 0.1s ease;
      box-shadow: 
        0 0 15px rgba(255,255,255,0.4),
        inset 0 0 8px rgba(255,255,255,0.7);
      will-change: transform;
    }
    
    /* Controles Z mejorados */
    .slider-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 100%;
      background: rgba(0,0,0,0.7);
      padding: 15px;
      border-radius: 12px;
      box-shadow: 
        0 4px 10px rgba(0,0,0,0.4),
        inset 0 0 8px rgba(255,255,255,0.15);
      backdrop-filter: blur(8px);
    }
    
    .slider-container label {
      font-size: 0.8rem;
      margin-bottom: 10px;
      text-align: center;
      color: #eee;
      font-weight: 500;
      text-shadow: 0 0 8px rgba(0,0,0,0.6);
    }
    
    input[type="range"] {
      width: 100%;
      -webkit-appearance: none;
      height: 10px;
      background: #333;
      border-radius: 5px;
      margin: 8px 0 12px 0;
      box-shadow: inset 0 0 8px rgba(0,0,0,0.7);
    }
    
    input[type="range"]::-webkit-slider-thumb {
      -webkit-appearance: none;
      width: 22px;
      height: 22px;
      background: #3b82f6;
      border-radius: 50%;
      cursor: pointer;
      border: 2px solid white;
      box-shadow: 
        0 0 8px rgba(0,0,0,0.7),
        0 0 15px rgba(59, 130, 246, 0.7);
      transition: all 0.2s;
    }
    
    input[type="range"]::-webkit-slider-thumb:active {
      transform: scale(1.2);
      box-shadow: 
        0 0 12px rgba(0,0,0,0.7),
        0 0 20px rgba(59, 130, 246, 1);
    }
    
    #btn-precionar {
      background: linear-gradient(to bottom, #dc2626, #b91c1c);
      color: white;
      padding: 12px 15px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      font-size: 0.95rem;
      margin-top: 8px;
      width: 100%;
      font-weight: bold;
      box-shadow: 
        0 4px 12px rgba(0,0,0,0.4),
        0 0 15px rgba(220, 38, 38, 0.4);
      transition: all 0.2s;
      text-shadow: 0 0 8px rgba(0,0,0,0.4);
      text-transform: uppercase;
      letter-spacing: 1px;
    }
    
    #btn-precionar:active {
      transform: scale(0.95);
      box-shadow: 
        0 2px 6px rgba(0,0,0,0.4),
        0 0 8px rgba(220, 38, 38, 0.4);
    }
    
    /* Mini mapa mejorado */
    #miniMapContainer {
      position: absolute;
      top: 40px;
      right: 30px;
      width: 170px;
      height: 120px;
      overflow: hidden;
      border-radius: 5px;
      z-index: 2000;
      background: rgba(0,0,0,0.85);
      border: 1px solid rgba(255,255,255,0.15);
      box-shadow: 
        0 6px 15px rgba(0,0,0,0.5),
        inset 0 0 15px rgba(255,255,255,0.15);
      backdrop-filter: blur(8px);
    }
    
    #eyeCrossSection {
      width: 100%;
      height: 100%;
      display: block;
      filter: drop-shadow(0 0 8px rgba(0,0,0,0.7));
    }
    
    /* Indicador de profundidad */
    .depth-indicator {
      position: absolute;
      bottom: 130px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(0,0,0,0.8);
      padding: 10px 20px;
      border-radius: 25px;
      color: white;
      font-size: 0.85rem;
      z-index: 2000;
      display: flex;
      align-items: center;
      backdrop-filter: blur(8px);
      box-shadow: 0 0 15px rgba(0,0,0,0.6);
      border: 1px solid var(--depth-color, #3b82f6);
      text-transform: uppercase;
      letter-spacing: 1px;
    }
    
    .depth-indicator::before {
      content: '';
      display: inline-block;
      width: 12px;
      height: 12px;
      border-radius: 50%;
      margin-right: 10px;
      background: var(--depth-color, #3b82f6);
      box-shadow: 0 0 8px var(--depth-color, #3b82f6);
    }
    
    /* Efectos visuales mejorados */
    .laser-spot {
      width: 30px;
      height: 30px;
      background: radial-gradient(circle, 
        rgba(255,80,80,0.95), 
        rgba(255,0,0,0.8) 70%, 
        transparent 90%);
      border-radius: 50%;
      position: absolute;
      pointer-events: none;
      animation: laser-fade 2.5s ease-out forwards;
      z-index: 15;
      filter: blur(1px);
      will-change: transform, opacity;
    }
    
    @keyframes laser-fade {
      0% { opacity: 1; transform: scale(1); }
      100% { opacity: 0; transform: scale(0.3); }
    }
    
    .laser-burn-permanent {
      width: 8px;
      height: 8px;
      background: radial-gradient(circle, 
        white 0%, 
        rgba(255,255,255,0.9) 60%, 
        transparent 80%);
      border-radius: 50%;
      position: absolute;
      pointer-events: none;
      z-index: 16;
      box-shadow: 0 0 15px rgba(255,120,120,0.6);
      animation: burnPulse 3s infinite alternate;
    }
    
    .vitreous-particle {
      width: 6px;
      height: 6px;
      background: rgba(255,255,255,0.95);
      border-radius: 50%;
      position: absolute;
      pointer-events: none;
      animation: float-particle 1.5s ease-out forwards;
      z-index: 12;
      filter: blur(0.8px);
      box-shadow: 0 0 8px rgba(255,255,255,0.9);
      will-change: transform, opacity;
    }
    
    @keyframes float-particle {
      100% { transform: translate(var(--tx, 0px), var(--ty, 0px)); opacity: 0; }
    }
    
    .injection-bubble {
      position: absolute;
      background: rgba(200,230,255,0.9);
      border-radius: 50%;
      pointer-events: none;
      z-index: 3;
      filter: blur(1.5px);
      animation: bubble-float 4s ease-in-out forwards;
      box-shadow: 
        0 0 8px rgba(100,150,255,0.7),
        inset 0 0 5px rgba(255,255,255,0.7);
      will-change: transform, opacity;
    }
    
    @keyframes bubble-float {
      100% { 
        transform: translate(calc(var(--tx)*1px), calc(var(--ty)*1px));
        opacity: 0;
      }
    }
    
    /* ================== NUEVOS ESTILOS PARA EL DESPRENDIMIENTO MEJORADO ================== */
    #retinal-hole {
      display: none;
      position: absolute;
      width: 30px;
      height: 30px;
      background: radial-gradient(circle, 
        rgba(255,255,255,0.8) 0%, 
        rgba(200,0,0,0.6) 70%, 
        transparent 100%);
      border-radius: 50%;
      z-index: 15;
      box-shadow: 0 0 15px rgba(255,255,255,0.5);
      border: 2px dashed rgba(255,255,255,0.7);
      pointer-events: none;
      animation: holePulse 2s infinite alternate;
    }
    
    @keyframes holePulse {
      0% { transform: scale(1); opacity: 0.8; }
      50% { transform: scale(1.1); opacity: 1; }
      100% { transform: scale(1); opacity: 0.8; }
    }
    
    /* Burbujas de gas mejoradas */
    .gas-bubble {
      position: absolute;
      background: rgba(255, 255, 255, 0.95);
      border-radius: 50%;
      pointer-events: none;
      z-index: 6;
      filter: blur(1px);
      box-shadow: 
        0 0 15px rgba(255, 255, 255, 0.9),
        inset 0 0 8px rgba(255, 255, 255, 0.8);
      transform: translate(-50%, -50%);
      will-change: transform, opacity;
      animation: bubbleFloat 4s ease-in-out forwards;
    }
    
    .gas-bubble-large {
      position: absolute;
      width: 40%;
      height: 40%;
      background: radial-gradient(circle, 
        rgba(255,255,255,0.95) 0%, 
        rgba(220,220,255,0.8) 70%, 
        transparent 100%);
      border-radius: 50%;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      z-index: 5;
      filter: blur(2px);
      box-shadow: 
        0 0 30px rgba(255,255,255,0.9),
        inset 0 0 20px rgba(255,255,255,0.8);
      animation: largeBubblePulse 3s infinite alternate;
    }
    
    @keyframes bubbleFloat {
      0% { transform: translate(-50%, -50%) scale(0.8); opacity: 0; }
      50% { transform: translate(-50%, -50%) scale(1.2); opacity: 0.9; }
      100% { transform: translate(calc(var(--tx)*1px), calc(var(--ty)*1px)) scale(1); opacity: 0; }
    }
    
    @keyframes largeBubblePulse {
      0% { transform: translate(-50%, -50%) scale(1); }
      50% { transform: translate(-50%, -50%) scale(1.1); }
      100% { transform: translate(-50%, -50%) scale(1); }
    }
    
    /* Efecto de arrugas en la retina desprendida */
    #retina-edge-detachment {
      filter: url('#wrinkleFilter');
    }
    
    /* Efecto de PFC mejorado */
    #detached-retina-overlay {
      transition: background 1.5s ease;
    }
    
    /* ================== CASO CLÍNICO ================== */
    #clinical-case {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.9);
      z-index: 20000;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      color: white;
      padding: 20px;
      box-sizing: border-box;
      backdrop-filter: blur(5px);
    }
    
    #clinical-case-content {
      max-width: 800px;
      background: rgba(20, 20, 40, 0.95);
      padding: 30px;
      border-radius: 15px;
      border-left: 5px solid #3b82f6;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.7);
    }
    
    #clinical-case h2 {
      color: #3b82f6;
      margin-top: 0;
      text-align: center;
      font-size: 1.8rem;
      margin-bottom: 20px;
    }
    
    #clinical-case h3 {
      color: #10b981;
      margin-bottom: 10px;
      font-size: 1.3rem;
    }
    
    #clinical-case ul {
      padding-left: 20px;
    }
    
    #clinical-case li {
      margin-bottom: 8px;
      line-height: 1.5;
    }
    
    #start-simulation-btn {
      background: #3b82f6;
      color: white;
      border: none;
      padding: 15px 30px;
      font-size: 1.2rem;
      border-radius: 8px;
      cursor: pointer;
      margin-top: 30px;
      transition: all 0.3s;
      box-shadow: 0 5px 15px rgba(59, 130, 246, 0.4);
    }
    
    #start-simulation-btn:hover {
      background: #2563eb;
      transform: translateY(-2px);
      box-shadow: 0 7px 20px rgba(59, 130, 246, 0.6);
    }
    
    #start-simulation-btn:active {
      transform: translateY(0);
      box-shadow: 0 5px 15px rgba(59, 130, 246, 0.4);
    }
    
    /* ================== RESPONSIVE ADJUSTMENTS ================== */
    @media (max-width: 768px) {
      .instrument-panel {
        top: 10px;
        left: 10px;
        right: auto;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        padding: 8px;
        width: calc(100% - 20px);
      }
      
      .toggle-btn {
        padding: 10px 12px;
        min-width: auto;
        font-size: 0.8rem;
        margin: 4px;
        flex: 1 1 calc(33% - 8px);
        max-width: calc(33% - 8px);
      }
      
      .control-panel {
        top: 120px;
        right: 10px;
        width: 150px;
        padding: 12px;
      }
      
      #miniMapContainer {
        top: 120px;
        left: 10px;
        width: 150px;
        height: 110px;
      }
      
      .joystick-container {
        bottom: 20px;
        width: 45%;
      }
      
      #joystick-light-container {
        left: 10px;
      }
      
      #joystick-vitrectomo-container {
        right: 10px;
      }
      
      .joystick {
        width: 80px;
        height: 80px;
      }
      
      .joystick .joystick-handle {
        width: 35px;
        height: 35px;
      }
      
      .slider-container {
        padding: 10px;
      }
      
      #btn-precionar {
        padding: 10px;
        font-size: 0.9rem;
      }
      
      .depth-indicator {
        bottom: 110px;
        font-size: 0.8rem;
      }
    }
    
    @media (max-width: 480px) {
      .retina-container {
        width: 95vmin;
        height: 95vmin;
      }
      
      .toggle-btn {
        font-size: 0.7rem;
        padding: 8px 10px;
      }
      
      .joystick {
        width: 70px;
        height: 70px;
      }
      
      .joystick .joystick-handle {
        width: 30px;
        height: 30px;
      }
      
      .control-panel {
        width: 130px;
        padding: 10px;
      }
      
      #miniMapContainer {
        width: 130px;
        height: 95px;
      }
      
      .depth-indicator {
        bottom: 100px;
        font-size: 0.75rem;
        padding: 8px 15px;
      }
    }
    /* Estilos para el instrumento único - PALO GRIS */
    .instrument-container {
      position: absolute;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 30;
    }
    
    .instrument-rod {
      position: absolute;
      height: 8px;
      background: linear-gradient(to right, #888, #ccc);
      border-radius: 4px;
      transform-origin: left center;
      z-index: 30;
      box-shadow: 0 0 10px rgba(0,0,0,0.5);
    }
    
    .instrument-tip {
      position: absolute;
      width: 20px;
      height: 20px;
      border-radius: 50%;
      background: #ccc;
      z-index: 31;
      box-shadow: 0 0 10px rgba(0,0,0,0.5);
      transform: translate(-50%, -50%);
    }
    
    .instrument-base {
      position: absolute;
      width: 30px;
      height: 30px;
      border-radius: 50%;
      background: #555;
      z-index: 29;
      box-shadow: 0 0 15px rgba(0,0,0,0.7);
    }
    
    .instrument-tip.active-vitrectomo {
      background: #3b82f6;
      box-shadow: 0 0 15px rgba(59, 130, 246, 0.7);
    }
    
    .instrument-tip.active-sf6 {
      background: #ffff55;
      box-shadow: 0 0 15px rgba(255, 255, 85, 0.7);
    }
    
    .instrument-tip.active-forceps {
      background: #00cc66;
      box-shadow: 0 0 15px rgba(0, 204, 102, 0.7);
      width: 15px;
      height: 25px;
      border-radius: 5px;
      clip-path: polygon(0% 0%, 100% 0%, 80% 100%, 20% 100%);
    }
    
    .instrument-tip.active-colorante {
      background: #00aaff;
      box-shadow: 0 0 15px rgba(0, 170, 255, 0.7);
    }
    
    /* Efecto de pinzas agarrando */
    .instrument-tip.grasping {
      animation: grasping 0.5s infinite alternate;
    }
    
    @keyframes grasping {
      0% { transform: translate(-50%, -50%) scale(1); }
      100% { transform: translate(-50%, -50%) scale(1.2); }
    }
    
    /* Indicador de acción */
    .instrument-action-indicator {
      position: absolute;
      top: -30px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(0,0,0,0.7);
      color: white;
      padding: 5px 10px;
      border-radius: 15px;
      font-size: 0.8rem;
      white-space: nowrap;
      z-index: 100;
      opacity: 0;
      pointer-events: none;
    }
    
    .instrument-action-indicator.active {
      opacity: 1;
    }
    
    /* Retina y efectos visuales */
    .macula {
      position: absolute;
      width: 30px;
      height: 30px;
      border-radius: 50%;
      background: rgba(200, 200, 100, 0.3);
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      box-shadow: 0 0 20px rgba(200, 200, 100, 0.5);
      z-index: 16;
    }
    
    /* Agujero macular mejorado */
    .macular-hole {
      position: absolute;
      width: 20px;
      height: 20px;
      border-radius: 50%;
      background: rgba(0,0,0,0.7);
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      z-index: 17;
      box-shadow: 0 0 10px rgba(0,0,0,0.8);
      border: 1px solid rgba(255,255,255,0.2);
    }
    
    /* Membrana Limitante Interna (MLI) - Mejorada con efecto de peeling */
    .mli-layer {
      position: absolute;
      width: 40%;
      height: 40%;
      border-radius: 50%;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: rgba(0,170,255,0.1);
      z-index: 18;
      pointer-events: none;
      opacity: 0;
      transition: opacity 0.5s ease;
      border: 1px dashed rgba(0,170,255,0.3);
      clip-path: circle(50% at 50% 50%);
      transform-style: preserve-3d;
    }
    
    .mli-layer.visible {
      opacity: 1;
    }
    
    .mli-peeling {
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background: rgba(0,170,255,0.15);
      z-index: 19;
      pointer-events: none;
      transform-origin: center;
      clip-path: circle(50% at 50% 50%);
      transform-style: preserve-3d;
    }
    
    .mli-flap {
      position: absolute;
      background: rgba(0,170,255,0.2);
      border: 1px dashed rgba(0,170,255,0.5);
      z-index: 20;
      transform-origin: center;
      transform-style: preserve-3d;
      pointer-events: none;
      border-radius: 50%;
    }
    
    /* Efectos de vitrectomía */
    .vitrectomy-effect {
      position: absolute;
      width: 60px;
      height: 60px;
      background: radial-gradient(circle, 
        rgba(255,255,255,0.2) 0%, 
        rgba(255,255,255,0.1) 40%, 
        transparent 70%);
      border-radius: 50%;
      pointer-events: none;
      z-index: 14;
      transform: scale(0);
      opacity: 0;
    }
    
    .suction-particle {
      position: absolute;
      width: 4px;
      height: 4px;
      background: rgba(200, 200, 255, 0.8);
      border-radius: 50%;
      pointer-events: none;
      z-index: 12;
      box-shadow: 0 0 5px rgba(255,255,255,0.6);
    }
    
    /* Efecto de colorante mejorado */
    .dye-effect {
      position: absolute;
      width: 100%;
      height: 100%;
      background: rgba(0, 170, 255, 0.3);
      border-radius: 50%;
      pointer-events: none;
      z-index: 13;
      opacity: 0;
      transition: opacity 0.5s ease;
    }
    
    .dye-effect.active {
      opacity: 1;
    }
    
    .dye-effect.residual {
      width: 40%;
      height: 40%;
      background: rgba(0, 170, 255, 0.15);
      opacity: 0;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }
    
    .dye-particle {
      position: absolute;
      width: 3px;
      height: 3px;
      background: #00aaff;
      border-radius: 50%;
      pointer-events: none;
      z-index: 12;
    }
    
    /* VISUALIZACIÓN OCT - Mejorada */
    #oct-container {
      position: absolute;
      right: 20px;
      width: 280px;
      height: 180px;
      background: rgba(0,0,0,0.7);
      border-radius: 1px;
      box-shadow: 0 0 15px rgba(0,0,0,0.5);
      border: 10px solid rgba(255,255,255,0.1);
    }
    
    #oct-scan {
      width: 100%;
      height: 100%;
    }
    
    .oct-layer {
      stroke-width: 1.5;
      fill: none;
    }
    
    #oct-retina {
      stroke: #00aaff;
      stroke-width: 2;
    }
    
    #oct-choroid {
      stroke: #0066aa;
    }
    
    #oct-sclera {
      stroke: #ffffff;
      stroke-width: 1;
    }
    
    #oct-macular-hole {
      stroke: #ff5555;
      stroke-width: 1;
      fill: rgba(255,100,100,0.3);
    }
    
    #oct-forceps {
      stroke: #00cc66;
      stroke-width: 2;
      fill: none;
      stroke-linecap: round;
    }
    
    #oct-mli {
      stroke: #00aaff;
      stroke-width: 1;
      stroke-dasharray: 3,3;
      fill: none;
    }
    
    .oct-scanline {
      stroke: rgba(255,255,255,0.8);
      stroke-width: 1;
      stroke-dasharray: 5,5;
      animation: scanline 2s linear infinite;
    }
    
    @keyframes scanline {
      from { transform: translateY(-100%); }
      to { transform: translateY(100%); }
    }
    
    /* Vítreo mejorado - sin filamentos */
    .vitreous {
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background: radial-gradient(circle, 
        rgba(255,255,255,0.2) 0%, 
        rgba(255,255,255,0.3) 50%, 
        rgba(255,255,255,0.1) 100%);
      z-index: 10;
      pointer-events: none;
      transition: opacity 0.5s ease;
      opacity: 1;
    }
    
    /* Retina central */
    .retina-sphere {
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background: radial-gradient(circle at center, #400000, #200000);
      overflow: hidden;
    }
    
    /* Control de gas SF6 */
    #sf6-control {
      margin-top: 10px;
      display: none;
    }
    
    #sf6-slider {
      width: 100%;
    }

    /* Burbujas de gas SF6 mejoradas */
    .sf6-bubble {
      position: absolute;
      background: rgba(255, 255, 255, 0.7);
      border-radius: 50%;
      pointer-events: none;
      z-index: 19;
      animation: bubble-float 4s ease-in forwards, bubble-pulse 2s infinite alternate;
    }

    @keyframes bubble-float {
      0% {
        transform: translateY(0) scale(0.5);
        opacity: 0.8;
      }
      100% {
        transform: translateY(-100px) scale(1.5);
        opacity: 0;
      }
    }
    
    @keyframes bubble-pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.2); }
      100% { transform: scale(1); }
    }

    /* Alertas específicas para cada paso */
    .alert-container {
      display: none;
      position: fixed;
      top: 20px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(0, 0, 0, 0.8);
      color: white;
      padding: 15px;
      border-radius: 10px;
      max-width: 500px;
      width: 90%;
      z-index: 10000;
      align-items: center;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
    }

    .alert-icon {
      font-size: 2rem;
      margin-right: 15px;
    }

    .alert-content {
      flex: 1;
    }

    .alert-timer {
      height: 4px;
      background: rgba(255, 255, 255, 0.3);
      margin-top: 10px;
      border-radius: 2px;
      width: 100%;
    }

    .alert-dismiss {
      background: none;
      border: none;
      color: white;
      font-size: 1.5rem;
      cursor: pointer;
      margin-left: 10px;
    }

    /* Nuevos estilos para el efecto de expansión del colorante */
    .dye-wave {
      position: absolute;
      border-radius: 50%;
      background: rgba(0, 170, 255, 0.5);
      transform: translate(-50%, -50%);
      pointer-events: none;
      z-index: 14;
      opacity: 0;
    }
  </style>
</head>
<body>
  <!-- CASO CLÍNICO -->
  <div id="clinical-case">
    <div id="clinical-case-content">
      <h2>CASO CLÍNICO: AGUJERO MACULAR</h2>
      
      <h3>Paciente:</h3>
      <p>Mujer de 62 años, disminución progresiva de agudeza visual en ojo derecho</p>
      
      <h3>Síntomas:</h3>
      <p>Metamorfopsia, escotoma central, pérdida de visión central</p>
      
      <h3>Hallazgos:</h3>
      <ul>
        <li>Agujero macular completo estadio IV</li>
        <li>Desprendimiento vítreo posterior</li>
        <li>Edema macular quístico leve</li>
        <li>Membrana limitante interna engrosada</li>
      </ul>
      
      <h3>OBJETIVOS QUIRÚRGICOS:</h3>
      <ol>
        <li>Realizar vitrectomía posterior</li>
        <li>Peeling de membrana limitante interna</li>
        <li>Aplicación de colorante para visualización</li>
        <li>Inyección de gas SF6 para aplanamiento retiniano</li>
      </ol>
      
      <button id="start-simulation-btn">Iniciar Simulación</button>
    </div>
  </div>

  <div id="container">
    <!-- SISTEMA DE ALERTAS PROFESIONAL -->
    <div id="alert-system">
      <div class="alert-container" id="wall-collision-alert">
        <div class="alert-icon">⚠️</div>
        <div class="alert-content">
          <h3>COLISIÓN CON ESTRUCTURA OCULAR</h3>
          <p>¡Advertencia! El instrumento ha contactado con la pared ocular. Retire inmediatamente y reevalúe la posición.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="retina-detachment-alert">
        <div class="alert-icon">❗</div>
        <div class="alert-content">
          <h3>DESPRENDIMIENTO DE RETINA</h3>
          <p>¡Emergencia! Se ha detectado desprendimiento retiniano. Suspender procedimiento e inyectar gas inmediatamente.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="high-iop-alert">
        <div class="alert-icon">📈</div>
        <div class="alert-content">
          <h3>PRESIÓN INTRAOCULAR ELEVADA</h3>
          <p>PIO > 25 mmHg. Riesgo de daño al nervio óptico. Reducir flujo de irrigación y considerar paracentesis.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="surgery-complete-alert">
        <div class="alert-icon">🏥</div>
        <div class="alert-content">
          <h3>CIRUGÍA COMPLETADA</h3>
          <p>Procedimiento finalizado con éxito. El agujero macular ha sido tratado y la retina está aplanada.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss" id="back-to-menu-btn">Volver al Menú</button>
      </div>
      
      <!-- NUEVAS ALERTAS PARA CADA PASO -->
      <div class="alert-container" id="vitreous-removed-alert">
        <div class="alert-icon">✅</div>
        <div class="alert-content">
          <h3>VITRECTOMÍA COMPLETADA</h3>
          <p>Se ha removido el 100% del vítreo. Proceda a aplicar el colorante para visualizar la membrana limitante interna.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="dye-removed-alert">
        <div class="alert-icon">✅</div>
        <div class="alert-content">
          <h3>COLORANTE REMOVIDO</h3>
          <p>El colorante ha sido removido satisfactoriamente. La membrana limitante interna es visible. Proceda con el peeling.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="peeling-complete-alert">
        <div class="alert-icon">✅</div>
        <div class="alert-content">
          <h3>PEELING COMPLETADO</h3>
          <p>La membrana limitante interna ha sido removida completamente. Proceda a inyectar gas SF6 para aplanamiento retiniano.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="sf6-adjust-alert">
        <div class="alert-icon">🔧</div>
        <div class="alert-content">
          <h3>AJUSTE DE GAS SF6</h3>
          <p>Por favor ajuste el nivel de gas SF6 al 18% usando el control deslizante en el panel de parámetros.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
    </div>

    <!-- MINI MAPA -->
    <div id="miniMapContainer">
      <svg id="eyeCrossSection" viewBox="0 0 800 600" preserveAspectRatio="xMidYMid meet">
        <defs>
          <radialGradient id="bgGradient" cx="50%" cy="50%" r="70%">
            <stop offset="0%" stop-color="#1E293B" />
            <stop offset="100%" stop-color="#0F172A" />
          </radialGradient>
          <radialGradient id="lensGradient" cx="50%" cy="50%" r="50%">
            <stop offset="0%" stop-color="#E0F7FA" stop-opacity="0.9" />
            <stop offset="70%" stop-color="#B2EBF2" stop-opacity="0.8" />
            <stop offset="100%" stop-color="#80DEEA" stop-opacity="0.7" />
          </radialGradient>
          <filter id="dropShadow" x="-20%" y="-20%" width="140%" height="140%">
            <feGaussianBlur in="SourceAlpha" stdDeviation="8" />
            <feOffset dx="0" dy="4" result="offsetblur" />
            <feComponentTransfer>
              <feFuncA type="linear" slope="0.3" />
            </feComponentTransfer>
            <feMerge>
              <feMergeNode />
              <feMergeNode in="SourceGraphic" />
            </feMerge>
          </filter>
        </defs>
        <rect width="800" height="600" fill="url(#bgGradient)" />
        <ellipse id="lens-minimap" cx="400" cy="150" rx="100" ry="65" fill="url(#lensGradient)" filter="url(#dropShadow)" />
        <ellipse cx="400" cy="150" rx="95" ry="60" fill="none" stroke="#B3E5FC" stroke-width="2" />
        <circle id="iris-minimap" cx="400" cy="150" r="55" fill="none" stroke="url(#irisGradient)" stroke-width="15" />
        <circle cx="400" cy="150" r="30" fill="#000000" />
        <circle id="retina-wall" cx="400" cy="300" r="250" fill="none" stroke="#E0E0E0" stroke-width="4" />
        <circle id="retina-minimap" cx="400" cy="300" r="240" fill="#400000" opacity="0.8" />
        <circle id="miniCenter" cx="400" cy="300" r="8" fill="#00f7ff" />
        <line id="probeLight" x1="225" y1="100" x2="350" y2="300" stroke="#FFFFFF" stroke-width="9" stroke-linecap="round" />
        <line id="probeLightInner" x1="225" y1="100" x2="350" y2="300" stroke="#FFFFFF" stroke-width="6" stroke-linecap="round" />
        <line id="probeForceps" x1="575" y1="100" x2="450" y2="300" stroke="#FFFFFF" stroke-width="9" stroke-linecap="round" />
        <line id="probeForcepsInner" x1="575" y1="100" x2="450" y2="300" stroke="#FFFFFF" stroke-width="6" stroke-linecap="round" />
      </svg>
    </div>

    <!-- PANEL DE INSTRUMENTOS -->
    <div class="instrument-panel">
      <button class="toggle-btn" id="btn-vitrectomo">Vitrectomo</button>
      <button class="toggle-btn" id="btn-peeling">Peeling</button>
      <button class="toggle-btn" id="btn-colorante">Colorante</button>
      <button class="toggle-btn" id="btn-sf6">SF6</button>
    </div>

    <!-- PANEL DE PARÁMETROS -->
    <div class="control-panel">
      <div class="vital-sign">
        <span class="vital-label">Presión Intraocular:</span>
        <span class="vital-value normal" id="iop-value">16 mmHg</span>
      </div>
      <div id="iop-gauge" class="gauge-container">
        <div class="gauge-level" id="iop-level"></div>
      </div>
      
      <div class="vital-sign">
        <span class="vital-label">Perfusión Retiniana:</span>
        <span class="vital-value normal" id="perfusion-value">95%</span>
      </div>
      <div id="perfusion-gauge" class="gauge-container">
        <div class="gauge-level" id="perfusion-level"></div>
      </div>
      
      <div class="vital-sign">
        <span class="vital-label">Vitreo Removido:</span>
        <span class="vital-value" id="vitreous-value">0%</span>
      </div>
      <div id="vitreous-gauge" class="gauge-container">
        <div class="gauge-level" id="vitreous-level"></div>
      </div>
      
      <div class="vital-sign">
        <span class="vital-label">Colorante Removido:</span>
        <span class="vital-value" id="dye-removed-value">0%</span>
      </div>
      <div id="dye-removed-gauge" class="gauge-container">
        <div class="gauge-level" id="dye-removed-level"></div>
      </div>
      
      <div class="vital-sign">
        <span class="vital-label">Nivel de SF6:</span>
        <span class="vital-value" id="sf6-value">0%</span>
      </div>
      <div id="sf6-control">
        <input type="range" id="sf6-slider" min="0" max="100" value="0">
      </div>
      
      <div class="vital-sign">
        <span class="vital-label">Estado Retina:</span>
        <span class="vital-value normal" id="retina-status">Estable</span>
      </div>
    </div>

    <!-- JOYSTICKS MEJORADOS -->
    <div id="joystick-light-container" class="joystick-container">
      <div id="joystick-light" class="joystick">
        <div class="joystick-handle"></div>
      </div>
      <div class="slider-container">
        <label>Luz (Z)</label>
        <input type="range" id="endo-z-slider" min="0" max="200" value="50">
      </div>
    </div>

    <div id="joystick-vitrectomo-container" class="joystick-container">
      <div id="joystick-vitrectomo" class="joystick">
        <div class="joystick-handle"></div>
      </div>
      <div class="slider-container">
        <label>Instrumento (Z)</label>
        <input type="range" id="vitrectomo-z-slider" min="-250" max="-50" value="-150">
        <button id="btn-precionar">Accionar</button>
      </div>
    </div>

    <!-- RETINA CENTRAL -->
   <div id="eye-chamber">
    <div class="retina-container" id="retina">
      <div class="retina-sphere">
        <div class="retina-texture"></div>
        <div class="retina-nerve"></div>
        <div class="optic-disc">
          <div class="optic-cup"></div>
        </div>
        <div class="macula"></div>
        <div class="macular-hole"></div>
        <div class="mli-layer"></div>
        <svg class="blood-vessels" viewBox="0 0 600 600">
          <path d="M100,300 Q250,200 300,300 T500,300" fill="none" stroke="#8b0000" stroke-width="2" stroke-opacity="0.7"/>
          <path d="M100,280 Q250,400 300,280 T500,280" fill="none" stroke="#8b0000" stroke-width="1.5" stroke-opacity="0.6"/>
          <path d="M300,100 Q200,250 300,300 T300,500" fill="none" stroke="#8b0000" stroke-width="1.8" stroke-opacity="0.7"/>
        </svg>
      </div>
      <div id="light-mask"></div>
      <div id="light-reflection"></div>
      <div id="light-scatter" class="light-scatter"></div>
      <div id="specular-highlight" class="specular-highlight"></div>
      
        <!-- Vítreo mejorado -->
        <div class="vitreous" id="vitreous"></div>
        
        <!-- Efectos de colorante mejorados -->
        <div id="dye-effects">
          <div class="dye-effect" id="full-dye"></div>
          <div class="dye-effect residual" id="residual-dye"></div>
        </div>
        
        <!-- Instrumento único completo -->
        <div class="instrument-container">
          <div id="instrument-base" class="instrument-base"></div>
          <div id="instrument-rod" class="instrument-rod"></div>
          <div id="instrument-tip" class="instrument-tip"></div>
          <div id="instrument-action-indicator" class="instrument-action-indicator">Vitrectomía</div>
        </div>
        
        <!-- Marcas permanentes -->
        <div id="permanent-marks"></div>
        
        <!-- Capa de peeling -->
        <div id="peeling-layer"></div>
      </div>
    </div>

    <!-- VISUALIZACIÓN OCT - Mejorada -->
    <div id="oct-container">
      <svg id="oct-scan" viewBox="0 0 300 120" preserveAspectRatio="xMidYMid meet">
        <defs>
          <linearGradient id="octGradient" x1="0%" y1="0%" x2="0%" y2="100%">
            <stop offset="0%" stop-color="#000000" stop-opacity="0" />
            <stop offset="100%" stop-color="#000000" stop-opacity="0.8" />
          </linearGradient>
          <pattern id="octPattern" patternUnits="userSpaceOnUse" width="300" height="120">
            <rect width="300" height="120" fill="url(#octGradient)" />
            <path id="oct-retina" class="oct-layer" d="M0,40 Q75,30 150,40 T300,40" />
            <path id="oct-choroid" class="oct-layer" d="M0,60 Q75,50 150,60 T300,60" />
            <path id="oct-sclera" class="oct-layer" d="M0,80 Q75,70 150,80 T300,80" />
            <path id="oct-macular-hole" d="M140,40 L140,60 L160,60 L160,40 Z" />
            <path id="oct-forceps" d="" />
            <path id="oct-mli" d="" />
          </pattern>
        </defs>
        <rect width="300" height="120" fill="url(#octPattern)" />
        <line class="oct-scanline" x1="0" y1="0" x2="300" y2="0" />
      </svg>
    </div>

    <!-- INDICADOR DE PROFUNDIDAD -->
    <div class="depth-indicator" id="depth-indicator">
      Profundidad: Media
    </div>
  </div>
  <script>
    /* ================== VARIABLES GLOBALES ================== */
    let lightJoystickX = 50, lightJoystickY = 50;
    let vitrectomoJoystickX = 50, vitrectomoJoystickY = 50;
    let currentDepth = parseInt(document.getElementById('vitrectomo-z-slider').value);
    let activeInstrument = null;
    let iop = 16; // mmHg
    let perfusion = 95; // %
    let vitreousRemoved = 0; // %
    let retinaStatus = 'Estable';
    let sf6Level = 0; // %
    let procedureStep = 0; // 0: vitrectomía, 1: colorante, 2: peeling, 3: SF6
    let isActionButtonPressed = false;
    let vitrectomyInterval = null;
    let peelingProgress = 0;
    let dyeApplied = 0; // Cantidad de colorante aplicado
    let dyeRemoved = 0; // Cantidad de colorante removido
    let dyeWaves = []; // Para almacenar las ondas de colorante
    let actionPressCount = 0; // Contador de presiones del botón accionar
    let sf6PressCount = 0; // Contador de presiones con SF6 activo
    let mliFlaps = []; // Para almacenar los fragmentos de MLI levantados
    let mliPeelingStarted = false; // Indica si se ha iniciado el peeling de la MLI
    let activeTouchId = null; // Para manejo multitouch
    let joystickSensitivity = 0.7; // Sensibilidad del joystick

    /* ================== INICIALIZACIÓN ================== */
    document.addEventListener('DOMContentLoaded', function() {
      // Mostrar caso clínico al inicio
      document.getElementById('start-simulation-btn').addEventListener('click', function() {
        anime({
          targets: '#clinical-case',
          opacity: 0,
          duration: 500,
          easing: 'easeInOutQuad',
          complete: () => {
            document.getElementById('clinical-case').style.display = 'none';
            initSimulation();
          }
        });
      });
      
      // Botón para volver al menú
      document.getElementById('back-to-menu-btn').addEventListener('click', function() {
        location.reload();
      });
    });

    function initSimulation() {
      // Configurar el instrumento único
      setupSingleInstrument();
      
      // Crear vítreo sin filamentos
      createVitreous();
      
      initJoysticks();
      setupEventListeners();
      setupAlertDismissButtons();
      updateVitals();
      updateEndoLightEffect(50, 50);
      
      requestAnimationFrame(updateInstrumentPositions);
    }

    function createVitreous() {
      const vitreous = document.getElementById('vitreous');
      vitreous.style.opacity = 1;
    }

    function setupSingleInstrument() {
      const retina = document.getElementById('retina');
      const retinaRect = retina.getBoundingClientRect();
      
      // Configurar la base del instrumento (fija en la parte inferior derecha)
      const base = document.getElementById('instrument-base');
      base.style.left = (retinaRect.width * 0.85 - 15) + 'px';
      base.style.top = (retinaRect.height * 0.85 - 15) + 'px';
      
      // Configurar la punta del instrumento (inicialmente en el centro)
      const tip = document.getElementById('instrument-tip');
      tip.style.left = (retinaRect.width / 2) + 'px';
      tip.style.top = (retinaRect.height / 2) + 'px';
      
      // Actualizar el palo para que conecte base y punta
      updateInstrumentRod();
    }

    function updateInstrumentRod() {
      const retina = document.getElementById('retina');
      const retinaRect = retina.getBoundingClientRect();
      const base = document.getElementById('instrument-base');
      const baseRect = base.getBoundingClientRect();
      const tip = document.getElementById('instrument-tip');
      const tipRect = tip.getBoundingClientRect();
      const rod = document.getElementById('instrument-rod');
      
      // Posiciones absolutas
      const baseX = baseRect.left - retinaRect.left + baseRect.width/2;
      const baseY = baseRect.top - retinaRect.top + baseRect.height/2;
      const tipX = tipRect.left - retinaRect.left + tipRect.width/2;
      const tipY = tipRect.top - retinaRect.top + tipRect.height/2;
      
      // Calcular longitud y ángulo
      const dx = tipX - baseX;
      const dy = tipY - baseY;
      const length = Math.sqrt(dx * dx + dy * dy);
      const angle = Math.atan2(dy, dx) * (180 / Math.PI);
      
      // Posicionar y rotar el palo
      rod.style.width = length + 'px';
      rod.style.left = baseX + 'px';
      rod.style.top = baseY + 'px';
      rod.style.transform = `rotate(${angle}deg)`;
      
      // Actualizar OCT
      updateOCTScan();
    }

    function updateOCTScan() {
      const retina = document.getElementById('retina');
      const retinaRect = retina.getBoundingClientRect();
      const octForceps = document.getElementById('oct-forceps');
      const octMli = document.getElementById('oct-mli');
      const tip = document.getElementById('instrument-tip');
      const tipRect = tip.getBoundingClientRect();
      
      // Calcular posición relativa de la punta del instrumento
      const tipRelX = (tipRect.left - retinaRect.left + tipRect.width/2) / retinaRect.width * 300;
      const tipRelY = (tipRect.top - retinaRect.top + tipRect.height/2) / retinaRect.height * 120;
      
      // Capas de retina normales con agujero macular
      let retinaPath = "M0,40 Q75,30 140,40 L140,60 L160,60 L160,40 Q225,30 300,40";
      let choroidPath = "M0,60 Q75,50 150,60 T300,60";
      let scleraPath = "M0,80 Q75,70 150,80 T300,80";
      
      // Mostrar pinza en OCT solo si está activa
      if (activeInstrument === 'peeling') {
        // Efecto de pinza en OCT - ahora aparece por encima de la retina
        let forcepsPath = `M${tipRelX},${Math.max(10, tipRelY - 20)} L${tipRelX},${Math.max(15, tipRelY - 15)}`;
        octForceps.setAttribute('d', forcepsPath);
        
        // Mostrar MLI en OCT cuando se hace peeling
        let mliPath = "M0,40 Q75,30 150,40 T300,40";
        
        // Si estamos haciendo peeling, mostrar deformación de la MLI
        if (isActionButtonPressed && mliPeelingStarted) {
          // Calcular deformación basada en la posición de la pinza
          const deformation = 5 * (1 - (peelingProgress / 100));
          mliPath = `M0,40 Q75,${30 + deformation} 150,${40 + deformation} T300,40`;
          
          // Mostrar la MLI levantada en el área del peeling
          octMli.setAttribute('d', `M${tipRelX - 30},${40 + deformation} Q${tipRelX},${40 + deformation + 5} ${tipRelX + 30},${40 + deformation}`);
        } else {
          octMli.setAttribute('d', '');
        }
      } else {
        octForceps.setAttribute('d', '');
        octMli.setAttribute('d', '');
      }
      
      // Actualizar capas de retina
      document.getElementById('oct-retina').setAttribute('d', retinaPath);
      document.getElementById('oct-choroid').setAttribute('d', choroidPath);
      document.getElementById('oct-sclera').setAttribute('d', scleraPath);
    }

    /* ================== SISTEMA DE ALERTAS MEJORADO ================== */
    function setupAlertDismissButtons() {
      document.querySelectorAll('.alert-dismiss').forEach(btn => {
        btn.addEventListener('click', function() {
          const alert = this.parentElement;
          anime({
            targets: alert,
            opacity: 0,
            translateY: -20,
            duration: 300,
            easing: 'easeOutQuad',
            complete: () => {
              alert.style.display = 'none';
              alert.style.opacity = '1';
              alert.style.transform = 'translateY(0)';
            }
          });
        });
      });
    }
    
    function showAlert(alertId, duration = 5000) {
      const alert = document.getElementById(alertId);
      if (!alert) return;
      
      alert.style.display = 'flex';
      anime({
        targets: alert,
        opacity: [0, 1],
        translateY: [-20, 0],
        duration: 300,
        easing: 'easeOutQuad'
      });
      
      const timer = alert.querySelector('.alert-timer');
      if (timer) {
        anime({
          targets: timer,
          width: ['100%', '0%'],
          duration: duration,
          easing: 'linear'
        });
      }
      
      if (duration > 0) {
        setTimeout(() => {
          anime({
            targets: alert,
            opacity: 0,
            translateY: -20,
            duration: 300,
            easing: 'easeOutQuad',
            complete: () => {
              alert.style.display = 'none';
              alert.style.opacity = '1';
              alert.style.transform = 'translateY(0)';
            }
          });
        }, duration);
      }
    }
    
    function checkWallCollision() {
      const miniMapRect = document.getElementById('miniMapContainer').getBoundingClientRect();
      const retinaRect = document.getElementById('retina').getBoundingClientRect();
      
      // Verificar colisión para la punta del instrumento
      const tip = document.getElementById('instrument-tip');
      const tipRect = tip.getBoundingClientRect();
      const tipCenterX = tipRect.left + tipRect.width/2;
      const tipCenterY = tipRect.top + tipRect.height/2;
      
      // Coordenadas relativas al mini mapa
      const relX = (tipCenterX - retinaRect.left) / retinaRect.width * miniMapRect.width;
      const relY = (tipCenterY - retinaRect.top) / retinaRect.height * miniMapRect.height;
      
      // Verificar contacto con retina en mini mapa
      const retinaWall = document.getElementById('retina-wall');
      const retinaX = 400 * (miniMapRect.width / 800);
      const retinaY = 300 * (miniMapRect.height / 600);
      const retinaRadius = 250 * (miniMapRect.width / 800);
      
      const distance = Math.sqrt(Math.pow(relX - retinaX, 2) + Math.pow(relY - retinaY, 2));
      
      // Solo mostrar alerta si estamos en el borde (90% del radio)
      if (distance > retinaRadius * 1.1) {
        showAlert('wall-collision-alert');
        return true;
      }
      
      // Verificar colisión para el endoiluminador
      const lightPosX = retinaRect.left + retinaRect.width * lightJoystickX / 100;
      const lightPosY = retinaRect.top + retinaRect.height * lightJoystickY / 100;
      
      // Coordenadas relativas al mini mapa
      const lightRelX = (lightPosX - retinaRect.left) / retinaRect.width * miniMapRect.width;
      const lightRelY = (lightPosY - retinaRect.top) / retinaRect.height * miniMapRect.height;
      
      // Verificar contacto con retina para el endoiluminador
      const lightDistance = Math.sqrt(Math.pow(lightRelX - retinaX, 2) + Math.pow(lightRelY - retinaY, 2));
      
      if (lightDistance > retinaRadius * 1.1) {
        showAlert('wall-collision-alert');
        return true;
      }
      
      return false;
    }
    
    function checkRetinaDetachment() {
      if (iop > 28) {
        showAlert('retina-detachment-alert');
        return true;
      }
      return false;
    }
    
    function checkHighIOP() {
      if (iop > 25) {
        showAlert('high-iop-alert', 0);
        document.querySelector('.retina-nerve').style.backgroundColor = 'rgba(255,255,255,0.8)';
        document.querySelector('.optic-disc').style.boxShadow = 'inset 0 0 20px rgba(255,255,255,0.6), 0 0 25px rgba(255,255,255,0.5)';
        return true;
      } else {
        document.querySelector('.retina-nerve').style.backgroundColor = '';
        document.querySelector('.optic-disc').style.boxShadow = 'inset 0 0 20px rgba(220,80,80,0.6), 0 0 25px rgba(220,80,80,0.5)';
        document.getElementById('high-iop-alert').style.display = 'none';
        return false;
      }
    }

    /* ================== CONTROL DE JOYSTICKS MEJORADO ================== */
    function initJoysticks() {
      const joystickVitrectomo = document.getElementById('joystick-vitrectomo');
      initJoystick(joystickVitrectomo, (x, y) => {
        vitrectomoJoystickX = x;
        vitrectomoJoystickY = y;
        updateInstrumentPosition(x, y);
        updateMiniLeftLine(x, y);
      }, 'instrument');
      
      const joystickLight = document.getElementById('joystick-light');
      initJoystick(joystickLight, (x, y) => {
        lightJoystickX = x;
        lightJoystickY = y;
        updateEndoLightEffect(x, y);
        updateMiniRightLine(x, y);
      }, 'light');
    }
    
    function initJoystick(joystickElement, updateCallback, type) {
      const handle = joystickElement.querySelector('.joystick-handle');
      const rect = joystickElement.getBoundingClientRect();
      const centerX = rect.width / 2;
      const centerY = rect.height / 2;
      const maxDistance = rect.width / 2 * joystickSensitivity;
      
      let isDragging = false;
      let touchId = null;
      
      function handleStart(e) {
        e.preventDefault();
        if (isDragging) return;
        
        isDragging = true;
        if (e.touches) {
          // Para multitouch, encontrar el touch correcto
          const touches = Array.from(e.touches);
          const touch = touches.find(t => {
            const element = document.elementFromPoint(t.clientX, t.clientY);
            return element === joystickElement || element === handle;
          });
          
          if (!touch) return;
          touchId = touch.identifier;
          activeTouchId = touchId;
          handleMove(e);
        } else {
          handleMove(e);
        }
      }
      
      function handleMove(e) {
        if (!isDragging) return;
        
        let clientX, clientY;
        
        if (e.touches) {
          // Encontrar el touch específico
          const touches = Array.from(e.touches);
          const touch = touches.find(t => t.identifier === touchId);
          if (!touch) return;
          
          clientX = touch.clientX;
          clientY = touch.clientY;
        } else {
          clientX = e.clientX;
          clientY = e.clientY;
        }
        
        const bounds = joystickElement.getBoundingClientRect();
        const x = clientX - bounds.left;
        const y = clientY - bounds.top;
        
        let deltaX = x - centerX;
        let deltaY = y - centerY;
        const distance = Math.sqrt(deltaX * deltaX + deltaY * deltaY);
        
        if (distance > maxDistance) {
          const angle = Math.atan2(deltaY, deltaX);
          deltaX = Math.cos(angle) * maxDistance;
          deltaY = Math.sin(angle) * maxDistance;
        }
        
        // Suavizar el movimiento con GSAP
        gsap.to(handle, {
          x: deltaX,
          y: deltaY,
          duration: 0.1,
          ease: "power1.out"
        });
        
        // Normalizar las coordenadas (0-100)
        const normalizedX = ((deltaX + maxDistance) / (2 * maxDistance)) * 100;
        const normalizedY = ((deltaY + maxDistance) / (2 * maxDistance)) * 100;
        
        updateCallback(normalizedX, normalizedY);
      }
      
      function handleEnd(e) {
        if (!isDragging) return;
        
        // Verificar si el touch que terminó es el que controla este joystick
        if (e.touches) {
          const touches = Array.from(e.changedTouches);
          const touch = touches.find(t => t.identifier === touchId);
          if (!touch) return;
        }
        
        isDragging = false;
        touchId = null;
        activeTouchId = null;
        
        // Suavizar el retorno a la posición central
        gsap.to(handle, {
          x: 0,
          y: 0,
          duration: 0.3,
          ease: "elastic.out(1, 0.5)"
        });
        
        // Resetear posición cuando se suelta
        updateCallback(50, 50);
      }
      
      // Eventos táctiles
      joystickElement.addEventListener('touchstart', handleStart, { passive: false });
      document.addEventListener('touchmove', (e) => {
        if (activeTouchId !== null) {
          const touches = Array.from(e.touches);
          if (touches.some(t => t.identifier === activeTouchId)) {
            handleMove(e);
          }
        }
      }, { passive: false });
      
      document.addEventListener('touchend', handleEnd);
      document.addEventListener('touchcancel', handleEnd);
      
      // Eventos de ratón
      joystickElement.addEventListener('mousedown', handleStart);
      document.addEventListener('mousemove', handleMove);
      document.addEventListener('mouseup', handleEnd);
    }

    /* ================== ACTUALIZACIÓN DE POSICIONES ================== */
    function updateInstrumentPositions() {
      updateInstrumentPosition(vitrectomoJoystickX, vitrectomoJoystickY);
      requestAnimationFrame(updateInstrumentPositions);
    }
    
    function updateInstrumentPosition(normX, normY) {
      const retina = document.getElementById('retina');
      const retinaRect = retina.getBoundingClientRect();
      const base = document.getElementById('instrument-base');
      const tip = document.getElementById('instrument-tip');
      
      // Calcular posición de la punta basada en el joystick
      const maxOffset = retinaRect.width * 0.4; // Reducir el rango para que no salga de la retina
      const offsetX = (normX - 50) / 50 * maxOffset;
      const offsetY = (normY - 50) / 50 * maxOffset;
      
      // Posición absoluta de la punta
      const tipX = retinaRect.width / 2 + offsetX;
      const tipY = retinaRect.height / 2 + offsetY;
      
      // Posición de la base (fija en la parte inferior derecha)
      const baseX = retinaRect.width * 0.85;
      const baseY = retinaRect.height * 0.85;
      
      // Actualizar posición de la punta
      tip.style.left = tipX + 'px';
      tip.style.top = tipY + 'px';
      
      // Actualizar el palo para que conecte base y punta
      updateInstrumentRod();
      
      // Actualizar profundidad
      tip.style.transform = `translateZ(${currentDepth}px)`;
      
      checkWallCollision();
      updateDepthIndicator();
      
      // Actualizar visualización OCT
      updateOCTScan();
      
      // Si estamos haciendo peeling, actualizar la posición de los fragmentos de MLI
      if (activeInstrument === 'peeling' && mliPeelingStarted) {
        updateMliFlapsPosition(tipX, tipY);
      }
    }
    
    function updateDepthIndicator() {
      const depthIndicator = document.getElementById('depth-indicator');
      
      if (currentDepth > -80) {
        depthIndicator.textContent = "Profundidad: Superficial";
        depthIndicator.style.setProperty('--depth-color', '#3b82f6');
      } else if (currentDepth > -150) {
        depthIndicator.textContent = "Profundidad: Media";
        depthIndicator.style.setProperty('--depth-color', '#10b981');
      } else {
        depthIndicator.textContent = "Profundidad: Profunda";
        depthIndicator.style.setProperty('--depth-color', '#ef4444');
      }
    }
    
    function updateEndoLightEffect(normX, normY) {
      document.documentElement.style.setProperty('--light-x', normX + '%');
      document.documentElement.style.setProperty('--light-y', normY + '%');
      
      const zVal = parseInt(document.getElementById('endo-z-slider').value);
      const retinaRect = document.getElementById('retina').getBoundingClientRect();
      const newLightSize = 20 + (retinaRect.width - 20) * (zVal / 200);
      
      document.documentElement.style.setProperty('--light-size', newLightSize + 'px');
      document.getElementById('light-reflection').style.opacity = 0.7;
      
      if (zVal > 100) {
        document.getElementById('light-scatter').classList.add('active');
        document.getElementById('specular-highlight').classList.add('active');
      } else {
        document.getElementById('light-scatter').classList.remove('active');
        document.getElementById('specular-highlight').classList.remove('active');
      }
    }
    
    function updateMiniLeftLine(normX, normY) {
      const defaultTipX = 350;
      const defaultTipY = 300;
      const scaleX = 2.5;
      const scaleY = 1.8;
      const offsetX = (normX - 50) * scaleX;
      const offsetY = (normY - 50) * scaleY;
      let tipX = defaultTipX + offsetX;
      let tipY = defaultTipY + offsetY;
      
      const miniLeft = document.getElementById('probeLight');
      const miniLeftInner = document.getElementById('probeLightInner');
      if(miniLeft && miniLeftInner) {
        miniLeft.setAttribute('x2', tipX);
        miniLeftInner.setAttribute('x2', tipX);
        miniLeft.setAttribute('y2', tipY);
        miniLeftInner.setAttribute('y2', tipY);
      }
    }
    
    function updateMiniRightLine(normX, normY) {
      const defaultTipX = 450;
      const defaultTipY = 300;
      const scaleX = 2.5;
      const scaleY = 1.8;
      const offsetX = (normX - 50) * scaleX;
      const offsetY = (normY - 50) * scaleY;
      let tipX = defaultTipX + offsetX;
      let tipY = defaultTipY + offsetY;
      
      const miniRight = document.getElementById('probeForceps');
      const miniRightInner = document.getElementById('probeForcepsInner');
      if(miniRight && miniRightInner) {
        miniRight.setAttribute('x2', tipX);
        miniRightInner.setAttribute('x2', tipX);
        miniRight.setAttribute('y2', tipY);
        miniRightInner.setAttribute('y2', tipY);
      }
    }

    /* ================== GESTIÓN DE INSTRUMENTOS ================== */
    function setupEventListeners() {
      document.getElementById('btn-vitrectomo').addEventListener('click', () => toggleInstrument('btn-vitrectomo', 'vitrectomo'));
      document.getElementById('btn-peeling').addEventListener('click', () => toggleInstrument('btn-peeling', 'peeling'));
      document.getElementById('btn-colorante').addEventListener('click', () => toggleInstrument('btn-colorante', 'colorante'));
      document.getElementById('btn-sf6').addEventListener('click', () => toggleInstrument('btn-sf6', 'sf6'));
      
      // Control deslizante de SF6
      document.getElementById('sf6-slider').addEventListener('input', function() {
        if (procedureStep === 3) {
          sf6Level = parseInt(this.value);
          document.getElementById('sf6-value').textContent = `${sf6Level}%`;
          document.getElementById('sf6-level').style.width = `${sf6Level}%`;
          
          // Afectar parámetros
          iop = 10 + (sf6Level / 100 * 15); // IOP entre 10 y 25
          perfusion = 70 + (sf6Level / 100 * 30); // Perfusión entre 70 y 100
          
          // Mostrar alerta cuando se ajusta al 18%
          if (Math.abs(sf6Level - 18) < 2 && !document.getElementById('sf6-adjust-alert').classList.contains('completed')) {
            document.getElementById('sf6-adjust-alert').classList.add('completed');
            showAlert('sf6-adjust-alert');
            
            // Crear efecto de burbujas de gas mejorado
            if (sf6Level >= 18) {
              createSF6Bubbles();
            }
          }
        }
      });
      
      // Mejorar los eventos para el botón de acción
      const actionButton = document.getElementById('btn-precionar');
      
      // Eventos de ratón
      actionButton.addEventListener('mousedown', startAction);
      actionButton.addEventListener('mouseup', stopAction);
      actionButton.addEventListener('mouseleave', stopAction);
      
      // Eventos táctiles mejorados
      actionButton.addEventListener('touchstart', function(e) {
        e.preventDefault();
        startAction();
      }, { passive: false });
      
      actionButton.addEventListener('touchend', function(e) {
        e.preventDefault();
        stopAction();
      });
      
      document.getElementById('vitrectomo-z-slider').addEventListener('input', function() {
        currentDepth = parseInt(this.value);
        updateInstrumentPosition(vitrectomoJoystickX, vitrectomoJoystickY);
      });
      
      document.getElementById('endo-z-slider').addEventListener('input', function() {
        updateEndoLightEffect(lightJoystickX, lightJoystickY);
      });
    }
    
    function createSF6Bubbles() {
      const retina = document.getElementById('retina');
      const retinaRect = retina.getBoundingClientRect();
      
      // Crear múltiples burbujas de gas mejoradas
      for (let i = 0; i < 100; i++) {
        const bubble = document.createElement('div');
        bubble.className = 'sf6-bubble';
        
        // Tamaño aleatorio entre 5 y 30px
        const size = 5 + Math.random() * 25;
        bubble.style.width = `${size}px`;
        bubble.style.height = `${size}px`;
        
        // Posición aleatoria en la retina
        const x = Math.random() * retinaRect.width;
        const y = Math.random() * retinaRect.height;
        bubble.style.left = `${x}px`;
        bubble.style.top = `${y}px`;
        
        // Duración aleatoria de la animación
        const duration = 2 + Math.random() * 3;
        bubble.style.animationDuration = `${duration}s`;
        
        // Retraso aleatorio para efecto más natural
        bubble.style.animationDelay = `${Math.random() * 2}s`;
        
        retina.appendChild(bubble);
        
        // Eliminar la burbuja después de la animación
        setTimeout(() => {
          if (bubble.parentNode) {
            bubble.parentNode.removeChild(bubble);
          }
        }, (duration + parseFloat(bubble.style.animationDelay)) * 1000);
      }
    }
    
    function startAction() {
      if (!activeInstrument) return;
      
      isActionButtonPressed = true;
      
      // Mostrar indicador de acción
      const actionIndicator = document.getElementById('instrument-action-indicator');
      if (actionIndicator) {
        actionIndicator.classList.add('active');
        
        // Actualizar texto según el instrumento activo
        if (activeInstrument === 'vitrectomo') {
          actionIndicator.textContent = procedureStep === 1 ? "Removiendo colorante" : "Vitrectomía";
        } else if (activeInstrument === 'sf6') {
          actionIndicator.textContent = "Inyectando SF6";
          sf6PressCount++;
          
          // Si se presiona 5 veces con SF6 activo, completar cirugía
          if (sf6PressCount >= 5) {
            showAlert('surgery-complete-alert');
            return;
          }
        } else if (activeInstrument === 'peeling') {
          actionIndicator.textContent = "Peeling MLI";
          // Mostrar la MLI cuando se activa el peeling
          document.querySelector('.mli-layer').classList.add('visible');
          
          // Iniciar el peeling si no se ha iniciado
          if (!mliPeelingStarted) {
            mliPeelingStarted = true;
            createInitialMliFlap();
          }
          
          // Verificar si está en el borde de la membrana
          const tip = document.getElementById('instrument-tip');
          const tipRect = tip.getBoundingClientRect();
          const retina = document.getElementById('retina');
          const retinaRect = retina.getBoundingClientRect();
          
          const tipX = tipRect.left + tipRect.width/2 - retinaRect.left;
          const tipY = tipRect.top + tipRect.height/2 - retinaRect.top;
          
          const centerX = retinaRect.width/2;
          const centerY = retinaRect.height/2;
          
          const distance = Math.sqrt(Math.pow(tipX - centerX, 2) + Math.pow(tipY - centerY, 2));
          const radius = retinaRect.width * 0.2; // Radio de la membrana (40% del diámetro)
          
          // Si está cerca del borde de la membrana, aumentar progreso
          if (Math.abs(distance - radius) < 20) {
            peelingProgress = Math.min(100, peelingProgress + 1);
            
            // Crear nuevo fragmento de MLI levantado
            createMliFlap(tipX, tipY);
            
            // Reducir la opacidad de la MLI mientras se hace peeling
            const mliOpacity = 1 - (peelingProgress / 100);
            document.querySelector('.mli-layer').style.opacity = mliOpacity;
            
            // Si se completa el peeling, pasar a la siguiente fase
            if (peelingProgress >= 100) {
              procedureStep = 3;
              document.getElementById('btn-peeling').classList.remove('active');
              document.getElementById('btn-sf6').classList.add('active');
              activeInstrument = 'sf6';
              updateInstrumentTipAppearance();
              
              // Ocultar la MLI
              document.querySelector('.mli-layer').classList.remove('visible');
              
              // Mostrar control de SF6
              document.getElementById('sf6-control').style.display = 'block';
              
              // Mostrar alerta de peeling completado
              showAlert('peeling-complete-alert');
              
              // Mostrar alerta para ajustar SF6 al 18%
              setTimeout(() => {
                showAlert('sf6-adjust-alert');
              }, 1500);
            }
          }
        } else if (activeInstrument === 'colorante') {
          actionIndicator.textContent = "Aplicando colorante";
          
          // Incrementar contador de presiones
          actionPressCount++;
          
          // Crear efecto de onda de colorante
          const tip = document.getElementById('instrument-tip');
          const tipRect = tip.getBoundingClientRect();
          createDyeWave(tipRect.left + tipRect.width/2, tipRect.top + tipRect.height/2);
          
          // Aumentar opacidad del colorante gradualmente (33% por cada presión)
          const newOpacity = Math.min(1, actionPressCount * 0.33);
          document.getElementById('full-dye').style.opacity = newOpacity;
          
          // Mostrar la membrana cuando se tiñe completamente
          if (newOpacity >= 1) {
            document.querySelector('.mli-layer').classList.add('visible');
          }
          
          // Si se han completado 3 presiones, activar vitrectomo para remoción
          if (actionPressCount >= 3) {
            // Cambiar a modo de remoción con vitrectomo
            document.getElementById('btn-colorante').classList.remove('active');
            document.getElementById('btn-vitrectomo').classList.add('active');
            activeInstrument = 'vitrectomo';
            updateInstrumentTipAppearance();
            
            // Cambiar el texto del indicador
            if (actionIndicator) {
              actionIndicator.textContent = "Removiendo colorante";
            }
            
            // Resetear contador
            actionPressCount = 0;
          }
        }
      }
      
      // Iniciar el efecto continuo solo para vitrectomo
      if (activeInstrument === 'vitrectomo') {
        vitrectomyInterval = setInterval(() => {
          if (!isActionButtonPressed || !activeInstrument) {
            clearInterval(vitrectomyInterval);
            return;
          }
          
          const retina = document.getElementById('retina');
          const retinaRect = retina.getBoundingClientRect();
          const tip = document.getElementById('instrument-tip');
          const tipRect = tip.getBoundingClientRect();
          
          // Calcular posición relativa al centro de la retina
          const offsetX = tipRect.left - retinaRect.left + tipRect.width/2 - retinaRect.width/2;
          const offsetY = tipRect.top - retinaRect.top + tipRect.height/2 - retinaRect.height/2;
          
          // Crear efecto visual según el instrumento activo
          if (activeInstrument === 'vitrectomo') {
            createVitrectomyEffect(tipRect.left + tipRect.width/2, tipRect.top + tipRect.height/2);
            
            // Actualizar parámetros fisiológicos
            iop = Math.max(18, iop - 0.1);
            perfusion = Math.min(100, perfusion + 0.05);
            
            // Aumentar porcentaje de vítreo removido (solo en fase de vitrectomía)
            if (procedureStep === 0) {
              vitreousRemoved = Math.min(100, vitreousRemoved + 0.5);
              document.getElementById('vitreous-value').textContent = `${Math.round(vitreousRemoved)}%`;
              document.getElementById('vitreous-level').style.width = `${vitreousRemoved}%`;
              
              // Actualizar opacidad del vítreo
              updateVitreousOpacity();
              
              // Si se ha removido suficiente vítreo, pasar a la siguiente fase
              if (vitreousRemoved >= 100 && procedureStep === 0) {
                procedureStep = 1;
                document.getElementById('btn-vitrectomo').classList.remove('active');
                document.getElementById('btn-colorante').classList.add('active');
                activeInstrument = 'colorante';
                updateInstrumentTipAppearance();
                
                // Mostrar alerta de vitrectomía completada
                showAlert('vitreous-removed-alert');
              }
            }
            
            // En fase de remoción de colorante
            if (procedureStep === 1) {
              // Aumentar colorante removido gradualmente
              dyeRemoved = Math.min(100, dyeRemoved + 0.5);
              document.getElementById('dye-removed-value').textContent = `${Math.round(dyeRemoved)}%`;
              document.getElementById('dye-removed-level').style.width = `${dyeRemoved}%`;
              
              // Actualizar efecto de colorante
              const fullDye = document.getElementById('full-dye');
              fullDye.style.opacity = (1 - (dyeRemoved / 100)) * 0.7;
              
              // Crear partículas de colorante removido
              createDyeRemovalParticles(tipRect.left + tipRect.width/2, tipRect.top + tipRect.height/2);
              
              // Cuando se ha removido suficiente colorante, pasar a la fase de peeling
              if (dyeRemoved >= 100) {
                procedureStep = 2;
                document.getElementById('btn-vitrectomo').classList.remove('active');
                document.getElementById('btn-peeling').classList.add('active');
                activeInstrument = 'peeling';
                updateInstrumentTipAppearance();
                
                // Mostrar la MLI
                document.querySelector('.mli-layer').classList.add('visible');
                
                // Mostrar alerta de colorante removido
                showAlert('dye-removed-alert');
              }
            }
          }
          
          // Actualizar UI inmediatamente
          updateVitals();
          
        }, 100); // Ejecutar cada 100ms mientras se mantenga presionado
      }
    }
    
    function createInitialMliFlap() {
      const retina = document.getElementById('retina');
      const retinaRect = retina.getBoundingClientRect();
      const centerX = retinaRect.width / 2;
      const centerY = retinaRect.height / 2;
      
      // Crear el primer fragmento de MLI levantado en el centro
      const flap = document.createElement('div');
      flap.className = 'mli-flap';
      flap.style.width = '30px';
      flap.style.height = '30px';
      flap.style.left = `${centerX - 15}px`;
      flap.style.top = `${centerY - 15}px`;
      flap.style.borderRadius = '50%';
      flap.style.transform = 'translateZ(10px)';
      
      retina.appendChild(flap);
      mliFlaps.push(flap);
    }
    
    function createMliFlap(x, y) {
      const retina = document.getElementById('retina');
      const retinaRect = retina.getBoundingClientRect();
      
      // Crear un nuevo fragmento de MLI levantado
      const flap = document.createElement('div');
      flap.className = 'mli-flap';
      
      // Tamaño aleatorio entre 10 y 20px
      const size = 10 + Math.random() * 10;
      flap.style.width = `${size}px`;
      flap.style.height = `${size}px`;
      
      // Posición cerca de la punta del instrumento
      flap.style.left = `${x - size/2 + (Math.random() * 20 - 10)}px`;
      flap.style.top = `${y - size/2 + (Math.random() * 20 - 10)}px`;
      
      // Elevación aleatoria
      const elevation = 5 + Math.random() * 15;
      flap.style.transform = `translateZ(${elevation}px)`;
      
      // Opacidad basada en el progreso del peeling
      flap.style.opacity = 0.7 - (peelingProgress / 200);
      
      // Añadir al DOM y a la lista de flaps
      retina.appendChild(flap);
      mliFlaps.push(flap);
      
      // Animación de levantamiento
      anime({
        targets: flap,
        translateZ: [0, elevation],
        opacity: [0, 0.7 - (peelingProgress / 200)],
        duration: 500,
        easing: 'easeOutQuad'
      });
    }
    
    function updateMliFlapsPosition(tipX, tipY) {
      // Actualizar la posición de los fragmentos de MLI para que sigan la punta del instrumento
      mliFlaps.forEach(flap => {
        const flapRect = flap.getBoundingClientRect();
        const flapCenterX = flapRect.left + flapRect.width/2;
        const flapCenterY = flapRect.top + flapRect.height/2;
        
        // Calcular distancia a la punta del instrumento
        const distanceX = tipX - flapCenterX;
        const distanceY = tipY - flapCenterY;
        const distance = Math.sqrt(distanceX * distanceX + distanceY * distanceY);
        
        // Si está cerca, mover el fragmento hacia la punta
        if (distance < 50) {
          const moveX = distanceX * 0.1;
          const moveY = distanceY * 0.1;
          
          anime({
            targets: flap,
            translateX: `+=${moveX}`,
            translateY: `+=${moveY}`,
            duration: 100,
            easing: 'linear'
          });
        }
      });
    }
    
    function createDyeWave(x, y) {
      const retina = document.getElementById('retina');
      const retinaRect = retina.getBoundingClientRect();
      
      const wave = document.createElement('div');
      wave.className = 'dye-wave';
      
      // Posicionar la onda en la punta del instrumento
      const waveX = x - retinaRect.left;
      const waveY = y - retinaRect.top;
      
      wave.style.left = `${waveX}px`;
      wave.style.top = `${waveY}px`;
      
      // Animación de la onda expandiéndose
      anime({
        targets: wave,
        width: ['10px', '300px'],
        height: ['10px', '300px'],
        opacity: [0.8, 0],
        duration: 1500,
        easing: 'easeOutQuad',
        complete: () => {
          if (wave.parentNode) {
            wave.parentNode.removeChild(wave);
          }
        }
      });
      
      retina.appendChild(wave);
    }
    
    function createDyeRemovalParticles(x, y) {
      const retina = document.getElementById('retina');
      const retinaRect = retina.getBoundingClientRect();
      
      // Crear partículas de colorante removido
      for (let i = 0; i < 3; i++) {
        const particle = document.createElement('div');
        particle.className = 'dye-particle';
        particle.style.left = (x - retinaRect.left + (Math.random()*20 - 10)) + 'px';
        particle.style.top = (y - retinaRect.top + (Math.random()*20 - 10)) + 'px';
        
        // Efecto de partícula moviéndose hacia afuera
        const angle = Math.random() * Math.PI * 2;
        const distance = 30 + Math.random() * 50;
        const targetX = Math.cos(angle) * distance;
        const targetY = Math.sin(angle) * distance;
        
        anime({
          targets: particle,
          translateX: targetX,
          translateY: targetY,
          opacity: [1, 0],
          duration: 1500,
          easing: 'easeOutQuad',
          complete: () => {
            if (particle.parentNode) {
              particle.parentNode.removeChild(particle);
            }
          }
        });
        
        retina.appendChild(particle);
      }
    }
    
    function stopAction() {
      isActionButtonPressed = false;
      
      // Ocultar indicador de acción
      const actionIndicator = document.getElementById('instrument-action-indicator');
      if (actionIndicator) {
        actionIndicator.classList.remove('active');
      }
      
      // Detener el efecto continuo
      if (vitrectomyInterval) {
        clearInterval(vitrectomyInterval);
        vitrectomyInterval = null;
      }
    }
    
    function createVitrectomyEffect(x, y) {
      const retina = document.getElementById('retina');
      const retinaRect = retina.getBoundingClientRect();
      
      // Efecto de vitrectomía
      const vitrectomyEffect = document.createElement('div');
      vitrectomyEffect.className = 'vitrectomy-effect';
      vitrectomyEffect.style.left = (x - retinaRect.left - 30) + 'px';
      vitrectomyEffect.style.top = (y - retinaRect.top - 30) + 'px';
      
      anime({
        targets: vitrectomyEffect,
        scale: [0, 1],
        opacity: [0.8, 0],
        duration: 800,
        easing: 'easeOutQuad',
        complete: () => {
          if (vitrectomyEffect.parentNode) {
            vitrectomyEffect.parentNode.removeChild(vitrectomyEffect);
          }
        }
      });
      
      retina.appendChild(vitrectomyEffect);
      
      // Partículas de succión
      for (let i = 0; i < 8; i++) {
        const particle = document.createElement('div');
        particle.className = 'vitreous-particle';
        particle.style.left = (x - retinaRect.left + (Math.random()*20 - 10)) + 'px';
        particle.style.top = (y - retinaRect.top + (Math.random()*20 - 10)) + 'px';
        particle.style.setProperty('--tx', Math.random() * 40 - 20);
        particle.style.setProperty('--ty', Math.random() * 40 - 20);
        
        anime({
          targets: particle,
          translateX: Math.random() * 40 - 20,
          translateY: Math.random() * 40 - 20,
          opacity: [1, 0],
          duration: 1500,
          easing: 'easeOutQuad',
          complete: () => {
            if (particle.parentNode) {
              particle.parentNode.removeChild(particle);
            }
          }
        });
        
        retina.appendChild(particle);
      }
    }
    
    function toggleInstrument(btnId, instrumentType) {
      const btn = document.getElementById(btnId);
      
      if(btn.classList.contains('active')) {
        btn.classList.remove('active');
        activeInstrument = null;
        isActionButtonPressed = false;
        
        // Resetear apariencia de la punta
        const tip = document.getElementById('instrument-tip');
        tip.className = 'instrument-tip';
        tip.classList.remove('grasping');
        
        // Detener cualquier intervalo activo
        if (vitrectomyInterval) {
          clearInterval(vitrectomyInterval);
          vitrectomyInterval = null;
        }
      } else {
        document.querySelectorAll('.toggle-btn').forEach(b => b.classList.remove('active'));
        
        btn.classList.add('active');
        activeInstrument = instrumentType;
        
        // Actualizar apariencia de la punta según el instrumento
        updateInstrumentTipAppearance();
      }
    }
    
    function updateInstrumentTipAppearance() {
      const tip = document.getElementById('instrument-tip');
      tip.className = 'instrument-tip';
      
      if (activeInstrument === 'vitrectomo') {
        tip.classList.add('active-vitrectomo');
      } else if (activeInstrument === 'sf6') {
        tip.classList.add('active-sf6');
      } else if (activeInstrument === 'peeling') {
        tip.classList.add('active-forceps');
        tip.classList.add('grasping');
      } else if (activeInstrument === 'colorante') {
        tip.classList.add('active-colorante');
      }
    }

    /* ================== ACTUALIZACIÓN DE PARÁMETROS ================== */
    function updateVitals() {
      // Variación aleatoria de los parámetros
      iop += (Math.random() - 0.5) * 0.1;
      perfusion += (Math.random() - 0.5) * 0.2;
      
      // Efecto de instrumentos activos
      if (activeInstrument === 'vitrectomo') {
        iop -= 0.05;
        if (procedureStep === 0) {
          vitreousRemoved = Math.min(100, vitreousRemoved + 0.1);
        }
      }
      
      // Efecto de SF6
      if (sf6Level > 0) {
        iop = Math.max(iop, 12 + sf6Level / 20);
        perfusion = Math.min(perfusion + sf6Level / 100, 100);
      }
      
      // Verificar alertas basadas en parámetros
      checkRetinaDetachment();
      checkHighIOP();
      
      // Limitar valores
      iop = Math.max(10, Math.min(30, iop));
      perfusion = Math.max(60, Math.min(100, perfusion));
      vitreousRemoved = Math.max(0, Math.min(100, vitreousRemoved));
      
      // Actualizar visualización
      document.getElementById('iop-value').innerText = iop.toFixed(1) + " mmHg";
      document.getElementById('iop-level').style.width = ((iop - 10) / 20 * 100) + "%";
      
      document.getElementById('perfusion-value').innerText = perfusion.toFixed(0) + "%";
      document.getElementById('perfusion-level').style.width = perfusion + "%";
      
      document.getElementById('vitreous-value').innerText = vitreousRemoved.toFixed(0) + "%";
      document.getElementById('vitreous-level').style.width = vitreousRemoved + "%";
      
      // Actualizar clase según presión
      const iopElement = document.getElementById('iop-value');
      iopElement.classList.remove('normal', 'warning', 'danger');
      
      if(iop < 15 || iop > 25) {
        iopElement.classList.add('danger');
        retinaStatus = 'Riesgo';
      } else if(iop < 18 || iop > 22) {
        iopElement.classList.add('warning');
        retinaStatus = 'Alerta';
      } else {
        iopElement.classList.add('normal');
        retinaStatus = 'Estable';
      }
      
      // Actualizar estado de la retina
      const retinaStatusElement = document.getElementById('retina-status');
      retinaStatusElement.innerText = retinaStatus;
      retinaStatusElement.className = `vital-value ${iopElement.className.split(' ')[1]}`;
      
      // Efectos de la perfusión en la retina
      if(perfusion < 70) {
        retinaStatusElement.classList.remove('normal', 'warning');
        retinaStatusElement.classList.add('danger');
        retinaStatus = 'Isquemia';
      } else if(perfusion < 85) {
        retinaStatusElement.classList.remove('normal', 'danger');
        retinaStatusElement.classList.add('warning');
        retinaStatus = 'Hipoperfusión';
      }
      
      // Actualizar cada segundo
      setTimeout(updateVitals, 1000);
    }
    
    function updateVitreousOpacity() {
      const vitreous = document.getElementById('vitreous');
      const opacity = 1 - (vitreousRemoved / 100);
      vitreous.style.opacity = opacity;
    }
  </script>
</body>
</html>
