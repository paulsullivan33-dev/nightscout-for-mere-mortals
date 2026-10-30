# Reports

By the end of this chapter you know
what the site's reports show and why
the history you have been storing is
the point of them. The reports are
pages the server builds from the
stored entries and treatments; the
lab verified the data underneath
them — every report is a view of the
same entries chapter 4 posted and
read back — while the rendered pages
themselves are taught from the
project's documentation, the lab
having no browser.

## Why reports exist

A single reading answers *what now*.
Care decisions answer *what usually*:
How often is this person in range?
Are the lows at night or after
sport? Is the pattern drifting over
the month? Those are questions about
thousands of readings, and they are
what a care team reviews at an
appointment. The sensor makers keep
their versions of these reports in
their own clouds; Nightscout builds
its from the database under your
server — the same `entries` and
`treatments` collections chapter 2
named — for any span you choose.

## The set

- **Day to day** — each day drawn as
  its own graph line, overlaid or
  side by side. The fastest way to
  see a routine: the weekday shape
  against the weekend shape, the
  breakfast spike that repeats.
- **Week to week / distribution** —
  the readings of a whole period
  summarised against the target band:
  what share of the time was spent in
  range, above it, below it. This is
  the headline number of diabetes
  care — **time in range** — and the
  report states it plainly.
- **Percentiles** — for each time of
  day, the spread of readings drawn
  as bands: the median line, the
  usual range around it, the tails.
  The 3 a.m. band answers "are nights
  stable?" in one picture.
- **Daily statistics and glucose
  distribution** — averages, spread,
  estimated A1C-style summaries: the
  arithmetic layer under the
  pictures, for teams that want
  numbers with their graphs.

Treatments from chapter 7's care
portal thread through all of them —
a pattern without its causes marked
is half a report.

## Reading them honestly

Reports describe; they do not
judge, and they carry the data's own
gaps: a week with the sensor off is
a week the report silently skips,
so a beautiful time-in-range over a
period when nobody was wearing the
sensor is a beautiful lie. Check
the span's coverage before quoting
any figure from it, and take the
reports to the care team rather
than treating them as verdicts —
the team reads them alongside
everything else they know, which is
the context no database holds.

## The check

The report is only ever as real as
its source. Before trusting a span,
ask the API how much data it holds:

```bash
curl -s "http://127.0.0.1:1337/api/v1/entries.json?count=1"
```

and compare the newest entry's date
with the oldest day in your report
span — and when the span matters,
count the readings across it (the
API's `count` endpoint and query
filters are the tools; a full day of
five-minute readings is about 288).
A report over a span the database
cannot cover is a wish, not a
report.
