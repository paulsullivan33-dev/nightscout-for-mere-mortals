# Your First Entries

By the end of this chapter the server
holds six glucose readings — invented
ones, posted through the real API with
the real authentication — and you have
read them back three ways. This is the
whole system proven without a sensor:
if these six behave, any failure later
belongs to the uploader chain of
chapter 6, not to your server.

## The secret, and its hash

Writers authenticate with the
**API secret** from chapter 3 — but
not by sending the passphrase itself.
They send its **SHA-1 hash**: a
fixed-length fingerprint of the
passphrase, computed like this (t04):

```bash
$ printf %s 'your-passphrase' | sha1sum
<40 hexadecimal characters>  -
```

Uploaders and scripts put that hash in
an `api-secret` header. The server
hashes the passphrase it was started
with and compares hashes, so the
passphrase itself never crosses the
wire. Keep both safe anyway: the hash
alone is enough to write to your
site, as the next step shows.

## The shape of a reading

A glucose reading — an **entry** — is
a small JSON object:

```json
{
  "type": "sgv",
  "sgv": 139,
  "direction": "SingleDown",
  "date": 1793333341638,
  "dateString": "2026-10-30T04:09:01.000Z",
  "device": "lab-fixture"
}
```

- `type: "sgv"` — a sensor glucose
  value (the only kind this guide
  posts).
- `sgv` — the value itself, in
  **mg/dL** (the unit chapter 5
  explains; much of the world uses
  mmol/L instead, and mixing them is
  chapter 13's favourite bug).
- `direction` — the trend, one of the
  named arrows of chapter 5: `Flat`,
  `SingleUp`, `SingleDown`, and kin.
- `date` — when the sensor took it,
  as **epoch milliseconds**; plus the
  same moment written out in
  `dateString` for humans.
- `device` — a label for the source;
  uploaders put their app name here.

The lab's six invented readings live
in this repository as
`nightscout-lab/entries.json`: values
112, 118, 126, 138, 145, 139, five
minutes apart, rising gently and
tipping down at the end. Their dates
are from the lab's run, so regenerate
the shape with fresh dates when you
try it — the numbers may stay; only
the clock must be yours. (Readings in
the far past or future are accepted
but draw a strange graph.)

## Posting them

```bash
curl -s -X POST http://127.0.0.1:1337/api/v1/entries.json \
  -H "Content-Type: application/json" \
  -H "api-secret: <the sha1 hash>" \
  -d @entries.json
```

The server answers with the stored
entries (t04) — all six, each now
carrying server-added fields: an
`_id`, and `srvCreated` / `srvModified`
timestamps. That answer is the receipt:
what came back is what the database
holds.

Leave the header off, or hash the
wrong passphrase, and the same request
fails (t07):

```json
{"status":401,"message":"Unauthorized","description":"Invalid/Missing"}
```

HTTP **401** — refused, and nothing
is stored. Try it once on purpose:
knowing this exact refusal saves a
long confusion the first time an
uploader's secret is mistyped.

There is also a polite way to check a
secret (t09):

```bash
curl -s http://127.0.0.1:1337/api/v1/verifyauth.json \
  -H "api-secret: <the sha1 hash>"
```

With the right hash it reports
`"canWrite":true,"isAdmin":true`;
without any header, the same address
reports `"canWrite":false` — and,
worth noticing, `"canRead":true`.
Reading is open by default; writing
is what the secret guards. Chapter 7
builds on that split.

## Reading them back

Newest first, the way the display
asks (t05):

```bash
$ curl -s "http://127.0.0.1:1337/api/v1/entries.json?count=6"
[ {"sgv":139,"direction":"SingleDown", ...},
  {"sgv":145,"direction":"Flat", ...},
  {"sgv":138,"direction":"SingleUp", ...}, ... ]
```

And just the latest, the endpoint
every dashboard and widget uses:

```bash
$ curl -s http://127.0.0.1:1337/api/v1/entries/current.json
[ {"sgv":139,"direction":"SingleDown", ...} ]
```

Finally, open `http://127.0.0.1:1337/`
in a browser. The lab had none, so it
verified what a browser is handed —
HTTP 200, a 43 KB page titled
`Nightscout` (t06) — but on your
machine the six invented readings
draw as a small rising line with a
139 and a downward arrow at its head.
The pipeline — post, store, serve,
draw — is proven end to end. What
remains is learning to read the page
properly (next chapter) and feeding
it real readings (chapter 6).
