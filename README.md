# Bolt Keyboard Wake

A systemd sleep hook for the Logitech MX Keys S and Logi Bolt USB receiver (`046d:c548`). It temporarily detaches the receiver’s keyboard interface before suspend and restores it on resume. USB wake stays enabled, so a keypress can still wake the computer.

Confirmed on a ThinkPad running Arch Linux with `s2idle`, with an MX Keys S and MX Master 3S sharing the receiver. The hook targets unwanted keyboard wakes. A mouse sharing the receiver can still generate a USB wake signal.

## Install

From the repository directory, run this on a Linux system with systemd. Leave USB wake enabled for the Bolt receiver.

```sh
sudo install -m 0755 50-bolt-keyboard-wake /usr/lib/systemd/system-sleep/50-bolt-keyboard-wake
systemctl suspend
```

Wait long enough to check for an unwanted wake, then press a key on the MX Keys S. If the key does not wake the system, use the power button. If keyboard input does not return after resume, unplug and reconnect the receiver.

To see the detach and restore messages, run:

```sh
journalctl -b -u systemd-suspend.service --grep='Bolt keyboard'
```

## Remove

```sh
sudo rm /usr/lib/systemd/system-sleep/50-bolt-keyboard-wake
```

## Credit

Tomas Leypold identified the interface-specific workaround and published the first hook in his [Linux kernel bug report](https://lkml.rescloud.iu.edu/hypermail/linux/kernel/2608.1/02181.html). This adaptation checks the receiver’s wake setting and tracks which interfaces it detached so it can restore them on resume.
