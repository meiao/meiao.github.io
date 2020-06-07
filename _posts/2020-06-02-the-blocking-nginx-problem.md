---
layout: post
title:  "The blocking Nginx problem"
category: servers
tags: aws efs nginx
---

Earlier this year, we had a really strange problem.
When there were lots of user getting into our application it would slow down to
a crawl and all requests would take seconds to return to the user. That was a BIG
problem.

This is a simplified version of our architecture. We have a load balancer
that distributes the traffic between two Nginx servers. These servers have three
jobs: deliver static assets (from an EFS drive), proxy REST calls to the REST
servers and proxy long polling calls to our WebSockets server.

First thing we checked was the REST servers, which are also load balanced.
Everything seemed normal, no CPU spikes, memory was fine, network was not an
issue. In their load balancer, we saw that requests were being returned in a
timely manner. After a few minutes the problem disappeared and we dismissed it
as some random hiccup.

Then it happened again in the next day, around the same time. So we created
scripts to spam our QA environment. The scripts made lots of calls to our worst
performing REST endpoints and requested lots of static assets. Our QA
environment, which has no redundancy, was able to handle that without a hitch.

One thing that we noticed was that the Nginx worker processes were using lots of
CPU, but running {{top}} didn't show the CPU usage as high. Another thing was
that the Nginx worker processes were blocking. So we thought that increasing the
number of workers would help.

At this point, my bets were on the proxy of the long polling calls,
since it was not tested by the scripts, it uses a third party Nginx plugin and
we have very little visibility into it.

Then it happened for the third time. The increased number of workers just
increased the number of blocked workers. So we rolled that back. After the
situation normalized we started going thru the AWS console for the machines and
the EFS drives. We noticed some discrepancies comparing the numbers from the
production environment and QA. The disk bandwidth was extremely different among
them.

Reading more on EFS, we found out that when we migrated from using the machines'
disk to EFS, we selected burstable throughput mode, which give you a low base
bandwidth and some credits to burst that speed whenever needed. Whenever you
use less than the base bandwidth you regain credits.

So what happened was that we consumed our initial credits within six months of
migrating EFS even though we regained a few credits when there was no traffic.
Then every time the traffic increased, we would go thru the credits we regained
from the last night and our base bandwidth would be 50KiB/s as that is what is
provided for each GB of data stored.

Since the servers had a 50KiB/s read from disk, it would take seconds to load
some static assets from the disk, thus the Nginx workers would be blocked by IO.
That is why it wouldn't show as CPU usage in {{top}}, but would show as both
increased load average and time that the process used the CPU.

Finally, the solution was to change the throughput mode to provisioned, with a
few MB/s of provisioned bandwidth.

We solved the problem. Problem solved.
