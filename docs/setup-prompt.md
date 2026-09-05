# Setup prompt for Claude Code

Fill in the four values in the first block, then paste the whole thing into
a Claude Code session started in the directory where you keep your projects.

---

I'm starting a new app from the GitHub template `appspan/local-only-pwa-template`. Please walk me through setup, doing everything you can yourself and stopping to tell me exactly what to type whenever a step needs my login or a decision only I can make.

My values:

- App name: `Sunny Days` (the human-readable name shown in the app)
- Short name: `Sunny` (Home Screen label, keep it under 12 characters)
- Repo and Vercel project name: `sunny-days` (lowercase, hyphens)
- Git identity for commits: `Alex Rivers <alex@example.com>` (must be an email verified on my Vercel account)

Work through these in order and check each one before moving on:

1. **Prerequisites.** Confirm `node` (18 or newer), `gh`, and `vercel` are installed and that `gh auth status` shows my account. If `gh` or `vercel` are missing, tell me the install command. If `gh` isn't logged in, tell me to run `gh auth login` myself and wait. Do not enter passwords or tokens for me.

2. **Create my repo from the template.** Run `gh repo create <repo-name> --template appspan/local-only-pwa-template --public --clone`, then `cd` into it and set `git config user.name` and `user.email` to my git identity above, repo-local only.

3. **Read the template's docs first.** Read `README.md`, `docs/ship-runbook.md`, and `template.config.json` before changing anything, and summarize the data promise back to me in two sentences so I know you understood it.

4. **Day-one config.** Update `template.config.json` with my app name, short name, and project names (`vercelProject` = my repo name, `vercelStagingProject` = my repo name plus `-staging`; leave `productionUrl` for now). Then update `public/index.html` (the `<title>`, `og:title`, `apple-mobile-web-app-title`, the `window.APP` block, and the `<h1>`) and both manifests to match. Pick a `storagePrefix` from my repo name. Run `npm test` and show me it passes; the config test is what proves the four files agree.

5. **Icon.** Ask me whether I have an icon idea. If I do, edit `public/icons/icon.svg` and `icon-stage.svg` accordingly and run `./build/make-icons.sh`. If I don't, leave the placeholder and move on.

6. **First real commit.** Commit the config changes as me and push. This matters: GitHub authored the template's initial commit with a noreply address, and Vercel's Hobby plan silently blocks deploys whose HEAD author it doesn't recognize. Confirm with `git log -1 --format='%an <%ae>'` that HEAD is mine.

7. **Local run.** Start `npm run serve`, tell me to open http://localhost:8000, and walk me through what to try: the welcome dialog, tapping the counter and reloading, Settings (gear) with dark mode and the What's new panel, and "Share my count" opened in a private window. Stop the server when I say I'm done.

8. **Vercel login.** Tell me to run `vercel login` myself and wait for me. Then check `vercel whoami`. Remind me that the email on my Vercel account must match my git commit email, or the deploy will hang at "Building…".

9. **First production deploy.** Run `npm run stamp && vercel deploy --prod --yes` from the repo root. Then run `vercel inspect <the deployment URL it printed>` and read the Aliases list. The stable production URL may not be `<repo-name>.vercel.app` if that name is taken. Put the real alias into `productionUrl` in `template.config.json`, commit, and push. Open the URL and confirm the title is my app name and `/build.json` returns the stamp.

10. **Staging project.** Run `DRY_RUN=1 ./build/deploy-staging.sh` and show me the three rebadged lines it prints. Then run `npm run deploy:staging` for real; the first run creates the staging Vercel project. Confirm the staging manifest says "(stage)" and the icon has the STAGE ribbon.

11. **Hand me the summary.** Finish with: the repo URL, the production URL, the staging URL, the commands for the two deploys, and a reminder that every release should add an entry to `public/app-changes.json` in the same commit. Then ask me what the app is actually going to do, and suggest which placeholder cards in `index.html` and which blocks in `app.js` I'll replace first versus which are the machinery to keep.

Throughout: never edit `public/build.json` (generated), never put storage calls anywhere except through `LocalState`, and if anything fails, show me the exact output rather than guessing.
