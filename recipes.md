# Recipes

Short jobs, each complete in itself.
Every recipe here was run by the lab
against the real server except the
two marked as configuration — and
those are labelled where they stand.

## Is it alive, in one line

```bash
curl -s http://127.0.0.1:1337/api/v1/status.json | jq .status
"ok"
```

The whole health check (t03): if
this prints `"ok"`, the server and
its database are answering. Wire it
into any monitoring you already run;
it is the check the rest of the
guide keeps returning to.

## The latest reading, in a terminal

```bash
$ curl -s http://127.0.0.1:1337/api/v1/entries/current.json \
    | jq '.[0] | "\(.sgv) \(.direction) at \(.dateString)"'
"139 SingleDown at 2026-10-30T04:09:01.000Z"
```

The answer the lab's server gave
(t05), in the shape yours will give.
This is the seed of every dashboard,
widget and announcement: one URL,
one small JSON answer, endlessly
reusable.

## Post one reading by hand

The chapter 4 pattern, sized for a
single manual entry — a finger-stick
check you want on the graph, say.
(`mbg` entries are the meter-reading
kind; an `sgv` posted by hand should
say so in its `device` field, as
this one does.)

```bash
NOW_MS=$(( $(date +%s) * 1000 ))
curl -s -X POST http://127.0.0.1:1337/api/v1/entries.json \
  -H "Content-Type: application/json" \
  -H "api-secret: <the sha1 hash>" \
  -d "[{\"type\":\"sgv\",\"sgv\":121,\"direction\":\"Flat\",
        \"date\":$NOW_MS,\"device\":\"posted-by-hand\"}]"
```

The server's reply — the stored
entry, `_id` and all (t04) — is the
receipt that it landed.

## Back it up, and prove the backup

The history is the database, so the
backup is a database dump (t08), run
while the server keeps working:

```bash
mongodump --uri mongodb://127.0.0.1:27017/nightscout --out backup
```

and the proof, a restore rehearsed
without touching the real data:

```bash
$ mongorestore --uri mongodb://127.0.0.1:27017 --dryRun backup/
... dry run completed
```

Schedule the dump the way you
schedule anything precious: nightly,
copied off the machine, and the
dry run often enough that a restore
is a procedure you have practised,
not a hope. (`mongodump` and
`mongorestore` are MongoDB's own
Database Tools — a separate, small
download from the same place as the
server in chapter 3; the lab ran
version 100.10.0.)

## Keep it running (configuration)

The lab started its server in a
terminal, which dies with the
terminal. On your own machine, give
it a service. A systemd unit holding
chapter 3's variables does it:

```ini
[Unit]
Description=Nightscout
After=network.target

[Service]
Environment=API_SECRET=a-long-passphrase-you-invent
Environment=MONGO_CONNECTION=mongodb://127.0.0.1:27017/nightscout
Environment=PORT=1337
Environment=INSECURE_USE_HTTP=true
WorkingDirectory=/home/you/nightscout
ExecStart=/usr/bin/node lib/server/server.js
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

This unit is assembled from the
lab-verified start command; the
lab's sandbox has no systemd to run
it under, so treat it as
configuration to check — `systemctl
status` plus the one-line health
check above — rather than a
transcript. And a warning that
matters: this file holds the API
secret in plain text. Keep its
permissions tight (`root` only), or
move the secret into a root-owned
environment file the unit reads.

## Switch the site to mmol/L (configuration)

For a household that thinks in
mmol/L, one variable at start time:

```bash
DISPLAY_UNITS=mmol
```

The server converts for display; the
stored data stays mg/dL, and chapter
5's factor of 18 is the only
arithmetic involved. The lab ran the
mg/dL default its status reported
(t03); this switch is the project's
documented variable, set the same
way as chapter 3's four.
