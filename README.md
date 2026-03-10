import { useState, useEffect, useRef, useCallback } from "react";

const CHAPTERS = [
  {
    id: 1,
    title: "CHAPTER 01: INFILTRATION",
    concept: "Variables & Data Types",
    conceptIcon: "📦",
    intro: `The year is 2031. NovaCorp controls the internet — every news feed, every social post, every election. They built an AI called NEXUS to do it for them.\n\nBut someone left a backdoor open.\n\nYou're Ghost. Seventeen years old, a laptop, and a head full of code. You've found the door. Now you need to get inside.\n\nFirst lesson: data. Every piece of information has a TYPE — text, numbers, true/false. Get the types wrong and the system rejects you.\n\nTime to see if you belong here.`,
    questions: [
      {
        story: "The login terminal prompts for your hacker alias. You type it in. The system crashes immediately. Something is wrong with how your name is stored.",
        code: `// Your alias should be stored as text\nlet alias = 42;  // ← this doesn't look right`,
        question: "Which variable correctly stores your hacker alias as text?",
        options: ['let alias = 42', 'let alias = "Ghost"', 'let alias = true', 'let alias = [Ghost]'],
        correct: 1,
        explanation: 'Strings store text using quotes: "Ghost". The value 42 is a Number, true is a Boolean, and [Ghost] is an invalid Array. The terminal only accepts a string.',
        storyFeedback: {
          correct: "Access granted. The terminal flickers green. You're in the lobby, Ghost.",
          wrong: "SYSTEM ERROR: Type mismatch. That's not how you store a name. Try again."
        }
      },
      {
        story: "A security badge scanner checks your clearance level — a whole number between 1 and 10. No text, no decimals. Just a plain number.",
        code: `// Clearance level must be a numeric value\nlet clearanceLevel = ???;`,
        question: "What correctly stores the number 7 as a numeric value?",
        options: ['"7"   // a string, not a number', 'true  // a yes/no boolean', '7     // a plain number', '[7]   // wrapped in an array'],
        correct: 2,
        explanation: "Numbers in JavaScript are written as plain digits — no quotes. Writing \"7\" makes it a string, so math and comparisons won't work. The scanner needs an actual Number type.",
        storyFeedback: {
          correct: "Badge accepted. Clearance confirmed. The corridor opens ahead of you.",
          wrong: "Scanner rejected the input. That data type doesn't match what it expects."
        }
      }
    ]
  },
  {
    id: 2,
    title: "CHAPTER 02: THE MAZE",
    concept: "Loops & Conditionals",
    conceptIcon: "🔄",
    intro: `You're deeper inside NovaCorp's network. The hallways here are guarded by logic gates — automated systems that check conditions and repeat actions endlessly.\n\nConditionals ask YES or NO: "Is this value greater than 5? Is this user authorized?" Based on the answer, code takes one path or another.\n\nLoops repeat code automatically. Need to scramble 100 cameras? One loop handles all of them.\n\nThese two tools control the flow of everything. Master them, and you control the maze.`,
    questions: [
      {
        story: "A security door checks your clearance. It should only open if your level is ABOVE 5. Someone broke the comparison operator.",
        code: `if (clearanceLevel ??? 5) {\n  openDoor();\n}`,
        question: "Which operator correctly checks if clearanceLevel is greater than 5?",
        options: ['clearanceLevel < 5   // less than', 'clearanceLevel = 5   // assigns 5', 'clearanceLevel > 5   // greater than', 'clearanceLevel == 5  // equals exactly 5'],
        correct: 2,
        explanation: "> means 'greater than'. Using = would assign a value, not compare. < would open doors for LOW clearance — the opposite. == checks for exact equality (only 5, not 6, 7, etc).",
        storyFeedback: {
          correct: "The door slides open with a hiss of pressurized air. You slip through.",
          wrong: "Door stays sealed. The condition is wrong. Guards are approaching — think fast."
        }
      },
      {
        story: "Five security cameras watch this corridor. You need to scramble all five — but writing the same line five times wastes precious seconds.",
        code: `for (let i = 0; i ??? 5; i++) {\n  scrambleCamera(i); // should run for i = 0,1,2,3,4\n}`,
        question: "Which condition makes the loop run exactly 5 times?",
        options: ['i < 3   // only 3 iterations', 'i <= 5  // runs 6 times (0 through 5)', 'i < 5   // runs 5 times (0 through 4)', 'i < 50  // way too many'],
        correct: 2,
        explanation: "Starting at i=0, the condition i < 5 produces values 0, 1, 2, 3, 4 — exactly 5 iterations. i <= 5 would run 6 times because it also includes i=5.",
        storyFeedback: {
          correct: "All five cameras: scrambled. The corridor goes dark. You move fast.",
          wrong: "Wrong number of cameras scrambled. Security still has eyes on you."
        }
      }
    ]
  },
  {
    id: 3,
    title: "CHAPTER 03: THE ARSENAL",
    concept: "Functions",
    conceptIcon: "⚡",
    intro: `You've found NovaCorp's tool vault. Inside: automated programs that control every system in the building.\n\nThese are functions. A function is a named block of code you define once and call anytime, anywhere. Write the logic once — use it a hundred times.\n\nFunctions can take inputs (called parameters) and send back results using return. They're the backbone of every serious codebase.\n\nThis is where amateurs become professionals. Build your arsenal, Ghost.`,
    questions: [
      {
        story: "You need a reusable tool to crack any door lock. Write it once, call it everywhere. That's exactly what functions are for.",
        code: `??? hackDoor(doorId) {\n  console.log("Cracking door #" + doorId);\n}`,
        question: "What keyword correctly defines a function in JavaScript?",
        options: ['method hackDoor(doorId) {}', 'function hackDoor(doorId) {}', 'def hackDoor(doorId) {}', 'make hackDoor(doorId) {}'],
        correct: 1,
        explanation: "JavaScript uses the 'function' keyword to define functions. 'def' is Python syntax. 'method' and 'make' are not valid JavaScript keywords — the compiler won't recognize them.",
        storyFeedback: {
          correct: "Function loaded. hackDoor() is armed and ready to deploy on any door.",
          wrong: "Syntax error. The compiler rejects it. The door stays locked."
        }
      },
      {
        story: "Your crackCode function needs to SEND BACK the access code it generates — not just print it, but return a usable value to the caller.",
        code: `function crackCode(difficulty) {\n  let code = difficulty * 1337;\n  ??? code;  // send this value back\n}`,
        question: "Which keyword sends a value back from a function to its caller?",
        options: ['send code', 'output code', 'return code', 'emit code'],
        correct: 2,
        explanation: "'return' sends a value back to wherever the function was called. Without it, the result disappears and the caller gets 'undefined' — a silent failure that keeps the door locked.",
        storyFeedback: {
          correct: "Access code transmitted. The vault unlocks. Your arsenal is complete.",
          wrong: "Function ran but returned nothing. The caller got 'undefined'. No access."
        }
      }
    ]
  },
  {
    id: 4,
    title: "CHAPTER 04: FINAL BATTLE",
    concept: "Debugging Logic",
    conceptIcon: "🐛",
    intro: `You've reached the core. NovaCorp's AI — NEXUS — runs on thousands of lines of code. It's powerful. It's dangerous.\n\nBut it has bugs.\n\nEvery system written by humans does. Logical errors, broken loops, inverted conditions — NEXUS is no different. Your job isn't to destroy it. It's to break it from the inside.\n\nDebuggers don't just write code. They READ it. They hunt for what's wrong. They fix what others missed.\n\nThis is the final mission, Ghost. Don't blink.`,
    questions: [
      {
        story: "NEXUS has an infinite loop — scanning the same data forever, locking up the entire system. The building is entering emergency lockdown.",
        code: `let count = 0;\nwhile (count < 3) {\n  console.log("Scanning...");\n  // ← count never changes!\n}`,
        question: "What line must be added inside the loop to make it stop after 3 runs?",
        options: ['break;         // forces immediate exit', 'count = 3;     // jumps straight to end', 'count++;       // increments count each time', 'return count;  // returns from a function'],
        correct: 2,
        explanation: "count++ adds 1 to count each iteration (0→1→2→3). Once count hits 3, the condition count < 3 is false and the loop stops. Without it, count is always 0 — an infinite loop.",
        storyFeedback: {
          correct: "Loop terminated. Lockdown halted. NEXUS is stuttering. Push forward.",
          wrong: "Loop still running. The lockdown tightens. Think carefully — what's missing?"
        }
      },
      {
        story: "NEXUS's final defense: it should DENY high-risk levels (above 10) and GRANT safe levels (10 and below). The logic is completely inverted.",
        code: `function checkAccess(level) {\n  if (level > 10) {\n    return "GRANTED"; // ← this is backwards!\n  }\n  return "DENIED";\n}`,
        question: "How do you fix the inverted condition so safe levels (≤10) are GRANTED?",
        options: ['Change "GRANTED" to true', 'Change level > 10 to level <= 10', 'Remove the if statement entirely', 'Add else before return "DENIED"'],
        correct: 1,
        explanation: "The condition is backwards — it grants high-risk levels (>10) access. Changing to level <= 10 flips the logic: levels 10 and below are GRANTED, everything above is DENIED.",
        storyFeedback: {
          correct: "NEXUS ACCESS: PERMANENTLY DENIED. The AI flatlines. You did it, Ghost.",
          wrong: "NEXUS laughs in corrupted binary. The condition is still wrong. One more shot."
        }
      }
    ]
  }
];

