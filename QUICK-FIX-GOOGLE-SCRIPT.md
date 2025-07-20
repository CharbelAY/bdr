# 🔧 Quick Fix for CORS Issue

## Problem
You're getting a CORS error when submitting the contact form. This happens because of how browsers handle requests to Google Apps Script.

## Solution
Update your Google Apps Script with this corrected code:

## Steps:

1. **Open your Google Apps Script**: Go to your Google Sheet → Extensions → Apps Script

2. **Replace ALL the existing code** with this updated version:

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

3. **Save the script** (Ctrl/Cmd + S)

4. **If you already deployed**: 
   - Go to Deploy → Manage deployments
   - Click the pencil icon next to your deployment
   - Click "Deploy" to update it

5. **Test your contact form again** - it should now work!

## What Changed
- Changed from JSON data handling to form parameter handling
- This avoids CORS preflight requests that cause the error
- Your website has also been updated to send data in the correct format

## Verification
- Your Google Sheet should now receive form submissions
- No more CORS errors in the browser console
- Form submissions will appear as new rows in your spreadsheet 