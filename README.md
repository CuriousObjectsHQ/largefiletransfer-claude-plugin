# LargeFileTransfer for Claude

Send files and folders from your computer with [LargeFileTransfer](https://largefiletransfer.com) and get a share link back, without leaving Claude. Claude can also email the link to people, download transfers to your computer, and create file requests so other people can upload files to you.

Ask something like:

- *"Send the renders folder to ana@example.com with LargeFileTransfer."*
- *"Download what the client sent me into ./assets."*
- *"Make a LargeFileTransfer file request for the client's logos."*

The plugin runs a small server on your computer, which is what lets it read and save the files you name. It works in Claude Code, and in Cowork when the session runs on your computer. Chat on claude.ai, in the desktop app and in the mobile apps skips it: to use LargeFileTransfer there, add `https://api.largefiletransfer.com/mcp` as a custom connector instead. That one can look things up in your account, but can't send or download files.

## Install

In Claude Code:

```
/plugin install largefiletransfer --marketplace CuriousObjectsHQ/largefiletransfer-claude-plugin
```

## Tools

- **send_files**: uploads files or whole folders (hidden files inside folders are skipped) and returns the share link. It can also email the link to people, set a password, turn comments on or off, and set when the link expires or keep it from expiring. An upload that takes longer than about 45 seconds keeps going in the background, and the link works once it finishes.
- **upload_status**: how an upload started in this session is going.
- **list_transfers**: your most recent transfers, sent or received, with their links and IDs.
- **download_files**: saves a transfer's files to a folder on your computer, from one of your transfers or any LargeFileTransfer share link. Files already in the folder are never overwritten.
- **download_status**: how a download started in this session is going.
- **create_file_request**: makes a link other people upload files to you through.
- **list_file_requests**: your file requests and how many uploads each has had.

The plugin also has a skill that tells Claude how to use these tools well: for example, to email links only to addresses you gave it, and to stop at a plan limit instead of working around it.

## Requirements

- Node.js 20 or later.
- A LargeFileTransfer account. Your plan's limits apply, such as file size, storage, transfers per month and how long links last. Passwords and links that never expire need a Plus or Pro plan.

## Connecting your account

The first time Claude uses one of the tools, your browser opens largefiletransfer.com to approve this computer, showing a short code to check against the one Claude gives you. Approve it and ask Claude again. It stays connected after that.

To disconnect, go to largefiletransfer.com → Settings → Connected apps, or delete the credentials file below.

## What this plugin runs, sends and stores

- **Runs** `npx -y largefiletransfer-mcp@0.1.1`: npx downloads that exact version of the [largefiletransfer-mcp](https://www.npmjs.com/package/largefiletransfer-mcp) package and its dependencies from the npm registry, and runs it on your computer with Node.js. Curious Objects, the company behind LargeFileTransfer, publishes it.
- **Reads** only the files and folders Claude passes to `send_files`.
- **Writes** the files `download_files` saves, into the folder Claude names (by default a folder named after the transfer in `~/Downloads`). It never overwrites a file that's already there.
- **Sends** to LargeFileTransfer:
  - the contents of the files you send, uploaded to LargeFileTransfer storage (`s3.largefiletransfer.com`) through short-lived signed links;
  - their names, sizes and types, and the transfer's title, message, expiry, password and comments setting, to the LargeFileTransfer API (`api.largefiletransfer.com`);
  - the email addresses and usernames you ask Claude to send a link to. LargeFileTransfer emails each address the title, message and link, and delivers it to each username's LargeFileTransfer inbox;
  - the title, description, password and upload limit of the file requests you create;
  - while connecting, a label made of the app's name and your computer's name (for example "Claude Code on Studio"), which you see on the approval page and under Connected apps.
- **Fetches** your transfers, file requests and upload progress from `api.largefiletransfer.com`, and the files you download from LargeFileTransfer storage (`s3.largefiletransfer.com`).
- **Opens** your default browser at largefiletransfer.com, once, to approve the connection.
- **Stores** the connection's tokens, with your account's ID, name and email, in `~/.config/lft-mcp/credentials.json`, with permissions that let only your user account read it.

It contacts nothing else and sends no analytics.

Anyone with a share link can download its files until the link expires, and anyone with a file request's link can upload files to you. How LargeFileTransfer handles what you send is covered by its [Privacy Policy](https://largefiletransfer.com/legal/privacy) and [Terms of Service](https://largefiletransfer.com/legal/terms).

## Support

[largefiletransfer.com/support](https://largefiletransfer.com/support), or email support@largefiletransfer.com.
