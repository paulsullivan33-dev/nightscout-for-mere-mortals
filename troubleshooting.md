# Troubleshooting

The failures this system actually
has, in the order the parts sit in
chapter 2's chain. Several were
produced on purpose in the lab; the
rest are the documented classics.

## The server will not start

**The boot log stalls at storage.**
The boot sequence names its steps
(t02): after `Executing
setupStorage`, a MongoDB that is not
running, or a `MONGO_CONNECTION`
pointing somewhere else, stops the
show there. Check the database
first — its log's `Waiting for
connections` line (t01) — then the
variable, character by character.
MongoDB local, server local, and
the connection string is the one
from chapter 3: that is the whole
triangle.

**`API_SECRET should be at least 12
characters`.** The server's own
words in its log when the passphrase
is too short. Invent a longer one —
the passphrase is typed rarely and
stored in configuration, so length
costs nothing.

**The port is taken.** `Listening
on port 1337` never arrives and an
address-in-use error does: something
else (often yesterday's Nightscout)
owns the port. Stop the other, or
choose another `PORT` — and update
every uploader and bookmark to
match.

## Uploads are refused

**401 `Invalid/Missing` on POST**
(t07). Three causes, in order of
frequency: the uploader has no
secret configured; it has the
*passphrase* where the *SHA-1 hash*
belongs (or the reverse, in clients
that hash for you — chapter 4 shows
which form the API takes directly);
or the server was restarted with a
different secret than the uploader
remembers. `verifyauth` (t09) tests
a credential in isolation: `canWrite:
true` means the credential is right
and the problem is elsewhere.

**401 for one app only.** Its token
or secret was revoked or edited —
subjects and tokens are per-app
(chapter 7); reissue that one, not
the master secret everything else
depends on.

## The graph is stale or frozen

Work the chain from the body
outward. Sensor warm-up or a sensor
session ended (the maker's app
shows this too)? Phone alive, and
the uploader app still permitted to
run in the background — phone
operating systems kill background
apps with enthusiasm, and "battery
optimisation" exemptions are the
documented fix? Network between
phone and server up? Server itself
answering its status check
(chapter 2)? The check that splits
the world in half: post a reading by
hand (chapter 10). If it appears,
the server is innocent and the
uploader chain is guilty; if it
does not, you have a server problem
and the sections above apply.

**Readings frozen at a tiny number**
— 4, 5, 6 — or a flat line along
the bottom: a units mix-up (chapter
5). Something is sending mmol/L
values into an mg/dL world. A
healthy 5.5 mmol/L filed as `sgv: 6`
draws as a permanent emergency that
isn't one; check the newest entry's
raw JSON (chapter 4's GET) and the
`device` field will usually name
the culprit.

**Alarms silent in an open
browser.** Browsers refuse to play
sound for a page nobody has
interacted with. Click or tap the
page once after opening it — the
first-night trap, best sprung in
daylight while testing chapter 8.

## The proxy warning

A site placed behind a reverse
proxy or hosting platform may print
the long `SECURITY:` notice the
lab's boot log carried (t02): with
`TRUST_PROXY` unset, the server's
failed-login throttling keys on
headers a caller can forge, so the
delay does not protect the login.
The notice is precise and worth
obeying literally: set `TRUST_PROXY`
to your proxy's address (or to
`false` when clients connect
directly), or restrict access at
the proxy itself. A warning this
specific, printed by the tool
itself, is a gift — most tools make
you learn it from an incident.
