# qui-veut-passer-en-4eme
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Qui Veut Devenir Étudiant de 4ème Année ?</title>
  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background: #0f172a;
      min-height: 100vh;
      color: white;
    }
    .btn {
      padding: 12px 24px;
      border-radius: 8px;
      font-weight: bold;
      cursor: pointer;
      transition: all 0.2s;
      border: none;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      font-size: 14px;
    }
    .btn:hover:not(:disabled) { transform: scale(1.05); }
    .btn:active:not(:disabled) { transform: scale(0.95); }
    .btn:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }
    .btn-primary {
      background: linear-gradient(135deg, #2563eb, #4f46e5);
      color: white;
      border: 1px solid #60a5fa;
    }
    .btn-secondary {
      background: #334155;
      color: white;
      border: 1px solid #475569;
    }
    .btn-gold {
      background: linear-gradient(135deg, #fbbf24, #d97706);
      color: black;
      border: 1px solid #fcd34d;
    }
    .input-field {
      width: 100%;
      padding: 12px 16px;
      border-radius: 8px;
      background: rgba(30, 41, 59, 0.8);
      border: 1px solid #475569;
      color: white;
      font-size: 16px;
    }
    .input-field:focus {
      outline: none;
      border-color: #3b82f6;
      box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.3);
    }
    .card {
      background: rgba(15, 23, 42, 0.95);
      backdrop-filter: blur(10px);
      border: 1px solid rgba(71, 85, 99, 0.5);
      border-radius: 12px;
      padding: 24px;
      box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
    }
    .answer-btn {
      width: 100%;
      padding: 20px 24px;
      border-radius: 12px;
      border: 2px solid #475569;
      background: #1e293b;
      color: white;
      font-size: 16px;
      cursor: pointer;
      transition: all 0.3s;
      text-align: left;
      display: flex;
      align-items: center;
      gap: 16px;
    }
    .answer-btn:hover:not(:disabled) {
      background: #334155;
      border-color: #94a3b8;
    }
    .answer-btn.correct {
      background: #16a34a !important;
      border-color: #4ade80 !important;
      animation: pulse 1s infinite;
    }
    .answer-btn.wrong {
      background: #dc2626 !important;
      border-color: #f87171 !important;
    }
    .answer-btn.eliminated {
      background: #0f172a !important;
      border-color: #1e293b !important;
      color: #64748b !important;
      opacity: 0.5;
      cursor: not-allowed;
    }
    .answer-letter {
      width: 32px;
      height: 32px;
      border-radius: 50%;
      background: rgba(0, 0, 0, 0.3);
      border: 1px solid #64748b;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: bold;
      font-size: 14px;
      flex-shrink: 0;
    }
    .lifeline-btn {
      padding: 12px;
      border-radius: 8px;
      background: rgba(30, 58, 138, 0.5);
      border: 1px solid #1e40af;
      color: #93c5fd;
      cursor: pointer;
      transition: all 0.2s;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 4px;
      min-width: 70px;
    }
    .lifeline-btn:hover:not(:disabled) {
      background: rgba(30, 64, 175, 0.8);
    }
    .lifeline-btn:disabled {
      background: #1e293b;
      border-color: #334155;
      color: #475569;
      cursor: not-allowed;
    }
    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.7; }
    }
    @keyframes fadeInUp {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .animate-fade-in { animation: fadeInUp 0.5s ease-out forwards; }
    .table-row:hover { background: rgba(51, 65, 85, 0.5); }
    .header-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px 24px;
      border-bottom: 1px solid rgba(71,85,99,0.5);
      background: rgba(15,23,42,0.8);
    }
    .logo-container {
      display: flex;
      align-items: center;
      gap: 12px;
    }
    .logo-icon {
      width: 40px;
      height: 40px;
      background: linear-gradient(135deg, #2563eb, #4f46e5);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 0 0 20px rgba(59,130,246,0.5);
    }
    .logo-text {
      font-size: 18px;
      font-weight: bold;
      background: linear-gradient(90deg, #60a5fa, #a78bfa);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    .main-container {
      flex: 1;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 24px;
    }
    .game-layout {
      width: 100%;
      max-width: 1200px;
      display: grid;
      grid-template-columns: 1fr 2fr;
      gap: 24px;
    }
    @media (max-width: 900px) {
      .game-layout {
        grid-template-columns: 1fr;
      }
    }
    .question-box {
      background: linear-gradient(135deg, #1e293b, #0f172a);
      border: 2px solid #475569;
      border-radius: 16px;
      padding: 32px;
      text-align: center;
      position: relative;
      overflow: hidden;
    }
    .question-box::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      height: 2px;
      background: linear-gradient(90deg, transparent, #3b82f6, transparent);
    }
    .answers-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
    }
    @media (max-width: 600px) {
      .answers-grid {
        grid-template-columns: 1fr;
      }
    }
    .prize-info {
      margin-top: auto;
      padding-top: 24px;
      border-top: 1px solid #334155;
    }
    .score-display {
      background: rgba(30,41,59,0.5);
      border-radius: 12px;
      padding: 24px;
      margin-bottom: 32px;
    }
    .end-icon {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 0 auto 24px;
      border: 4px solid;
    }
    .end-icon.win {
      background: rgba(251,191,36,0.2);
      border-color: #fbbf24;
      color: #fbbf24;
    }
    .end-icon.lose {
      background: rgba(220,38,38,0.2);
      border-color: #dc2626;
      color: #dc2626;
    }
    .bg-overlay {
      position: absolute;
      inset: 0;
      background: linear-gradient(to bottom, rgba(15,23,42,0.85) 0%, rgba(15,23,42,0.7) 50%, rgba(15,23,42,0.95) 100%);
      z-index: 1;
    }
    .bg-image {
      position: absolute;
      inset: 0;
      background-size: cover;
      background-position: center;
      opacity: 0.3;
      z-index: 0;
    }
    .content-wrapper {
      position: relative;
      z-index: 10;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }
    .nav-buttons {
      display: flex;
      gap: 12px;
    }
    .lifelines-container {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 8px;
      margin-top: 16px;
    }
    .table-container {
      overflow-x: auto;
    }
    table {
      width: 100%;
      border-collapse: collapse;
    }
    th {
      padding: 16px;
      text-align: left;
      font-size: 12px;
      text-transform: uppercase;
      color: #94a3b8;
      border-bottom: 1px solid #334155;
    }
    td {
      padding: 16px;
      border-bottom: 1px solid #1e293b;
    }
    .player-info {
      margin-bottom: 24px;
    }
    .player-name {
      font-size: 24px;
      font-weight: bold;
      margin-bottom: 4px;
    }
    .player-level {
      color: #94a3b8;
      font-size: 14px;
    }
    .lifelines-title {
      font-size: 12px;
      font-weight: bold;
      color: #64748b;
      text-transform: uppercase;
      letter-spacing: 1px;
      margin-bottom: 12px;
      margin-top: 32px;
    }
    .form-container {
      text-align: center;
      margin-bottom: 32px;
    }
    .form-icon {
      width: 80px;
      height: 80px;
      background: linear-gradient(135deg, #2563eb, #4f46e5);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 0 auto 16px;
      box-shadow: 0 0 30px rgba(59,130,246,0.4);
    }
    .form-title {
      font-size: 28px;
      font-weight: bold;
      margin-bottom: 8px;
    }
    .form-subtitle {
      color: #94a3b8;
    }
    .form-group {
      margin-bottom: 24px;
      text-align: left;
    }
    .form-label {
      display: block;
      font-size: 14px;
      font-weight: 500;
      color: #cbd5e1;
      margin-bottom: 8px;
    }
    .button-group {
      display: flex;
      gap: 12px;
      justify-content: center;
      flex-wrap: wrap;
    }
    .leaderboard-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 24px;
      flex-wrap: wrap;
      gap: 12px;
    }
    .leaderboard-title {
      font-size: 24px;
      font-weight: bold;
      display: flex;
      align-items: center;
      gap: 12px;
    }
    .empty-state {
      padding: 40px;
      text-align: center;
      color: #64748b;
    }
    .guaranteed-label {
      font-size: 12px;
      color: #64748b;
    }
    .guaranteed-amount {
      font-size: 20px;
      font-weight: bold;
      color: #fbbf24;
      font-family: monospace;
    }
    .question-text {
      font-size: 20px;
      line-height: 1.6;
      color: #f1f5f9;
    }
    .end-title {
      font-size: 28px;
      font-weight: bold;
      margin-bottom: 8px;
    }
    .end-subtitle {
      color: #94a3b8;
    }
    .final-score-label {
      font-size: 12px;
      color: #64748b;
      text-transform: uppercase;
      margin-bottom: 8px;
    }
    .final-score-value {
      font-size: 36px;
      font-weight: bold;
      font-family: monospace;
      color: #f1f5f9;
    }
    svg {
      display: block;
    }
    .error-message {
      color: #f87171;
      font-size: 14px;
      margin-top: 8px;
    }
  </style>
</head>
<body>
  <div id="root"></div>
  
  <script type="text/babel">
    const { useState, useEffect } = React;

    // SVG Icon Components
    const TrophyIcon = () => (
      <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <path d="M6 9H4.5a2.5 2.5 0 0 1 0-5H6"/>
        <path d="M18 9h1.5a2.5 2.5 0 0 0 0-5H18"/>
        <path d="M4 22h16"/>
        <path d="M10 14.66V17c0 .55-.47.98-.97 1.21C7.85 18.75 7 20.24 7 22"/>
        <path d="M14 14.66V17c0 .55.47.98.97 1.21C16.15 18.75 17 20.24 17 22"/>
        <path d="M18 2H6v7a6 6 0 0 0 12 0V2Z"/>
      </svg>
    );

    const MailIcon = () => (
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <rect width="20" height="16" x="2" y="4" rx="2"/>
        <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/>
      </svg>
    );

    const UserIcon = () => (
      <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"/>
        <circle cx="12" cy="7" r="4"/>
      </svg>
    );

    const PhoneIcon = () => (
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
      </svg>
    );

    const UsersIcon = () => (
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/>
        <circle cx="9" cy="7" r="4"/>
        <path d="M22 21v-2a4 4 0 0 0-3-3.87"/>
        <path d="M16 3.13a4 4 0 0 1 0 7.75"/>
      </svg>
    );

    const HelpIcon = () => (
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <circle cx="12" cy="12" r="10"/>
        <path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/>
        <path d="M12 17h.01"/>
      </svg>
    );

    const RotateIcon = () => (
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <path d="M21 12a9 9 0 0 0-9-9 9.75 9.75 0 0 0-6.74 2.74L3 8"/>
        <path d="M3 3v5h5"/>
        <path d="M3 12a9 9 0 0 0 9 9 9.75 9.75 0 0 0 6.74-2.74L21 16"/>
        <path d="M16 21h5v-5"/>
      </svg>
    );

    const ArrowIcon = () => (
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <path d="M5 12h14"/>
        <path d="m12 5 7 7-7 7"/>
      </svg>
    );

    const CheckIcon = () => (
      <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/>
        <path d="m9 11 3 3L22 4"/>
      </svg>
    );

    const XIcon = () => (
      <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <circle cx="12" cy="12" r="10"/>
        <path d="m15 9-6 6"/>
        <path d="m9 9 6 6"/>
      </svg>
    );

    // Questions Data
    const QUESTIONS = [
      { question: "La chromatographie est une technique qui permet principalement :", answers: ["La synthèse des composés", "La séparation des constituants d'un mélange", "La neutralisation des solutions", "La dilution des analytes"], correct: 1 },
      { question: "La chromatographie en phase gazeuse est adaptée surtout aux composés :", answers: ["Ionisés", "Non volatils", "Volatils et thermostables", "Insolubles"], correct: 2 },
      { question: "Une extraction liquide–liquide nécessite obligatoirement :", answers: ["Deux solvants miscibles", "Deux solvants non miscibles", "Un solide et un liquide", "Une réaction chimique"], correct: 1 },
      { question: "Dans une extraction, le soluté se répartit entre deux phases selon :", answers: ["Sa masse molaire", "Sa couleur", "Sa solubilité relative", "Son volume"], correct: 2 },
      { question: "Le partage d'un soluté correspond :", answers: ["À sa dégradation dans l'eau", "À sa répartition entre deux phases liquides", "À sa volatilisation", "À sa réaction avec le solvant"], correct: 1 },
      { question: "Un acide organique est mieux extrait par un solvant organique lorsqu'il est :", answers: ["Ionisé", "Sous forme de sel", "Non ionisé", "Complexé"], correct: 2 },
      { question: "En chromatographie gazeuse, la phase mobile est :", answers: ["Un liquide", "Un solide", "Un gaz", "Un gel"], correct: 2 },
      { question: "Une augmentation du débit de la phase mobile entraîne généralement :", answers: ["Une augmentation du temps de rétention", "Une diminution du temps de rétention", "Aucune influence", "Une disparition des pics"], correct: 1 },
      { question: "La séparation en chromatographie gazeuse dépend principalement :", answers: ["Du volume injecté uniquement", "De la volatilité et de la polarité", "De la couleur des composés", "Du rendement d'extraction"], correct: 1 },
      { question: "Une phase stationnaire polaire est plus adaptée à la séparation de composés :", answers: ["Apolaires", "Très lourds", "Polaires ou moyennement polaires", "Ionisés uniquement"], correct: 2 },
      { question: "Le coefficient de partage d'un soluté dépend principalement :", answers: ["Du nombre d'extractions", "De la nature des solvants et de la température", "Du rendement expérimental", "Du volume de la phase aqueuse"], correct: 1 },
      { question: "Effectuer deux extractions successives avec un petit volume est généralement :", answers: ["Moins efficace qu'une seule extraction", "Aussi efficace", "Plus efficace", "Sans influence sur le rendement"], correct: 2 },
      { question: "L'analogie de polarité entre l'analyte et la phase stationnaire permet :", answers: ["D'augmenter la volatilité", "De réduire le temps d'analyse", "De renforcer les interactions et améliorer la séparation", "D'éliminer les impuretés"], correct: 2 },
      { question: "En chromatographie gazeuse, un débit de phase mobile trop élevé entraîne :", answers: ["Une meilleure résolution", "Une perte d'efficacité de séparation", "Une augmentation du nombre de plateaux", "Une meilleure symétrie des pics"], correct: 1 },
      { question: "La méthode de l'étalon interne est utilisée afin de :", answers: ["Supprimer la phase stationnaire", "Corriger les variations liées à l'injection et à la matrice", "Éviter toute courbe d'étalonnage", "Augmenter la volatilité de l'analyte"], correct: 1 }
    ];

    const PRIZE_MILESTONES = [0, 500, 1000, 2000, 5000, 10000, 20000, 50000, 100000, 250000, 500000, 1000000, 2000000, 5000000, 10000000, 20000000];

    // Main App Component
    function App() {
      const [screen, setScreen] = useState('login');
      const [user, setUser] = useState({ name: '', score: 0, level: 0 });
      const [currentQuestionIndex, setCurrentQuestionIndex] = useState(0);
      const [lifelines, setLifelines] = useState({ fifty: false, phone: false, audience: false });
      const [gameState, setGameState] = useState('waiting');
      const [selectedAnswer, setSelectedAnswer] = useState(null);
      const [leaderboard, setLeaderboard] = useState([]);
      const [eliminatedAnswers, setEliminatedAnswers] = useState([]);
      const [loginName, setLoginName] = useState('');
      const [error, setError] = useState('');

      // Load leaderboard on mount
      useEffect(function() {
        try {
          const saved = localStorage.getItem('wbtm_leaderboard');
          if (saved) {
            setLeaderboard(JSON.parse(saved));
          }
        } catch (e) {
          console.error('Error loading leaderboard:', e);
        }
      }, []);

      // Save new entry to leaderboard
      function saveLeaderboard(newEntry) {
        try {
          const updated = [...leaderboard, newEntry]
            .sort(function(a, b) { return b.level - a.level; })
            .slice(0, 10);
          setLeaderboard(updated);
          localStorage.setItem('wbtm_leaderboard', JSON.stringify(updated));
        } catch (e) {
          console.error('Error saving leaderboard:', e);
        }
      }

      // Handle login - FIXED VERSION
      function handleLogin(e) {
        if (e) {
          e.preventDefault();
        }
        
        var trimmedName = loginName.trim();
        
        if (!trimmedName || trimmedName.length === 0) {
          setError('Veuillez entrer un pseudonyme');
          return;
        }
        
        try {
          setUser({ name: trimmedName, score: 0, level: 0 });
          setCurrentQuestionIndex(0);
          setLifelines({ fifty: false, phone: false, audience: false });
          setGameState('waiting');
          setSelectedAnswer(null);
          setEliminatedAnswers([]);
          setError('');
          setScreen('game');
        } catch (err) {
          console.error('Login error:', err);
          setError('Une erreur est survenue. Veuillez réessayer.');
        }
      }

      // Handle answer selection
      function handleAnswer(index) {
        if (gameState !== 'waiting') return;
        setSelectedAnswer(index);
        setGameState('answering');

        setTimeout(function() {
          var isCorrect = index === QUESTIONS[currentQuestionIndex].correct;
          if (isCorrect) {
            setGameState('correct');
            setTimeout(function() {
              if (currentQuestionIndex === QUESTIONS.length - 1) {
                var finalScore = PRIZE_MILESTONES[QUESTIONS.length];
                var result = { 
                  name: user.name, 
                  level: QUESTIONS.length, 
                  score: finalScore, 
                  date: new Date().toLocaleDateString('fr-FR') 
                };
                saveLeaderboard(result);
                setUser({ name: user.name, score: finalScore, level: QUESTIONS.length });
                setScreen('end');
              } else {
                setCurrentQuestionIndex(function(prev) { return prev + 1; });
                setGameState('waiting');
                setSelectedAnswer(null);
                setEliminatedAnswers([]);
              }
            }, 1500);
          } else {
            setGameState('wrong');
            setTimeout(function() {
              var result = { 
                name: user.name, 
                level: currentQuestionIndex, 
                score: PRIZE_MILESTONES[currentQuestionIndex], 
                date: new Date().toLocaleDateString('fr-FR') 
              };
              saveLeaderboard(result);
              setUser({ name: user.name, score: PRIZE_MILESTONES[currentQuestionIndex], level: currentQuestionIndex });
              setScreen('end');
            }, 2000);
          }
        }, 2000);
      }

      // Use 50/50 lifeline
      function useFiftyFifty() {
        if (lifelines.fifty || gameState !== 'waiting') return;
        var correct = QUESTIONS[currentQuestionIndex].correct;
        var wrongIndices = [0, 1, 2, 3].filter(function(i) { return i !== correct; });
        var toRemove = wrongIndices.sort(function() { return 0.5 - Math.random(); }).slice(0, 2);
        setEliminatedAnswers(toRemove);
        setLifelines(function(prev) { return { fifty: true, phone: prev.phone, audience: prev.audience }; });
      }

      // Use Phone a Friend lifeline
      function usePhone() {
        if (lifelines.phone || gameState !== 'waiting') return;
        setLifelines(function(prev) { return { fifty: prev.fifty, phone: true, audience: prev.audience }; });
        var correctLetter = String.fromCharCode(65 + QUESTIONS[currentQuestionIndex].correct);
        alert('📞 Appel à un ami:\n\n"Je pense que la bonne réponse est ' + correctLetter + ')"');
      }

      // Use Ask the Audience lifeline
      function useAudience() {
        if (lifelines.audience || gameState !== 'waiting') return;
        setLifelines(function(prev) { return { fifty: prev.fifty, phone: prev.phone, audience: true }; });
        var correct = QUESTIONS[currentQuestionIndex].correct;
        var dist = [0, 0, 0, 0];
        dist[correct] = 60 + Math.floor(Math.random() * 20);
        var remaining = 100 - dist[correct];
        var others = [0, 1, 2, 3].filter(function(i) { return i !== correct; });
        dist[others[0]] = Math.floor(remaining * 0.6);
        dist[others[1]] = Math.floor(remaining * 0.3);
        dist[others[2]] = remaining - dist[others[0]] - dist[others[1]];
        
        alert('👥 Résultat du public:\n\nA) ' + dist[0] + '%\nB) ' + dist[1] + '%\nC) ' + dist[2] + '%\nD) ' + dist[3] + '%');
      }

      // Send email report
      function sendEmailReport() {
        var subject = encodeURIComponent('Rapport Leaderboard - Qui Veut Devenir Étudiant de 4ème Année');
        var body = 'Bonjour,\n\nVoici le classement actuel des étudiants:\n\n';
        leaderboard.forEach(function(entry, i) {
          body += (i + 1) + '. ' + entry.name + ' - Niveau ' + entry.level + ' (' + entry.score.toLocaleString('fr-FR') + ' pts)\n';
        });
        body += '\nCordialement,\nSystème de Quiz.';
        window.location.href = 'mailto:ia.tarek.kamergi@gmail.com?subject=' + subject + '&body=' + encodeURIComponent(body);
      }

      var bgImageUrl = 'https://image.qwenlm.ai/public_source/578b15c5-2e5b-45b8-8d8a-9d36d99cb351/16e955662-fd0c-42db-9363-435dab05ba64.png';

      return (
        <div style={{ minHeight: '100vh', background: 'linear-gradient(135deg, #0f172a 0%, #1e3a8a 50%, #0f172a 100%)', position: 'relative', overflow: 'hidden' }}>
          <div className="bg-image" style={{ backgroundImage: 'url(' + bgImageUrl + ')' }} />
          <div className="bg-overlay" />
          
          <div className="content-wrapper">
            {/* Header */}
            <header className="header-bar">
              <div className="logo-container">
                <div className="logo-icon">
                  <TrophyIcon />
                </div>
                <span className="logo-text">Qui Veut Devenir Étudiant de 4ème Année ?</span>
              </div>
              <div className="nav-buttons">
                <button onClick={function() { setScreen('leaderboard'); }} className="btn btn-secondary">
                  <TrophyIcon /> Classement
                </button>
                <button onClick={function() { setScreen('login'); }} className="btn btn-secondary">
                  <RotateIcon /> Accueil
                </button>
              </div>
            </header>

            {/* Main Content */}
            <main className="main-container">
              
              {/* Login Screen */}
              {screen === 'login' && (
                <div className="card animate-fade-in" style={{ width: '100%', maxWidth: '450px' }}>
                  <div className="form-container">
                    <div className="form-icon">
                      <UserIcon />
                    </div>
                    <h2 className="form-title">Bienvenue</h2>
                    <p className="form-subtitle">Entrez votre pseudonyme pour commencer le quiz</p>
                  </div>
                  <form onSubmit={handleLogin}>
                    <div className="form-group">
                      <label className="form-label">Pseudonyme</label>
                      <input
                        type="text"
                        value={loginName}
                        onChange={function(e) { setLoginName(e.target.value); setError(''); }}
                        placeholder="Ex: ChimistePro"
                        className="input-field"
                        required
                      />
                      {error && <p className="error-message">{error}</p>}
                    </div>
                    <button type="submit" className="btn btn-primary" style={{ width: '100%', justifyContent: 'center' }}>
                      Commencer le Quiz <ArrowIcon />
                    </button>
                  </form>
                </div>
              )}

              {/* Game Screen */}
              {screen === 'game' && (
                <div className="game-layout">
                  {/* Left Panel */}
                  <div style={{ display: 'flex', flexDirection: 'column', gap: '16px' }}>
                    <div className="card" style={{ flex: 1 }}>
                      <h3 style={{ fontSize: '16px', fontWeight: 'bold', color: '#60a5fa', marginBottom: '16px', display: 'flex', alignItems: 'center', gap: '8px' }}>
                        <UserIcon /> Joueur
                      </h3>
                      <div className="player-info">
                        <div className="player-name">{user.name}</div>
                        <div className="player-level">Question {currentQuestionIndex + 1} / {QUESTIONS.length}</div>
                      </div>
                      
                      <div className="lifelines-title">Jokers Disponibles</div>
                      <div className="lifelines-container">
                        <button onClick={useFiftyFifty} disabled={lifelines.fifty} className="lifeline-btn">
                          <HelpIcon />
                          <span style={{ fontSize: '11px' }}>50/50</span>
                        </button>
                        <button onClick={usePhone} disabled={lifelines.phone} className="lifeline-btn">
                          <PhoneIcon />
                          <span style={{ fontSize: '11px' }}>Ami</span>
                        </button>
                        <button onClick={useAudience} disabled={lifelines.audience} className="lifeline-btn">
                          <UsersIcon />
                          <span style={{ fontSize: '11px' }}>Public</span>
                        </button>
                      </div>
                      
                      <div className="prize-info">
                        <div className="guaranteed-label">Gains garantis</div>
                        <div className="guaranteed-amount">
                          {PRIZE_MILESTONES[currentQuestionIndex].toLocaleString('fr-FR')} pts
                        </div>
                      </div>
                    </div>
                  </div>

                  {/* Right Panel - Question & Answers */}
                  <div style={{ display: 'flex', flexDirection: 'column', gap: '24px' }}>
                    {/* Question Box */}
                    <div className="question-box">
                      <h2 className="question-text">
                        {QUESTIONS[currentQuestionIndex].question}
                      </h2>
                    </div>

                    {/* Answers */}
                    <div className="answers-grid">
                      {QUESTIONS[currentQuestionIndex].answers.map(function(answer, idx) {
                        var isEliminated = eliminatedAnswers.includes(idx);
                        var isSelected = selectedAnswer === idx;
                        var isCorrect = idx === QUESTIONS[currentQuestionIndex].correct;
                        
                        var btnClass = 'answer-btn';
                        if (gameState === 'correct' && isCorrect) btnClass += ' correct';
                        if (gameState === 'wrong' && isSelected) btnClass += ' wrong';
                        if (gameState === 'wrong' && isCorrect) btnClass += ' correct';
                        if (isEliminated) btnClass += ' eliminated';

                        return (
                          <button
                            key={idx}
                            onClick={function() { handleAnswer(idx); }}
                            disabled={gameState !== 'waiting' || isEliminated}
                            className={btnClass}
                          >
                            <span className="answer-letter">{String.fromCharCode(65 + idx)}</span>
                            <span style={{ flex: 1 }}>{answer}</span>
                          </button>
                        );
                      })}
                    </div>
                  </div>
                </div>
              )}

              {/* Leaderboard Screen */}
              {screen === 'leaderboard' && (
                <div className="card animate-fade-in" style={{ width: '100%', maxWidth: '900px' }}>
                  <div className="leaderboard-header">
                    <h2 className="leaderboard-title">
                      <span style={{ color: '#fbbf24' }}><TrophyIcon /></span>
                      Classement des Étudiants
                    </h2>
                    <button onClick={sendEmailReport} className="btn btn-primary">
                      <MailIcon /> Envoyer par Email
                    </button>
                  </div>
                  
                  <div className="table-container">
                    <table>
                      <thead>
                        <tr>
                          <th>Rang</th>
                          <th>Pseudonyme</th>
                          <th>Niveau</th>
                          <th style={{ textAlign: 'right' }}>Score</th>
                          <th style={{ textAlign: 'right' }}>Date</th>
                        </tr>
                      </thead>
                      <tbody>
                        {leaderboard.length === 0 ? (
                          <tr>
                            <td colSpan="5" className="empty-state">
                              Aucun classement disponible
                            </td>
                          </tr>
                        ) : (
                          leaderboard.map(function(entry, idx) {
                            return (
                              <tr key={idx} className="table-row">
                                <td style={{ fontWeight: 'bold', color: '#cbd5e1' }}>#{idx + 1}</td>
                                <td style={{ fontWeight: '500' }}>{entry.name}</td>
                                <td style={{ color: '#60a5fa' }}>{entry.level} / {QUESTIONS.length}</td>
                                <td style={{ textAlign: 'right', fontFamily: 'monospace', color: '#fbbf24' }}>
                                  {entry.score.toLocaleString('fr-FR')}
                                </td>
                                <td style={{ textAlign: 'right', color: '#64748b', fontSize: '14px' }}>{entry.date}</td>
                              </tr>
                            );
                          })
                        )}
                      </tbody>
                    </table>
                  </div>
                  
                  <div style={{ marginTop: '24px', display: 'flex', justifyContent: 'flex-end' }}>
                    <button onClick={function() { setScreen('login'); }} className="btn btn-secondary">Retour</button>
                  </div>
                </div>
              )}

              {/* End Screen */}
              {screen === 'end' && (
                <div className="card animate-fade-in" style={{ width: '100%', maxWidth: '500px', textAlign: 'center' }}>
                  <div style={{ marginBottom: '24px' }}>
                    <div className={'end-icon ' + (user.level === QUESTIONS.length ? 'win' : 'lose')}>
                      {user.level === QUESTIONS.length ? <CheckIcon /> : <XIcon />}
                    </div>
                    <h2 className="end-title">
                      {user.level === QUESTIONS.length ? '🎉 Félicitations !' : 'Partie Terminée'}
                    </h2>
                    <p className="end-subtitle">
                      {user.level === QUESTIONS.length 
                        ? 'Vous avez obtenu le titre d\'Étudiant de 4ème Année !' 
                        : 'Vous avez atteint le niveau ' + user.level + '.'}
                    </p>
                  </div>

                  <div className="score-display">
                    <div className="final-score-label">Score Final</div>
                    <div className="final-score-value">
                      {user.score.toLocaleString('fr-FR')} pts
                    </div>
                  </div>

                  <div className="button-group">
                    <button onClick={function() { setScreen('leaderboard'); }} className="btn btn-gold">
                      <TrophyIcon /> Classement
                    </button>
                    <button onClick={function() { setScreen('login'); }} className="btn btn-secondary">
                      <RotateIcon /> Rejouer
                    </button>
                  </div>
                </div>
              )}

            </main>
          </div>
        </div>
      );
    }

    // Render the app
    var container = document.getElementById('root');
    var root = ReactDOM.createRoot(container);
    root.render(React.createElement(App));
  </script>
</body>
</html>


