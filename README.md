# pipelingo

Build a full-stack web app called Pipelingo — a Duolingo-style daily 
practice app for data engineers. Here is everything you need to build it:

---

TECH STACK
- React (Vite) frontend
- Tailwind CSS for styling
- Supabase for auth + database
- Dark theme throughout (background #0f1117, accent #f97316 orange)

---

CORE FEATURES TO BUILD

1. ONBOARDING FLOW
   - Welcome screen with app name "Pipelingo ⚡" and tagline 
     "5 minutes a day. Every day. Until it sticks."
   - Stack selector: user picks one or more from:
     AWS, Azure, GCP, Apache Spark, dbt, Airflow, Kafka, 
     Snowflake, SQL, Python for Data, LLMOps/AI
   - Experience level selector: Junior / Mid / Senior
   - Guest mode — no signup required to start
   - Store selections in localStorage

2. HOME DASHBOARD
   - Display: streak counter (🔥), XP total, daily progress bar
   - "Today's Drill" card — featured module with Start button
   - Module grid showing user's selected stack items with % progress
   - "What's New" section showing 2-3 placeholder tech update cards
   - Leaderboard teaser showing user rank

3. MODULE MAP SCREEN
   - When user clicks a module (e.g. AWS), show a vertical lesson map
   - Lessons grouped into: Beginner / Intermediate / Advanced
   - Each lesson shows: title, locked/unlocked state, completion checkmark
   - Locked lessons show 🔒, completed show ✅, current shows pulsing ring
   - "New This Month" section at bottom with 🔴 badge

4. LESSON FLOW (the core loop)

   PHASE 1 — LEARN CARD
   - Show concept title and a short explanation card (2-3 paragraphs max)
   - "Got It — Test Me ▶️" button to proceed
   - "Read More ↓" expands extra detail

   PHASE 2 — DRILL QUESTIONS (5 per session)
   - Progress bar at top showing Q1/5 through Q5/5
   - Countdown timer (90 seconds per question, visible progress arc)
   - Pause button stops the timer
   - Scenario box with context (grey background card)
   - 4 answer options as tappable cards
   - On tap: immediately show correct (green) / wrong (red) highlighting
   - Show explanation panel below answers after selection
   - "Next →" button to advance

   PHASE 3 — DRILL COMPLETE SCREEN
   - Score: X/5 with filled stars
   - XP earned
   - Streak updated
   - "Weak spots detected" — list any questions missed
   - Shareable score card (styled div, "Copy Image" button using 
     html2canvas)
   - "Home" and "Next Drill →" buttons

5. GAMIFICATION
   - XP system: +10 per correct answer, +50 bonus for 5/5 perfect
   - Streak: increments if user completes at least one drill per day
     Store last_drill_date in localStorage, compare to today
   - Streak at risk warning if user hasn't drilled by 8pm
   - Simple leaderboard page showing top 10 users by XP (mock data ok)

6. SETTINGS / PROFILE PAGE
   - Edit stack selection
   - View total XP, streak, lessons completed
   - Toggle: "Daily reminder" (browser notification permission)

---

SAMPLE CONTENT — hardcode these as seed data in a content.js file

Create at least 3 complete modules, each with:
- 2 learn cards
- 5 drill questions per learn card

MODULE 1: AWS — S3 & Storage
Learn card 1 title: "S3 Fundamentals"
Learn card 2 title: "S3 Performance Patterns"

Sample question for AWS module:
{
  scenario: "Your Spark job reads 10,000 small JSON files from S3. 
             It's running 5x slower than expected.",
  question: "What is the most likely cause?",
  options: [
    "S3 bucket is in wrong region",
    "Too many small files — the small file problem",  // correct
    "Spark executor memory too low",
    "S3 versioning is enabled"
  ],
  correct: 1,
  explanation: "The small file problem occurs when Spark creates one 
    task per file. 10,000 files = 10,000 tasks = massive overhead. 
    Fix: use S3DistCp or Spark's coalesce to merge files before 
    processing, or use S3 Inventory to identify and compact them.",
  pro_tip: "Aim for files between 128MB–1GB for optimal Spark 
    performance. Parquet with Snappy compression is the gold standard."
}

MODULE 2: Apache Spark — Core Concepts  
Learn card 1 title: "Spark Architecture"
Learn card 2 title: "Partitions & Parallelism"

MODULE 3: dbt — Models & Testing
Learn card 1 title: "dbt Model Types"  
Learn card 2 title: "dbt Tests & Data Quality"

---

UI DESIGN REQUIREMENTS

Colors:
- Background: #0f1117
- Card background: #1a1d27  
- Border: #2a2d3a
- Accent/CTA: #f97316 (orange)
- Success: #22c55e (green)
- Error: #ef4444 (red)
- Text primary: #ffffff
- Text secondary: #94a3b8

Typography:
- Font: Inter (Google Fonts)
- App name: bold, large
- Questions: 16px, readable line height

Components needed:
- ProgressBar (fills with orange)
- ModuleCard (icon, name, % bar, click to open)
- QuestionCard (scenario, 4 options, timer arc)
- LearnCard (title, content, expand button)
- StreakBadge (🔥 + number)
- XPCounter (⚡ + number)
- ScoreCard (shareable result)

---

NAVIGATION
- Bottom nav bar on mobile with 4 icons:
  Home | Modules | Leaderboard | Profile
- React Router for routing
- Routes: / | /module/:id | /drill/:moduleId/:lessonId | /leaderboard | /profile

---

RESPONSIVE DESIGN
- Mobile-first (375px base)
- Max width 480px centered on desktop
- Feels like a native mobile app in browser

---

STRETCH GOALS (build if time allows)
- Spaced repetition: track which questions user got wrong, 
  resurface them in future drills
- "Tech Pulse" feed page: static list of recent tech updates 
  formatted as news cards, each with a "Drill It" button
- Cross-cloud question type: show same concept for AWS vs Azure 
  side by side after answering

---

FILE STRUCTURE
src/
  components/
    ProgressBar.jsx
    ModuleCard.jsx
    QuestionCard.jsx
    LearnCard.jsx
    StreakBadge.jsx
    ScoreCard.jsx
    BottomNav.jsx
  pages/
    Home.jsx
    ModuleMap.jsx
    Drill.jsx
    Leaderboard.jsx
    Profile.jsx
    Onboarding.jsx
  data/
    content.js        ← all questions and learn cards here
  hooks/
    useStreak.js      ← streak logic
    useXP.js          ← XP logic
    useProgress.js    ← lesson completion tracking
  App.jsx
  main.jsx

---

START by building:
1. The content.js file with all seed data
2. The Onboarding flow
3. The Home dashboard
4. The Drill flow (learn card → questions → score screen)

This is the core loop. Everything else is secondary.

CAREER TRACKS — expand the app to support multiple roles

Replace the stack-selector onboarding with a CAREER TRACK selector.
User picks their primary role from:

  - Data Engineer        (🔧)
  - AI Engineer          (🤖)  
  - Data Analyst         (📊)
  - Data Architect       (🏛️)
  - ML Engineer          (🔬)
  - Analytics Engineer   (📈)
  - "I do multiple roles" → picks 2-3 tracks, gets blended curriculum

Each track has its own module list defined in content.js.
The Home dashboard shows only modules relevant to the user's track.

DATA STRUCTURE for tracks in content.js:

const tracks = {
  data_engineer: {
    label: "Data Engineer",
    icon: "🔧",
    description: "Pipelines, Spark, dbt, Kafka, cloud storage",
    modules: ["aws", "azure", "spark", "dbt", "airflow", "kafka", 
              "delta_lake", "sql_advanced"]
  },
  ai_engineer: {
    label: "AI Engineer", 
    icon: "🤖",
    description: "LLMs, RAG, embeddings, agents, LLMOps",
    modules: ["llm_fundamentals", "rag_pipelines", "vector_databases",
              "prompt_engineering", "ai_agents", "llmops", 
              "openai_api", "fine_tuning"]
  },
  data_analyst: {
    label: "Data Analyst",
    icon: "📊", 
    description: "SQL, dashboards, statistics, storytelling",
    modules: ["sql_fundamentals", "sql_advanced", "power_bi", 
              "tableau", "statistics", "ab_testing", "python_analysis"]
  },
  data_architect: {
    label: "Data Architect",
    icon: "🏛️",
    description: "System design, data modeling, platform decisions",
    modules: ["data_modeling", "lakehouse_design", "warehouse_vs_lake",
              "medallion_architecture", "data_mesh", "governance_basics",
              "cloud_platform_selection"]
  },
  ml_engineer: {
    label: "ML Engineer",
    icon: "🔬",
    description: "MLflow, feature stores, model serving, drift",
    modules: ["ml_pipelines", "feature_engineering", "mlflow",
              "model_serving", "sagemaker", "model_drift", "mlops_cicd"]
  },
  analytics_engineer: {
    label: "Analytics Engineer",
    icon: "📈",
    description: "dbt, semantic layers, data modeling, quality",
    modules: ["dbt_deep", "semantic_layers", "dimensional_modeling",
              "data_contracts", "data_quality", "metrics_layer"]
  }
}

SEED CONTENT — add one complete module for AI Engineer:

MODULE: llm_fundamentals
Learn card 1: "How LLMs Actually Work"
Learn card 2: "Tokens, Context Windows & Costs"

Sample question:
{
  scenario: "Your RAG chatbot gives accurate answers for short 
             documents but starts hallucinating on long PDFs. 
             Your chunk size is set to 5000 tokens.",
  question: "What is the most likely cause?",
  options: [
    "The embedding model is too small",
    "Chunks are too large — exceeding useful context for retrieval", // correct
    "You need a larger LLM",
    "The vector database index needs rebuilding"
  ],
  correct: 1,
  explanation: "Large chunks reduce retrieval precision. When you 
    retrieve a 5000-token chunk, most of it is noise around the 
    actual answer. Smaller chunks (256-512 tokens) with a sliding 
    window overlap give the retriever a better signal-to-noise ratio.",
  pro_tip: "Use chunk sizes of 256-512 tokens with 10-15% overlap. 
    Then use re-ranking (Cohere Rerank, BGE Reranker) to improve 
    precision after initial retrieval."
}

MODULE: sql_fundamentals (shared across Analyst + DE tracks)
Learn card 1: "Window Functions"
Learn card 2: "CTEs vs Subqueries"

Sample question:
{
  scenario: "You need to find the top 3 selling products 
             per category from a sales table. 
             The table has: product_id, category, revenue.",
  question: "Which SQL approach is correct?",
  options: [
    "GROUP BY category, product_id ORDER BY revenue DESC LIMIT 3",
    "ROW_NUMBER() OVER (PARTITION BY category ORDER BY revenue DESC)",  // correct
    "MAX(revenue) GROUP BY category",
    "DISTINCT TOP 3 revenue per category"
  ],
  correct: 1,
  explanation: "ROW_NUMBER() with PARTITION BY category resets the 
    counter for each category, letting you rank products within 
    their group. Wrap it in a CTE and filter WHERE row_num <= 3.",
  pro_tip: "Use RANK() instead of ROW_NUMBER() if you want ties 
    to share the same rank position. DENSE_RANK() eliminates gaps 
    in ranking numbers after ties."
}

ONBOARDING CHANGE:
After career track selection, add a second step:
"Now pick your cloud" 
  ☐ AWS  ☐ Azure  ☐ GCP  ☐ Not cloud-specific yet

This appends cloud modules to the user's curriculum on top of 
their role-based modules.

HOME DASHBOARD CHANGE:
Show the career track badge prominently:
  "🔧 Data Engineer · Mid Level"

Add a "Switch Track" option in Profile settings.
XP and streak always carry over when switching tracks.
