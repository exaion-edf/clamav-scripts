ClamAV scripts for Linux desktop
================================

This is a collection of tools to setup the ClamAV antivirus on Linux desktops, in a suitable way for sensitive environments.
 * Periodic scan of all home directories
 * Realtime scan of major virus sources: internet browsing and removable media
 * User notification when malware is found

# Periodic scan

This script launches clamav once a week on all home directories, moving any malware found to the quarantine directory.

To install:
* `clamav-periodic-scan` → `/usr/local/sbin/`
* `clamav_iosched.conf` → `/etc/systemd/system/clamav-daemon.service.d/iosched.conf`
* `clamav-periodic.service` → `/etc/systemd/system/`
* `clamav-periodic.timer` → `/etc/systemd/system/`

To setup:
* Tweak `clamav-periodic.timer` to your liking.
* `mkdir -p -m 1733 /home/quarantine`
* `systemctl daemon-reload`
* `systemctl enable clamav-periodic.timer`
* `systemctl start clamav-periodic.timer`

# Realtime scan of download directory

This script scans the user download directory, moving any malware found to the quarantine directory.

To install:
* `clamav-session` → `/usr/local/bin/`
* `clamav-session.desktop` → `/etc/xdg/autostart/`

To setup:
* `mkdir -p -m 1733 /home/quarantine`

It is highly recommended to ship this with a browser configuration that prevents saving files to arbitrary directories.

For example with Firefox ESR, you can setup a /usr/share/firefox-esr/distribution/policies.json with the following:
```json
{
  "policies": {
    "PromptForDownloadLocation": false,
  }
}
```

# On-access scan for removable media

This script launches a **clamonacc** instance for each inserted removable media, preventing access to infected files. Note there is no quarantine for removable media.

To install:
* `clamav-monitor-mounts` → `/usr/local/sbin/`
* `clamav-mountpoint` → `/usr/local/sbin/`
* `clamav-monitor-mounts.service` → `/etc/systemd/system`

To setup:
* `systemctl daemon-reload`
* `systemctl enable clamav-monitor-mounts`
* `systemctl start clamav-mountpoint`

