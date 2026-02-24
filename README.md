# qui-veut-passer-en-4eme
import React, { useState, useEffect, useRef } from 'react';
import ReactDOM from 'react-dom/client';
import { createRoot } from 'react-dom/client';
import { Trophy, Mail, User, CheckCircle, XCircle, HelpCircle, Phone, Users, ArrowRight, RotateCcw } from 'lucide-react';

// --- DATA: Questions ---
const QUESTIONS = [
  {
    question: "La chromatographie est une technique qui permet principalement :",
    answers: ["La synthèse des composés", "La séparation des constituants d'un mélange", "La neutralisation des solutions", "La dilution des analytes"],
    correct: 1
  },
  {
    question: "La chromatographie en phase gazeuse est adaptée surtout aux composés :",
    answers: ["Ionisés", "Non volatils", "Volatils et thermostables", "Insolubles"],
    correct: 2
  },
  {
    question: "Une extraction liquide–liquide nécessite obligatoirement :",
    answers: ["Deux solvants miscibles", "Deux solvants non miscibles", "Un solide et un liquide", "Une réaction chimique"],
    correct: 1
  },
  {
    question: "Dans une extraction, le soluté se répartit entre deux phases selon :",
    answers: ["Sa masse molaire", "Sa couleur", "Sa solubilité relative", "Son volume"],
    correct: 2
  },
  {
    question: "Le partage d'un soluté correspond :",
    answers: ["À sa dégradation dans l'eau", "À sa répartition entre deux phases liquides", "À sa volatilisation", "À sa réaction avec le solvant"],
    correct: 1
  },
  {
    question: "Un acide organique est mieux extrait par un solvant organique lorsqu'il est :",
    answers: ["Ionisé", "Sous forme de sel", "Non ionisé", "Complexé"],
    correct: 2
  },
  {
    question: "En chromatographie gazeuse, la phase mobile est :",
    answers: ["Un liquide", "Un solide", "Un gaz", "Un gel"],
    correct: 2
  },
  {
    question: "Une augmentation du débit de la phase mobile entraîne généralement :",
    answers: ["Une augmentation du temps de rétention", "Une diminution du temps de rétention", "Aucune influence", "Une disparition des pics"],
    correct: 1
  },
  {
    question: "La séparation en chromatographie gazeuse dépend principalement :",
    answers: ["Du volume injecté uniquement", "De la volatilité et de la polarité", "De la couleur des composés", "Du rendement d'extraction"],
    correct: 1
  },
  {
    question: "Une phase stationnaire polaire est plus adaptée à la séparation de composés :",
    answers: ["Apolaires", "Très lourds", "Polaires ou moyennement polaires", "Ionisés uniquement"],
    correct: 2
  },
  {
    question: "Le coefficient de partage d'un soluté dépend principalement :",
    answers: ["Du nombre d'extractions", "De la nature des solvants et de la température", "Du rendement expérimental", "Du volume de la phase aqueuse"],
    correct: 1
  },
  {
    question: "Effectuer deux extractions successives avec un petit volume est généralement :",
    answers: ["Moins efficace qu'une seule extraction", "Aussi efficace", "Plus efficace", "Sans influence sur le rendement"],
    correct: 2
  },
  {
    question: "L'analogie de polarité entre l'analyte et la phase stationnaire permet :",
    answers: ["D'augmenter la volatilité", "De réduire le temps d'analyse", "De renforcer les interactions et améliorer la séparation", "D'éliminer les impuretés"],
    correct: 2
  },
  {
    question: "En chromatographie gazeuse, un débit de phase mobile trop élevé entraîne :",
    answers: ["Une meilleure résolution", "Une perte d'efficacité de séparation", "Une augmentation du nombre de plateaux", "Une meilleure symétrie des pics"],
    correct: 1
  },
  {
    question: "La méthode de l'étalon interne est utilisée afin de :",
    answers: ["Supprimer la phase stationnaire", "Corriger les variations liées à l'injection et à la matrice", "Éviter toute courbe d'étalonnage", "Augmenter la volatilité de l'analyte"],
    correct: 1
  }
];

const PRIZE_MILESTONES = [0, 500, 1000, 2000, 5000, 10000, 20000, 50000, 100000, 250000, 500000, 1000000, 2000000, 5000000, 10000000, 20000000];

// --- COMPONENTS ---

