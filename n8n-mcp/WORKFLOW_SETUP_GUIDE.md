# LinkedIn AI/ML Learning Content Automation - Setup Guide

## 🎯 Workflow Overview

This advanced n8n workflow automatically posts educational LinkedIn content about AI and Machine Learning every 2 days. It features:

- **15 AI/ML Topics** with intelligent rotation (no repeats until all topics are covered)
- **Groq AI Agent** (llama-3.3-70b-versatile) for content generation
- **Automatic image search and download** from Unsplash
- **Google Sheets deduplication** tracking
- **Tavily AI search** for latest trends
- **Error handling** with email alerts and logging

---

## 📋 Prerequisites

### Required API Keys & Credentials

1. **Groq API** (Free tier available)
   - Sign up at: https://console.groq.com/
   - Get API key from dashboard
   - Add to n8n credentials as "Groq API"

2. **Unsplash API** (Free tier: 50 requests/hour)
   - Sign up at: https://unsplash.com/developers
   - Create a new application
   - Copy the "Access Key"
   - Set environment variable: `UNSPLASH_ACCESS_KEY`

3. **Tavily API** (Free tier: 1000 searches/month)
   - Sign up at: https://tavily.com/
   - Get API key
   - Set environment variable: `TAVILY_API_KEY`

4. **LinkedIn OAuth2**
   - Go to https://www.linkedin.com/developers/
   - Create an app with "Share on LinkedIn" permission
   - Add OAuth2 credentials in n8n
   - Get your Person URN: `LINKEDIN_PERSON_URN`

5. **Google Sheets OAuth2**
   - Enable Google Sheets API in Google Cloud Console
   - Create OAuth2 credentials
   - Add to n8n

6. **SMTP Email** (for error alerts)
   - Use Gmail, SendGrid, or any SMTP service
   - Configure in n8n SMTP credentials

---

## 🔧 Environment Variables Setup

Add these to your n8n environment variables (Settings → Variables):

```env
# Required
UNSPLASH_ACCESS_KEY=your_unsplash_access_key
TAVILY_API_KEY=tvly-your_tavily_api_key
LINKEDIN_PERSON_URN=urn:li:person:YOUR_PERSON_ID
GOOGLE_SHEETS_DOC_ID=your_google_sheet_id
GOOGLE_SHEETS_SHEET_NAME=PostHistory

# Optional (for error alerts)
ALERT_EMAIL=your.email@example.com
```

---

## 📊 Google Sheets Setup

### Create a Google Sheet with TWO tabs:

#### Tab 1: "PostHistory" (or your custom name)
| Timestamp | Topic | PostText | ImageCredit | Status |
|-----------|-------|----------|-------------|--------|
| (auto)    | (auto)| (auto)   | (auto)      | (auto) |

#### Tab 2: "ErrorLog"
| Timestamp | Workflow | FailedNode | ErrorMessage |
|-----------|----------|------------|--------------|
| (auto)    | (auto)   | (auto)     | (auto)       |

**Share the sheet with your Google Service Account email for n8n access.**

---

## 🎨 Topics Covered (15 AI/ML Topics)

The workflow rotates through these topics without repetition:

1. **Python for AI** - Programming fundamentals
2. **Machine Learning Fundamentals** - Core ML concepts
3. **Deep Learning Basics** - Neural networks
4. **Natural Language Processing** - Text processing & transformers
5. **Computer Vision** - Image processing & CNNs
6. **Data Science with Python** - NumPy, Pandas, visualization
7. **Mathematics for ML** - Linear algebra, calculus, stats
8. **TensorFlow and Keras** - Model building
9. **PyTorch** - Deep learning framework
10. **Scikit-Learn** - Classical ML algorithms
11. **Reinforcement Learning** - Agent-based learning
12. **MLOps and Deployment** - Production ML systems
13. **Generative AI** - GANs, VAEs, diffusion models
14. **Large Language Models** - LLMs, GPT, BERT
15. **Time Series Analysis** - Forecasting & sequential data

Each topic includes curated **free resources** like Kaggle, Coursera, YouTube channels, official docs, etc.

---

## 🚀 Workflow Features

### 1. **Smart Topic Rotation**
- Reads Google Sheets to check posted topics
- Selects unposted topics randomly
- Resets when all 15 topics are posted
- Never repeats until cycle completes

### 2. **Groq AI Content Generation**
- Uses `llama-3.3-70b-versatile` model
- Temperature: 0.7 for creative yet focused output
- Max tokens: 1000
- Creates engaging educational posts with:
  - Compelling hook
  - Learning path explanation
  - 3-4 FREE resources
  - Current trends
  - Motivational CTA
  - 3-5 hashtags
  - 2-3 emojis

### 3. **Intelligent Image Selection**
- Searches Unsplash based on topic
- Filters for landscape orientation
- Selects high-quality images
- Downloads and attaches to LinkedIn post
- Credits photographer

### 4. **Trend Integration**
- Uses Tavily AI search for latest developments
- Incorporates 2024-2025 trends
- Makes content current and relevant

### 5. **Error Handling**
- Captures all workflow errors
- Sends email alerts
- Logs errors to Google Sheets
- Provides detailed debugging info

---

## 📅 Schedule

**Every 2 days at 10:00 AM**

The workflow uses `scheduleTrigger` with:
- Field: `days`
- Interval: `2`
- Hour: `10`
- Minute: `0`

