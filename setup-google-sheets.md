# Setup Instructions: Contact Form → Google Sheets

## Step 1: Create Google Sheet

1. Go to [Google Sheets](https://sheets.google.com)
2. Create a new spreadsheet
3. Name it "Website Contact Form Submissions"
4. In the first row, add these headers:
   - A1: `Timestamp`
   - B1: `Name` 
   - C1: `Email`
   - D1: `Phone`
   - E1: `Service`
   - F1: `Message`
   - G1: `Language`

## Step 2: Create Google Apps Script

1. In your Google Sheet, go to **Extensions → Apps Script**
2. Delete any existing code
3. Copy and paste this code:

```javascript
function doPost(e) {
  try {
    // Get the active spreadsheet
    const sheet = SpreadsheetApp.getActiveSheet();
    
    // Get form data from POST parameters
    const data = e.parameter;
    
    // Get current timestamp
    const timestamp = new Date();
    
    // Add data to the sheet
    sheet.appendRow([
      timestamp,
      data.name || '',
      data.email || '',
      data.phone || '',
      data.service || '',
      data.message || '',
      data.language || ''
    ]);
    
    // Return success response
    return ContentService
      .createTextOutput("Form submitted successfully!")
      .setMimeType(ContentService.MimeType.TEXT);
      
  } catch (error) {
    // Return error response
    return ContentService
      .createTextOutput("Error: " + error.toString())
      .setMimeType(ContentService.MimeType.TEXT);
  }
}

function doGet(e) {
  // Handle GET requests (for testing)
  return ContentService
    .createTextOutput("Contact form endpoint is working!")
    .setMimeType(ContentService.MimeType.TEXT);
}
```

4. Save the script (Ctrl/Cmd + S)
5. Name it "Contact Form Handler"

## Step 3: Deploy the Script

1. Click **Deploy → New deployment**
2. Choose type: **Web app**
3. Fill in:
   - Description: "Contact Form Handler"
   - Execute as: "Me"
   - Who has access: "Anyone"
4. Click **Deploy**
5. **Copy the Web app URL** - you'll need this!
6. Click **Done**

## Step 4: Test the Setup

1. Visit your Web app URL in browser - should show "Contact form endpoint is working!"
2. Your Google Sheet is now ready to receive form submissions!

## Step 5: Update Website Form

Now copy the Web app URL and give it to your developer to update the website form.

## Security & Privacy Notes

- Only you can see the spreadsheet data
- The script runs under your Google account
- No sensitive data is exposed
- You can revoke access anytime by disabling the deployment

## Troubleshooting

If submissions aren't appearing:
1. Check the Apps Script logs (Executions tab)
2. Make sure the deployment is set to "Anyone" access
3. Try redeploying the script
4. Check your Google Sheet permissions 