# In Memory of SMG4 – Memorial Website

A respectful tribute page for **SMG4**, where visitors can leave messages of thanks and remembrance. Messages are displayed in a clean “wall” layout, with server-side moderation to ensure respectful content.

## Features

- **Tribute Wall:** Visitors can submit their name and a short message of thanks.
- **Server-side moderation:** All messages are checked against a banned words list on Google Apps Script before being saved.
- **Live updates:** The wall updates automatically when new messages are submitted.
- **Responsive design:** Optimized for desktop and mobile devices.
- **Manual moderation:** Administrators can remove inappropriate messages directly from the Google Spreadsheet.

## Demo

![SMG4 Memorial Screenshot](smg4.png)

## Project Structure

index.html # Main HTML page for the memorial  
style.css # Optional: custom CSS (can be inline)  
script.js # Client-side JS for submitting messages & loading wall  
badwords.js # A list of English profanity  
Google Apps Script  
└── Code.gs # doGet/doPost + server-side bad words filter  

## Setup Instructions

### 1. Google Spreadsheet

1. Create a new Google Spreadsheet.  
2. Add columns: `Name | Message | Timestamp`.  
3. Note the sheet name (default is the first sheet).  

### 2. Google Apps Script

1. Go to **Extensions → Apps Script** in your Google Spreadsheet.  
2. Replace the default code with:

function doPost(e) {  
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();  
  const data = JSON.parse(e.postData.contents);  

  sheet.appendRow([  
    data.name,  
    data.message,  
    new Date()  
  ]);  

  return ContentService  
    .createTextOutput(JSON.stringify({ success: true }))  
    .setMimeType(ContentService.MimeType.JSON);  
}  

function doGet() {  
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();  
  const rows = sheet.getDataRange().getValues();  

  const messages = rows.map(r => ({  
    name: r[0],  
    message: r[1],  
    time: r[2]  
  }));  

  return ContentService  
    .createTextOutput(JSON.stringify(messages))  
    .setMimeType(ContentService.MimeType.JSON);  
}  

Deploy as a web app:  
Click Deploy → New Deployment → Web App  

Set access to Anyone  

Copy the URL → replace SCRIPT_URL in index.html.  

### 3. Hosting the Website

You can host index.html on GitHub Pages, Netlify, or any static host.  
Make sure SCRIPT_URL points to your deployed Google Apps Script.  

### 4. Usage

Open the memorial site in a browser.  
Enter your name and a message in the tribute form.  
Click Leave Tribute.  
Messages are filtered automatically for banned words.  
Messages appear on the wall in real time.  

## Moderation

Any inappropriate messages that bypass the filter can be manually removed from the spreadsheet.  
To update the banned words list, edit the bannedWords array in the Apps Script.  

## Credits

Created by Mahdiisdumb  
Tribute to SMG4
