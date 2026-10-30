# The Neighbours

By the end of this chapter you can
place Nightscout among the tools
around it — what each neighbour does
that it doesn't, and the one thing
it does that none of them do. These
tools are characterised from their
documentation, not run by the lab;
the descriptions are of roles, and
roles stay true longer than version
numbers.

## The sensor makers' own apps

Every CGM arrives with an official
world: the phone app that displays
readings and sounds the primary
alarms, and the maker's cloud
(Dexcom Clarity, Abbott LibreView
and their kin) that stores the
history and produces the clinical
reports. **What they do that
Nightscout doesn't:** they are the
supported, regulated path — the
app is part of the medical device
system, its alarms are the ones
chapter 8 calls the primary safety
net, and its reports are the ones
many clinics load directly.
**What Nightscout does that they
don't:** it shows the data to
anyone the wearer chooses, on any
screen, from a database the wearer
owns — the makers' sharing features
are narrower, account-bound, and
theirs to change or withdraw.

## Tidepool

A nonprofit data platform that takes
uploads from many pumps and sensors
and builds clinician-grade reports,
donating its work back to the
community. **Tidepool's strength**
is the appointment: standardised,
shareable-with-the-clinic views of
combined pump-and-glucose data.
**Nightscout's difference** is
tense: Tidepool is mostly about the
record after the fact, Nightscout
about the number *right now* and
who can watch it live. Many
households run both, and the data
flows to each by different roads.

## xDrip+

A community Android app that reads
sensors directly and displays them
richly — arguably the most capable
glucose *app* there is, with its own
alarms, predictions and watch
support. **What it does that
Nightscout doesn't:** it *is* an
uploader and a display in one,
living on the wearer's phone.
**What Nightscout adds:** the copy
on your own server — the shareable,
backable-up, API-open record that
outlives any phone. Chapter 6's
classic pairing (xDrip+ reads,
Nightscout holds) exists precisely
because the two are complements,
not rivals.

## The loop systems (Loop, AAPS, and kin)

Community closed-loop systems read
the same sensor data and *act* on
it, adjusting insulin delivery
through a pump. They are medical
software in the fullest sense, with
their own communities, guides and
safety cultures, and they are far
outside this guide's scope. Their
place in this chapter is one line:
most of them report to Nightscout
as their dashboard, which is why a
Nightscout graph may show doses,
predictions and pump events far
beyond this guide's chapters. If
yours does, the loop's own
documentation is the guide to
reading it.

## Everything with an API-shaped hole

Home-automation platforms, watch
faces, desk clocks, announcement
systems, a relative's photo frame
with a number in the corner — the
long tail of *displays that are not
Nightscout* all drink from the same
well: the entries API of chapter 4.
That openness is the deepest
difference between Nightscout and
its neighbours. The makers' systems
end at their own apps. Nightscout
ends wherever you can write a line
like chapter 10's latest-reading
recipe — which is to say, it
doesn't.
