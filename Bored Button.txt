import React, { useState, useEffect } from 'react';
import { Home, Grid, Heart, RefreshCcw, Sun, Moon } from "lucide-react";
import * as Tone from 'tone';

// --- Components for Interactive Content ---

// A simple Rock, Paper, Scissors game component
const RockPaperScissors = ({ setResult }) => {
  const handleRPS = (userChoice) => {
    const choices = ["rock", "paper", "scissors"];
    const computerChoice = choices[Math.floor(Math.random() * choices.length)];
    let newResult = "";
    if (userChoice === computerChoice) newResult = `You chose ${userChoice}, the computer chose ${computerChoice}. It's a draw!`;
    else if ((userChoice === "rock" && computerChoice === "scissors") || (userChoice === "paper" && computerChoice === "rock") || (userChoice === "scissors" && computerChoice === "paper")) newResult = `You chose ${userChoice}, the computer chose ${computerChoice}. You win!`;
    else newResult = `You chose ${userChoice}, the computer chose ${computerChoice}. You lose!`;
    setResult(newResult);
  };
  return (
    <div className="flex flex-col items-center">
      <p className="text-xl text-gray-600 font-medium mb-4">Rock, Paper, Scissors!</p>
      <div className="flex space-x-4">
        <button onClick={() => handleRPS('rock')} className="bg-gray-300 p-4 rounded-full text-3xl hover:bg-gray-400 transition-colors">✊</button>
        <button onClick={() => handleRPS('paper')} className="bg-gray-300 p-4 rounded-full text-3xl hover:bg-gray-400 transition-colors">✋</button>
        <button onClick={() => handleRPS('scissors')} className="bg-gray-300 p-4 rounded-full text-3xl hover:bg-gray-400 transition-colors">✌️</button>
      </div>
    </div>
  );
};

// A simple Would You Rather component
const WouldYouRather = ({ content, setResult }) => {
  const handleWyr = (userChoice) => {
    const percentages = { [content.options[0]]: Math.floor(Math.random() * 60) + 20, [content.options[1]]: 0 };
    percentages[content.options[1]] = 100 - percentages[content.options[0]];
    setResult(`You chose "${userChoice}". ${percentages[userChoice]}% of people agree with you!`);
  }
  return (
    <div className="flex flex-col items-center">
      <p className="text-xl text-gray-600 font-medium mb-4">{content.question}</p>
      <div className="flex flex-col w-full space-y-2">
        <button onClick={() => handleWyr(content.options[0])} className="bg-indigo-500 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-600 transition-colors">{content.options[0]}</button>
        <button onClick={() => handleWyr(content.options[1])} className="bg-indigo-500 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-600 transition-colors">{content.options[1]}</button>
      </div>
    </div>
  );
};

// A simple Word Scramble game component
const WordScrambleGame = ({ setResult }) => {
  const words = ['react', 'javascript', 'tailwind', 'component', 'application'];
  const [currentWord, setCurrentWord] = useState('');
  const [shuffledWord, setShuffledWord] = useState('');
  const [userGuess, setUserGuess] = useState('');

  useEffect(() => {
    const newWord = words[Math.floor(Math.random() * words.length)];
    setCurrentWord(newWord);
    setShuffledWord(newWord.split('').sort(() => Math.random() - 0.5).join(''));
    setUserGuess('');
  }, []);

  const handleCheckGuess = () => {
    if (userGuess.toLowerCase() === currentWord) setResult("Correct! You unscrambled the word.");
    else setResult(`That's incorrect. The correct word was "${currentWord}".`);
  };

  return (
    <div className="flex flex-col items-center">
      <p className="text-xl text-gray-600 font-medium mb-4">Unscramble this word:</p>
      <p className="text-2xl font-bold tracking-widest mb-4">{shuffledWord}</p>
      <input type="text" value={userGuess} onChange={(e) => setUserGuess(e.target.value)} className="w-full text-center p-2 rounded-lg mb-4 focus:outline-none focus:ring-2 focus:ring-indigo-500" placeholder="Your guess..." />
      <button onClick={handleCheckGuess} className="w-full bg-green-500 text-white font-bold py-2 px-4 rounded-xl shadow-lg transition-all duration-300 hover:bg-green-600 active:scale-95">Check</button>
    </div>
  );
};

