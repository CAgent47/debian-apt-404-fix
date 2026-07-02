Fix APT 404 Not Found Error on Debian/Ubuntu

Problem

While installing or repairing packages, APT returned errors similar to:

404 Not Found
E: Failed to fetch ...
E: Unable to fetch some archives

Running:

sudo apt --fix-broken install

alone did not solve the problem.

---

Cause

The local APT package index was outdated.

APT was trying to download an old package version that had already been removed from the Debian mirror.

---

Solution

Run the following commands in order:

sudo apt clean
sudo rm -rf /var/lib/apt/lists/*
sudo apt update
sudo apt --fix-broken install

---

Explanation

1. Remove cached packages

sudo apt clean

Deletes downloaded package cache from:

/var/cache/apt/archives/

---

2. Remove old package indexes

sudo rm -rf /var/lib/apt/lists/*

Deletes the locally stored repository indexes.

This does NOT remove installed software.

---

3. Download a fresh repository index

sudo apt update

Downloads the latest package lists from Debian repositories.

---

4. Repair broken dependencies

sudo apt --fix-broken install

Repairs broken packages and installs missing dependencies using the updated repository information.

---

Why this works

The package indexes on the system were outdated.

After deleting the old indexes and downloading fresh ones, APT searched for the newest package versions instead of requesting files that no longer existed on the mirror.

---

Tested On

- Debian 12 (Bookworm)

---

Tags

- Debian
- Linux
- APT
- apt
- 404
- Package Manager
- Troubleshooting
- Dependency
- Package Not Found

---

License

MIT