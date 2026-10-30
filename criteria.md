# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's readings are six invented numbers on a smooth arc.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 12 names what the tool
   cannot do: it reads no sensor by itself, it runs minutes
   behind by design, it is community software with no medical
   approval and no help desk, its default posture lets anyone
   who reaches it read the data, and its history is only as
   complete as what uploaders delivered.
8. **The safety rule is in the text, early.** Nightscout handles
   health data and sits next to medical decisions, so the
   README and chapter 1 state its line before any procedure
   depends on it — it is a display and a logbook, not a medical
   device; treatment decisions belong with the sensor maker's
   app and the wearer's care team — and the privacy consequence
   of its open-by-default reading is stated in the same place.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a 2-CPU, 7.7 GB Ubuntu 24.04 sandbox,
shared with other work while the lab ran. It has **no
container runtime it may use, no browser, no CGM hardware and
no sensor-maker accounts**, and those four facts shape what
could be verified. MongoDB **8.0.4** was installed from the
official tarball `mongodb-linux-x86_64-ubuntu2404-8.0.4.tgz`
(the first version tried, a 7.0.14 file, does not exist for
Ubuntu 24.04 and its download was refused by the mirror — the
8.0.4 tarball is what ran). Nightscout **15.0.9**
(cgm-remote-monitor, cloned from the project's repository) ran
unmodified on the machine's Node.js **24.22.1**, installed
with `npm ci` from its lockfile, against that MongoDB on the
same machine. MongoDB's Database Tools **100.10.0** provided
the backup check. MongoDB was run with a deliberately small
cache (0.25 GB) out of politeness to the shared machine; no
guide claim depends on it. All transcripts live in
`nightscout-lab/transcripts/`; the six invented readings are
`nightscout-lab/entries.json`.

Run for real, with transcripts as evidence:

- Versions of every piece (node 24.22.1, npm 10.9.4, mongod
  8.0.4, Nightscout 15.0.9), and the project's stated engine
  requirement (node >= 20) read from its own package.json
  (t00).
- MongoDB started from the tarball with its own dbpath; the
  `Waiting for connections` line on port 27017 in its log,
  and a driver ping answering `{ ok: 1 }` (t01).
- The full server boot: `API_SECRET has 135 bits of entropy`,
  `Successfully established connection to MongoDB`, the
  feature list, the SECURITY notice about TRUST_PROXY quoted
  in chapter 13, and `Listening on port 1337` (t02).
- The status endpoint: `status: ok`, version, `apiEnabled:
  true`, `careportalEnabled: true`, units `mg/dl`, and the
  default thresholds — target 80–180, bgLow 55, bgHigh 260 —
  plus all four alarm switches and their snooze ladders
  (t03).
- Six invented entries POSTed to `/api/v1/entries.json` with
  the `api-secret` header carrying the SHA-1 hash of the lab
  passphrase: the server returned all six stored, each with
  its own `_id` and server timestamps (t04).
- The same entries read back newest-first with `count=6`,
  and the latest alone from `/api/v1/entries/current.json` —
  139, SingleDown (t05).
- The web display as a browser receives it: HTTP 200, a
  43,011-byte page titled `Nightscout` (t06).
- Refusals: a POST with no secret and a POST with a wrong
  secret both answered HTTP 401 `Invalid/Missing`, and stored
  nothing (t07).
- A full `mongodump` of the lab database and a
  `mongorestore --dryRun` over the dump, completed cleanly
  (t08).
- `verifyauth` both ways: with the hash, `canWrite: true,
  isAdmin: true`; with no credential, `canRead: true,
  canWrite: false` — the read/write split of chapter 7 (t09).
- The trend-arrow vocabulary in chapter 5 was read from the
  direction table in the installed server's own source
  (`lib/plugins/direction.js`), as was the 12-character
  minimum for API_SECRET (`lib/constants.json`).

Documented only — described from the project's documentation,
not run, and the text does not claim otherwise:

- **All real data routes of chapter 6** — the Dexcom Share
  bridge, xDrip+ and the iOS uploaders, LibreLinkUp bridges,
  Loop and AAPS uploading. They need a real sensor, phone
  apps, and vendor accounts the lab does not have. Only their
  destination (the entries API of chapter 4) is verified.
- The display as a *rendered page*: chapter 5's reading of
  the graph, colours and stale greying rests on the verified
  settings and page shell plus the documentation; the lab
  cannot screenshot or see pixels.
- Alarms as experienced — sound, browser audio rules in
  practice, Pushover delivery (the boot log verified only
  that the feature exists and was unconfigured), and any
  home-automation reaction to them (chapter 8).
- The reports of chapter 9 as rendered pages; the data they
  summarise is the verified entries store, no more.
- Subject and token creation through the site's admin pages
  (chapter 7); the permission model underneath them is what
  t09 verified.
- The systemd unit and the `DISPLAY_UNITS=mmol` recipe in
  chapter 10: assembled from the lab-verified start command
  and the project's documented variables; the sandbox has no
  systemd to run the unit under, and the lab site ran the
  mg/dL default.
- Docker, Docker Compose, cloud and hosted installs: named
  in chapter 3 as the routes many people take, not taught —
  the sandbox cannot start containers.
- The neighbour tools of chapter 11 (Dexcom Clarity,
  LibreView, Tidepool, xDrip+, Loop, AAPS) — characterised by
  role, not run or benchmarked.

Fixtures are this guide's own and deliberately generic: six
invented glucose values (112–145 mg/dL) on a smooth five-minute
arc, a device label of `lab-fixture`, and an obviously fake
lab passphrase. No real person's readings, accounts, machines
or organisations appear anywhere in the guide or its lab.