// A simple Text Adventure game component
const TextAdventureGame = () => {
  const [scene, setScene] = useState('start');
  const [history, setHistory] = useState([]);
  const gameData = {
    start: { text: "You find yourself at a fork in the road. Which path do you take?", choices: [{ text: "Go into the forest", nextScene: 'forest' }, { text: "Enter the cave", nextScene: 'cave' }] },
    forest: { text: "The forest is eerily quiet. A wolf stares at you from a distance. Do you try to befriend it or run away?", choices: [{ text: "Befriend the wolf", nextScene: 'befriend' }, { text: "Run away", nextScene: 'run' }] },
    cave: { text: "The cave is filled with glowing crystals. You see a large treasure chest. Do you open it or leave it alone?", choices: [{ text: "Open the chest", nextScene: 'treasure' }, { text: "Leave it alone", nextScene: 'safe' }] },
    befriend: { text: "The wolf wags its tail and leads you to a hidden village. You are a hero!", choices: [] },
    run: { text: "You run away and get lost in the forest. You lose...", choices: [] },
    treasure: { text: "The chest is a trap! A dragon awakens and breathes fire. You lose...", choices: [] },
    safe: { text: "You wisely leave the treasure alone and find a different, safer path out. You win!", choices: [] },
  };
  const handleChoice = (choice) => {
    setHistory([...history, gameData[scene].text, `You chose: ${choice.text}`]);
    setScene(choice.nextScene);
  };
  const handleRestart = () => {
    setHistory([]);
    setScene('start');
  };
  const currentSceneData = gameData[scene];
  return (
    <div className="flex flex-col items-center text-center w-full">
      <div className="bg-gray-200 p-4 rounded-lg min-h-[150px] overflow-auto mb-4 w-full">
        {history.map((line, index) => <p key={index} className="text-gray-700 text-sm mb-1">{line}</p>)}
        <p className="text-gray-900 font-bold mt-2">{currentSceneData.text}</p>
      </div>
      <div className="flex flex-col w-full space-y-2">
        {currentSceneData.choices.map((choice, index) => <button key={index} onClick={() => handleChoice(choice)} className="bg-indigo-500 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-600 transition-colors">{choice.text}</button>)}
      </div>
      {currentSceneData.choices.length === 0 && <button onClick={handleRestart} className="mt-4 bg-green-500 text-white font-bold py-2 px-4 rounded-xl hover:bg-green-600 transition-colors">Play Again</button>}
    </div>
  );
};

// New Tic-Tac-Toe component
const TicTacToe = ({ setResult }) => {
  const [board, setBoard] = useState(Array(9).fill(null));
  const [xIsNext, setXIsNext] = useState(true);
  const [winner, setWinner] = useState(null);

  const calculateWinner = (squares) => {
    const lines = [
      [0, 1, 2], [3, 4, 5], [6, 7, 8],
      [0, 3, 6], [1, 4, 7], [2, 5, 8],
      [0, 4, 8], [2, 4, 6],
    ];
    for (let i = 0; i < lines.length; i++) {
      const [a, b, c] = lines[i];
      if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
        return squares[a];
      }
    }
    return null;
  };

  const handleClick = (i) => {
    if (winner || board[i]) {
      return;
    }
    const newBoard = board.slice();
    newBoard[i] = xIsNext ? 'X' : 'O';
    setBoard(newBoard);
    setXIsNext(!xIsNext);
    const newWinner = calculateWinner(newBoard);
    if (newWinner) {
      setWinner(newWinner);
      setResult(`Winner: ${newWinner}`);
    } else if (newBoard.every(square => square !== null)) {
      setWinner('Draw');
      setResult('It\'s a draw!');
    }
  };

  const handleRestart = () => {
    setBoard(Array(9).fill(null));
    setXIsNext(true);
    setWinner(null);
    setResult('');
  };

  return (
    <div className="flex flex-col items-center">
      <p className="text-xl text-gray-600 font-medium mb-4">{winner ? (winner === 'Draw' ? "It's a draw!" : `Winner: ${winner}`) : `Next player: ${xIsNext ? 'X' : 'O'}`}</p>
      <div className="grid grid-cols-3 gap-2">
        {board.map((square, i) => (
          <button
            key={i}
            className={`w-20 h-20 bg-gray-300 text-4xl font-bold rounded-lg flex items-center justify-center transition-colors ${square === 'X' ? 'text-indigo-600' : 'text-purple-600'} hover:bg-gray-400`}
            onClick={() => handleClick(i)}
          >
            {square}
          </button>
        ))}
      </div>
      {(winner || board.every(square => square !== null)) && (
        <button onClick={handleRestart} className="mt-4 bg-green-500 text-white font-bold py-2 px-4 rounded-xl hover:bg-green-600 transition-colors">Play Again</button>
      )}
    </div>
  );
};

