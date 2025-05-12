# Gmail-Integrated-Job-Application-Pipeline-Manager-Sheet-CRM-V2
An enhanced Google Apps Script (v2.0) to automate your job application workflow. Parses Gmail emails, logs details to a Google Sheet (acting as a mini-CRM &amp; pipeline manager), manages stages with nested labels, and includes one-click setup for the sheet, labels, and automated triggers.

# Automated Job Application Tracker & Pipeline Manager v2.0

**Author:** Francis John LiButti
**Version:** 2.0.0
**Last Updated:** May 12, 2024 *(User: Please update this to the current date when you post)*

## Short Description

An enhanced Google Apps Script (v2.0) to automate your job application workflow. Parses Gmail emails, logs details to a Google Sheet (acting as a mini-CRM & pipeline manager), manages stages with nested labels, and includes one-click setup for the sheet, labels, and automated triggers.

## Overview

This "Automated Job Application Tracker & Pipeline Manager v2.0" is a Google Apps Script designed to significantly streamline the often-tedious process of tracking job applications. By integrating directly with your Gmail and Google Sheets, it provides a centralized system to:

*   Automatically process emails related to your job applications that you label in Gmail.
*   Intelligently parse these emails for key information such as company name, job title, and the current application status (e.g., Applied, Interview Scheduled, Rejected).
*   Log all extracted details into a structured Google Sheet, which serves as your central application pipeline and mini-CRM.
*   Organize your Gmail using a nested label system specific to this tracker (e.g., "Job Application Pipeline v2 / To Process").
*   Automate the initial setup of necessary Gmail labels, the Google Sheet (complete with detailed formatting like frozen headers, specific column widths, text wrapping, and alternating row colors), and time-driven triggers for ongoing automation.
*   Offer flexibility in sheet configuration: use a specific, pre-existing Google Sheet by providing its ID, or let the script dynamically create and manage its own data sheet.
*   Automatically identify and update "stale" applications (e.g., those with no updates for several weeks) to a "Rejected" status, helping keep your pipeline current.

## Features

*   **Centralized Configuration:** All major settings managed in a `CONFIG` object within the script for easier customization.
*   **Automated Email Parsing:** Extracts company, job title, and attempts to infer application status from Gmail threads.
*   **Google Sheet as CRM/Pipeline:** Creates and maintains a formatted spreadsheet for comprehensive application tracking.
*   **Nested Gmail Label System:** Provides clear visual organization of application emails within Gmail.
*   **One-Click Initial Setup:** A dedicated menu command (`0. Initial Setup...`) automates the creation of labels, the sheet (with detailed styling), and essential time-driven triggers.
*   **Hybrid Sheet Management:** Supports using a user-specified `FIXED_SPREADSHEET_ID` or dynamic sheet creation/discovery by filename (with ID stored in `PropertiesService`).
*   **Detailed Sheet Formatting:** New data sheets are automatically formatted with:
    *   Bold, centered, and frozen header row.
    *   Generous default row heights and specific column widths for readability.
    *   Text wrapping for most data, with clipping for the "Email Link" column.
    *   Alternating row colors (blue and white banding) for data rows.
    *   Automatic hiding of unused columns.
*   **Stale Application Flagging:** A function (and trigger) to automatically mark applications as "Rejected" if they haven't been updated beyond a configurable time threshold (default 7 weeks) and are not already in a final status.
*   **Customizable Keyword Engine:** Relies on user-defined keyword lists in the `CONFIG` object for more accurate email parsing.
*   **Programmatic Trigger Setup:** Automates the creation of:
    *   An **hourly** trigger for processing new job application emails.
    *   A **daily** (around midnight) trigger for checking and marking stale applications.
*   **User-Friendly Menu:** Adds a custom menu in Google Sheets for easy access to setup, manual processing, and trigger verification.

## Prerequisites

*   A Google Account (for Gmail, Google Sheets, Google Drive, and Apps Script).
*   Willingness to copy/paste code into the Google Apps Script editor and authorize its permissions.

## Setup Instructions

**1. Copy the Script Code:**
   *   Navigate to the `Code.gs` file (or the primary `.gs` file) in this repository.
   *   Select and copy all the code within that file.

**2. Open or Create a Google Sheet:**
   *   It's recommended to create a new, blank Google Sheet (e.g., name it "Job Tracker Control Panel"). This sheet will host the script's custom menu.
   *   Alternatively, you can use an existing sheet.

**3. Access the Apps Script Editor:**
   *   With your Google Sheet open, click on **Extensions > Apps Script**. This will open the editor bound to that sheet.
   *   *(If you prefer a standalone script, go to `script.google.com` and start a new project, but a sheet-bound script makes menu access easier).*