const Button = ({ children, onClick, className = "", disabled = false, variant = "primary" }) => {
  const baseStyle = "px-6 py-3 rounded-lg font-bold transition-all duration-200 transform hover:scale-105 active:scale-95 disabled:opacity-50 disabled:cursor-not-allowed shadow-lg";
  const variants = {
    primary: "bg-gradient-to-r from-blue-600 to-indigo-600 text-white hover:from-blue-500 hover:to-indigo-500 border border-blue-400",
    secondary: "bg-slate-700 text-white hover:bg-slate-600 border border-slate-500",
    danger: "bg-gradient-to-r from-red-600 to-pink-600 text-white hover:from-red-500 hover:to-pink-500",
    gold: "bg-gradient-to-r from-amber-400 to-yellow-600 text-black hover:from-amber-300 hover:to-yellow-500 border border-yellow-200"
  };

  return (
    <button onClick={onClick} disabled={disabled} className={`${baseStyle} ${variants[variant]} ${className}`}>
      {children}
    </button>
  );
};

const Input = ({ value, onChange, placeholder, className = "" }) => (
  <input
    type="text"
    value={value}
    onChange={onChange}
    placeholder={placeholder}
    className={`w-full px-4 py-3 rounded-lg bg-slate-800/80 border border-slate-600 text-white placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent backdrop-blur-sm ${className}`}
  />
);

const Card = ({ children, className = "" }) => (
  <div className={`bg-slate-900/80 backdrop-blur-md border border-slate-700/50 rounded-xl p-6 shadow-2xl ${className}`}>
    {children}
  </div>
);

// --- MAIN APP ---

