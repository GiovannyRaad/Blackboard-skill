# Blackboard Skill

An agent skill that lets your AI agent read your Blackboard account, so you can ask things like
*"what's due this week?"* or *"did anything get posted?"* and get answers from live data.

It reads two things:

- **Calendar**: assignments with a due date.
- **Activity stream**: announcements, uploaded files, and exam or quiz notices from the homepage.

Both matter. Instructors often announce exams in the stream without ever setting a due date,
so the skill tells the agent to check both when you ask what's coming up.

> Built for LAU's Blackboard. The login goes through LAU's SAML page, so other universities
> would need changes to `cookieFetcher.py`.

## Install

Clone it into your agent's skills folder. For Claude Code:

```bash
git clone https://github.com/GiovannyRaad/Blackboard-skill.git ~/.claude/skills/blackboard
cd ~/.claude/skills/blackboard
pip install -r requirements.txt
playwright install firefox
```

Then copy `.env.example` to `.env` and fill it in:

```
UNI_USER=your-username
UNI_PASS=your-password
TIMEZONE=Asia/Beirut
```

Your credentials stay on your machine. `.env` and the cached session (`cookies.json`) are gitignored.

## Use

Just ask your agent about your coursework. It reads [`SKILL.md`](SKILL.md) to decide which script to run.

You can also run the scripts yourself. Each one prints JSON:

```bash
python eventsFetcher.py              # upcoming deadlines
python streamFetcher.py              # stream posts from the last 7 days
python streamFetcher.py --days 30    # wider window
```

The first run logs in with a headless browser, which takes about 40 seconds. After that the session
is cached, and a run takes a few seconds.

## Files

| File | Does |
|---|---|
| `SKILL.md` | Instructions for the agent |
| `eventsFetcher.py` | Reads the calendar |
| `streamFetcher.py` | Reads the activity stream |
| `cookieFetcher.py` | Logs in and caches the session |

## History

This started as [Blackboard-Deadline-Automation](https://github.com/GiovannyRaad/Blackboard-Deadline-Automation),
a script that sent my deadlines to WhatsApp on a schedule. Turning it into a skill made it more useful:
instead of getting whatever the script decided to send, I can ask about anything.

## License

[MIT](LICENSE)
