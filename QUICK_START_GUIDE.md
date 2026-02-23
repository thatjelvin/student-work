# 🚀 Quick Start Guide - HVAC AI Email Outreach

## Get Up and Running in 15 Minutes

---

## Step 1: Prepare Your Google Sheet (5 minutes)

1. **Create a new Google Sheet**
   - Go to [sheets.google.com](https://sheets.google.com)
   - Click **+ Blank**

2. **Add these headers in Row 1:**
   ```
   firstName | businessName | email | emailStatus | lastContactDate
   ```

3. **Add your client data starting from Row 2:**
   ```
   John | Cool Breeze HVAC | john@coolbreeze.com | | 
   Sarah | Climate Pro | sarah@climatepro.com | | 
   ```
   ⚠️ Leave `emailStatus` and `lastContactDate` columns EMPTY

4. **Copy your Sheet ID:**
   - From the URL: `https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit`
   - Save `YOUR_SHEET_ID` for Step 3

---

## Step 2: Import Workflow to n8n (2 minutes)

1. Open n8n
2. Click **Workflows** → **Import from File**
3. Select `hvac-ai-email-outreach-workflow.json`
4. Click **Import**

---

## Step 3: Set Environment Variables (3 minutes)

In n8n, go to **Settings** → **Environment Variables** and add:

```bash
GOOGLE_SHEETS_DOC_ID=paste_your_sheet_id_here
GOOGLE_SHEETS_SHEET_NAME=Clients
SENDER_EMAIL=your-email@yourdomain.com
SENDER_NAME=Your Name
COMPANY_NAME=Your Company Name
```

Click **Save**

---

## Step 4: Configure Credentials (5 minutes)

### A) Google Sheets OAuth2
1. In the workflow, click on **"Read Client Database"** node
2. Click **Credential to connect with**
3. Click **+ Create New Credential**
4. Select **Google Sheets OAuth2 API**
5. Follow the OAuth flow to connect
6. Save as `Google Sheets OAuth2`

### B) OpenAI API
1. Get your API key from [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Click on **"AI Personalize Email Content"** node
3. Create new credential: **OpenAI API**
4. Paste your API key
5. Save

### C) SMTP Email
1. Click on **"Send Personalized Email"** node
2. Create new credential: **SMTP**
3. Enter your email settings:

**For Gmail:**
```
Host: smtp.gmail.com
Port: 587
Secure: Use STARTTLS
User: your-email@gmail.com
Password: [App Password - get from Google Account Security]
```

**For SendGrid:**
```
Host: smtp.sendgrid.net
Port: 587
User: apikey
Password: [Your SendGrid API Key]
```

4. Save

---

## Step 5: Test the Workflow (2 minutes)

1. **Add a test client to your Google Sheet:**
   ```
   Test | Test HVAC Co | your-personal-email@gmail.com | | 
   ```

2. **Run a manual test:**
   - Click the **"Daily Trigger (9 AM Weekdays)"** node
   - Click **Execute Node** (play button)
   - Watch the workflow execute

3. **Verify:**
   - ✅ Check your inbox for the test email
   - ✅ Check Google Sheet - `emailStatus` should now be `sent`
   - ✅ `lastContactDate` should show timestamp

---

## Step 6: Activate (1 minute)

1. Click **Active** toggle at the top of the workflow (turn it ON)
2. The workflow will now run automatically every weekday at 9 AM
3. It will send up to 150 emails per day to clients with blank/pending status

---

## ✅ You're Done!

Your workflow is now:
- ✅ **Running automatically** every weekday at 9 AM
- ✅ **Sending personalized emails** with AI
- ✅ **Tracking sent status** to prevent duplicates
- ✅ **Limiting to 150 emails/day**

---

## 🎯 What Happens Next?

**Every Weekday at 9 AM:**
1. Workflow reads your Google Sheet
2. Finds clients with NO status (blank or "pending")
3. Generates personalized email using AI
4. Sends email
5. Marks as "sent" in Google Sheet
6. Stops after 150 emails

**To Send More:**
- Just add new rows to your Google Sheet
- Leave `emailStatus` blank
- Workflow will pick them up next run

---

## 🛠️ Common Issues

**Q: No emails sent?**
- Check that clients have blank `emailStatus` (not "sent")
- Verify email addresses are valid
- Check execution logs in n8n

**Q: Emails going to spam?**
- Use professional SMTP service (SendGrid, Mailgun)
- Start with smaller batches (50/day)
- Set up SPF/DKIM records

**Q: Google Sheets not updating?**
- Verify column names match exactly
- Re-authenticate Google OAuth2

**Q: OpenAI errors?**
- Check API key is valid
- Verify you have credits/billing set up
- Check usage limits

---

## 📚 Next Steps

- Read the full **HVAC_EMAIL_OUTREACH_SETUP_GUIDE.md** for detailed configuration
- Review **GOOGLE_SHEETS_TEMPLATE.md** for advanced sheet setup
- Customize email templates and AI prompts
- Monitor execution logs and adjust as needed

---

## 💡 Pro Tips

1. **Start Small**: Test with 10-20 clients first
2. **Monitor Results**: Check bounce rates and responses
3. **Warm Up**: Start with 50/day, increase gradually
4. **Personalize**: Tweak AI prompts for better engagement
5. **Track**: Use the EmailLog sheet to monitor campaigns

---

**Need Help?** Check the troubleshooting section in the main setup guide!

**Good luck with your outreach!** 🎉
