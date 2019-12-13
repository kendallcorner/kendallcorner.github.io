---
title: Sync-Process
date: 2019-12-13 12:49:23
tags: 
- Outreachy
- Mozilla
- Multi Account Containers
---

I've already met with people from several teams including the Firefox Privacy and Security team, the containers team, and the sync team to dicuss paths forward with this project. There were many ideas on what to accomplish here.

We decided that my first task with the [Multi Account Containers](https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/) project is to enable "Sync" with the containers between browsers that are logged in with the same [Firefox Account](https://www.mozilla.org/en-US/firefox/accounts/). Sync is a Firefox service that syncs your bookmarks, passwords, open tabs, and history between firefox installs on all your devices. It's a really handy feature, but unfortunately containers are not currently sycned. Even worse, the site assignments (which are in my opinion the most tedious thing to set up), aren't even stored in Firefox and so wouldn't be synced if the containers were.

We discussed where to add this feature: In the browser or in the extension, and we chose the extension for the sake of getting the project shipped while I am still an intern. Future work here would obviously be to pull the containers and site assignments (and syncing) fully into Firefox itself.

Paths forward for syncing were also dicussed, including using the container name as a unique identifier. This might cause problems or make it harder to rename or delete a container, so we decided to add a [uuid](https://en.wikipedia.org/wiki/Universally_unique_identifier) for the containers in the MAC extension.

## Workflow

The initial sync on versions of the addon that have never been synced will 

1. add a uuid for each of the containers
2. flag all assigned sites with that uuid while keeping their contextual identity number

Currently Firefox Contextual Identities (what the containers are called inside the browser itself) are not given uuids and this could cause collision issues when syncing data. That will need to be updated within firefox at some point. Maybe a furture project for me.

3. The new containers with uuids and site assignment data will be stored in sync
  * this must remain less than 100 kB, the sync limit for addons)

When syncing with a previously unsynced Firefox MAC addon, the addon should: 

4. compare the synced containers with the containers on the browser

If site assignments are the same for a given container name: 

6. The containers will be assumed to be the same and will be merged with a single uddid from the synced container

If the site assignments are different or a synced container is found that is not in the browser:

7. a new container will be made on the browser

After the initial sync, the addon will sync based on uuids and maintain a list of deleted container uuids.

<hr>
This blog post is part of the [Outreachy](https://www.outreachy.com) project.
