# LargeFileTransfer for Claude

Send files and folders from your computer with [LargeFileTransfer](https://largefiletransfer.com) and get a share link back, without leaving Claude. Ask something like *"send the renders folder with LargeFileTransfer and give me the link"*, and Claude uploads it and replies with a link anyone can download from.

The plugin runs a small server on your computer, which is what lets it read the files you name. It works in Claude Code, and in Cowork when the session runs on your computer. Chat on claude.ai, in the desktop app and in the mobile apps skips it: to use LargeFileTransfer there, add `https://api.largefiletransfer.com/mcp` as a custom connector instead. That one can list and look up your transfers, but can't send files.

## Install

In Claude Code:

```
/plugin install largefiletransfer --marketplace CuriousObjectsHQ/largefiletransfer-claude-plugin
```

## Tools

- **send_files**: uploads files or whole folders (hidden files inside folders are skipped), with an optional title, message and expiry, and returns the share link. An upload that takes longer than about 45 seconds keeps going in the background, and the link works once it finishes.
- **upload_status**: how an upload started in this session is going.
- **list_transfers**: your most recent transfers and their links.

## Requirements

- Node.js 20 or later.
- A LargeFileTransfer account. Your plan's limits apply, such as file size, storage, transfers per month and how long links last.

## Connecting your account

The first time Claude uses one of the tools, your browser opens largefiletransfer.com to approve this computer, showing a short code to check against the one Claude gives you. Approve it and ask Claude again. It stays connected after that.

To disconnect, go to largefiletransfer.com → Settings → Connected apps, or delete the credentials file below.

## What this plugin runs, sends and stores

- **Runs** `npx -y largefiletransfer-mcp@0.1.0`: npx downloads that exact version of the [largefiletransfer-mcp](https://www.npmjs.com/package/largefiletransfer-mcp) package and its dependencies from the npm registry, and runs it on your computer with Node.js. Curious Objects, the company behind LargeFileTransfer, publishes it.
- **Reads** only the files and folders Claude passes to `send_files`.
- **Sends** to LargeFileTransfer:
  - the contents of those files, uploaded to LargeFileTransfer storage (`s3.largefiletransfer.com`) through short-lived signed links;
  - their names, sizes and types, and the title, message and expiry, to the LargeFileTransfer API (`api.largefiletransfer.com`);
  - while connecting, a label made of the app's name and your computer's name (for example "Claude Code on Studio"), which you see on the approval page and under Connected apps.
- **Fetches** your transfers and upload progress from `api.largefiletransfer.com`.
- **Opens** your default browser at largefiletransfer.com, once, to approve the connection.
- **Stores** the connection's tokens, with your account's ID, name and email, in `~/.config/lft-mcp/credentials.json`, with permissions that let only your user account read it.

It contacts nothing else and sends no analytics.

Anyone with a share link can download its files until the link expires. How LargeFileTransfer handles what you send is covered by its [Privacy Policy](https://largefiletransfer.com/legal/privacy) and [Terms of Service](https://largefiletransfer.com/legal/terms).

## Support

[largefiletransfer.com/support](https://largefiletransfer.com/support), or email support@largefiletransfer.com.
