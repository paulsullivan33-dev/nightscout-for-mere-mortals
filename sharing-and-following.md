# Sharing and Following

By the end of this chapter you know
exactly who can see a Nightscout
site, who can change it, and how to
share a wearer's readings with the
specific people who follow them —
without handing everyone the master
key. The permission split at the
heart of it is one the lab verified
against the live server; the
account-creating screens are the
project's, taught from its
documentation.

## The default: open to read, locked to write

A fresh Nightscout makes a deliberate
split (t09). Ask the server who an
anonymous caller is:

```bash
$ curl -s http://127.0.0.1:1337/api/v1/verifyauth.json
{"...": {"canRead":true,"canWrite":false,
         "isAdmin":false,"message":"UNAUTHORIZED", ...}}
```

and who the holder of the API secret
is:

```bash
{...: {"canRead":true,"canWrite":true,
       "isAdmin":true,"message":"OK", ...}}
```

So: **anyone who can reach the site
can view the readings** — every
reading, the whole history — and
**only a caller with a credential can
write or administer**. The lab's read
checks in chapter 4 carried no secret
at all and succeeded; its writes
without one were refused with 401
(t05, t07).

Why designed that way? A follower at
3 a.m. — a parent with a bookmark, a
partner on a night shift — needs the
graph *now*, not a login flow. The
cost is the mirror image: the URL is
effectively public to whoever learns
it. Whether that is acceptable is a
decision about *where the server
lives* (a home network, a VPN, behind
a proxy login — chapter 12), and
Nightscout leaves it to you because
families' answers honestly differ.

## Sharing without the master key

The API secret is the master key: it
writes, administers, everything.
Never give it to followers, and think
twice before giving it to apps you
do not control. Instead, the site's
admin tools (under the gear menu,
signed in with the secret) let you
create **subjects**: named entries
with a **role** — readable,
careportal, admin, and others the
project defines — each with its own
**access token**. A follower's
bookmark then carries their token
(`https://your-site/?token=...`),
granting exactly their role and
nothing else, and a token can be
revoked on its own when a phone is
lost or a follower's role ends,
without disturbing anyone else.

Uploaders you trust least get the
same treatment: a subject with only
the right to write entries, rather
than the master secret, where the
app allows choosing.

## The care portal: notes that ride along

Glucose is not the whole diary.
Meals, insulin doses, exercise,
illness, a sensor change — the events
that explain *why* the line moved —
can be recorded as **treatments**
through the site's care portal (the
status reply's `"careportalEnabled":
true`, visible in t03). Followers see
them plotted against the graph: the
spike with "birthday cake" attached
needs no detective work. Who may add
treatments is its own role, separate
from viewing — a follower can be
eyes-only while the wearer, or a
parent, keeps the pen.

## The check

Before sharing a link with anyone,
run chapter 4's two probes from a
browser or machine that is *not*
signed in: the site should show the
graph (that is the point), and a
treatment or entry should be
impossible to add (that is the
guard). If you can write while
anonymous, a token with broad rights
has leaked into a bookmark — revoke
subjects until the anonymous answer
is read-only again.
