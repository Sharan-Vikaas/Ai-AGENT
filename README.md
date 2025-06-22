Project Documentation: Malaary – AI Automation 
with PDF-to-Video Flow 
Overview 
Project Name: Malaary 
Function: Accepts PDF books via Telegram → Analyzes content (Claude AI) → Sorts into categories → 
Sends prompts to Google Veo → Generates videos → Stores results in Google Drive 
Tech Stack: 
● Automation Engine: n8n 
● Hosting: Render (Free tier) 
● Messaging: Telegram Bot 
● File Storage: Google Drive 
● LLM: Claude (via API) 
● Video Generation: Google Veo (via prompt input) 
● GitHub Repo: Ai-AGENT 
● Domain: sharmen.store (custom domain) 
Deployment Checklist 
1. Set up GitHub Repo 
Structure: 
cpp 
CopyEdit 
Ai-AGENT/ 
├── Dockerfile 
├── render.yaml 
├── docker-compose.yml (optional) 
2. Create Dockerfile 
Dockerfile (no extension): 
Dockerfile 
CopyEdit 
FROM n8nio/n8n 
ENV N8N_BASIC_AUTH_ACTIVE=true 
ENV N8N_BASIC_AUTH_USER=malaary 
ENV N8N_BASIC_AUTH_PASSWORD=piedium 
EXPOSE 5678 
Save using Notepad: 
● File → Save As → Name: "Dockerfile" (with quotes) 
● Save as type: All Files (*.*) 
3. Create render.yaml 
render.yaml: 
yaml 
CopyEdit 
services: 
- type: web 
name: malaary-n8n 
env: docker 
repo: https://github.com/Sharan-Vikaas/Ai-AGENT 
plan: free 
branch: main 
dockerfilePath: ./Dockerfile 
4. Push Code to GitHub 
In Git Bash: 
bash 
CopyEdit 
git add Dockerfile render.yaml 
git commit -m "Initial deploy setup" 
git push origin main 
If error: 
bash 
CopyEdit 
git pull origin main --rebase 
git push origin main 
5. Deploy on Render 
1. Go to Render Dashboard 
2. Create new Web Service: 
○ Environment: Docker 
○ Repo: Sharan-Vikaas/Ai-AGENT 
○ Branch: main 
○ Name: malaary-n8n 
3. Manual Deploy → Deploy latest commit 
4. Wait for logs to show: n8n ready at 0.0.0.0:5678 
6. Access the App 
Once live, access via: 
arduino 
CopyEdit 
https://malaary-n8n.onrender.com 
Login with: 
● Username: malaary 
● Password: piedium 
7. Custom Domain Setup (Optional) 
If using sharmen.store: 
● Go to Render → Service → Settings → Custom Domains 
● Add sharmen.store 
● Update DNS A record or CNAME to point to Render IP 
Telegram Integration 
● Use @BotFather on Telegram 
● Create bot → Copy Token 
● In n8n: 
○ Add a Telegram Trigger node 
○ Paste your token into new credentials 
○ Use webhook URL (must be HTTPS → Render handles this) 
Planned Flow Summary 
1. Telegram Upload: 
○ Send a PDF to Telegram bot 
2. Storage in Google Drive: 
○ n8n saves the uploaded file into a Google Drive folder (via credentials) 
3. Processing with Claude AI: 
○ Each PDF page is categorized into: 
■ Video Generation → simple + short 
■ Modeling Required → visual-heavy 
■ Analysis of Admin → empty/filler/activity pages 
○ Claude summarizes each page 
4. Google Veo Prompting: 
○ n8n creates prompts using Claude’s output 
○ Sends to Google Veo 
○ Receives generated videos 
5. Local/Drive Storage: 
○ Page-wise storage: 
■ Video file 
■ Summary 
■ Page image (if applicable) 
Next Steps (What’s Left) 
● Telegram → GDrive integration 
● Claude AI API + sort logic in n8n 
● Veo prompt generation & video storage 
● Add interface (optional dashboard or Airtable) 
