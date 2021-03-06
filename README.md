# <i>CK Bunker</i>

![](https://github.com/Coldcard/ck-bunker/blob/master/static/screen-shot.jpg)

Vist [Github repo for CK-Bunker](https://github.com/Coldcard/ck-bunker).

## What is the Coinkite Bunker?

"Basically the cost of a Bitcoin HSM with custom policies is now the cost of a coldcard and you don't need a thirty party to maintain it." - Francis P.

It's a python program that you run on a computer attached to a
Coldcard. It will setup and operate the Coldcard in "HSM Mode" where
it signs without a human pressing the OK key.  To keep your
funds safe, the Coldcard implements a complex set of spending rules
which cannot be changed once HSM mode is started.

Using the `tord` (Tor deamon) you already have, the CK Bunker can
make itself available as a hidden service for remote access over
Tor.  A pretty website for setup and operation allows access to all
HSM-related Coldcard features, including:

- transaction signing, by uploading a PSBT; can broadcast signed txn using Blockstream.info (onion)
- define policy rules, spending limits, velocity controls, logging policy
- user setup (TOTP QR scan to enroll on Coldcard, or random passwords (Coldcard) or known password

The bunker encrypts its own settings and stores the private key for that inside Coldcard's
storage locker (which is inside the secure element). The private key for the onion
service, for example, is held on the bunker's disk encrypted like that.

## What is Coldcard?

Coldcard is a Cheap, Ultra-secure & Opensource Hardware Wallet for Bitcoin.
Get yours at [ColdcardWallet.com](http://coldcardwallet.com)

[Follow @COLDCARDwallet on Twitter](https://twitter.com/coldcardwallet) to keep up
with the latest updates and security alerts. 

## Check-out and Setup

Do a checkout, recursively to get all the submodules:

    git clone --recursive https://github.com/Coldcard/ck-bunker.git

Then:

- `virtualenv -p python3 ENV` (Python 3.7 or higher is required)
- `source ENV/bin/activate` (or `source ENV/bin/activate.csh` based on shell preference)
- `pip install -r requirements.txt`
- `pip install --editable .`

## Usage

The executable is called `ck-bunker`:

```sh
$ ck-bunker --help
Usage: ck-bunker [OPTIONS] COMMAND [ARGS]...

Options:
  -s, --serial HEX  Operate on specific unit (default: first found)
  --help            Show this message and exit.

Commands:
  list     List all attached Coldcard devices
  example  Show an example config file, using the default values
  run      Start the CK-Bunker for normal operation
  setup    Configure your transaction signing policy, install it and then...
```

There are two modes for the Bunker: "setup" and "run mode". In setup
mode, Tor connections are disabled, as is the login screen.

You would typically use the setup mode for picking the onion address, the
master login password and all the details of the HSM policy.

```sh
$ ck-bunker setup
```

Open this URL in your local web browser (must be same machine):
<http://localhost:9823>

Once the Coldcard is running in HSM mode, with your policy installed,
it makes sense to operate in normal "run" mode:

```sh
$ ck-bunker run
```

You may also run with remote connections (and login) disabled. This would be useful
if you have some existing web proxy already in place.

```sh
$ ck-bunker --local run
```

## Tor Use

To access over Tor as a hidden service, you must have `tord` running
on the same machine. For desktop systems, keeping TorBrowser open
is enough to acheive this. On servers, start tord with default options,
and ck-bunker will use the control port (localhost port 9051 or 9151).

If you use the bunker to broadcast the final (signed) transaction,
the socks proxy of tord (port 9050) will also be used.


## Operational Requirements

You will need:

- this code
- a Coldcard connected via USB
- `tord` (Tor program)
- Internet connection
- a Tor-capable brwoser, like "Tor Browser" or Tails.
- (optional) a microSD card, for logging of transactions on Coldcard

