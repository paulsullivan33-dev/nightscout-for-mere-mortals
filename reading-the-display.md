# Reading the Display

By the end of this chapter you can
look at a Nightscout page and say what
every element means — and, just as
importantly, tell at a glance whether
the number in front of you is fresh
or a leftover. The lab verified this
page's machinery (its settings, its
data feeds, the page itself); the
reading of its face comes from the
project's documentation and the
settings the server itself reported.

## The big number and the arrow

The centre of the page is the latest
reading, large: its value, and beside
it a **trend arrow** — the direction
the sensor reported with it. The
arrows are a fixed vocabulary (the
server's own direction table, read
from the installed code):

| Direction | Arrow | Roughly means |
|---|---|---|
| DoubleUp | ⇈ | rising fast |
| SingleUp | ↑ | rising |
| FortyFiveUp | ↗ | rising slowly |
| Flat | → | steady |
| FortyFiveDown | ↘ | falling slowly |
| SingleDown | ↓ | falling |
| DoubleDown | ⇊ | falling fast |

Two more symbols matter: `-` when the
trend could not be computed (a new
sensor warming up, a gap in the data),
and the **delta** shown near the
number — how much the value changed
since the previous reading. An arrow
is a five-minute story, not a
prediction; chapter 8's alarms treat
it with the respect, and the
suspicion, it deserves.

## The graph and its band

Behind and below the number, the
graph draws the last hours as a line
of dots, one per reading. Across it
runs a **target band**: the range the
wearer's care team aims for. The
server's own status reported the
factory defaults (t03):

```json
"thresholds": {"bgHigh": 260, "bgTargetTop": 180,
               "bgTargetBottom": 80, "bgLow": 55}
```

Readings inside 80–180 draw in the
calm colour; above and below draw in
warning colours, deepening outside
260 and 55. These are **settings, not
medical facts**: a care team sets the
band for a person, and Nightscout's
defaults are only where an
unconfigured site begins. Change them
in the site's settings (the same
names appear there) and the status
check of chapter 2 shows the new
values back at you — the guide's way
of confirming a setting took.

## The two unit systems

Glucose is measured two ways in the
world: **mg/dL** (the United States
and some others) and **mmol/L** (most
everywhere else). They differ by a
factor of 18: mmol/L × 18 = mg/dL.
So 180 mg/dL is 10.0 mmol/L; 80 is
about 4.4; 55 is about 3.0.

The site has one display unit —
`units` in the status reply, `mg/dl`
by default, set at install time by
the `DISPLAY_UNITS` variable — and
every number on the page uses it.
Data inside is stored as mg/dL
regardless; uploaders send mg/dL.
The trap this sets is chapter 13's:
a value that is perfectly sane in
mmol/L, posted as if it were mg/dL,
draws as a flat line near the bottom
of the graph. Learn the two scales'
shapes — triple digits are mg/dL,
single digits with a decimal point
are mmol/L — and you will spot it
in one look.

## Fresh, or a leftover?

A CGM reports about every five
minutes, so a healthy site is never
more than a few minutes behind. The
page says so honestly: as the latest
reading ages, the display dims and
greys the number, and past a threshold
it treats the data as **stale** —
the graph stops pretending to be the
present. This is the single most
important reading skill on the page.
A bold, bright number is *now*; a
grey one is *the last anyone heard*.
When the graph goes stale, a part of
chapter 2's chain has stopped —
usually the uploader's phone — and
chapter 13 works that chain in order.

## The check

Your settings, in the server's own
words, any time:

```bash
curl -s http://127.0.0.1:1337/api/v1/status.json \
  | jq '.settings | {units, thresholds}'
```

What comes back is what the display
is using — if the page and this reply
ever disagree, believe the reply and
restart the browser tab, not the
server.
