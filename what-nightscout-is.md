# What Nightscout Is

You have a number that matters — a
glucose reading, refreshed every few
minutes, day and night — and it lives
inside one company's app, on one
person's phone. Anyone who helps look
after that person, a parent, a partner,
a school nurse, either leans over the
phone or does without. The history,
the part a doctor actually reviews,
sits in the same company's cloud, in
the shape that company chose.

**Nightscout is that number, set
free, on a website of your own.** It
is a program you run on a computer you
control. Readings from a **continuous
glucose monitor** (a CGM — a sensor
worn on the body, measuring glucose in
the fluid under the skin) are sent to
your Nightscout server as they happen.
The server stores every one of them
and serves a web page that draws them:
the current value in large type, an
arrow showing which way it is moving,
and a graph of the hours behind it.
Any browser that can reach your server
can be that page — a phone, a work
laptop, a tablet propped in a kitchen,
another phone in another city.

This started, in 2013, with parents of
children with type 1 diabetes who
wanted to see their child's readings
from the next room, and then from
anywhere. The sensor makers' systems
of the time did not allow it. So the
community built the missing piece
itself, under the banner *We Are Not
Waiting*, and has run it ever since.
Nightscout is free and open source,
and the project is a community, not a
company: there is no help desk, which
chapter 12 counts honestly among its
limits.

## A first taste

Once the server is installed
(chapter 3), it introduces itself to
anyone who asks:

```bash
curl -s http://127.0.0.1:1337/api/v1/status.json
```

```json
{"status":"ok","name":"nightscout","version":"15.0.9",
 "apiEnabled":true, ... }
```

That one reply — the server's name,
its version, its settings — is the
check this guide returns to whenever
something seems wrong. If the status
answers, the stack is alive, and the
problem is somewhere specific. Chapter
4 then feeds the server invented
readings through its **API** (the
machine-readable door uploaders use)
and reads them back out, which is the
whole system in miniature.

## What it is not

Nightscout is **not a sensor**: it
reads nothing by itself. Something —
a phone app, a bridge program — must
carry each reading from the sensor's
world into Nightscout, and chapter 6
is about those carriers. It is **not
a medical device**: it is community
software that *displays* readings a
few minutes after the sensor took
them, and it must never be the basis
for a dose or a treatment decision —
that is what the sensor maker's own
app and a care team's advice are for.
It is **not an insulin pump
controller**: systems that close the
loop and dose automatically are
separate projects (chapter 11), and
they merely *use* Nightscout as a
dashboard. And it is not a polished
commercial service: you are the
hosting company, with the duties that
brings — chapters 10 and 12 cover
backups and the rest.

## Who it is for

The parent who wants a child's
overnight readings on their own
bedside screen, with an alarm that
does not depend on a vendor's cloud
being awake. The adult with diabetes
who wants their history in their own
database, in open formats, for as
long as they choose to keep it. The
partner or carer who follows from a
distance and wants the real graph,
not a texted number. The tinkerer
whose home systems — dashboards,
announcements, automations — could
do something useful with a live
glucose value, through an API that
asks nothing proprietary. The common
thread is one wearer, one stream of
readings, and people who would
rather hold that data themselves.

## The honest catch, up front

Glucose data is health data — among
the most personal data there is. A
default Nightscout install lets
anyone who can reach the site *view*
the readings (chapter 7 explains the
split: viewing is open, writing needs
a secret). That openness is deliberate
— a follower should not need an
account at 3 a.m. — but it means
*where* you put the server is a
privacy decision, not a technicality.
On a home network, or behind a login
you control, it is a family tool. On
the open internet with no thought
given, it publishes a person's body
to strangers. This guide installs it
on a home machine first, and chapter
12 returns to the question before
anyone is tempted to expose it.
