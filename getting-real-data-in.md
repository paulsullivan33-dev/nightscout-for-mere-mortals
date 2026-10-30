# Getting Real Data In

By the end of this chapter you know
every road a real reading can take
into your server, and which one fits
a given sensor. Read the label first:
**this chapter is documented-only.**
The lab had no CGM, no phone
uploaders and no sensor-maker
accounts, so none of these routes was
run — they are described from the
project's documentation, and
`criteria.md` says so again. What
*was* proven, in chapter 4, is the
destination they all arrive at: the
same entries API, the same JSON, the
same secret. Every route below ends
at the door you have already watched
accept data.

## The pattern every route follows

An **uploader** is anything that
POSTs entries to your site with the
right to write. Routes differ only in
where the uploader gets the reading:
from the sensor directly, or from the
maker's cloud, where the maker's own
app already sent it. Cloud routes are
simpler to set up and lag a little
more; direct routes are fresher and
depend on community apps. Both end
at `https://your-site/api/v1/entries.json`,
authenticated by the API secret's
hash or by a personal token from
chapter 7.

## The built-in bridge (Dexcom)

Nightscout carries a **bridge** for
Dexcom's Share service. Given the
Dexcom account's username and
password (the `BRIDGE_USER_NAME` and
`BRIDGE_PASSWORD` variables) and its
region (`BRIDGE_SERVER`, US or
outside-US), the server itself signs
in to the maker's cloud, fetches new
readings every few minutes, and
stores them as its own entries. No
phone app is involved on your side.
Two honest consequences: your server
holds the keys to a health account,
which belongs in the privacy thinking
of chapters 1 and 12; and if the
maker changes their service, the
bridge waits for the community to
catch up.

## Phone uploaders

- **xDrip+** (Android) reads many
  sensors and transmitters directly
  and can upload to Nightscout as one
  of its sync targets — the classic
  direct route, and for many wearers
  the display they watch most, with
  Nightscout as the shareable copy.
- **xDrip for iOS / Spike and their
  kin** fill the same role on iPhone,
  within what iOS allows background
  apps to do.
- **Loop and AAPS**, the community
  closed-loop systems of chapter 11,
  upload their readings (and much
  more — pump events, doses, their own
  predictions) to Nightscout as their
  standard reporting step.

Each is configured with your site's
URL and the API secret, in the app's
own settings; each then speaks the
chapter 4 protocol on its own.

## Libre routes

FreeStyle Libre sensors reach
Nightscout by a few roads: through
the maker's LibreLinkUp sharing
service, fetched by community bridge
programs (and by xDrip+); or, for
Libre 2 and 3 read directly by phone,
by the phone uploaders above. The
names and apps in this lane change
faster than any other in the chapter;
the project's current documentation
is the authority, and the shape —
*something* fetches, then POSTs
entries — does not change.

## Proving a route works

Whichever route you choose, the
acceptance test is the same, and it
is this guide's own checks, not the
uploader's cheerful settings screen:

1. The status check answers
   (chapter 2).
2. `entries/current.json` shows a
   reading minutes old, with the
   uploader's name in its `device`
   field (chapter 4).
3. The graph draws it, fresh and
   ungreyed (chapter 5).

If step 2 fails, the uploader is not
arriving — its URL, its secret, or
its own sensor link is wrong — and no
amount of display-refreshing will
help. That separation, server proven
in chapter 4 and route proven here,
is why the invented readings came
first.
