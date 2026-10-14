---
name: lovelyauth
description: Install and connect the LovelyAuth CLI, list vault accounts, and retrieve TOTP verification codes for authorized sign-ins. Use when asked to set up LovelyAuth or get an MFA or 2FA code from a LovelyAuth vault.
---

# LovelyAuth

Use the public npm package `lovelyauth` and its `lovelyauth` command. The CLI connects to `https://app.lovelyauth.com` without API configuration. An internet connection is required.

## Install and connect

When asked to set up LovelyAuth, carry out installation and login; do not stop at giving the user commands.

1. Check `node --version` and whether `lovelyauth --version` succeeds. Node.js 24 or newer is required. If needed, install or select a compatible Node version using the environment's existing tooling. If that cannot be done, explain the prerequisite that blocks setup.
2. If the CLI is missing, install it globally:

   ```sh
   npm install -g lovelyauth
   lovelyauth --version
   ```

   If global installation is unavailable, explain the failure and offer `npx lovelyauth` as a fallback, replacing `lovelyauth` with `npx lovelyauth` in subsequent commands. Do not report a global installation as complete when using the fallback.
3. Run `lovelyauth list` to check for an existing usable session. If sign-in is required, start:

   ```sh
   lovelyauth login device
   ```

   Keep the process running using a resumable terminal session. Read its output and promptly show the user the exact approval URL and device code. Ask them to open the link, sign in, and approve this CLI. They can use a browser on another computer or phone. The CLI creates the device code; the user does not need to supply one in advance.
4. Resume checking the same process while approval is pending. Do not claim success until the command confirms connection. If it expires, start a new device login when the user is ready and share the new link and code. Do not repeatedly create approval requests.
5. Run `lovelyauth list` after connection. Confirm setup and use the returned slugs for later code requests. An empty vault still means setup succeeded; direct the user to `https://app.lovelyauth.com` to add accounts.

If the user prefers email login, run `lovelyauth login email <email>` in an interactive terminal and enter the user-provided emailed code at its hidden prompt. This login code is separate from a vault account's TOTP code. Successful login is saved for subsequent commands; no browser approval is needed for every retrieval.

## Retrieve a code

Use the requested account's slug from `lovelyauth list`. If the account is ambiguous, ask which one to use.

```sh
lovelyauth get <slug>
```

Check the exit status before consuming output. On success, the first line is the verification code and the second is its remaining lifetime. Preserve the code as a string, including leading zeros. Use it promptly in the authorized sign-in.

Use `lovelyauth get <slug> --fresh` when a new code period is needed; it waits for the current period to end before returning. Ordinary `get` prints once and exits, making it suitable for agents. Avoid `view` and `--watch` for automated retrieval because they can remain interactive. There is no JSON output flag; `--silent` does not make `get` code-only.

If the session has expired, reconnect. If the slug is missing, list accounts again. If a code expires before submission, retrieve a new one.

## Handle access carefully

- Keep vault codes, setup keys, and session tokens out of persistent logs, files, commits, and routine status messages. Deliver a code directly only when the user asks for it; otherwise use it in the requested sign-in.
- Let the CLI manage `~/.lovelyauth/session.json`; do not read or copy its token to authenticate manually.
- Setup and code retrieval do not authorize vault changes. Run add, edit, delete, restore, or logout only when requested. `lovelyauth empty-trash` immediately and permanently removes every trashed account without confirmation; use it only with explicit authorization for that deletion.

For command syntax, run `lovelyauth help`. Public guide: https://lovelyauth.com/cli.
