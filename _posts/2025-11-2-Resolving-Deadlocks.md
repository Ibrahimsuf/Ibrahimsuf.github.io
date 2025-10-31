---
layout: post
title:  "Resolving Deadlocks"
date:   2025-11-2 9:00:00 -0500
categories: jekyll update
---

I ran into a bug a few days ago when trying to run database seeds. I saw an error that mariadb was timing out because a process could not get a hold of the database lock. My supervisor showed me that you can see a list of all the locks on the database in the `innodb_lock_waits` table in the mariadb system database. From that we were able to see that there was a lock on the database that had been holding the lock for over an hour. I could then kill that process and rerun the seeds which worked because there was no longer a lock on the database.