You can adjust this in the "Schedule Every 2 Days" node.

---

## 🔍 How Deduplication Works

1. Workflow reads `PostHistory` sheet
2. Extracts all posted topics into array
3. Compares against 15 available topics
4. Filters out already-posted topics
5. Randomly selects from remaining topics
6. If all 15 posted → resets and uses full list
7. After posting → logs topic to sheet

**Result:** No topic repeats until all 15 are covered!

---

## 🛠️ Installation Steps

1. **Import the workflow:**
   - Open n8n
   - Click "Import from File"
   - Select `linkedin-ai-ml-learning-workflow.json`

2. **Configure credentials:**
   - Groq API
   - LinkedIn OAuth2
   - Google Sheets OAuth2
   - SMTP (for errors)

3. **Set environment variables:**
   - Add all required variables (see above)

4. **Create Google Sheets:**
   - Create sheet with PostHistory and ErrorLog tabs
   - Share with n8n service account

5. **Test the workflow:**
   - Click "Execute Workflow" manually first
   - Check each node output
   - Verify Google Sheets logging
   - Confirm LinkedIn post (or comment out for testing)

6. **Activate:**
   - Toggle "Active" switch
   - Workflow will run every 2 days automatically

---

## 🎯 Customization Options

### Change Posting Frequency
Edit "Schedule Every 2 Days" node:
- Every day: `daysInterval: 1`
- Every 3 days: `daysInterval: 3`
- Weekly: `daysInterval: 7`

### Add More Topics
Edit "Topic Selector with Rotation" node → Add to `topics` array:
```javascript
{
  "main": "Your Topic Name",
  "description": "Topic description",
  "resources": ["Resource 1", "Resource 2", "Resource 3"]
}
```

### Change AI Model
Edit "Groq Chat Model" node:
- Models: `llama-3.3-70b-versatile`, `mixtral-8x7b-32768`, `gemma2-9b-it`
- Temperature: 0.1-1.0 (lower = more focused, higher = more creative)

### Customize Image Search
Edit "Search Unsplash Images" node → Change query parameters:
- Add/remove keywords
- Change orientation (landscape, portrait, squarish)
- Adjust per_page (number of results)

### Modify Post Format
Edit "Groq AI Content Agent" node → Update the prompt or system message

---

## 📊 Monitoring & Maintenance

### Check Workflow Status
- n8n Dashboard → Executions tab
- View successful/failed runs
- Inspect node outputs

### Review Posted Content
- Open Google Sheets PostHistory tab
- See all topics posted with timestamps
- Verify no duplicates

### Error Tracking
- Check ErrorLog sheet tab
- Review email alerts
- Investigate failed nodes

### Rate Limits
- **Groq:** Free tier has limits (check console)
- **Unsplash:** 50 requests/hour
- **Tavily:** 1000 searches/month
- **LinkedIn:** Posting limits apply

---

## 🆘 Troubleshooting

### "Read Post History" fails
- ✅ Check Google Sheets credentials
- ✅ Verify sheet ID in environment variables
- ✅ Confirm sheet name matches exactly
- ✅ Ensure service account has access

### "Groq AI Content Agent" fails
- ✅ Verify Groq API key is valid
- ✅ Check rate limits in Groq console
- ✅ Reduce maxTokens if timeout occurs

### "Search Unsplash Images" fails
- ✅ Confirm UNSPLASH_ACCESS_KEY is set
- ✅ Check rate limit (50/hour)
- ✅ Verify API key is active

### "Post to LinkedIn" fails
- ✅ Refresh LinkedIn OAuth2 token
- ✅ Verify LINKEDIN_PERSON_URN is correct
- ✅ Check LinkedIn API permissions
- ✅ Ensure image size is within limits

### No images found
- Fallback image is used automatically
- Check Unsplash query in node
- Try broader search terms

---

## 🎓 Best Practices

1. **Test manually first** before activating schedule
2. **Monitor first week** to ensure smooth operation
3. **Review content quality** - adjust prompts if needed
4. **Check Google Sheets regularly** for deduplication accuracy
5. **Set up email alerts** for peace of mind
6. **Backup workflow JSON** periodically
7. **Keep API keys secure** - use environment variables only

---

## 📈 Expected Results

### Content Quality
- Educational and actionable
- Professional yet accessible tone
- Includes practical free resources
- Incorporates current trends
- Engaging with emojis and hashtags

### Posting Schedule
- Every 2 days at 10:00 AM
- 15 unique topics before any repeat
- Approximately 2 posts per week
- Full topic cycle every 30 days

### Engagement
- High-quality AI-focused images
- Valuable free learning resources
- Timely industry trends
- Clear call-to-action

---

## 🔗 Useful Links

- **n8n Documentation:** https://docs.n8n.io/
- **Groq Console:** https://console.groq.com/
- **Unsplash API:** https://unsplash.com/developers
- **Tavily API:** https://tavily.com/
- **LinkedIn Developers:** https://www.linkedin.com/developers/

---

## 📝 Support

For issues or questions:
1. Check n8n execution logs
2. Review error messages in ErrorLog sheet
3. Consult n8n community forum
4. Review this documentation

---

**Happy Automating! 🚀**

This workflow will help you consistently share valuable AI/ML learning content with your LinkedIn network, positioning you as a helpful educator in the AI space.
