# SKillStack
FrontEnd/BackEnd for SkillStack Website

SkillStackEdu — Learn anything, free, from the best YouTube videos — organized like a course.

SKillStackEdu is an AI platform that intelligently navigates the complex environment of freely available youtube content to curate a full stack educational course on any topic you would like.

SkillStackEdu is an AI-powered web platform that dynamically curates structured, topic-specific learning paths from publicly available online resources, optimizing content sequencing for efficient knowledge acquisition.

# MVP scope (v0.1)

Input: “I want to learn ___ (goal, level, time).”

Syllabus: AI generates a 6–10 module path (beginner → advanced).

Curation: Pull 3–5 YouTube videos per module, with reasons why each is chosen.

Lesson pages: Embed video + auto summary, key takeaways, timestamps, and a quick quiz.

Export/share: One public course page you can send to friends.

MVP Scope

✅ AI syllabus generator

✅ YouTube video fetcher + ranking

✅ AI lesson summaries/quizzes

✅ Public course pages

✅ 1 free course limit (upsell to Pro)


# Tech Stack

Architecture

Frontend: Next.js (React) for speed + SEO.

Backend/DB: Supabase (auth, Postgres, video/course storage).

AI: OpenAI GPT-4o (syllabus, summaries, quizzes).

Video Data: YouTube Data API (search, metadata); transcript fetcher library/API.

Payments: Stripe (Pro plan, monthly, single purchase).

Hosting: Vercel (frontend/backend together), Supabase cloud DB.

APIs / Endpoints
Frontend → Backend

POST /api/syllabus – Takes user input, returns modules + search queries.

POST /api/videos – Given search queries, returns ranked video metadata.

POST /api/lesson – Given video ID, returns summary, takeaways, timestamps, quiz.

Backend → External

OpenAI API – Prompt engineering for course + lesson generation.

YouTube API – Search + video metadata.

Transcript API – Fetch transcripts for summarization.


# Video Ranking Function

Score each candidate video

 = 0.35*(views_norm) + 
        0.20*(recency_norm) + 
        0.20*(like_ratio) + 
        0.15*(duration_fit) + 
        0.10*(channel_reputation)

views_norm: Normalize to 0–1 across search results.

recency_norm: Penalize >5 years old unless timeless topic.

duration_fit: Favor videos close to target lesson length (e.g., 10–20 min).

channel_reputation: Weighted by subscriber count and average engagement.

Then filter/dedupe by transcript keywords to cover each syllabus topic without repeats.

# Prompt skeletons you can reuse
Syllabus prompt

“Create a course outline for [TOPIC] for a [BEGINNER/INTERMEDIATE] who has [GOAL] in [TIME]. 8 modules max. Each module: title, objective, 3–5 subtopics, search queries to find suitable videos.”

Lesson summary prompt

“Given this transcript, write: (1) 5-sentence summary, (2) bullet key takeaways, (3) 5 timestamped checkpoints, (4) 3 quiz questions with answers.”

DB sketch (tables)
users

courses (topic, level, goal, time_budget, owner_id)

modules (course_id, order, title, objective)

lessons (module_id, order, youtube_id, title, channel, duration, score)

lesson_notes (lesson_id, summary, takeaways_json, timestamps_json, quiz_json)


# 7 Day Sprint Build

7-day build sprint
Day 1: Name, landing page, waitlist, spec the flows.

Day 2: YouTube search + scoring; fetch transcripts when available.

Day 3: Syllabus generation + mapping videos to modules.

Day 4: Course page UI (embed + summaries + quizzes).

Day 5: Accounts + save state (Supabase).

Day 6: Stripe paywall; limit free usage.

Day 7: Dogfood with 5–10 friends; measure time-to-first-course and satisfaction.

Success metrics (MVP)
Time to a complete course < 60 seconds

First-session “start a course” rate > 40%

Share rate (copied course link) > 20% of completions