// New Trivia Quiz component
const TriviaQuiz = ({ setResult }) => {
  const [question, setQuestion] = useState(null);
  const [selectedAnswer, setSelectedAnswer] = useState(null);
  const [isAnswered, setIsAnswered] = useState(false);

  const triviaQuestions = [
    {
      question: "What is the capital of France?",
      options: ["Berlin", "Madrid", "Paris", "Rome"],
      answer: "Paris"
    },
    {
      question: "Which planet is known as the Red Planet?",
      options: ["Jupiter", "Mars", "Venus", "Saturn"],
      answer: "Mars"
    },
    {
      question: "What is the largest ocean on Earth?",
      options: ["Atlantic Ocean", "Indian Ocean", "Arctic Ocean", "Pacific Ocean"],
      answer: "Pacific Ocean"
    },
    {
      question: "Who wrote 'To Kill a Mockingbird'?",
      options: ["Harper Lee", "Mark Twain", "Ernest Hemingway", "F. Scott Fitzgerald"],
      answer: "Harper Lee"
    }
  ];

  const getNewQuestion = () => {
    const randomIndex = Math.floor(Math.random() * triviaQuestions.length);
    setQuestion(triviaQuestions[randomIndex]);
    setSelectedAnswer(null);
    setIsAnswered(false);
    setResult("New trivia question loaded!");
  };

  useEffect(() => {
    getNewQuestion();
  }, []);

  const handleAnswerClick = (option) => {
    setSelectedAnswer(option);
    setIsAnswered(true);
    if (option === question.answer) {
      setResult("Correct! ✨");
    } else {
      setResult(`Incorrect. The answer was "${question.answer}".`);
    }
  };

  if (!question) {
    return <p className="text-xl text-gray-600 font-medium">Loading trivia question...</p>;
  }

  return (
    <div className="flex flex-col items-center w-full">
      <p className="text-xl text-gray-600 font-medium mb-4">{question.question}</p>
      <div className="flex flex-col w-full space-y-2">
        {question.options.map((option, index) => (
          <button
            key={index}
            onClick={() => !isAnswered && handleAnswerClick(option)}
            className={`
              w-full text-white font-bold py-2 px-4 rounded-lg transition-colors
              ${isAnswered && option === question.answer ? 'bg-green-500' : ''}
              ${isAnswered && option !== question.answer && option === selectedAnswer ? 'bg-red-500' : ''}
              ${!isAnswered ? 'bg-indigo-500 hover:bg-indigo-600' : 'bg-gray-400 cursor-not-allowed'}
            `}
          >
            {option}
          </button>
        ))}
      </div>
      {isAnswered && (
        <button
          onClick={getNewQuestion}
          className="mt-4 w-full bg-purple-500 text-white font-bold py-2 px-4 rounded-xl shadow-lg transition-all duration-300 hover:bg-purple-600 active:scale-95"
        >
          New Question
        </button>
      )}
    </div>
  );
};


