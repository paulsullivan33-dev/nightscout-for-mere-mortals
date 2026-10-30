# Glossary

The terms this guide uses, in one
place. Definitions are this guide's
own plain versions.

**A1C (HbA1c)** — a blood test
summarising average glucose over
roughly three months; reports
estimate in its direction but are
not it.

**API** — the machine-readable door
to the server: fixed web addresses
that accept and return JSON, used by
uploaders, displays and scripts.

**API secret** — the site's master
passphrase (12 characters or more).
Its SHA-1 hash is what callers send
to prove the right to write.

**Basal / bolus** — background
insulin, and mealtime/correction
insulin. They appear in treatments
and reports; this guide records
them, and never advises on them.

**Bridge** — a program (one is built
into Nightscout) that fetches
readings from a sensor maker's cloud
and posts them to your server —
carrying readings across the gap
between two systems.

**Care portal** — the site's form
for recording treatments and events
alongside the glucose graph.

**CGM (continuous glucose
monitor)** — a worn sensor measuring
glucose in the fluid under the skin,
reporting every few minutes.

**Collection** — one of MongoDB's
drawers of like records: `entries`,
`treatments`, `devicestatus`,
`profile`.

**Delta** — the change between the
latest reading and the one before
it, shown beside the big number.

**Direction** — the named trend a
reading carries: `Flat`, `SingleUp`,
`DoubleDown` and kin, drawn as
arrows.

**Entries** — the readings
themselves; also the API address
(`/api/v1/entries.json`) they live
at.

**Environment variable** — a named
value handed to a program at start;
Nightscout's whole configuration
travels this way.

**Epoch milliseconds** — the count
of milliseconds since 1 January
1970, UTC: the form entries' `date`
field takes.

**Loop (closed loop)** — a system
that doses insulin automatically
from sensor readings; a neighbour
(chapter 11), not this guide's
subject.

**mg/dL and mmol/L** — the two
glucose unit systems; multiply
mmol/L by 18 to get mg/dL.

**MongoDB** — the database under
the server; where the history
actually lives.

**sgv** — a sensor glucose value:
the `type` of an entry, and the name
of its value field.

**SHA-1 hash** — a fixed-length
fingerprint of a text; the server
compares hashes so the passphrase
itself never crosses the wire.

**Stale** — the display's state when
the latest reading is too old to be
called current; drawn dimmed, and
the first sign an uploader stopped.

**Subject / token** — a named,
revocable credential with a role
(readable, careportal, admin),
given to one follower or app instead
of the master secret.

**Time in range** — the share of a
period spent inside the target band;
the headline figure of the reports.

**Treatment** — a recorded event —
meal, insulin, exercise, sensor
change — stored beside the readings
and drawn on the graph.

**Trend arrow** — the arrow drawn
from a reading's direction; chapter
5 has the full table.

**Uploader** — software that gets
readings out of the sensor's world
and POSTs them to the server: phone
apps, bridges, loop systems.
