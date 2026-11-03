---
name: clip-a-video
description: Turn one of the user's long videos into short captioned clips with Reeltopus, export the best ones and optionally schedule posts. Use when the user asks to clip, cut, shorten or repurpose a video into shorts, reels or TikToks.
---

# Clip a video with Reeltopus

1. Call `get_account` and tell the user how many hours they have; processing spends the video's length.
2. Find the video: `list_projects` for one already in Reeltopus. For a file on this computer, `start_upload`
   (filename, sizeBytes, mimeType), PUT each part with `curl -sS -X PUT --upload-file <part> -D - '<url>'`,
   keep each ETag, then `complete_upload`. Otherwise `get_upload_link` and give the user the link to upload it at
   reeltopus.com. Only clip videos the user owns or has the rights to.
3. Poll `get_status` every 30-60 seconds until the status is `ready`.
4. `get_clips` returns clips ranked best first. Summarise the top three (title, hook, length) and ask which to keep.
5. Apply the user's changes with `edit_clip` (caption style, trim, silence removal, title, hook).
6. `export_clip`, then poll `get_export` until `ready` and share the MP4 link (it expires after an hour).
7. Only if the user asks to post: `list_connections`, confirm account, title, visibility and time, then `schedule_post`.
   To pick the best clip and schedule it in one go, use the `clip-and-schedule` skill instead.