// The main App component that orchestrates the entire application
const App = () => {
  const [currentScreen, setCurrentScreen] = useState('home');
  const [currentActivity, setCurrentActivity] = useState('');
  const [isLoading, setIsLoading] = useState(false);
  const [favorites, setFavorites] = useState([]);
  const [interactiveContent, setInteractiveContent] = useState(null);
  const [selectedMood, setSelectedMood] = useState(null);
  const [message, setMessage] = useState(null);
  const [toneSynth, setToneSynth] = useState(null);
  const [tonePing, setTonePing] = useState(null);
  const [audio, setAudio] = useState(null);
  const [theme, setTheme] = useState('light');

  useEffect(() => {
    const storedFavorites = JSON.parse(localStorage.getItem('boredButtonFavorites'));
    if (storedFavorites) setFavorites(storedFavorites);
    const synth = new Tone.Synth({ oscillator: { type: 'sine' }, envelope: { attack: 0.005, decay: 0.1, sustain: 0, release: 0.1 } }).toDestination();
    setToneSynth(synth);
    const ping = new Tone.MembraneSynth().toDestination();
    setTonePing(ping);
    return () => { if (synth) synth.dispose(); if (ping) ping.dispose(); };
  }, []);

  useEffect(() => { localStorage.setItem('boredButtonFavorites', JSON.stringify(favorites)); }, [favorites]);

  const activities = {
    quickDistractions: {
      moods: ['relaxed', 'energetic'],
      content: ["Listen to a podcast episode about a topic you know nothing about.", "Take a short quiz to learn something new about yourself.", "Browse old photos and reminisce about a favorite memory.", "Learn a new word today and try to use it in a conversation.", "Find a new recipe for a drink you've never tried before.", "Practice a few minutes of mindful breathing or meditation.", "Organize your top five apps on your phone or computer."]
    },
    creativeActivities: {
      moods: ['creative', 'relaxed'],
      content: ["Write a short story about a talking pet.", "Draw a self-portrait with your non-dominant hand.", "Create a playlist for a specific mood or fictional character.", "Write down a short poem about the last thing you ate.", "Try to build a sculpture out of whatever you can find in the room.", "Write a letter to your future self and seal it away."]
    },
    miniGames: {
      moods: ['energetic', 'focused'],
      content: ["rockPaperScissors", "wordScramble", "textAdventure", "ticTacToe", "triviaQuiz"]
    },
    brainTeasers: {
      moods: ['focused'],
      content: ["I have cities, but no houses. I have mountains, but no trees. I have water, but no fish. What am I? (A map)", "What has to be broken before you can use it? (An egg)", "What is full of holes but still holds water? (A sponge)", "What has a neck and no head, two arms and no hands? (A shirt)", "What question can you never answer yes to? (Are you asleep yet?)"]
    },
    physicalChallenges: {
      moods: ['energetic', 'adventurous'],
      content: ["Do 10 jumping jacks.", "Hold a plank for 30 seconds.", "Do a wall sit for 45 seconds.", "Do a set of 20 squats.", "Try a simple yoga stretch for 5 minutes."]
    },
    apiContent: {
      moods: ['relaxed', 'focused'],
      content: { 'joke': "Get a random dad joke.", 'fact': "Get a random fact of the day." }
    },
    wouldYouRather: {
      moods: ['relaxed', 'social', 'silly'],
      content: [{ question: "Would you rather be able to talk to animals or be fluent in every human language?", options: ["Talk to animals", "Fluent in all languages"] }, { question: "Would you rather have a personal chef or a personal masseuse?", options: ["Personal chef", "Personal masseuse"] }, { question: "Would you rather be able to fly or be invisible?", options: ["Fly", "Invisible"] }, { question: "Would you rather live in a house made of candy or a house made of pillows?", options: ["Candy house", "Pillow house"] }, { question: "Would you rather be a master of all musical instruments or a master of all languages?", options: ["Instruments", "Languages"] }]
    },
    reflective: {
      moods: ['reflective'],
      content: ["Think about a goal you have and write down one small step you can take today to achieve it.", "Write a list of your top 5 favorite things about yourself.", "Consider a mistake you made and what you learned from it.", "Write down a list of 5 places you would love to visit and why."]
    },
    silly: {
      moods: ['silly'],
      content: ["Try to say a tongue twister five times fast.", "Make up a silly song about the closest object to you.", "Attempt to juggle three random items from your desk.", "Tell yourself a joke in a mirror until you laugh."]
    },
    adventurous: {
      moods: ['adventurous'],
      content: ["Find a new trail or park to walk in.", "Explore a local neighborhood you've never been to before.", "Try a food from a different culture you've never had.", "Do something that pushes you slightly outside your comfort zone."]
    }
  };
  
  const moods = ['Relaxed', 'Creative', 'Energetic', 'Focused', 'Social', 'Adventurous', 'Reflective', 'Silly'];

  const playSynth = (note) => { if (toneSynth) { Tone.start(); toneSynth.triggerAttackRelease(note, "8n"); } };
  const playPing = () => { if (tonePing) { Tone.start(); tonePing.triggerAttackRelease("C2", "8n"); } };
  const showMessage = (msg) => { setMessage(msg); setTimeout(() => setMessage(null), 3000); };
  
  const fetchRandomJoke = async () => {
    try {
      setIsLoading(true);
      const response = await fetch('https://icanhazdadjoke.com/', { headers: { 'Accept': 'application/json' } });
      const data = await response.json();
      return data.joke;
    } catch (error) {
      console.error("Failed to fetch joke:", error);
      return "Failed to fetch a joke. Try a different activity!";
    } finally {
      setIsLoading(false);
    }
  };
  const fetchRandomFact = async () => {
    try {
      setIsLoading(true);
      const response = await fetch('https://uselessfacts.jsph.pl/random.json?language=en');
      const data = await response.json();
      return data.text;
    } catch (error) {
      console.error("Failed to fetch fact:", error);
      return "Failed to fetch a fact. Try a different activity!";
    } finally {
      setIsLoading(false);
    }
  };

  const getRandomActivity = async (category = null) => {
    playSynth("C4");
    setInteractiveContent(null);
    let availableCategories = Object.keys(activities);
    if (selectedMood) {
      const moodLowerCase = selectedMood.toLowerCase();
      availableCategories = availableCategories.filter(cat => activities[cat].moods.includes(moodLowerCase));
    }
    if (availableCategories.length === 0) {
      showMessage("No activities found for this mood. Try another mood!");
      setCurrentActivity("No activities found for this mood. Try another mood!");
      return;
    }
    let randomCategory = category;
    if (!randomCategory) randomCategory = availableCategories[Math.floor(Math.random() * availableCategories.length)];
    else if (!availableCategories.includes(randomCategory)) { showMessage(`No activities for "${category}" found in your current mood.`); setCurrentActivity(`No activities for "${category}" found in your current mood.`); return; }
    
    let newActivity;
    let newInteractiveContent = null;
    
    if (randomCategory === 'apiContent') {
      const apiActions = Object.keys(activities.apiContent.content);
      const apiAction = apiActions[Math.floor(Math.random() * apiActions.length)];
      if (apiAction === 'joke') newActivity = await fetchRandomJoke();
      else if (apiAction === 'fact') newActivity = await fetchRandomFact();
    } else if (randomCategory === 'miniGames') {
        const game = activities[randomCategory].content[Math.floor(Math.random() * activities[randomCategory].content.length)];
        newInteractiveContent = { type: game, setResult: (msg) => { showMessage(msg); setCurrentActivity(msg); }, setIsLoading: setIsLoading };
        newActivity = "Let's play a game!";
    } else if (randomCategory === 'wouldYouRather') {
      const randomIndex = Math.floor(Math.random() * activities[randomCategory].content.length);
      const wyr = activities[randomCategory].content[randomIndex];
      newInteractiveContent = { type: 'wouldYouRather', ...wyr, setResult: showMessage };
      newActivity = wyr.question;
    } else {
      const randomActivityIndex = Math.floor(Math.random() * activities[randomCategory].content.length);
      newActivity = activities[randomCategory].content[randomActivityIndex];
    }
    
    setCurrentActivity(newActivity);
    setInteractiveContent(newInteractiveContent);
  };

  const handleSaveFavorite = () => {
    if (currentActivity && !favorites.includes(currentActivity)) {
      setFavorites([...favorites, currentActivity]);
      playPing();
      showMessage("Added to favorites!");
    } else if (currentActivity) {
      showMessage("This is already in your favorites!");
    }
  };

  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light');
  };

  const NavigationBar = () => (
    <nav className="fixed bottom-0 left-0 right-0 bg-white shadow-lg rounded-t-3xl p-4 flex justify-around z-10">
      <NavItem icon={<Home size={24} />} label="Home" active={currentScreen === 'home'} onClick={() => setCurrentScreen('home')} />
      <NavItem icon={<Grid size={24} />} label="Categories" active={currentScreen === 'categories'} onClick={() => setCurrentScreen('categories')} />
      <NavItem icon={<Heart size={24} />} label="Favorites" active={currentScreen === 'favorites'} onClick={() => setCurrentScreen('favorites')} />
    </nav>
  );

  const NavItem = ({ icon, label, active, onClick }) => (
    <button onClick={() => { onClick(); playSynth("D4"); }} className={`flex flex-col items-center p-2 rounded-xl transition-all duration-300 ${active ? 'text-indigo-600 bg-indigo-100' : 'text-gray-400 hover:text-indigo-600 hover:bg-gray-50'}`}>
      {icon}
      <span className="text-xs font-medium mt-1">{label}</span>
    </button>
  );

  const HomeScreen = () => (
    <div className="flex flex-col items-center justify-center h-full">
      <h1 className="text-4xl font-bold text-gray-800 dark:text-gray-100 mb-4">Feeling Bored?</h1>
      <div className="grid grid-cols-3 md:grid-cols-4 gap-2 mb-6 w-full max-w-sm sm:max-w-md md:max-w-lg mx-auto">
        {moods.map(mood => (
          <button key={mood} onClick={() => { setSelectedMood(mood); playSynth("E4"); }} className={`px-4 py-2 text-sm rounded-full transition-all duration-300 ${selectedMood === mood ? 'bg-indigo-600 text-white shadow-md' : 'bg-gray-200 text-gray-600 hover:bg-gray-300 dark:bg-gray-700 dark:text-gray-200 dark:hover:bg-gray-600'}`}>
            {mood}
          </button>
        ))}
      </div>
      <div className="bg-gray-100 dark:bg-gray-800 p-6 rounded-2xl mb-8 min-h-[120px] w-full flex items-center justify-center transition-all duration-300 relative text-center">
        {isLoading ? (
          <div className="animate-spin rounded-full h-12 w-12 border-4 border-t-indigo-500 border-gray-200"></div>
        ) : interactiveContent ? (
          <div className="w-full">
            {interactiveContent.type === 'rockPaperScissors' && <RockPaperScissors setResult={interactiveContent.setResult} />}
            {interactiveContent.type === 'wouldYouRather' && <WouldYouRather content={interactiveContent} setResult={interactiveContent.setResult} />}
            {interactiveContent.type === 'wordScramble' && <WordScrambleGame setResult={interactiveContent.setResult} />}
            {interactiveContent.type === 'textAdventure' && <TextAdventureGame />}
            {interactiveContent.type === 'ticTacToe' && <TicTacToe setResult={interactiveContent.setResult} />}
            {interactiveContent.type === 'triviaQuiz' && <TriviaQuiz setResult={interactiveContent.setResult} />}
          </div>
        ) : (
          <p className="text-xl text-gray-600 dark:text-gray-300 font-medium leading-relaxed animate-fade-in">
            {currentActivity || "Click the button to get an idea!"}
          </p>
        )}
      </div>
      <div className="flex w-full space-x-2">
        <button onClick={handleSaveFavorite} className="bg-red-500 text-white font-bold p-4 rounded-xl shadow-lg transition-all duration-300 hover:bg-red-600 hover:shadow-xl focus:outline-none focus:ring-4 focus:ring-red-400 focus:ring-opacity-50 active:scale-95" disabled={!currentActivity || favorites.includes(currentActivity)}>
          <Heart size={24} />
        </button>
        <button id="bored-button" onClick={() => getRandomActivity()} className="w-full bg-indigo-600 text-white font-bold py-4 px-6 rounded-xl shadow-lg transition-all duration-300 hover:bg-indigo-700 hover:shadow-xl focus:outline-none focus:ring-4 focus:ring-indigo-500 focus:ring-opacity-50 active:scale-95 flex items-center justify-center">
          <RefreshCcw size={24} className="mr-2" /> Press Me!
        </button>
      </div>
    </div>
  );

  const CategoriesScreen = () => (
    <div className="flex flex-col h-full">
      <h1 className="text-4xl font-bold text-gray-800 dark:text-gray-100 mb-6 text-center">Categories</h1>
      <div className="grid grid-cols-2 gap-4">
        {Object.keys(activities).map((category, index) => (
          <button key={index} onClick={() => { getRandomActivity(category); setCurrentScreen('home'); }} className="bg-indigo-500 text-white p-6 rounded-xl shadow-md font-bold text-lg transition-all duration-300 hover:bg-indigo-600 active:scale-95">
            {category.replace(/([A-Z])/g, ' $1').trim()}
          </button>
        ))}
      </div>
      <button onClick={() => { getRandomActivity(); setCurrentScreen('home'); }} className="w-full mt-8 bg-purple-500 text-white font-bold py-4 px-6 rounded-xl shadow-lg transition-all duration-300 hover:bg-purple-600 active:scale-95">
        Surprise Me!
      </button>
    </div>
  );

  const FavoritesScreen = () => (
    <div className="flex flex-col h-full">
      <h1 className="text-4xl font-bold text-gray-800 dark:text-gray-100 mb-6 text-center">Favorites</h1>
      {favorites.length === 0 ? (<p className="text-center text-gray-500 dark:text-gray-400">You haven't saved any activities yet.</p>) : (
        <ul className="space-y-4">
          {favorites.map((fav, index) => (
            <li key={index} className="bg-gray-100 dark:bg-gray-800 p-4 rounded-xl flex items-center justify-between">
              <p className="text-gray-700 dark:text-gray-300">{fav}</p>
              <button onClick={() => setFavorites(favorites.filter(item => item !== fav))} className="text-red-500 hover:text-red-700 transition-colors">
                <Heart size={20} fill="currentColor" />
              </button>
            </li>
          ))}
        </ul>
      )}
    </div>
  );
  
  const MessageModal = ({ message }) => (
    <div className="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50 animate-fade-in">
      <div className="bg-white dark:bg-gray-800 p-6 rounded-xl shadow-2xl">
        <p className="text-lg font-bold text-center text-gray-900 dark:text-gray-100">{message}</p>
      </div>
    </div>
  );

  const renderScreen = () => {
    switch (currentScreen) {
      case 'home': return <HomeScreen />;
      case 'categories': return <CategoriesScreen />;
      case 'favorites': return <FavoritesScreen />;
      default: return <HomeScreen />;
    }
  };

  return (
    <div className={`${theme === 'dark' ? 'dark' : ''} min-h-screen`}>
      <div className="bg-gradient-to-br from-indigo-500 to-purple-600 dark:from-gray-900 dark:to-gray-800 min-h-screen flex flex-col items-center justify-center p-4">
        <button
          onClick={toggleTheme}
          className="absolute top-4 right-4 p-2 rounded-full bg-white dark:bg-gray-800 shadow-lg text-gray-800 dark:text-gray-200 transition-colors"
        >
          {theme === 'light' ? <Moon size={20} /> : <Sun size={20} />}
        </button>
        {message && <MessageModal message={message} />}
        <div className="bg-white dark:bg-gray-900 p-8 rounded-3xl shadow-2xl max-w-xl w-full text-center flex-grow flex flex-col justify-center mb-20 transform transition-all duration-300 hover:scale-105">
          {renderScreen()}
        </div>
        <NavigationBar />
      </div>
    </div>
  );
};
export default App;