**4. Paste and Configure the Script:**
   *   In the Apps Script editor, if there's a `Code.gs` file with default content, delete that content.
   *   Paste the entire script code you copied from GitHub.
   *   **CRITICAL: Configure Constants (near the top of the script):**
        *   **`CONFIG.FIXED_SPREADSHEET_ID`**:
            *   **Option A (Use a specific existing sheet for data):** Find the ID of your existing Google Sheet (from its URL: `.../d/SHEET_ID_IS_HERE/edit`) and replace `"YOUR_SPREADSHEET_ID_HERE"` with that ID.
            *   **Option B (Let the script create/manage its data sheet - Recommended for new users):** Leave `FIXED_SPREADSHEET_ID` as `"YOUR_SPREADSHEET_ID_HERE"` or set it to `""`. The script will then use `CONFIG.TARGET_SPREADSHEET_FILENAME` (default: "Automated Job App Tracker Data v2").
        *   **`CONFIG.GMAIL_LABEL_PARENT`**: Default is "Job Application Pipeline v2". Change if desired (this will be the parent label in Gmail).
        *   **`CONFIG.KEYWORDS`**: **THIS IS VERY IMPORTANT.** Review and extensively customize the `REJECTION`, `ACCEPTANCE`, `INTERVIEW`, `ASSESSMENT`, `PLATFORM_DOMAIN`, and `IGNORED_DOMAINS` lists to match the types of emails you receive. Better keywords lead to much better parsing.
        *   Review other settings in `CONFIG` like `WEEKS_THRESHOLD_FOR_STALE` if needed.

**5. Save the Script:**
   *   Click the "Save project" icon (floppy disk) in the Apps Script editor.
   *   Give the Apps Script project a name if prompted (e.g., "JobApplicationManagerV2").

**6. Run Initial Setup (One-Time):**
   *   Go back to your Google Sheet (the one from Step 2). **Refresh the browser tab/window.**
   *   After a few moments, a new menu named **"Job App Pipeline v2"** (or similar, based on `CONFIG`) should appear.
   *   Click **Job App Pipeline v2 > 0. Initial Setup (Labels, Sheet, Triggers)**.
   *   **Authorization Required:**
        *   A dialog box will prompt for authorization. Click "Continue."
        *   Select your Google Account.
        *   If you see a "Google hasn’t verified this app" screen, click "Advanced," then "Go to [Your Script Name] (unsafe)." This is standard for personal scripts.
        *   Review the permissions requested (Gmail, Sheets, Drive (if needed for dynamic sheet), Triggers). Click "Allow."
   *   The setup function will now execute, creating Gmail labels, setting up the Google Sheet (with formatting), and creating the two automated triggers. It will attempt to show a summary alert.
   *   **Check Execution Logs:** In the Apps Script editor, go to **View > Executions**. Click the most recent run of `initialSetup_Full` to see detailed logs of each setup step.

**7. Verify Setup:**
    *   **Gmail:** Look for the parent label (e.g., "Job Application Pipeline v2") and its nested children: "/To Process", "/Processed", "/Manual Review Needed".
    *   **Google Drive/Sheets:** Find the data spreadsheet (either your fixed one or "Automated Job App Tracker Data v2"). Open it. Ensure the "Applications" tab exists and has the detailed formatting.
    *   **Apps Script Triggers:** In the Apps Script editor, go to the "Triggers" page (alarm clock icon). Confirm the hourly trigger for `processJobApplicationEmails` and the daily trigger for `markStaleApplicationsAsRejected` are present and correctly configured.

## How to Use

**1. Label Emails for Processing:**
   *   In Gmail, when you send an application or receive relevant correspondence, apply the **`[Your Parent Label]/To Process`** label (e.g., `Job Application Pipeline v2/To Process`) to that email thread.

**2. Processing Application Emails:**
   *   **Manually:** From the Google Sheet menu, select **Job App Pipeline v2 > 1. Process New Emails Now**.
   *   **Automatically:** The hourly trigger will execute `processJobApplicationEmails` to scan for and process labeled emails.
   *   Data is logged to the "Applications" tab. Gmail labels are updated (e.g., to `/Processed` or `/Manual Review Needed`).

**3. Marking Stale Applications:**
   *   **Manually:** Select **Job App Pipeline v2 > 2. Mark Stale Applications as Rejected**.
   *   **Automatically:** The daily (around midnight) trigger runs `markStaleApplicationsAsRejected` to update applications older than `CONFIG.WEEKS_THRESHOLD_FOR_STALE`.

**4. Managing Triggers:**
   *   The menu provides options (`3.` and `4.`) to verify or re-attempt the creation of specific triggers if needed.
   *   You can also view, edit, or delete triggers directly on the "Triggers" page in the Apps Script editor.

## Shortcomings and Considerations

*   **Parsing Reliant on Keywords:** Email parsing accuracy is heavily dependent on the keyword lists in `CONFIG.KEYWORDS`. Regular review and customization are essential.
*   **Complex Email Structures:** Very unusual or complex HTML email formats might not parse perfectly. Manual review for such cases is advisable.
*   **Google Services Quotas/Limits:** While generally generous for personal use, be mindful of Google's daily quotas for Gmail, Sheets, and script execution time if processing extremely high volumes.
*   **Trigger Execution Timing:** Time-driven triggers run approximately at the scheduled time; Google may randomize the exact minute.
*   **Error Handling:** The script includes logging. If issues occur, check the Apps Script "Executions" log for details.
*   **No Dedicated UI for Settings:** All configuration (keywords, thresholds) is done by editing the `CONFIG` object in the script code.
*   **Consolidation Function Placeholder:** `performProcessConsolidation_()` is included but contains no logic. Users can add custom actions here if desired.

## Future Enhancements (Ideas)

*   More robust parsing for a wider variety of email templates.
*   A simple web app interface for viewing stats or managing settings.
*   Integration with Google Calendar for interview scheduling.
*   Direct "apply from sheet" functionality (more complex).

---
This `README.md` should give users a comprehensive guide to setting up and using your script. Remember to fill in any bracketed placeholders like `*(User: Please update this to the current date when you post)*`.
