# How to Set Up Google Sheets as Your Lead Database

## Step 1: Create a Google Sheet

1. Go to https://sheets.google.com
2. Create a new blank spreadsheet
3. Name it "Mitolyn Leads"
4. In Row 1, add these headers:
   - A1: `First Name`
   - B1: `Email`
   - C1: `Timestamp`
   - D1: `Source`

## Step 2: Create a Google Apps Script

1. In your Google Sheet, click **Extensions** → **Apps Script**
2. Delete any code in the editor
3. Paste this code:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    
    sheet.appendRow([
      data.firstName,
      data.email,
      new Date().toISOString(),
      data.source || 'mitolyn-landing-page'
    ]);
    
    return ContentService
      .createTextOutput(JSON.stringify({ success: true, message: 'Lead saved!' }))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch (error) {
    return ContentService
      .createTextOutput(JSON.stringify({ success: false, message: error.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doGet(e) {
  return ContentService
    .createTextOutput(JSON.stringify({ status: 'Lead capture API is running' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

4. Click **Save** (Ctrl+S)
5. Name the project: "Mitolyn Lead API"

## Step 3: Deploy as Web App

1. Click **Deploy** → **New deployment**
2. Click the gear icon ⚙️ next to "Select type" → Choose **Web app**
3. Settings:
   - Description: "Mitolyn Lead Capture"
   - Execute as: **Me**
   - Who has access: **Anyone**
4. Click **Deploy**
5. Click **Authorize access** → Select your Google account → Allow
6. **COPY THE WEB APP URL** - it looks like:
   `https://script.google.com/macros/s/XXXXX.../exec`

## Step 4: Update Your HTML

Open `index.html` and replace this line:
```javascript
const GOOGLE_SCRIPT_URL = 'YOUR_GOOGLE_SCRIPT_URL_HERE';
```

With your actual URL:
```javascript
const GOOGLE_SCRIPT_URL = 'https://script.google.com/macros/s/XXXXX.../exec';
```

## That's It! 🎉

Now when users submit the form:
1. Lead is saved to your Google Sheet automatically
2. Email is sent via EmailJS
3. User is redirected to ClickBank

## Benefits:
- ✅ Works on Vercel (no server needed)
- ✅ Free forever
- ✅ View leads in Google Sheets (like Excel)
- ✅ Download as Excel/CSV anytime
- ✅ Access from anywhere
- ✅ Share with team members