const App = () => {
  const [screen, setScreen] = useState('login'); // login, game, leaderboard, end
  const [user, setUser] = useState({ name: '', score: 0, level: 0 });
  const [currentQuestionIndex, setCurrentQuestionIndex] = useState(0);
  const [lifelines, setLifelines] = useState({ fifty: false, phone: false, audience: false });
  const [gameState, setGameState] = useState('waiting'); // waiting, answering, correct, wrong
  const [selectedAnswer, setSelectedAnswer] = useState(null);
  const [leaderboard, setLeaderboard] = useState([]);
  const [eliminatedAnswers, setEliminatedAnswers] = useState([]);

  // Load leaderboard on mount
  useEffect(() => {
    const saved = localStorage.getItem('wbtm_leaderboard');
    if (saved) setLeaderboard(JSON.parse(saved));
  }, []);

  const saveLeaderboard = (newEntry) => {
    const updated = [...leaderboard, newEntry].sort((a, b) => b.level - a.level).slice(0, 10);
    setLeaderboard(updated);
    localStorage.setItem('wbtm_leaderboard', JSON.stringify(updated));
  };

  const handleLogin = (name) => {
    setUser({ name, score: 0, level: 0 });
    setCurrentQuestionIndex(0);
    setLifelines({ fifty: false, phone: false, audience: false });
    setGameState('waiting');
    setScreen('game');
  };

  const handleAnswer = (index) => {
    if (gameState !== 'waiting') return;
    setSelectedAnswer(index);
    setGameState('answering');

    setTimeout(() => {
      const isCorrect = index === QUESTIONS[currentQuestionIndex].correct;
      if (isCorrect) {
        setGameState('correct');
        setTimeout(() => {
          if (currentQuestionIndex === QUESTIONS.length - 1) {
            // Won the game
            const finalScore = PRIZE_MILESTONES[QUESTIONS.length];
            const result = { name: user.name, level: QUESTIONS.length, score: finalScore, date: new Date().toLocaleDateString() };
            saveLeaderboard(result);
            setUser({ ...user, score: finalScore, level: QUESTIONS.length });
            setScreen('end');
          } else {
            setCurrentQuestionIndex(prev => prev + 1);
            setGameState('waiting');
            setSelectedAnswer(null);
            setEliminatedAnswers([]);
          }
        }, 1500);
      } else {
        setGameState('wrong');
        setTimeout(() => {
          const result = { name: user.name, level: currentQuestionIndex, score: PRIZE_MILESTONES[currentQuestionIndex], date: new Date().toLocaleDateString() };
          saveLeaderboard(result);
          setUser({ ...user, score: PRIZE_MILESTONES[currentQuestionIndex], level: currentQuestionIndex });
          setScreen('end');
        }, 2000);
      }
    }, 2000); // Suspense delay
  };

  const useFiftyFifty = () => {
    if (lifelines.fifty || gameState !== 'waiting') return;
    const correct = QUESTIONS[currentQuestionIndex].correct;
    const wrongIndices = [0, 1, 2, 3].filter(i => i !== correct);
    // Remove 2 wrong answers
    const toRemove = wrongIndices.sort(() => 0.5 - Math.random()).slice(0, 2);
    setEliminatedAnswers(toRemove);
    setLifelines(prev => ({ ...prev, fifty: true }));
  };

  const usePhone = () => {
    if (lifelines.phone || gameState !== 'waiting') return;
    setLifelines(prev => ({ ...prev, phone: true }));
    alert(`Appel à un ami: "Je suis sûr à 80% que la réponse est ${String.fromCharCode(65 + QUESTIONS[currentQuestionIndex].correct)})"`);
  };

  const useAudience = () => {
    if (lifelines.audience || gameState !== 'waiting') return;
    setLifelines(prev => ({ ...prev, audience: true }));
    const correct = QUESTIONS[currentQuestionIndex].correct;
    // Simulate audience distribution
    const dist = [0, 0, 0, 0];
    dist[correct] = 60 + Math.floor(Math.random() * 20); // 60-80% for correct
    const remaining = 100 - dist[correct];
    const others = [0, 1, 2, 3].filter(i => i !== correct);
    dist[others[0]] = Math.floor(remaining * 0.6);
    dist[others[1]] = Math.floor(remaining * 0.3);
    dist[others[2]] = remaining - dist[others[0]] - dist[others[1]];
    
    alert(`Résultat du public:\nA: ${dist[0]}%\nB: ${dist[1]}%\nC: ${dist[2]}%\nD: ${dist[3]}%`);
  };

  const sendEmailReport = () => {
    const subject = encodeURIComponent("Rapport Leaderboard - Qui Veut Devenir Étudiant de 4ème Année");
    const body = encodeURIComponent(
      `Bonjour,\n\nVoici le classement actuel des étudiants:\n\n` +
      leaderboard.map((entry, i) => `${i + 1}. ${entry.name} - Niveau ${entry.level} (${entry.score} pts)`).join('\n') +
      `\n\nCordialement,\nSystème de Quiz.`
    );
    window.location.href = `mailto:ia.tarek.kamergi@gmail.com?subject=${subject}&body=${body}`;
  };

  return (
    <div className="min-h-screen bg-slate-950 text-white font-sans overflow-hidden relative selection:bg-blue-500 selection:text-white">
      {/* Background Image */}
      <div 
        className="absolute inset-0 z-0 opacity-40 bg-cover bg-center"
        style={{ backgroundImage: `url('https://image.qwenlm.ai/public_source/578b15c5-2e5b-45b8-8d8a-9d36d99cb351/1edddb2e0-89e7-4ff6-9ced-0397de19d820.png')` }}
      />
      <div className="absolute inset-0 z-0 bg-gradient-to-b from-slate-950/80 via-slate-900/60 to-slate-950/90" />

      <div className="relative z-10 container mx-auto px-4 py-8 h-screen flex flex-col">
        
        {/* Header */}
        <header className="flex justify-between items-center mb-8">
          <div className="flex items-center gap-3">
            <div className="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center shadow-lg shadow-blue-500/50">
              <Trophy className="w-6 h-6 text-white" />
            </div>
            <h1 className="text-2xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-blue-400 to-indigo-300">
              Qui Veut Devenir Étudiant de 4ème Année ?
            </h1>
          </div>
          <div className="flex gap-4">
            <Button variant="secondary" onClick={() => setScreen('leaderboard')} className="text-sm py-2 px-4">
              <Trophy className="w-4 h-4 mr-2 inline" /> Classement
            </Button>
            <Button variant="secondary" onClick={() => setScreen('login')} className="text-sm py-2 px-4">
              <RotateCcw className="w-4 h-4 mr-2 inline" /> Accueil
            </Button>
          </div>
        </header>

        {/* Main Content Area */}
        <main className="flex-1 flex items-center justify-center">
          
          {screen === 'login' && (
            <Card className="w-full max-w-md animate-fade-in-up">
              <div className="text-center mb-8">
                <h2 className="text-3xl font-bold mb-2 text-white">Bienvenue</h2>
                <p className="text-slate-400">Entrez votre pseudonyme pour commencer le quiz.</p>
              </div>
              <form onSubmit={(e) => { e.preventDefault(); handleLogin(e.target.elements.username.value); }}>
                <div className="mb-6">
                  <label className="block text-sm font-medium text-slate-300 mb-2">Pseudonyme</label>
                  <Input name="username" placeholder="Ex: ChimistePro" required />
                </div>
                <Button type="submit" className="w-full">
                  Commencer le Quiz <ArrowRight className="w-4 h-4 ml-2 inline" />
                </Button>
              </form>
            </Card>
          )}

          {screen === 'game' && (
            <div className="w-full max-w-5xl grid grid-cols-1 lg:grid-cols-3 gap-6">
              
              {/* Left: Lifelines & Info */}
              <div className="lg:col-span-1 space-y-4">
                <Card className="h-full flex flex-col justify-between">
                  <div>
                    <h3 className="text-lg font-bold text-blue-400 mb-4 flex items-center gap-2">
                      <User className="w-5 h-5" /> Joueur
                    </h3>
                    <div className="text-2xl font-bold mb-1">{user.name}</div>
                    <div className="text-slate-400 text-sm">Niveau actuel: {currentQuestionIndex + 1} / {QUESTIONS.length}</div>
                    
                    <div className="mt-8">
                      <h4 className="text-sm font-bold text-slate-500 uppercase tracking-wider mb-3">Jokers Disponibles</h4>
                      <div className="grid grid-cols-3 gap-2">
                        <button 
                          onClick={useFiftyFifty} disabled={lifelines.fifty}
                          className={`p-3 rounded-lg flex flex-col items-center justify-center transition-all ${lifelines.fifty ? 'bg-slate-800 text-slate-600' : 'bg-blue-900/50 hover:bg-blue-800 text-blue-200 border border-blue-700'}`}
                        >
                          <HelpCircle className="w-6 h-6 mb-1" />
                          <span className="text-xs">50/50</span>
                        </button>
                        <button 
                          onClick={usePhone} disabled={lifelines.phone}
                          className={`p-3 rounded-lg flex flex-col items-center justify-center transition-all ${lifelines.phone ? 'bg-slate-800 text-slate-600' : 'bg-blue-900/50 hover:bg-blue-800 text-blue-200 border border-blue-700'}`}
                        >
                          <Phone className="w-6 h-6 mb-1" />
                          <span className="text-xs">Ami</span>
                        </button>
                        <button 
                          onClick={useAudience} disabled={lifelines.audience}
                          className={`p-3 rounded-lg flex flex-col items-center justify-center transition-all ${lifelines.audience ? 'bg-slate-800 text-slate-600' : 'bg-blue-900/50 hover:bg-blue-800 text-blue-200 border border-blue-700'}`}
                        >
                          <Users className="w-6 h-6 mb-1" />
                          <span className="text-xs">Public</span>
                        </button>
                      </div>
                    </div>
                  </div>
                  <div className="mt-8 pt-6 border-t border-slate-700">
                     <div className="text-xs text-slate-500">Gains actuels garantis:</div>
                     <div className="text-xl font-mono text-amber-400">{PRIZE_MILESTONES[currentQuestionIndex].toLocaleString()} pts</div>
                  </div>
                </Card>
              </div>

              {/* Center: Question Area */}
              <div className="lg:col-span-2 flex flex-col gap-6">
                {/* Question Box */}
                <div className="bg-gradient-to-r from-slate-800 to-slate-900 border-2 border-slate-600 rounded-2xl p-8 text-center shadow-2xl relative overflow-hidden">
                  <div className="absolute top-0 left-0 w-full h-1 bg-gradient-to-r from-transparent via-blue-500 to-transparent opacity-50" />
                  <h2 className="text-xl md:text-2xl font-medium leading-relaxed text-white">
                    {QUESTIONS[currentQuestionIndex].question}
                  </h2>
                </div>

                {/* Answers Grid */}
                <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                  {QUESTIONS[currentQuestionIndex].answers.map((answer, idx) => {
                    const isEliminated = eliminatedAnswers.includes(idx);
                    const isSelected = selectedAnswer === idx;
                    const isCorrect = idx === QUESTIONS[currentQuestionIndex].correct;
                    
                    let statusClass = "bg-slate-800 border-slate-600 hover:bg-slate-700 hover:border-slate-400"; // Default
                    
                    if (gameState === 'correct' && isCorrect) statusClass = "bg-green-600 border-green-400 animate-pulse";
                    if (gameState === 'wrong' && isSelected) statusClass = "bg-red-600 border-red-400";
                    if (gameState === 'wrong' && isCorrect) statusClass = "bg-green-600 border-green-400"; // Show correct answer on loss
                    if (isEliminated) statusClass = "bg-slate-900 border-slate-800 text-slate-600 opacity-50 cursor-not-allowed";

                    return (
                      <button
                        key={idx}
                        onClick={() => handleAnswer(idx)}
                        disabled={gameState !== 'waiting' || isEliminated}
                        className={`relative p-6 rounded-xl border-2 text-left transition-all duration-300 group ${statusClass}`}
                      >
                        <span className="absolute left-4 top-1/2 -translate-y-1/2 w-8 h-8 rounded-full bg-slate-950/50 border border-slate-500 flex items-center justify-center text-sm font-bold text-slate-300 group-hover:border-white group-hover:text-white">
                          {String.fromCharCode(65 + idx)}
                        </span>
                        <span className="ml-8 text-lg font-medium">{answer}</span>
                      </button>
                    );
                  })}
                </div>
              </div>
            </div>
          )}

          {screen === 'leaderboard' && (
            <Card className="w-full max-w-4xl animate-fade-in">
              <div className="flex justify-between items-center mb-6">
                <h2 className="text-2xl font-bold flex items-center gap-2">
                  <Trophy className="w-6 h-6 text-amber-400" /> Classement des Étudiants
                </h2>
                <Button variant="primary" onClick={sendEmailReport} className="text-sm">
                  <Mail className="w-4 h-4 mr-2 inline" /> Envoyer par Email
                </Button>
              </div>
              
              <div className="overflow-x-auto">
                <table className="w-full text-left border-collapse">
                  <thead>
                    <tr className="border-b border-slate-700 text-slate-400 text-sm uppercase tracking-wider">
                      <th className="p-4">Rang</th>
                      <th className="p-4">Pseudonyme</th>
                      <th className="p-4">Niveau Atteint</th>
                      <th className="p-4 text-right">Score</th>
                      <th className="p-4 text-right">Date</th>
                    </tr>
                  </thead>
                  <tbody>
                    {leaderboard.length === 0 ? (
                      <tr>
                        <td colSpan="5" className="p-8 text-center text-slate-500">Aucun classement disponible pour le moment.</td>
                      </tr>
                    ) : (
                      leaderboard.map((entry, idx) => (
                        <tr key={idx} className="border-b border-slate-800 hover:bg-slate-800/50 transition-colors">
                          <td className="p-4 font-bold text-slate-300">#{idx + 1}</td>
                          <td className="p-4 font-medium text-white">{entry.name}</td>
                          <td className="p-4 text-blue-300">{entry.level} / {QUESTIONS.length}</td>
                          <td className="p-4 text-right font-mono text-amber-400">{entry.score.toLocaleString()}</td>
                          <td className="p-4 text-right text-slate-500 text-sm">{entry.date}</td>
                        </tr>
                      ))
                    )}
                  </tbody>
                </table>
              </div>
              <div className="mt-6 flex justify-end">
                <Button variant="secondary" onClick={() => setScreen('login')}>Retour</Button>
              </div>
            </Card>
          )}

          {screen === 'end' && (
            <Card className="w-full max-w-lg text-center animate-fade-in-up">
              <div className="mb-6">
                {user.level === QUESTIONS.length ? (
                  <div className="w-24 h-24 bg-amber-500/20 rounded-full flex items-center justify-center mx-auto mb-4 border-4 border-amber-500">
                    <Trophy className="w-12 h-12 text-amber-400" />
                  </div>
                ) : (
                  <div className="w-24 h-24 bg-red-500/20 rounded-full flex items-center justify-center mx-auto mb-4 border-4 border-red-500">
                    <XCircle className="w-12 h-12 text-red-400" />
                  </div>
                )}
                <h2 className="text-3xl font-bold mb-2">
                  {user.level === QUESTIONS.length ? "Félicitations !" : "Partie Terminée"}
                </h2>
                <p className="text-slate-400">
                  {user.level === QUESTIONS.length 
                    ? "Vous avez atteint le niveau maximum et obtenu le titre d'Étudiant de 4ème Année !" 
                    : `Vous avez atteint le niveau ${user.level}.`}
                </p>
              </div>

              <div className="bg-slate-800/50 rounded-lg p-6 mb-8">
                <div className="text-sm text-slate-400 uppercase tracking-wider mb-1">Score Final</div>
                <div className="text-4xl font-mono font-bold text-white">{user.score.toLocaleString()} pts</div>
              </div>

              <div className="flex gap-4 justify-center">
                <Button variant="gold" onClick={() => setScreen('leaderboard')}>
                  Voir le Classement
                </Button>
                <Button variant="secondary" onClick={() => setScreen('login')}>
                  Rejouer
                </Button>
              </div>
            </Card>
          )}

        </main>
      </div>
      
      <style jsx global>{`
        @keyframes fade-in-up {
          from { opacity: 0; transform: translateY(20px); }
          to { opacity: 1; transform: translateY(0); }
        }
        @keyframes fade-in {
          from { opacity: 0; }
          to { opacity: 1; }
        }
        .animate-fade-in-up { animation: fade-in-up 0.5s ease-out forwards; }
        .animate-fade-in { animation: fade-in 0.3s ease-out forwards; }
      `}</style>
    </div>
  );
};

const root = createRoot(document.getElementById('root'));
root.render(<App />);
