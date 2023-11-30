# Copy Email as PDF

To create a Google Apps Script that searches for emails with a specific subject, converts them to PDF, and saves them in a specific Google Drive folder structure based on the current year and month, you can follow these steps. This script will be designed to be reusable and maintainable, with constants extracted to the top.

First, ensure you have the necessary permissions to access Gmail and Google Drive in your Google Apps Script project.

Here's a script that accomplishes the following requirements:

```javascript

// Constants
const EMAIL_SUBJECT = "Meta data needed"; // Subject to search in emails
const PARENT_FOLDER_ID = "your_parent_folder_id_here"; // Replace with your actual parent folder ID

const EMAIL_SUBJECT_PREFIX = "Datastore Inventory Report ({0}) Temp"; // Base subject to search in emails

/**
 * Main function to be executed by the trigger.
 */
function saveEmailsAsPDFAndNotify() {
  try {
    const currentDate = new Date();
    const previousDate = new Date();
    previousDate.setDate(currentDate.getDate() - 1);

    const formattedCurrentDate = Utilities.formatDate(currentDate, Session.getScriptTimeZone(), "MM/dd/yy");
    const formattedPreviousDate = Utilities.formatDate(previousDate, Session.getScriptTimeZone(), "MM/dd/yy");

    const subjectToday =  formatString(EMAIL_SUBJECT_PREFIX, formattedCurrentDate);
    const subjectYesterday =  formatString(EMAIL_SUBJECT_PREFIX, formattedPreviousDate);

    const emailsToday = GmailApp.search('subject:' + subjectToday);
    const emailsYesterday = GmailApp.search('subject:' + subjectYesterday);

    const allEmails = emailsToday.concat(emailsYesterday).sort((a, b) => b.getDate() - a.getDate());

    if (allEmails.length > 0 && !isEmailSaved(allEmails[0])) {
      saveEmailToPDF(allEmails[0]);
    }

    sendNotification(PROJECT_OWNER_EMAIL, "Daily Email Processing Completed", "The script has successfully run and processed emails.");
  } catch (error) {
    sendNotification(PROJECT_OWNER_EMAIL, "Error in Daily Email Processing", "An error occurred: " + error.message);
  }
}

/**
 * Saves an email as a PDF in the designated Google Drive folder.
 * @param {GmailMessage} email - The email to save.
 */
function saveEmailToPDF(email) {
  const emails = GmailApp.search('subject:' + EMAIL_SUBJECT);
  const currentDate = new Date();
  const currentYear = currentDate.getFullYear();
  const currentMonth = currentDate.getMonth() + 1; // JavaScript months are 0-indexed
  const folderName = 'Weekly Emails';

  // Get or create the necessary folders
  const yearFolder = getOrCreateFolder(PARENT_FOLDER_ID, currentYear.toString());
  const monthFolder = getOrCreateFolder(yearFolder.getId(), currentMonth.toString());
  const targetFolder = getOrCreateFolder(monthFolder.getId(), folderName);

  // Process each email
  emails.forEach(email => {
    const emailThread = email.getThread();
    const messages = emailThread.getMessages();

  messages.forEach((message, index) => {
      const subject = message.getSubject();
      const safeSubject = subject.replace(/\//g, '_'); // Replace '/' with '_'
      const pdf = convertEmailToPDF(message);
      const pdfName = safeSubject + ' - ' + (index + 1) + '.pdf';
      targetFolder.createFile(pdf).setName(pdfName);
    });
  });
}

/**
 * Checks if the most recent email has already been saved as a PDF.
 * @param {GmailMessage} email - The email to check.
 * @return {boolean} True if the email has been saved, false otherwise.
 */
function isEmailSaved(email) {
  const currentDate = new Date();
  const currentYear = currentDate.getFullYear();
  const currentMonth = currentDate.getMonth() + 1; // JavaScript months are 0-indexed
  const folderName = 'Weekly Emails';

  // Get the necessary folders
  const yearFolder = getOrCreateFolder(PARENT_FOLDER_ID, currentYear.toString());
  const monthFolder = getOrCreateFolder(yearFolder.getId(), currentMonth.toString());
  const targetFolder = getOrCreateFolder(monthFolder.getId(), folderName);

  const subject = email.getSubject();
  const safeSubject = subject.replace(/\//g, '_'); // Replace '/' with '_'

  // Check if a file with the same name exists
  const files = targetFolder.getFiles();
  while (files.hasNext()) {
    const file = files.next();
    if (file.getName().startsWith(safeSubject)) {
      return true; // File with the same subject already exists
    }
  }
  return false; // No file with the same subject found
}

/**
 * Converts an individual email message to a PDF Blob.
 * @param {GmailMessage} message - The email message to convert.
 * @return {Blob} The PDF blob.
 */
function convertEmailToPDF(message) {
  const body = message.getBody();
  const blob = Utilities.newBlob(body, 'text/html', 'email.html');
  return blob.getAs('application/pdf');
}

/**
 * Retrieves a folder by name under a parent folder, or creates it if it doesn't exist.
 * @param {string} parentFolderId - The ID of the parent folder.
 * @param {string} folderName - The name of the folder to find or create.
 * @return {GoogleAppsScript.Drive.Folder} The retrieved or created folder.
 */
function getOrCreateFolder(parentFolderId, folderName) {
  const parentFolder = DriveApp.getFolderById(parentFolderId);
  const folders = parentFolder.getFoldersByName(folderName);

  if (folders.hasNext()) {
    return folders.next();
  } else {
    return parentFolder.createFolder(folderName);
  }
}

/**
 * Formats a string by replacing placeholders with provided arguments.
 * This function mimics a simplified version of the string formatting found in some other languages.
 * Placeholders in the string are indicated by {index}, where 'index' is the position of the argument to substitute.
 * 
 * Example usage:
 *   formatString("Hello {0}, your balance is {1}", "Alice", "$100")
 *   // returns "Hello Alice, your balance is $100"
 *
 * @param {string} str - The base string containing placeholders.
 * @param {...any} args - A list of arguments to replace placeholders.
 * @return {string} The formatted string with placeholders replaced by provided arguments.
 */
function formatString(str, ...args) {
  return str.replace(/{(\d+)}/g, function(match, number) { 
    // The replace function searches for pattern {number} in 'str'
    // 'number' is captured from the pattern and used to access the corresponding element in 'args'
    // If the element exists, it replaces the pattern; otherwise, the pattern remains unchanged
    return typeof args[number] != 'undefined'
      ? args[number] 
      : match;
  });
}

/**
 * Sends a notification email to a specified recipient.
 * @param {string} recipient - The email address of the recipient.
 * @param {string} subject - The subject of the email.
 * @param {string} body - The body of the email.
 */
function sendNotification(recipient, subject, body) {
  GmailApp.sendEmail(recipient, subject, body);
}

// Other utility functions (convertEmailToPDF, getOrCreateFolder) remain the same

// Function to set up the trigger
function setUpTrigger() {
  ScriptApp.newTrigger('saveEmailsAsPDFAndNotify')
    .timeBased()
    .atHour(17) // 5:00 PM
    .everyDays(1)
    .inTimezone(Session.getScriptTimeZone())
    .create();
}

```
How to Use This Script

    Replace 'your_parent_folder_id_here' with the actual ID of your parent folder in Google Drive.
    Run the saveEmailsAsPDF function to execute the script.

What This Script Does

    Searches for Emails: It looks for emails with the subject containing "Meta data needed".
    Folder Structure: It organizes the saved PDFs in a folder structure based on the current year, month, and a subfolder named 'Weekly Emails'.
    Converts and Saves Emails: Each email found is converted to a PDF and saved in the designated folder.

Notes

    Ensure that your Google Apps Script project has permissions to access Gmail and Google Drive.
    The script assumes that the email body is in HTML format, which is standard for Gmail.
    The naming convention for PDFs is the email subject followed by the message index in the thread.
    The script does not handle cases where the email subject changes in a thread. Each email in a thread is treated as a separate PDF.

Remember to test the script in a controlled environment before using it extensively to ensure it meets your needs.
