# How to push this project to GitHub

## One-time setup (do this once ever)

1. Install Git: https://git-scm.com/downloads
2. Set your name and email:
   git config --global user.name "Your Name"
   git config --global user.email "you@email.com"
3. Create a GitHub account at github.com

## Push this project

# Step 1 — go into the project folder
cd lm386-audio-amplifier

# Step 2 — initialise git
git init

# Step 3 — stage all files
git add .

# Step 4 — first commit
git commit -m "Initial commit — LM386 amp breadboard prototype, 3 iterations documented"

# Step 5 — go to github.com → New repository
# Name it: lm386-audio-amplifier
# Set to Public, do NOT initialise with README (you already have one)
# Copy the repo URL shown (looks like: https://github.com/yourname/lm386-audio-amplifier.git)

# Step 6 — link and push
git remote add origin https://github.com/yourname/lm386-audio-amplifier.git
git branch -M main
git push -u origin main

## Every time you make changes after this

git add .
git commit -m "describe what changed and what you observed"
git push
