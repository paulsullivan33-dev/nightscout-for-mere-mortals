# Alarms and Alerts

By the end of this chapter you know
what makes a Nightscout alarm fire,
what can carry its sound, and exactly
how far to trust it — which is
*partly*, for reasons this chapter
states plainly. The alarm *settings*
below are the ones the lab's server
reported about itself (t03). How the
alarms feel at 3 a.m. is the
documentation's and your own
household's territory; no lab without
a sleeper in it can verify that, and
this one does not pretend to.

## The four lines

Alarms hang on the thresholds of
chapter 5. The status reply showed
each switch and its snooze ladder:

```json
"alarmUrgentLow": true,  "alarmUrgentLowMins": [15,30,45],
"alarmLow": true,        "alarmLowMins": [15,30,45,60],
"alarmHigh": true,       "alarmHighMins": [30,60,90,120],
"alarmUrgentHigh": true, "alarmUrgentHighMins": [30,60,90,120]
```

Four lines, then: **urgent low** and
**low** below the target band, **high**
and **urgent high** above it, each
with its own times. When a reading
crosses a line and *stays* crossed,
the alarm sounds on open displays;
**snoozing** silences it for one of
the ladder's choices — 15 minutes for
a low being treated, say — after
which a still-crossed line sounds
again. The ladders differ by line
because the right patience differs:
a low wants a short leash, a high a
longer one.

The server also enables a prediction
feature (`ar2`, in the boot log's
feature list, t02) that warns when
the recent trend *points at* a line
crossing soon, not only after the
crossing itself. Treat predictions
as a tap on the shoulder, not a
measurement: they extrapolate a few
readings, and readings are noisy.

## Where the sound comes out

An alarm is only an event until
something makes noise. The carriers,
in rough order of reliability for a
sleeping household:

- **An open display** — a browser tab
  left on the site speaks and sounds
  (browsers require one interaction
  before allowing audio; the classic
  first-night discovery, in chapter
  13).
- **Push services** — the server can
  announce to **Pushover** (the boot
  log noted, honestly, `Pushover was
  NOT configured` on the lab's site,
  t02): configured with your Pushover
  keys, alarms become phone
  notifications that can break
  through a phone's quiet hours.
- **Other programs** — anything
  watching the API or the site's
  alarm feed: home-automation systems
  (there is a Home Assistant guide in
  this series), lamps, speakers.
  Documented territory, and powerful,
  and entirely your own wiring.

## How far to trust it

An alarm crosses the whole chain of
chapter 2 before it makes a sound:
sensor, phone, network, server,
display. Any link asleep — a phone
that killed its uploader app, a
router rebooting, a browser tab
frozen — silences the alarm *without
announcing its own failure*; the
only symptom is the graph going
stale (chapter 5), which no one may
be awake to see. The maker's own
receiver and app alarms, running on
the wearer's own devices, do not
have this chain, and they remain the
primary safety net. Nightscout
alarms are the *second* net — the
one that reaches the parent in the
next room or the partner across town
— and households that understand it
that way are the ones it serves
well. Set the thresholds with the
care team, keep the snoozes honest,
and never let a green-looking graph
talk you out of the sensor's own
alarms.
