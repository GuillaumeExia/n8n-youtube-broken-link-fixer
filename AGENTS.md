# Instructions for the agent finishing this repo

This repo publishes one n8n workflow: `workflow.json`. It was not accepted in the n8n template directory, so GitHub is where people will find it. Your job: fill in the README, the GitHub description and the topics. Follow every rule below.

## 1. Source of truth

- The **Overview** sticky note inside `workflow.json` is the reference for what the workflow does. The README expands on it and must never contradict it.
- Don't invent features, numbers or behaviour. If you can't verify a claim from the JSON or an official doc, leave it out.
- Don't change workflow logic. If you have to touch `workflow.json` (for a typo in a sticky note, say), keep the node names and connections as they are, and run the check in section 4 again.
- Write in English. Keep it concise and technical: short sentences, no marketing filler, no emoji.

## 2. README (`README.md`, already outlined)

Fill in each `TODO(agent)` and then delete the comment. Keep the section order.

- **Value proposition:** one sentence, the problem first (dead affiliate links and moved pages cost creators clicks and revenue).
- **Screenshot:** `assets/workflow.png`, a clean full-canvas capture with sticky notes visible. If you can't produce it, keep the image link and tell the user it's missing. Don't delete the link.
- **How it works:** numbered steps that follow the five steps in the Overview note: weekly scan, single check per link (soft 404s, redirects to the homepage), Data Table memory, Slack replace or ignore with a 7-day re-ask, weekly email.
- **Requirements:**
  - The minimum n8n version. Derive it from the Data Table node and from Slack/Email *send and wait*. Check the n8n release notes or docs; don't guess.
  - Credentials: YouTube OAuth2, Slack OAuth2, SMTP.
  - A note on YouTube Data API quota. Write updates cost much more than reads. Take the figures from Google's official quota table, or don't give any.
- **Setup:**
  - Data Table `YouTube Link Issues` with the text columns `url`, `status`, `reason`, `replacement`. List the possible `status` values, read from the workflow: `open`, `verify`, `fixed`, `ignored`.
  - The fields of the **Configuration** node. Read their exact names from the JSON. Include the UC→UU tip for the uploads playlist ID.
  - The workflow must be **published (activated)** because fix requests run as separate executions of the same workflow (Execute Workflow → *When Fix Is Requested*). Say this explicitly. It is the most likely setup mistake.
- **Import:** two options. (a) In n8n, *Import from URL* with the raw GitHub URL of `workflow.json` (`https://raw.githubusercontent.com/<owner>/n8n-youtube-broken-link-fixer/main/workflow.json`; ask the user for the owner if it isn't known). (b) Download the file and import it. Then reconnect the credentials on every node that uses one.
- **Customization:** soft-404 phrases in **Configuration**, broken status codes in **Classify Result**.
- **Limitations:** only the first 100 comment threads per video are read. Translated descriptions keep the old link. Only comments posted by the channel itself are rewritten.
- Keep the MIT licence section and the Cloud Labs footer.

## 3. GitHub metadata

Set these with `gh repo edit` if you have access. Otherwise, give the commands to the user.

- **Description** (under 120 characters), e.g. `n8n workflow: find broken links in YouTube descriptions & comments weekly and fix them from Slack.`
- **Topics:** `n8n`, `n8n-workflow`, `n8n-template`, `youtube`, `youtube-api`, `slack`, `broken-links`, `link-checker`, `human-in-the-loop`, `automation`
- **Homepage:** leave it empty unless the user gives one.
- Turn Issues on. Turn the Wiki and Projects off.

## 4. Sanitization check, before every commit that touches `workflow.json`

The file is clean now. Keep it that way:

- Every `credentials.*.id` is the placeholder `credential-id`. No real credential IDs.
- `pinData` is empty `{}`. There is no `meta.instanceId`.
- The only emails are `@example.com` placeholders. The Slack channel is `C0123456789`. The playlist is `UUxxxxxxxxxxxxxxxxxxxxxx`.
- There are no real URLs except the YouTube API, the `youtu.be`/`youtube.com` patterns, `example.com` and the Cloud Labs branding.
- The `webhookId` values on the Slack and Email send-and-wait nodes are expected. Leave them as they are.

## 5. Distribution (draft only)

Draft a short post for the n8n community forum, in the **Built with n8n** category. It has a title, two or three lines on the problem, the screenshot and the repo link. Give it to the user. **Don't post it.**

## 6. Commits

Use small, conventional commits (`docs: …`, `chore: …`). Don't force-push `main`.
