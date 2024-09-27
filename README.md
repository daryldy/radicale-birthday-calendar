# Radicale Birthday Calendar
Server-side script to have a calendar filled with the birthdays of your contacts

## Overview

Android has no build-in way to show the birthdays of your addressbook contacts in your calendar. When you run the CalDav and CardDav [Radicale server](https://radicale.org) to store your contacts, use this repository script to make radicale maintain a birthday calendar for you. You can then synchronize this calendar to any device.

The script is meant to execute as part of the [Radicale storage hook](https://radicale.org/v3.html#versioning-with-git). As soon as you change any of your contacts in Radicale, the repective birthday is updated in the calendar.

## Setup

1. Store this script at only location on disk (e.g. `/create_birthday_calendar.py`).
2. Following the hook example for [versioning with git](https://radicale.org/v3.html#versioning-with-git), extend your hook command to something like
```
(git status --porcelain | awk '{print $2}' | python3 /create_birthday_calendar.py || true) && git add -A && ...
```

Restart Radicale. Once you change one of your contacts, the script creates a calendar called `birthdays`.

## Debug

To simulate a change
1. Locate the file system folder for collections (e.g. `/var/lib/radicale/collections`)
2. Locate the contact you simulate to have changed (e.g. `collection-root/anna/1264849c-8f4b-4bd2-ae18-033641fccd4b/db67431c-ad1c-448b-a5e9-827ff07a4aa8.vcf`)
3. Then, as your Radicale user
```
$ cd /var/lib/radicale/collections
$ echo 'anna/1264849c-8f4b-4bd2-ae18-033641fccd4b/db67431c-ad1c-448b-a5e9-827ff07a4aa8.vcf' | python3 /create_birthday_calendar.py
```
