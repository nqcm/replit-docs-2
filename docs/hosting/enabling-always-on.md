---
sidebar_position: 5
---

# Enabling Always-on

Repls typically go to sleep after a period of inactivity. To make sure that your repl is restarted, you can use our Always-on functionality.

Note that this function is currently only available on our [Hacker plan](https://replit.com/site/pricing).

## Enabling Always-on

Always-on is controlled by a toggle inside your repl. To enable it:
1. Navigate to your repl.
2. open the info panel by clicking on your repl's name. 
3. Enable Always-on by clicking on the toggle at the bottom:

![screenshot of Always-on toggle](https://replit-docs-images.bardia.repl.co/images/repls/always-on-toggle.png)

After Always-on is enabled, you will see a green indicator next to your repl's name:

![screenshot of repl with Always-on enabled](https://replit-docs-images.bardia.repl.co/images/repls/always-on-enabled.png)

That's it! We will run your repl whenever it goes to sleep.

## What does Always-on do?

Repls ordinarily do not run unless someone presses the "Run" button or if [the repl receives HTTP traffic](/hosting/deploying-http-servers). Always-on runs your repl when neither of those occur. When running your repl, Always-on will install packages and respect [your Run button configuration](/repls/dot-replit).

Always-on does not extend your repl's lifetime. All repls are subject to go to sleep at any time. Always-on will, however, immediately run your repl again whenever this happens. If your process exits on its own, Always-on will not restart it. We recommend using [Database](/hosting/database-faq) to persist information outside of your process.

<!--
## Feedback

We are interested in hearing from you about your experience with Always-on. Link to some thread where this can happen.
-->
