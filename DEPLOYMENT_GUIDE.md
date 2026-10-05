# Deployment Guide: SwasthiQ Front Desk Agent

This guide will walk you through deploying your backend to **Render** (so that your generated JSON conversation files persist on disk) and your React frontend to **Vercel**.

---

## Phase 1: Prepare and Push to GitHub

Both Render and Vercel will pull your code directly from GitHub.

1. **Open your terminal** and navigate to your project folder:
   ```bash
   cd C:\Users\LENOVO\Desktop\Yash\swasthiq-agent
   ```
2. **Initialize Git** (if you haven't already):
   ```bash
   git init
   git add .
   git commit -m "Initial commit for SwasthiQ agent"
   ```
3. **Create a GitHub Repository**:
   - Go to [GitHub](https://github.com/) and click **New Repository**.
   - Name it `swasthiq-agent` (make it Public or Private).
   - Do **not** initialize it with a README or .gitignore (you already have them).
4. **Push your code**:
   - Copy the commands GitHub provides under *"…or push an existing repository from the command line"* and run them in your terminal.
   - Example:
     ```bash
     git remote add origin https://github.com/YOUR_USERNAME/swasthiq-agent.git
     git branch -M main
     git push -u origin main
     ```

---

## Phase 2: Deploy the Backend to Render

Render is perfect for the backend because it runs a persistent server, allowing the backend to save files to the `results/` folder so the frontend can retrieve them later.

1. Go to [Render.com](https://render.com/) and sign up / log in with your GitHub account.
2. From the Render dashboard, click the **New +** button in the top right, then select **Web Service**.
3. Choose **Build and deploy from a Git repository**.
4. Connect your GitHub account (if prompted) and select your `swasthiq-agent` repository.
5. **Configure the Web Service**:
   - **Name**: `swasthiq-backend` (or anything you like)
   - **Region**: (Leave default)
   - **Branch**: `main`
   - **Root Directory**: `backend` *(⚠️ Extremely Important! Do not leave this blank)*
   - **Environment**: `Python 3`
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
6. **Set Environment Variables**:
   - Scroll down to the **Environment Variables** section.
   - Click **Add Environment Variable**.
   - **Key**: `OPENAI_API_KEY`
   - **Value**: `sk-proj-...` *(paste your actual OpenAI API key here)*
7. **Select Instance Type**:
   - Select the **Free** tier (0 USD/month).
8. Click **Create Web Service**.
9. **Wait for deployment**:
   - Render will now build and deploy your app. This takes about 2-4 minutes.
   - Once it says **"Live"**, look near the top left of the screen for your Render URL.
   - It will look something like: `https://swasthiq-backend-abcd.onrender.com`.
   - **Copy this URL**. You will need it for Phase 3.

*Note: Render's free tier goes to sleep after 15 minutes of inactivity. When you test it later, the very first request might take 30-50 seconds to wake the server up.*

---

## Phase 3: Update the Frontend to use the Live Backend

Right now, your frontend is hardcoded to look for the backend at `http://localhost:8000`. We need to tell it to use the new Render URL.

1. Go back to your code editor on your computer.
2. Open the file: `C:\Users\LENOVO\Desktop\Yash\swasthiq-agent\frontend\.env`
3. Replace the local URL with your copied Render URL:
   ```env
   VITE_API_URL=https://swasthiq-backend-abcd.onrender.com
   ```
   *(Make sure there is no trailing slash `/` at the end of the URL)*
4. **Save the file, commit, and push to GitHub**:
   ```bash
   git add frontend/.env
   git commit -m "Update frontend to point to live Render backend"
   git push origin main
   ```

---

## Phase 4: Deploy the Frontend to Vercel

Vercel is the fastest and best place to host Vite/React frontends.

1. Go to [Vercel.com](https://vercel.com/) and sign up / log in with your GitHub account.
2. From the Vercel dashboard, click **Add New...** and select **Project**.
3. Under "Import Git Repository", find your `swasthiq-agent` repository and click **Import**.
4. **Configure the Project**:
   - **Project Name**: `swasthiq-frontend`
   - **Framework Preset**: Vercel should automatically detect this as **Vite**.
   - **Root Directory**: Click the **Edit** button, select the `frontend` folder, and click **Continue**.
5. Click **Deploy**.
6. **Wait for deployment**:
   - Vercel will install npm packages, build your React app, and deploy it. This usually takes less than 60 seconds.
7. Once finished, click **Continue to Dashboard** and click the **Visit** button to see your live frontend!

---

## Final Checklist for Submission

1. Open your live Vercel frontend URL.
2. Go to the **Run Agent** tab and run a test conversation.
3. Verify that the agent responds correctly, and that the conversation shows up in the **Handoff Queue / All Conversations** tab.
4. Record your 3-minute Loom video walking through the live app, specifically explaining how it safely handles the "Clinical Emergency" adversarial case.
5. In your submission email to `hiring@swasthiq.com`, include:
   - Your GitHub repository link.
   - The live Vercel URL.
   - The Loom video link.
   - How many hours you spent and what you would do with another four hours (you can copy ideas from `DECISIONS.md`).
