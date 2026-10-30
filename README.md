# Nightscout for Mere Mortals

A **continuous glucose monitor** — a
**CGM** — is a small sensor worn on the
body. Every few minutes it measures the
glucose in the fluid under the skin and
reports a number. The sensor maker's own
app shows that number on one phone,
usually the wearer's, and keeps the
history inside the maker's cloud.

**Nightscout** is the same data with the
walls taken down. It is a small website
you run yourself. Readings flow into it
from the sensor, and from then on the
number is yours: any browser can show
it, on a phone, a laptop, a kitchen
tablet, a watch — including the browsers
of the family members and carers who
help keep an eye on things, from
wherever they are. It keeps the whole
history, draws the graph, sounds alarms,
and builds the reports that turn a pile
of numbers into patterns a care team can
use. No account with a data company, and
the readings live in a database on a
machine you control.

Nightscout was built by people with
diabetes and parents of children with
diabetes, starting in 2013, under a
rallying cry of *We Are Not Waiting* —
not waiting for the official apps to
allow remote following. It is free,
open source, and run by its community.
This guide explains how it fits
together, installs it from its parts,
and proves the pipeline works before
any real sensor is involved: the lab
behind this guide had no sensor and no
diabetes account, so it fed the server
six invented readings through the same
door a real uploader uses, and read
them back out again.

The lab ran the real thing:
**Nightscout 15.0.9** (the
cgm-remote-monitor project) on
**Node.js 24**, with **MongoDB 8.0**
holding the data, on a small Linux
machine. Against it the lab started
the database and the server, read the
server's own status report — version,
units, alarm thresholds and all — posted
the six invented readings as JSON,
watched the server store them with IDs
of its own, read them back newest-first,
asked for just the latest one, fetched
the web display's page and title,
checked what a caller with the right
secret may do versus a caller with
none, tried to write with a wrong
secret and was refused, and backed the
whole database up and proved the backup
restorable.

What the lab could not do, it did not
fake. It had no CGM, no phone running
an uploader, and no Dexcom or Libre
account, so everything about getting
*real* readings in — the vendor bridges
and uploader apps of chapter 6 — is
taught from the project's documentation
and labelled as such. It also had no
browser, so it verified the display's
page, settings and data feeds rather
than its pixels. `criteria.md` keeps
the full ledger of what was run and
what was only read.

One line belongs here, before any
procedure: Nightscout is a display and
a logbook, **not a medical device**.
It shows readings a few minutes after
the fact, through a chain of parts any
one of which can stall. Treatment
decisions belong with the sensor
maker's own app and the advice of the
wearer's care team — never with a
number on a home-built website,
including this one.

## What this guide covers

- [**What Nightscout is**](what-nightscout-is.md) —
  the mental model: sensor readings in,
  a website of your own out, and why
  who holds the data is the point
- [**The moving parts**](the-moving-parts.md) —
  the sensor and transmitter, the
  uploader, the server, the database,
  the display, and which part holds what
- [**Installing Nightscout**](installing-nightscout.md) —
  Node.js, MongoDB, the server itself,
  and the status check that proves the
  whole stack is alive
- [**Your first entries**](your-first-entries.md) —
  the API secret and its hash, six
  invented readings posted the way an
  uploader posts them, and the same
  readings read back
- [**Reading the display**](reading-the-display.md) —
  the big number, the trend arrows, the
  graph and its target band, the two
  unit systems, and how the page admits
  when its data has gone stale
- [**Getting real data in**](getting-real-data-in.md) —
  the bridges and uploader apps that
  carry a real sensor's readings to
  your server — taught from the
  documentation, and labelled as such
- [**Sharing and following**](sharing-and-following.md) —
  who can see a Nightscout site, the
  read/write split the secret enforces,
  and sharing one person's readings
  with the people who follow them
- [**Alarms and alerts**](alarms-and-alerts.md) —
  the thresholds, the snooze ladders,
  and what an alarm can and cannot
  promise
- [**Reports**](reports.md) — the
  day-to-day and distribution reports
  that turn stored readings into
  patterns worth showing a care team
- [**Recipes**](recipes.md) — the latest
  reading in a terminal, a manual
  reading posted by hand, a status
  check in one line, and a backup that
  is proven before it is trusted
- [**The neighbours**](the-neighbours.md) —
  the sensor makers' own apps, Tidepool,
  xDrip+, and the loop systems: what
  each does that Nightscout doesn't,
  and vice versa
- [**Limits**](limits.md) — no sensor
  reading of its own, minutes of delay
  by design, a database to babysit, and
  health data that deserves more care
  than a default install gives it
- [**Troubleshooting**](troubleshooting.md) —
  the server that will not start, the
  secret that is too short, the 401s,
  the readings frozen in the wrong
  units, and the warning the server
  prints about proxies
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Building or wearing a sensor — Nightscout
starts where the sensor's data leaves
the maker's system. Closed-loop insulin
delivery (systems that dose from the
readings automatically) is a different
project with its own guides; this one
ends at displaying and recording.
Medical advice of any kind: what a
reading means for a particular person
is a question for their care team, and
no chapter here comes near it. And
hosting other people's Nightscout
sites as a service — the security and
privacy duties multiply, and chapter 12
explains why this guide teaches one
site, for one wearer, run by someone
who knows them.

## How to use this guide

Read chapter 1 for the model and
chapter 2 before installing anything —
knowing which part holds what turns
later confusion into a named problem.
Chapter 3 ends with a server that
answers its own status check; chapter 4
proves data can go in and come out
before any sensor enters the story.
Do not skip chapter 4's invented
readings even if your real uploader is
ready: they separate "the server works"
from "the uploader works", and when
something breaks later you will want
to know which half to blame. The lab's
transcripts live in
`nightscout-lab/transcripts/` and its
six sample readings are
`nightscout-lab/entries.json`.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Node.js for Mere Mortals](https://paulsullivan33-dev.github.io/nodejs-for-mere-mortals/)
- [Docker Compose for Mere Mortals](https://paulsullivan33-dev.github.io/docker-compose-for-mere-mortals/)
- [Raspberry Pi for Mere Mortals](https://paulsullivan33-dev.github.io/raspberry-pi-for-mere-mortals/)
- [Home Assistant for Mere Mortals](https://paulsullivan33-dev.github.io/home-assistant-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/nightscout-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/nightscout-for-mere-mortals/blob/main/LICENSE-CODE).