const TOTAL_QS = CHAPTERS.reduce((s, c) => s + c.questions.length, 0);

function useTypewriter(text, speed = 18) {
  const [idx, setIdx] = useState(0);
  const timerRef = useRef(null);

  useEffect(() => {
    setIdx(0);
    if (timerRef.current) clearInterval(timerRef.current);
    if (!text) return;
    timerRef.current = setInterval(() => {
      setIdx(i => {
        if (i >= text.length) { clearInterval(timerRef.current); return i; }
        return i + 1;
      });
    }, speed);
    return () => { if (timerRef.current) clearInterval(timerRef.current); };
  }, [text, speed]);

  const skip = useCallback(() => {
    if (timerRef.current) clearInterval(timerRef.current);
    setIdx(text ? text.length : 0);
  }, [text]);

  return {
    displayed: text ? text.slice(0, idx) : "",
    done: !text || idx >= text.length,
    skip
  };
}

const CSS = `
  @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Share+Tech+Mono&display=swap');
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  
  .gp-root {
    min-height: 100vh;
    background: #04080f;
    color: #a8bfd4;
    font-family: 'Share Tech Mono', monospace;
    position: relative;
    overflow-x: hidden;
  }
  .gp-grid {
    position: fixed; inset: 0;
    background-image:
      linear-gradient(rgba(0,230,255,0.025) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,230,255,0.025) 1px, transparent 1px);
    background-size: 44px 44px;
    pointer-events: none; z-index: 0;
  }
  .gp-scanlines {
    position: fixed; inset: 0;
    background: repeating-linear-gradient(0deg, transparent, transparent 3px, rgba(0,0,0,0.12) 3px, rgba(0,0,0,0.12) 4px);
    pointer-events: none; z-index: 999;
  }
  .gp-inner {
    position: relative; z-index: 1;
    min-height: 100vh;
    display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    padding: 24px 16px;
  }
  .panel {
    background: rgba(4,12,24,0.97);
    border: 1px solid rgba(0,230,255,0.25);
    border-radius: 2px;
    padding: 36px 40px;
    max-width: 740px; width: 100%;
    box-shadow: 0 0 40px rgba(0,230,255,0.07), 0 0 80px rgba(0,0,0,0.6);
    position: relative;
  }
  .panel::before {
    content: ''; position: absolute;
    top: -1px; left: 24px; width: 56px; height: 2px;
    background: #00e6ff; box-shadow: 0 0 12px #00e6ff;
  }
  .panel::after {
    content: ''; position: absolute;
    bottom: -1px; right: 24px; width: 36px; height: 2px;
    background: #ff1d6a; box-shadow: 0 0 8px #ff1d6a;
  }
  
  .glitch-title {
    font-family: 'Orbitron', sans-serif;
    font-size: clamp(40px, 10vw, 72px);
    font-weight: 900;
    color: #00e6ff;
    text-shadow: 0 0 20px rgba(0,230,255,0.8), 0 0 50px rgba(0,230,255,0.3);
    letter-spacing: 6px;
    line-height: 1;
    animation: glitch 5s infinite;
  }
  @keyframes glitch {
    0%,88%,100% { text-shadow: 0 0 20px rgba(0,230,255,0.8); transform: none; }
    89% { transform: translate(-3px,0); text-shadow: 3px 0 #ff1d6a, -3px 0 #00e6ff; }
    90% { transform: translate(3px,0); text-shadow: -3px 0 #ff1d6a, 3px 0 #00e6ff; }
    91% { transform: none; text-shadow: 0 0 20px rgba(0,230,255,0.8); }
    92% { transform: translate(0,-2px); text-shadow: 0 2px #ff1d6a; }
    93% { transform: none; }
  }
  
  .ch-title {
    font-family: 'Orbitron', sans-serif;
    font-size: clamp(14px, 2.5vw, 20px);
    font-weight: 700;
    color: #00e6ff;
    text-shadow: 0 0 10px rgba(0,230,255,0.4);
    margin-bottom: 18px;
    letter-spacing: 2px;
  }
  .tag {
    font-family: 'Orbitron', sans-serif;
    font-size: 9px; letter-spacing: 4px;
    color: #ff1d6a; text-transform: uppercase;
    margin-bottom: 6px;
  }
  .concept-pill {
    display: inline-flex; align-items: center; gap: 8px;
    background: rgba(255,29,106,0.08);
    border: 1px solid rgba(255,29,106,0.3);
    border-radius: 100px; padding: 5px 16px;
    font-family: 'Orbitron', sans-serif;
    font-size: 10px; color: #ff1d6a; letter-spacing: 2px;
    margin-bottom: 20px;
  }
  
  .story-text {
    font-size: 14px; line-height: 2;
    color: #8eaac4; white-space: pre-wrap; min-height: 140px;
  }
  .cursor {
    display: inline-block; width: 9px; height: 16px;
    background: #00e6ff; margin-left: 2px;
    animation: blink 0.9s step-end infinite; vertical-align: middle;
  }
  @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }
  
  .hr { border: none; border-top: 1px solid rgba(0,230,255,0.1); margin: 22px 0; }
  
  .btn {
    font-family: 'Orbitron', sans-serif;
    font-size: 11px; font-weight: 700; letter-spacing: 3px;
    padding: 13px 32px; border: 1px solid #00e6ff;
    background: transparent; color: #00e6ff;
    cursor: pointer; transition: all 0.18s; position: relative; overflow: hidden;
  }
  .btn::before {
    content: ''; position: absolute; top:0; left:-100%; width:100%; height:100%;
    background: rgba(0,230,255,0.08); transition: left 0.25s;
  }
  .btn:hover::before { left: 0; }
  .btn:hover { box-shadow: 0 0 20px rgba(0,230,255,0.35); text-shadow: 0 0 8px #00e6ff; }
  .btn-pink { border-color: #ff1d6a; color: #ff1d6a; }
  .btn-pink::before { background: rgba(255,29,106,0.08); }
  .btn-pink:hover { box-shadow: 0 0 20px rgba(255,29,106,0.35); text-shadow: 0 0 8px #ff1d6a; }
  
  .hud {
    display: flex; justify-content: space-between; align-items: center;
    margin-bottom: 20px; padding-bottom: 16px;
    border-bottom: 1px solid rgba(0,230,255,0.1);
  }
  .hud-val { font-family: 'Orbitron', sans-serif; font-size: 11px; color: #00e6ff; letter-spacing: 2px; }
  .hud-label { font-size: 9px; color: #3a5470; letter-spacing: 2px; margin-bottom: 2px; }
  .hearts { display:flex; gap:5px; font-size:13px; }
  
  .prog-wrap { background: rgba(0,230,255,0.08); height:3px; border-radius:2px; margin-bottom:22px; overflow:hidden; }
  .prog-fill {
    height:100%;
    background: linear-gradient(90deg, #00e6ff, #ff1d6a);
    border-radius:2px; box-shadow:0 0 8px rgba(0,230,255,0.5);
    transition: width 0.5s ease;
  }
  
  .q-context {
    font-size: 13px; color: #5a7a96; font-style: italic;
    margin-bottom: 18px; padding-left: 14px;
    border-left: 2px solid #ff1d6a; line-height: 1.8;
  }
  .code-block {
    background: rgba(0,0,0,0.7); border: 1px solid rgba(57,255,50,0.25);
    border-radius: 3px; padding: 16px 20px;
    font-family: 'Share Tech Mono', monospace; font-size: 12.5px;
    color: #39ff32; margin-bottom: 20px; white-space: pre; overflow-x: auto;
    text-shadow: 0 0 6px rgba(57,255,50,0.4);
    box-shadow: inset 0 0 20px rgba(0,0,0,0.4), 0 0 15px rgba(57,255,50,0.05);
  }
  .q-text {
    font-family: 'Orbitron', sans-serif; font-size: 12px; font-weight: 700;
    color: #ddeeff; margin-bottom: 16px; letter-spacing: 1px; line-height: 1.6;
  }
  .options { display: flex; flex-direction: column; gap: 10px; margin-bottom: 18px; }
  .opt {
    background: rgba(0,18,36,0.8); border: 1px solid rgba(0,230,255,0.15);
    border-radius: 2px; padding: 12px 18px;
    cursor: pointer; font-family: 'Share Tech Mono', monospace;
    font-size: 12.5px; color: #6a8aaa; transition: all 0.15s; text-align: left; width: 100%;
  }
  .opt:not(.opt-done):hover { border-color: #00e6ff; color: #00e6ff; background: rgba(0,230,255,0.04); box-shadow: 0 0 12px rgba(0,230,255,0.1); }
  .opt.opt-done { cursor: default; }
  .opt.opt-correct { border-color: #39ff32; color: #39ff32; background: rgba(57,255,50,0.07); box-shadow: 0 0 14px rgba(57,255,50,0.15); }
  .opt.opt-wrong { border-color: #ff1d6a; color: #ff1d6a; background: rgba(255,29,106,0.07); }
  .opt.opt-reveal { border-color: rgba(57,255,50,0.4); color: rgba(57,255,50,0.55); }
  
  .fb-box { margin-top: 4px; padding: 16px 18px; border-radius: 3px; font-size: 13px; line-height: 1.75; animation: fadeUp 0.3s ease; }
  @keyframes fadeUp { from{opacity:0;transform:translateY(6px)} to{opacity:1;transform:none} }
  .fb-correct { background: rgba(57,255,50,0.06); border: 1px solid rgba(57,255,50,0.25); color: #a8dca8; }
  .fb-wrong { background: rgba(255,29,106,0.06); border: 1px solid rgba(255,29,106,0.25); color: #dca8b8; }
  .fb-label { font-family:'Orbitron',sans-serif; font-size:10px; font-weight:700; letter-spacing:3px; margin-bottom:7px; }
  .fb-correct .fb-label { color: #39ff32; }
  .fb-wrong .fb-label { color: #ff1d6a; }
  .fb-comment { opacity:0.65; font-size:12px; margin-top:7px; }
  
  .score-big {
    font-family: 'Orbitron', sans-serif;
    font-size: clamp(60px,15vw,96px); font-weight:900; color:#00e6ff;
    text-shadow: 0 0 30px rgba(0,230,255,0.9), 0 0 60px rgba(0,230,255,0.3);
    text-align:center; animation: scorePulse 2s ease-in-out infinite;
  }
  @keyframes scorePulse {
    0%,100%{text-shadow:0 0 30px rgba(0,230,255,0.9)}
    50%{text-shadow:0 0 50px rgba(0,230,255,1),0 0 80px rgba(0,230,255,0.4)}
  }
  .rank { font-family:'Orbitron',sans-serif; font-size:13px; letter-spacing:4px; color:#ff1d6a; text-align:center; margin-top:8px; }
  .concept-grid { display:grid; grid-template-columns:1fr 1fr; gap:12px; }
  .concept-card {
    background: rgba(0,230,255,0.04); border: 1px solid rgba(0,230,255,0.12);
    border-radius:3px; padding:14px 18px; text-align:left;
  }
  .concept-card-icon { font-size:20px; margin-bottom:6px; }
  .concept-card-name { font-family:'Orbitron',sans-serif; font-size:9px; color:#00e6ff; letter-spacing:1.5px; }
`;

