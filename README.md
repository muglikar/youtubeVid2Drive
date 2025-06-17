YouTube Downloader Web App for Google Colab

This project provides a complete, self-hosted web application for downloading YouTube videos, audio, or transcripts directly to any user's Google Drive. It runs entirely within a Google Colab notebook and is made publicly accessible via a permanent ngrok URL.

The key feature is its robust, multi-user authentication system. Each person who uses your app link will be prompted to log in with their own Google account, and all files will be saved securely to their own Google Drive, not the developer's.

<!-- It's a good idea to take a screenshot and upload it to a service like imgur to include here -->

Features

Multi-User Authentication: Securely saves files to each user's own Google Drive.

Download Modes: Supports downloading as Video, Audio-only, or Transcript (.vtt).

Persistent & Secure: Uses a permanent static ngrok URL and a standard OAuth 2.0 web flow.

User-Friendly UI: Clean, responsive interface with format selection, download history, and progress indicators.

Long-Running Sessions: Includes an automatic keep-alive mechanism to prevent Colab's inactivity timeout.

Trimming: Optional video trimming via ffmpeg before upload.

How It Works

The application is a Flask web server running within a Google Colab notebook.

ngrok creates a secure public tunnel to the Flask app, making it accessible on the internet via a permanent URL.

Google Cloud Platform is used to register the application, providing the necessary credentials to use Google's OAuth 2.0 login system.

When a user logs in, the app securely stores their temporary permission (OAuth token) in a server-side session. It never stores passwords.

All downloads happen on the Colab server's temporary disk.

The app then uses the specific logged-in user's permission to upload the downloaded file from the server's disk to the user's personal Google Drive.

Setup Guide: From Zero to Running App

This is a one-time setup process that takes about 10-15 minutes. Follow these steps meticulously.

Part 1: Get Your Ngrok Static Domain and Auth Token (Free)

ngrok is the service that gives our Colab notebook a permanent public URL.

Create an ngrok Account: Go to https://ngrok.com/ and sign up for a free account.

Get Your Auth Token: Go to your ngrok dashboard. On the left menu, click Your Authtoken. Copy this token. You will need it later.

Get Your Static Domain: On the left menu, click Cloud Edge -> Domains. Click + Create Domain (or "New Domain"). Ngrok will create a permanent domain for you (e.g., [some-words].ngrok-free.app). Copy this domain name.

Part 2: Configure Your Google Cloud Project (One-Time Setup)

Our app needs to be registered with Google to use their login service.

Create a Google Cloud Project:

Go to the Google Cloud Console Project Creator.

Give your project a name (e.g., "My Colab Downloader") and click Create.

Enable the Google Drive API:

Go to the Google Drive API Library.

Make sure your new project is selected in the dropdown at the top of the page.

Click the Enable button.

Configure the OAuth Consent Screen:

Go to the OAuth Consent Screen page.

Select External for the User Type and click Create.

On the next page, fill in the required fields:

App name: "UtubeDowner" (or any name you like).

User support email: Your Gmail address.

Developer contact information: Your Gmail address.

Click SAVE AND CONTINUE on all subsequent steps. You do not need to add scopes or test users here.

Finally, click PUBLISH APP and confirm. This allows any Google user to use your app.

Create and Configure the OAuth Client ID:

Go back to the Credentials page.

Click + CREATE CREDENTIALS and select OAuth client ID.

For Application type, select Web application.

Under Authorized redirect URIs, click + ADD URI.

Paste your static ngrok domain from Part 1 and add /oauth2callback to the end. It must look exactly like this:
https://[your-static-domain].ngrok-free.app/oauth2callback

Click Create.

Download Your Client Secrets:

A box will pop up with your credentials. Click DOWNLOAD JSON.

The file will be named something like client_secret_....json.

Rename this file to client_secrets.json. This exact filename is required.

Part 3: Run the Application in Google Colab

This is the final step, which you will do each time you want to start the server.

Open the Colab Notebook: Open the .ipynb file for this project in Google Colab.

Restart the Runtime: To ensure a clean environment, it's best to restart the runtime. In the top menu, click Runtime -> Restart runtime....

Upload Your client_secrets.json File:

In the file browser panel on the left side of Colab, click the "Upload to session storage" button.

Select the client_secrets.json file you downloaded and renamed in Part 2.

Fill in Your Credentials:

In the first code cell, find the configuration section.

Paste your ngrok static domain (from Part 1, Step 3) into the NGROK_STATIC_DOMAIN variable. Do not include https://.

Paste your ngrok auth token (from Part 1, Step 2) into the NGROK_AUTH_TOKEN variable.

Run the Code:

Run the single code cell in the notebook.

The script will install dependencies, configure the server, and start the ngrok tunnel.

The output will show ✅ YouTube Downloader is RUNNING! along with your permanent public URL.

Your application is now live and can be shared with anyone using that permanent URL. It will stay online for ~12 hours thanks to the automatic keep-alive feature.
