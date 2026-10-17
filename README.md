// ==========================================
// 1. QUIZ LOGIC (For quiz.html)
// ==========================================
const quizData = [
  {
    question: "What is the primary role of AI in education?",
    options: ["Personalized learning", "Replacing classrooms completely", "Storing physical books"],
    correct: 0
  },
  {
    question: "Which subject focuses on circuits and electronic components?",
    options: ["Civil Engineering", "Basic Electronics", "Environmental Science"],
    correct: 1
  },
  {
    question: "What helps improve student performance the most?",
    options: ["Consistent feedback and practice", "Skipping assessments", "Memorizing without understanding"],
    correct: 0
  }
];

let currentQuestionIndex = 0;
let score = 0;

const questionEl = document.getElementById("question");
const optionsEl = document.getElementById("options");
const nextBtn = document.getElementById("next-btn");
const resultEl = document.getElementById("result");

function loadQuestion() {
  if (!questionEl) return;

  const currentQuiz = quizData[currentQuestionIndex];
  questionEl.innerText = `${currentQuestionIndex + 1}. ${currentQuiz.question}`;
  optionsEl.innerHTML = "";

  currentQuiz.options.forEach((option, index) => {
    const button = document.createElement("button");
    button.innerText = option;
    button.className = "quiz-opt-btn";
    button.onclick = () => selectOption(index);
    optionsEl.appendChild(button);
  });
}

function selectOption(selectedIndex) {
  if (selectedIndex === quizData[currentQuestionIndex].correct) {
    score++;
  }
  currentQuestionIndex++;

  if (currentQuestionIndex < quizData.length) {
    loadQuestion();
  } else {
    showQuizResult();
  }
}

function showQuizResult() {
  const quizBox = document.getElementById("quiz-container");
  if (quizBox) quizBox.style.display = "none";

  if (resultEl) {
    resultEl.style.display = "block";
    resultEl.innerHTML = `
      <h3>Quiz Completed!</h3>
      <p>Your Score: <strong>${score} / ${quizData.length}</strong></p>
      <a href="feedback.html" class="btn btn-primary" style="margin-top:10px;">Check Your Feedback</a>
    `;
  }
}

if (questionEl) {
  loadQuestion();
}

// ==========================================
// 2. AI ASSISTANT CHAT LOGIC (For AI assistant.html)
// ==========================================
const sendBtn = document.getElementById("send-btn");
const userInput = document.getElementById("user-input");
const chatBox = document.getElementById("chat-box");

if (sendBtn && userInput && chatBox) {
  sendBtn.addEventListener("click", () => {
    const query = userInput.value.trim();
    if (!query) return;

    const userMsg = document.createElement("div");
    userMsg.className = "message user";
    userMsg.innerText = query;
    chatBox.appendChild(userMsg);

    userInput.value = "";
    chatBox.scrollTop = chatBox.scrollHeight;

    setTimeout(() => {
      const aiMsg = document.createElement("div");
      aiMsg.className = "message ai";
      aiMsg.innerText = `Study Buddy AI: Great question about "${query}"! Focus on understanding the core concepts and practicing step-by-step examples.`;
      chatBox.appendChild(aiMsg);
      chatBox.scrollTop = chatBox.scrollHeight;
    }, 600);
  });

  userInput.addEventListener("keypress", (e) => {
    if (e.key === "Enter") sendBtn.click();
  });
}
