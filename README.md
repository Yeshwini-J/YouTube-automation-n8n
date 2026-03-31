# YouTube-automation-n8n
An automated YouTube content publishing pipeline built using n8n that reads video metadata from Google Sheets, fetches video files from Google Drive, and uploads them to YouTube using the YouTube Data API. The system eliminates manual uploads by orchestrating a scheduled, end-to-end workflow with dynamic data handling and multi-service integration.

 --> What is n8n?
n8n is a workflow automation tool that allows you to connect different services and automate tasks without writing extensive code.
It works by creating workflows made up of nodes, where each node performs a specific action such as:
* Fetching data
* Filtering data
* Calling APIs
* Triggering events

-->❗ Problem
Uploading videos daily involves repetitive manual steps:
Selecting files
Adding title and description
Uploading and scheduling
This process is time-consuming and inefficient.

-->✅ Solution
An automated workflow that:
Reads video data from Google Sheets
Fetches video from Google Drive
Uploads it to YouTube automatically


-->🔄 Workflow Overview
The automation follows this pipeline:
Schedule Trigger → Google Sheets → IF Condition → Limit → Google Drive → YouTube Upload
Each step processes data and passes it to the next node.


1. Schedule Trigger (Cron):
The workflow begins with a Schedule Trigger node.
Purpose:
* Automatically starts the workflow at a fixed time interval
Configuration:
* Initially set to run every minute for testing
* Can be changed to daily (e.g., 6 PM) for real use
Why used:
This removes the need to manually start the workflow and ensures consistent execution.

2. Google Sheets (Data Source):
This node reads video metadata from a Google Sheet.
Data stored:
| video_name | title      | description | status  |
| ---------- | ---------- | ----------- | ------- |
| test.mp4   | Test video | testing     | pending |
Why used:
Acts as a simple database to manage video uploads.

3. IF Condition (Filtering)
This node filters only the videos that are ready to be uploaded.
Condition used:
status = "pending"
Why used:
Ensures that only videos marked as pending are processed, avoiding duplicate uploads.

4. Limit Node
This node ensures that only one video is processed at a time.
Configuration:
* Max items = 1
* Keep = First item
Why used:
Prevents multiple uploads in a single execution and allows controlled scheduling (one video per run).

5. Google Drive (File Fetching)
This node downloads the video file from Google Drive.
Action:
* Selects the video file
* Converts it into binary data for upload
Why used:
Since n8n cloud cannot access local files, Google Drive acts as a storage system for video files.

6. YouTube Upload
This node uploads the video to YouTube using the YouTube Data API.
Dynamic fields:
* Title → {{$json["title"]}}
* Description → {{$json["description"]}}
Binary input:
* File from Google Drive
Why used:
Automates the final step of publishing content without manual intervention.

🔐 Authentication Setup (OAuth)

To connect YouTube, Google Drive, and Google Sheets, OAuth authentication was used.
Steps involved:
* Created a project in Google Cloud Console
* Enabled YouTube Data API v3
* Configured OAuth consent screen
* Generated Client ID and Client Secret
* Connected account in n8n
This enables secure access to Google services.

-->✅ Result

The workflow successfully:
* Reads video data from Google Sheets
* Filters pending videos
* Fetches video from Google Drive
* Uploads it to YouTube automatically
This eliminates manual uploads and ensures consistent content publishing.

