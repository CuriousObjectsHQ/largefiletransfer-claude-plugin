---
name: send-and-receive-files
description: Use when the user wants to send or share files from this computer with LargeFileTransfer, email a LargeFileTransfer link to people, download a LargeFileTransfer transfer or link to this computer, or ask someone for files with a LargeFileTransfer file request.
---

# Sending and receiving files with LargeFileTransfer

The LargeFileTransfer tools run on the user's computer, so they can read the files the user names and save the files they download.

## Sending

- Use `send_files` with the files or folders the user named. Send a folder as it is: its structure is kept, so don't zip it first.
- Email a link only to addresses or usernames the user gave you in this conversation. If they say "send it to Ana" without an address, ask for it. Never guess one.
- Set a password, turn comments off, or keep a link from expiring only when the user asks. Tell them a password is on, without repeating it.
- Give the user the link from the result. It works as soon as the upload finishes.

## The first time

If a tool says this computer isn't connected yet, give the user the link and code from the result and wait for them to say they've approved it. Then call the tool again. Don't call it repeatedly while you wait.

## Long uploads and downloads

After about 45 seconds a tool answers and the transfer carries on in the background. Pass on what it says, and check `upload_status` or `download_status` when the user asks how it's going. The work stops if the app running Claude closes, so for a big transfer, tell the user to keep it open until it's done.

## Receiving

- To get files from someone, create a file request with `create_file_request` and give the user the link to send them.
- What people upload appears in `list_transfers` with kind "received". Save it with `download_files` and the transfer ID.
- `download_files` also takes any LargeFileTransfer share link. If the link has a password, ask the user for it.
- Downloads go to the folder the user names, or to a folder named after the transfer in ~/Downloads. Existing files are never overwritten: a new file with the same name gets "-2".

## Plan limits

If a tool reports a plan limit, such as file size, storage or transfers per month, tell the user what it said and where the plans are. Don't retry, and don't work around the limit by splitting files or sending more transfers. Passwords and links that never expire need a Plus or Pro plan.
