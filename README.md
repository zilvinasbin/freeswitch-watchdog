# freeswitch-watchdog
A daemon to restart BigBlueButton's FreeSWITCH service when [Issue 9667](https://github.com/bigbluebutton/bigbluebutton/issues/9667) occurs.

Yes, this is an awful hack, but it's all we have for now. Users will be thrown out of audio when the error occurs, but they will be able to reattach to audio after a minute.

## setup

Get the debian package (either from this directory or by building from source using the instructions below).

Go to the directory where the package is:
```bash
dpkg -i ./freeswitch-watchdog_0.1.3_amd64.deb
```

Enable and start the service:
```bash
systemctl enable freeswitch-watchdog.service
systemctl start freeswitch-watchdog.service
```

You can check the output in the journal using
```
journalctl -xefu freeswitch-watchdog.service
```
There should be messages if a problem with FreeSWITCH is detected.

## building from source
We recommend to build on Ubuntu 16.04, so the dependency versions will be the same as on the server.

Install some system dependencies:
```bash
sudo apt install libssl-dev pkg-config build-essential
```

If not already present, install Rust:
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | bash -s -- -y
source $HOME/.cargo/env
```

Install cargo-deb so we can create a Debian package:
```bash
cargo install cargo-deb
```

Now, clone the repo:
```bash
git clone https://gitlab.senfcall.de/johannesbarthel/freeswitch-watchdog
cd freeswitch-watchdog
```

Compile and create the package:
```bash
cargo deb
```

The package will end up in the `target/debian` directory.
