# SVIDIA VMS update channel

The auto-update feed and installer downloads for **SVIDIA VMS**.

**Current version: 9.1.26.324** — [download the installer](https://github.com/SVIDIA/vms_update/releases/latest)

An installed copy checks this channel by itself and applies an update overnight (between 01:00 and
05:00 local time), so there is normally nothing to do here. The download above is for a first
install, or for putting VMS on a machine that has no internet access. You can also check on demand
with the update button in the VMS toolbar, which appears when an update is waiting.

One installer, `vms-setup.msi`, covers both ways of installing: **for everyone on this computer**
(needs administrator rights) or **just for me** (no admin rights needed). The installer asks which
you want; updates then follow whichever you chose.

---

## What's new

### 9.1.26.324 — 7 September 2026

**See where an NVR is reachable from the internet.** The NVR Information dialog now shows a
**Public addr** row — `<name>.svidia.net:<port>` — for any NVR registered with SVIDIA's dynamic-DNS
service. That is the address to hand to someone connecting from off-site, in place of hunting for
the site's WAN address. If a port is not reachable from the internet the row says so, with a note
that the router still needs to forward it; the address is still shown, because it keeps working over
a LAN or VPN.

The **Messages** tab now also reports when that address is registered, when the NVR's public IP
changes, when a requested name is already taken, and when the address is released.

Both need an NVR whose public-address (dynamic DNS) feature is switched on. Older NVRs are
unaffected — the row is simply not shown, and nothing else in the dialog changes.

### 9.1.26.323 — 1 September 2026

- **Fixed:** in NVR Configuration, reconnecting a camera with the **Connected** toggle did not take
  effect.

### 9.1.26.322 — 1 September 2026

**One installer, replacing the previous two-part setup.** A single `vms-setup.msi` now handles both
install types, silent enterprise rollout and auto-updates.

- **Uninstall now asks about your settings** instead of always leaving them behind. Scripted
  uninstalls can decide with `REMOVESETTINGS=0` or `REMOVESETTINGS=1`.
- **Settings moved** to `%AppData%\SVIDIA LLC\VMS2026.*`. Settings from the two previous versions are
  migrated automatically the first time the new build runs.
- Installing for everyone on a computer now also removes a copy the same user had installed just for
  themselves, so one machine no longer ends up with two.
- Updates are staged per install type, so per-user and per-machine fleets can be rolled out
  separately.

### 9.1.26.316 — 28 August 2026

- **Fixed:** copies installed **just for me** never received automatic updates.
- **Fixed:** the installer failed on locked files when VMS was running — it now closes VMS first.
- **Fixed:** the installer preselected an option that a user without administrator rights could not
  complete.
- **Check for updates** in the app now uses the current updater.
- **R-CAD:** new **Watchlist** node for licence-plate allow/deny lists, and its alarms.
- **Fixed:** system events and operator messages could stop arriving entirely — a camera's status LED
  could also show a stale state in the tree.

---

## For administrators

Silent install, for scripted or enterprise rollout:

```
:: for everyone on the computer (run elevated)
msiexec /i vms-setup.msi /qn ALLUSERS=1 REBOOT=ReallySuppress

:: just for the current user
msiexec /i vms-setup.msi /qn MSIINSTALLPERUSER=1 ALLUSERS=2 REBOOT=ReallySuppress

:: uninstall - always state what should happen to the settings
msiexec /x vms-setup.msi /qn REMOVESETTINGS=0
```

Every release asset is signed by **SVIDIA LLC** and published with its SHA-256; the updater refuses
anything that fails either check.
