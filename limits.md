# Limits

What Nightscout cannot do, stated
plainly — the chapter to read before
the site becomes load-bearing in a
household. Every item here was
visible from the lab's run or follows
directly from the design chapters 1
and 2 laid out.

## It reads nothing by itself

The server is a mailbox, not a
sensor. Stop the uploader — a phone
reboots, an app is swiped away, a
bridge password expires — and the
site does not degrade loudly; it
just shows the last reading, greying
(chapter 5), indefinitely. The
system's most common failure is
silence in a part Nightscout cannot
see, and no setting inside
Nightscout fixes it.

## It is minutes behind, twice over

Interstitial glucose trails blood
glucose when levels move fast, and
uploads add their own delay on top:
a reading can be five to fifteen
minutes old by the time a follower
sees it. That is fine for patterns,
company and most alarms. It is one
more reason — with chapter 1's — that
treatment decisions belong to the
wearer's own devices and care team,
never to this display.

## It is not a medical device, and nobody's help desk

Nightscout is community software,
given away, maintained by
volunteers. It has not been through
a regulator's approval as a medical
display, its support channel is a
community forum, and when it breaks
at 3 a.m. the on-call engineer is
whoever installed it. Households
lean on it heavily and happily — but
with the maker's own alarms kept
primary (chapter 8), which is the
arrangement this limit demands.

## Its default posture is open

Reads are public to anyone who can
reach the site (chapter 7, verified
in t09). That is a considered
default, and it makes the installer
the privacy officer for some of the
most personal data a person
generates. Exposing a default
install to the open internet is
publishing a health record; a home
network, a VPN, or a login at the
door are not optional polish for an
internet-facing site, they are the
deployment.

## You are the hosting company

MongoDB needs disk, backups
(chapter 10) and the occasional
upgrade; Node.js ages; the server
project releases fixes you must
apply; community bridges depend on
makers' clouds that change without
asking. None of this is heavy — the
lab's instance is a few hundred
megabytes and one small process —
but it is *forever*, or as long as
the site matters. A lapsed domain, a
full disk, a dead home server: each
is a gap in a health record and a
silent night for followers.

## Its numbers are only its numbers

The reports of chapter 9 summarise
what arrived. Missed days, a sensor
in its inaccurate first hours, a
uploader that backfilled wrongly —
the graphs do not flag any of it,
and estimated figures (averages,
A1C-style summaries) are arithmetic
on that data, not laboratory
measurements. Quote them to a care
team with the coverage attached, or
not at all.
