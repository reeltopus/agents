---
name: clip-and-schedule
description: Clip the user's latest video with Reeltopus and schedule its best clip to a connected social account. Use when the user asks to "clip my latest video and schedule the best one", to post the best moment of a video, or to turn a recording into a scheduled short, reel or TikTok.
argument-hint: "[video file or name] [platform] [when]"
---

# Clip the latest video and schedule the best clip

Arguments, all optional, in any order: a video (a file path, or the name of a video already in Reeltopus), a
platform (youtube, tiktok, instagram, linkedin, …) and a time ("tomorrow 9am", "friday 6pm", "now"): $ARGUMENTS

Use the `reeltopus` MCP tools. If they aren't available, tell the user to run `/mcp` and authenticate Reeltopus
(or add it: `claude mcp add --transport http reeltopus https://reeltopus.com/api/mcp`), then stop.

1. **Hours**: `get_account`. Say how many hours are left; processing spends the video's length.
2. **The video**, in this order:
   - A file path in the arguments: upload it (step 3).
   - A name in the arguments: find it with `list_projects`.
   - Nothing given: the newest project from `list_projects`, if it was created in the last 7 days. Otherwise
     look for the newest video file on this computer
     (`ls -t ~/Movies ~/Desktop ~/Downloads 2>/dev/null | grep -iE '\.(mp4|mov|m4v|mkv|webm)$' | head -3`), and
     ask the user which one to clip, or whether to use the newest Reeltopus project after all.
3. **Upload a file** (only after the user named or confirmed it):
   - Only upload videos the user owns or has the rights to clip; ask if unsure.
   - `start_upload` with `filename`, `sizeBytes` (`stat -f%z` on macOS, `stat -c%s` on Linux) and `mimeType`
     (`video/mp4`, `video/quicktime` for .mov, …). It returns `partSize` and a URL per part.
   - Split the file with `split -b <partSize> <file> part_` and PUT each part in order:
     `curl -sS -X PUT --upload-file part_aa -D - '<url>' | grep -i etag`. Keep every ETag with its part number.
   - `complete_upload` with every `partNumber` and `etag`. Delete the `part_*` files.
4. **Wait**: `get_status` every 30-60 seconds until `ready`. A 30-minute video takes a few minutes. If it
   `failed`, show the error (its hours were refunded) and stop.
5. **Pick the clip**: `get_clips`. Show the top three (rank, title, hook, length). Take rank 1 unless the user
   picks another.
6. **Pick the account**: `list_connections`. Use the platform from the arguments; ask if several accounts fit or
   none does (accounts are connected in the Reeltopus app, Settings → Connections). Skip expired ones.
7. **Packaging**: `get_packaging_variants` with the `clipId` and `connectionId`; use its `pick`. Export it:
   `export_clip` for variant A, `export_variant` for a challenger. Poll `get_export` (A) or
   `get_packaging_variants` every 15-30 seconds until the export is ready.
8. **Confirm**: show the clip title, account, post title, description, visibility and time (the argument, or
   the next sensible slot such as tomorrow 9am in the user's time zone) and wait for an explicit yes. On TikTok,
   ask who can watch.
9. **Schedule**: `schedule_post` with `scheduledAt` in ISO 8601 (and `variantId` when the pick isn't A), then
   `get_post`. Report the time it goes out and that `cancel_post` can stop it.

Never post without the user's yes in step 8.
