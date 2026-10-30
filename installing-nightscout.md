# Installing Nightscout

By the end of this chapter a
Nightscout server is running on your
machine and answering its status
check. The recipe is the one this
guide's lab ran, piece for piece:
MongoDB from its official tarball,
Node.js, the server from its source
repository, and four environment
variables to hold it together. (Many
people run Nightscout in Docker or on
a small cloud host instead; those
routes use the same variables and the
same checks, but the lab could not
run containers, so they are named
here, not taught — `criteria.md`
keeps the distinction.)

## What you need

- A machine that stays on: a home
  server, a small PC, a Raspberry Pi.
  Nightscout itself is light — the
  lab's shared 2-CPU machine did not
  notice it.
- **Node.js 20 or newer.** The
  project's `package.json` states
  `"node": ">=20.x"` — the lab ran
  24.22.1 (transcript t00).
- **MongoDB.** The lab used 8.0.4
  from the official tarball. Any
  currently supported MongoDB works;
  the server treats it as a filing
  cabinet, nothing exotic.

## Step 1 — MongoDB

Download the tarball for your system
from MongoDB's download centre, unpack
it somewhere you control, and start
the database with a data folder of
its own:

```bash
tar xzf mongodb-linux-x86_64-ubuntu2404-8.0.4.tgz
mkdir -p db logs
./mongodb-linux-x86_64-ubuntu2404-8.0.4/bin/mongod \
  --dbpath ./db --bind_ip 127.0.0.1 --port 27017 \
  --fork --logpath ./logs/mongod.log
```

`--bind_ip 127.0.0.1` keeps the
database listening only to this
machine — Nightscout talks to it
locally, and nobody else should talk
to it at all. The check (t01): its
log carries a `Waiting for
connections` line naming port 27017,
and a ping through a MongoDB driver
answers `{ ok: 1 }`.

## Step 2 — the server

```bash
git clone https://github.com/nightscout/cgm-remote-monitor.git nightscout
cd nightscout
npm ci
```

`npm ci` installs exactly the
dependency versions the project's
lockfile pins — the lab's install
took about three and a half minutes
on a shared machine. The server
program itself is
`lib/server/server.js`.

## Step 3 — the four variables

Nightscout is configured through its
environment. Four variables are the
working set:

```bash
export API_SECRET='a-long-passphrase-you-invent'
export MONGO_CONNECTION='mongodb://127.0.0.1:27017/nightscout'
export PORT=1337
export INSECURE_USE_HTTP=true
```

- **API_SECRET** is the master
  passphrase for the site — the right
  to write data and administer it.
  It must be **at least 12 characters**;
  the server checks, and says so in
  its log (`API_SECRET should be at
  least 12 characters`) when one is
  too short. Invent a long one. The
  lab used an obviously fake one for
  its transcripts; yours stays out of
  anything you share.
- **MONGO_CONNECTION** points at the
  database: the address from step 1
  and the database name (`nightscout`)
  to use inside it. The server creates
  what it needs on first connection.
- **PORT** is where the website
  listens; 1337 is Nightscout's
  traditional default.
- **INSECURE_USE_HTTP=true** allows
  plain HTTP. Right for a first run on
  your own machine or home network;
  chapter 12 is about what changes
  before the site faces the internet.

## Step 4 — start it, and check

```bash
node lib/server/server.js
```

The boot log tells the story in
order (t02): the secret's strength
(`API_SECRET has ... bits of
entropy`), `Successfully established
connection to MongoDB`, a list of
enabled features, a security notice
about proxies this guide returns to
in chapter 13, and finally
`Listening on port 1337`. Then the
check that proves the whole stack —
server, configuration, database —
in one line:

```bash
$ curl -s http://127.0.0.1:1337/api/v1/status.json | head -c 120
{"status":"ok","name":"nightscout","version":"15.0.9", ...
```

If that answers, leave the server
running and go to chapter 4, which
puts data through it. If it does
not, chapter 13 works the failure
modes in the order the boot log
meets them.
