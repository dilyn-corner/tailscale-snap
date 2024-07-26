# tailscale as a snap


## Introduction

Please note: this is an unofficial snap of the tailscale debian package.
See [here](https://github.com/tailscale/tailscale/issues/267#issuecomment-2249033007) for more information.

This is the upstream repository for the **unofficial** tailscale snap!

Builds are regularly done via the GitHub CI/CD system, though none are (currently) published.

If you experience any issues, please see [issues](#issues).

The only use-cases being regularly tested are my own; I use tailscale to gain
access to my build machines and servers while on the road for work, and I
don't require much to do that. I am always willing to accept PRs to add things
required for additional features! Please see [contributing](#contributing).


This snap contains some software distributed under the following licenses:

* Apache 2.0
* BSD 2-Clause
* BSD 3-Clause
* ISC
* MIT

For an up-to-date list of the software utilized by tailscale, see their
[licenses](https://github.com/tailscale/tailscale/blob/main/licenses/tailscale.md) information.

The content under `snap/` and `src/` are licensed CC BY-NC 4.0.


## Building

Clone this repository and build using snapcraft

```
  snap install --classic --channel=8.x/stable snapcraft
  git clone https://github.com/dilyn-corner/tailscale-snap && cd tailscale-snap
  snapcraft
```


## Installing


If you are working on a locally built-version to do some debug work or testing
(and hopefully making a PR!):

```
  snap install --dangerous tailscale*.snap
```

You can use snappy-debug to determine if you need additional interfaces (please
know that there will be a lot of noise, justification will be required to add
any additional interfaces):

```
  snap install snappy-debug
  sudo journalctl --output=short --follow --all | sudo snappy-debug
```

You could also install locally built snaps with `--jailmode` or `--devmode` if
you want to test confinement or other functionality:

```
  snap install <--jailmode --dangerous|--devmode> tailscale*.snap
```


## Usage

Ensure all interfaces are connected:

```
  snap connections tailscale
```


Make sure the tailscaled service is running:

```
  snap info tailscale
  # Should say "tailscale.tailscaled: simple, enabled, active"
  # If it isn't running:
  snap start --enable tailscale.tailscaled
```


If a different port for tailscale to use is required (default: 41641):

```
  snap set tailscale port=<port>
  snap restart tailscale
```

Set the tailscale interface 'up':

```
  sudo tailscale --socket /var/snap/tailscale/common/tailscaled.socket up
```


Open the URL that command spits out into a web browser and login to your
tailscale account. That connection will remain active for as long as the session
is valid.


# Issues

If you experience any issues using this snap, please file an issue against this
repository. __DO NOT__ bother tailscale upstream, they have nothing to do with
these efforts and will not be able to help!

In your issue, please include the following information:

1) What you were trying to do (a specific command, a specific action, etc.)
2) The expected outcome
3) The FULL output of what occurred
4) The FULL output from the tailscaled daemon (`snap logs -f tailscale.tailscaled`)


# Contributing

Contributions are welcome! I am a bit picky though.

All of your commits which encompass non-code related changes (I would
consider the contents of `src/` and `snap/` to be *non* code unless otherwise
__immensely__ robust) **MUST** be licensed CC BY-NC.

The current CC BY-NC license can be viewed [here](https://creativecommons.org/licenses/by-nc/4.0/).


Commits should be formatted as such:

* Entire message should read as:

```
<{file/folder} name>: short and sweet description

Further details if required

Signed-off-by: <author name> <author email>
```

You can include a signoff for each commit using `git commit -s`

Commits need not be signed with a GPG key, but are welcome (`git commit -S`)

To amend prior commits with a signoff, do `git rebase --signoff HEAD~<number of commits>`

* If you are modifying the root of the repository (e.g. modifying/adding 
`.github`, `/README.md`, etc.), you should use one of `github`, `README`,
`gitignore`, `LICENSE`, etc.

* If you are adding something to a particular file, it would be sufficient to
say `<file/folder name>: short and sweet description` and include an elaboration
of what is being added to which example in further details. Short-hand here is
also allowed (e.g. when modifying `snap/snapcraft.yaml`, `snapcraft.yaml:` may
be a sufficient heading in the commit message)

* Commits should attempt to be atomic though PRs need not be restricted to a
single folder or file, as long as the content change can be logically grouped.
