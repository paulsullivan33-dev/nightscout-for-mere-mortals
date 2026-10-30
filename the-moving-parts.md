# The Moving Parts

By the end of this chapter you can
name every part a reading passes
through, and say which part holds
what — the skill that turns "it
stopped working" into "the uploader
stopped". A reading crosses five
parts to reach your eye:

```text
sensor → transmitter → uploader → server → display
                        (phone)   (yours)   (any browser)
                              \→ database under the server
```

## The sensor and the transmitter

The **sensor** is a small disc worn
on the body with a filament just
under the skin. It measures glucose
in the fluid between cells — not in
blood directly, which is why its
readings trail blood glucose by some
minutes when levels move fast. Most
sensors report about every five
minutes. A **transmitter** (built
into the sensor on some brands,
a separate snap-on part on others)
radios each reading to a phone or a
dedicated receiver nearby. Neither
part knows Nightscout exists, and
this guide does not touch them: they
are the maker’s medical hardware.

## The uploader

The **uploader** is software — usually
an app on the wearer's phone — that
gets each reading out of the maker's
world and posts it to your server. It
speaks to Nightscout's **API** over
ordinary web requests: "here is a
reading, its value, its time, its
trend". To prove it may write, it
presents the site's **API secret** —
or rather its hash, as chapter 4
shows. Some uploaders read the sensor
directly (community apps such as
xDrip+); some fetch from the maker's
cloud minutes later (the **bridges**
of chapter 6). Everything a reader
can do to fix a silent Nightscout
usually happens here, because this is
the part with a battery, a phone
operating system, and opinions about
background apps.

## The server

The **Nightscout server** — the
project's formal name is
**cgm-remote-monitor** — is a Node.js
program. It does three jobs: it
accepts readings and records through
the API, it answers API questions
("the latest reading", "the last six
hours"), and it serves the web pages
that draw them. It is configured
mostly through **environment
variables** — the API secret, the
database address, the port, the
units — which chapter 3 sets. It
keeps almost nothing in files of its
own: its memory of the world is the
database.

## The database

Under the server sits **MongoDB**, a
general-purpose database. Nightscout
keeps separate **collections** (think
drawers) for separate kinds of fact:
`entries` for glucose readings,
`treatments` for the notes of chapter
7 (food, insulin, events),
`devicestatus` for uploader health,
`profile` for the wearer's settings.
When chapter 10 backs the site up, it
is this database being copied — the
server program can be reinstalled any
day, but the history lives here.

## The display

The **display** is a web page the
server serves, drawn fresh from the
API each time it refreshes. It holds
no data of its own: open it on two
screens and both ask the same server
the same questions. Chapter 5 reads
it closely. Other displays exist —
watch faces, widgets, other programs
entirely — and all of them are just
API callers with better tailoring.

## The check

With the server running (chapter 3),
ask it to list what it is:

```bash
curl -s http://127.0.0.1:1337/api/v1/status.json | head -c 200
```

```json
{"status":"ok","name":"nightscout","version":"15.0.9",
 "serverTime":"...","apiEnabled":true, ... }
```

`"apiEnabled":true` means the API
door is open for uploaders;
`"status":"ok"` means the server and,
behind it, the database are both
answering. Two of the five parts
proven in one line — the rest of the
guide is the other three.