export default function GhostProtocol() {
  const [screen, setScreen] = useState("title");
  const [chapterIdx, setChapterIdx] = useState(0);
  const [questionIdx, setQuestionIdx] = useState(0);
  const [score, setScore] = useState(0);
  const [lives, setLives] = useState(3);
  const [selected, setSelected] = useState(null);
  const [isCorrect, setIsCorrect] = useState(null);
  const [answered, setAnswered] = useState(false);

  const chapter = CHAPTERS[chapterIdx];
  const question = chapter?.questions[questionIdx];
  const qNumber = CHAPTERS.slice(0, chapterIdx).reduce((s, c) => s + c.questions.length, 0) + questionIdx;

  const storyText = screen === "story" ? chapter.intro : "";
  const { displayed, done, skip } = useTypewriter(storyText, 18);

  const handleAnswer = (idx) => {
    if (answered) return;
    setSelected(idx);
    setAnswered(true);
    const correct = idx === question.correct;
    setIsCorrect(correct);
    if (correct) setScore(s => s + 1);
    else setLives(l => Math.max(0, l - 1));
  };

  const handleContinue = () => {
    setSelected(null); setIsCorrect(null); setAnswered(false);
    const nextQ = questionIdx + 1;
    if (nextQ < chapter.questions.length) {
      setQuestionIdx(nextQ);
    } else {
      const nextC = chapterIdx + 1;
      if (nextC < CHAPTERS.length) {
        setChapterIdx(nextC); setQuestionIdx(0); setScreen("story");
      } else {
        setScreen("victory");
      }
    }
  };

  const restart = () => {
    setScreen("title"); setChapterIdx(0); setQuestionIdx(0);
    setScore(0); setLives(3); setSelected(null); setIsCorrect(null); setAnswered(false);
  };

  const optClass = (idx) => {
    let c = "opt";
    if (!answered) return c;
    c += " opt-done";
    if (idx === selected) c += isCorrect ? " opt-correct" : " opt-wrong";
    else if (idx === question.correct && !isCorrect) c += " opt-reveal";
    return c;
  };

  const getRank = () => {
    if (score === TOTAL_QS) return "PERFECT HACK — LEGEND STATUS";
    if (score >= 6) return "ELITE HACKER";
    if (score >= 4) return "SKILLED CODER";
    return "KEEP TRAINING, GHOST";
  };

  return (
    <>
      <style>{CSS}</style>
      <div className="gp-root">
        <div className="gp-grid" />
        <div className="gp-scanlines" />
        <div className="gp-inner">

          {/* ── TITLE ── */}
          {screen === "title" && (
            <div className="panel" style={{ textAlign: "center" }}>
              <div style={{ fontFamily: "Orbitron", fontSize: 10, letterSpacing: 6, color: "#ff1d6a", marginBottom: 12 }}>▸ CLASSIFIED OPERATION ▸</div>
              <div className="glitch-title">GHOST</div>
              <div className="glitch-title" style={{ fontSize: "clamp(22px,5vw,44px)", marginTop: -4, marginBottom: 4 }}>PROTOCOL</div>
              <div style={{ fontFamily: "Orbitron", fontSize: 11, letterSpacing: 5, color: "#ff1d6a", marginBottom: 28 }}>A CODING ADVENTURE</div>
              <hr className="hr" />
              <p style={{ fontSize: 13, color: "#5a7a96", lineHeight: 2, marginBottom: 28 }}>
                The year is 2031. NovaCorp controls everything.<br />
                You're <span style={{ color: "#00e6ff" }}>Ghost</span> — a teenage hacker with one mission:<br />
                infiltrate their AI and <span style={{ color: "#ff1d6a" }}>expose the truth.</span><br /><br />
                Master{" "}
                <span style={{ color: "#ff1d6a" }}>variables</span>,{" "}
                <span style={{ color: "#ff1d6a" }}>loops</span>,{" "}
                <span style={{ color: "#ff1d6a" }}>functions</span>, and{" "}
                <span style={{ color: "#ff1d6a" }}>debugging</span><br />
                to hack through 4 chapters and take down NEXUS.
              </p>
              <button className="btn" onClick={() => setScreen("story")}>▶ BEGIN MISSION</button>
              <p style={{ marginTop: 20, fontSize: 10, color: "#253545", letterSpacing: 3, fontFamily: "Orbitron" }}>
                8 CHALLENGES · 4 CONCEPTS · 3 LIVES
              </p>
            </div>
          )}

          {/* ── STORY ── */}
          {screen === "story" && (
            <div className="panel">
              <div className="tag">◈ INCOMING TRANSMISSION</div>
              <div className="ch-title">{chapter.title}</div>
              <div className="concept-pill"><span>{chapter.conceptIcon}</span><span>{chapter.concept}</span></div>
              <div className="story-text">
                {displayed}
                {!done && <span className="cursor" />}
              </div>
              <hr className="hr" />
              <div style={{ display: "flex", justifyContent: "flex-end", gap: 12 }}>
                {!done && <button className="btn btn-pink" onClick={skip}>SKIP ▶▶</button>}
                {done && <button className="btn" onClick={() => setScreen("challenge")}>ENGAGE MISSION ▶</button>}
              </div>
            </div>
          )}

          {/* ── CHALLENGE ── */}
          {screen === "challenge" && question && (
            <div className="panel">
              <div className="hud">
                <div>
                  <div className="hud-label">SCORE</div>
                  <div className="hud-val">{score} / {TOTAL_QS}</div>
                </div>
                <div style={{ textAlign: "center" }}>
                  <div className="hud-label">QUESTION</div>
                  <div className="hud-val">{qNumber + 1} / {TOTAL_QS}</div>
                </div>
                <div style={{ textAlign: "right" }}>
                  <div className="hud-label">LIVES</div>
                  <div className="hearts">
                    {[0, 1, 2].map(i => (
                      <span key={i} style={{ color: "#ff1d6a", opacity: i < lives ? 1 : 0.18, filter: i < lives ? "drop-shadow(0 0 5px #ff1d6a)" : "none" }}>♥</span>
                    ))}
                  </div>
                </div>
              </div>
              <div className="prog-wrap">
                <div className="prog-fill" style={{ width: `${(qNumber / TOTAL_QS) * 100}%` }} />
              </div>
              <div className="concept-pill"><span>{chapter.conceptIcon}</span><span>{chapter.concept}</span></div>
              <p className="q-context">{question.story}</p>
              <div className="code-block">{question.code}</div>
              <p className="q-text">▸ {question.question}</p>
              <div className="options">
                {question.options.map((opt, idx) => (
                  <button key={idx} className={optClass(idx)} onClick={() => handleAnswer(idx)}>
                    <span style={{ color: "#2a4a6a", marginRight: 12 }}>{String.fromCharCode(65 + idx)}.</span>{opt}
                  </button>
                ))}
              </div>
              {answered && (
                <div className={`fb-box ${isCorrect ? "fb-correct" : "fb-wrong"}`}>
                  <div className="fb-label">{isCorrect ? "✓ ACCESS GRANTED" : "✗ SYSTEM ERROR"}</div>
                  <p>{isCorrect ? question.storyFeedback.correct : question.storyFeedback.wrong}</p>
                  <p className="fb-comment"><span style={{ opacity: 0.45 }}>// </span>{question.explanation}</p>
                </div>
              )}
              {answered && (
                <div style={{ display: "flex", justifyContent: "flex-end", marginTop: 18 }}>
                  <button className="btn" onClick={handleContinue}>CONTINUE ▶</button>
                </div>
              )}
            </div>
          )}

          {/* ── VICTORY ── */}
          {screen === "victory" && (
            <div className="panel" style={{ textAlign: "center" }}>
              <div className="tag" style={{ marginBottom: 10 }}>◈ MISSION COMPLETE</div>
              <div className="glitch-title" style={{ fontSize: "clamp(18px,4vw,30px)", marginBottom: 16 }}>NEXUS OFFLINE</div>
              <p style={{ color: "#5a7a96", fontSize: 13, lineHeight: 2, marginBottom: 28 }}>
                You did it, Ghost. NovaCorp's AI is down for good.<br />
                The truth is free. The world is watching.
              </p>
              <div className="score-big">{score}/{TOTAL_QS}</div>
              <div className="rank">{getRank()}</div>
              <hr className="hr" />
              <p style={{ fontFamily: "Orbitron", fontSize: 9, letterSpacing: 3, color: "#3a5470", marginBottom: 16 }}>CONCEPTS MASTERED</p>
              <div className="concept-grid" style={{ marginBottom: 28 }}>
                {CHAPTERS.map(c => (
                  <div key={c.id} className="concept-card">
                    <div className="concept-card-icon">{c.conceptIcon}</div>
                    <div className="concept-card-name">{c.concept}</div>
                  </div>
                ))}
              </div>
              <button className="btn" onClick={restart}>↩ PLAY AGAIN</button>
            </div>
          )}

        </div>
      </div>
    </>
  );
}
