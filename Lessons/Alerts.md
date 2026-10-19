<!-- Run as a slideshow: reveal-md README.md -w -->
# 🐳 Alerts & Notifications

⭐️ **GOAL**: Self-host [Uptime Kuma] on your CapRover PaaS to monitor uptime, send alert notifications, and publish a status page. Then prove your alerts work by taking an app down on purpose.

<!-- omit in toc -->
## ⏰ Agenda (120m)

- [[**10m**] **Warm Up**: Disaster Scenarios](#10m-warm-up-disaster-scenarios)
- [[**5m**] **Discuss**: Disaster Scenarios](#5m-discuss-disaster-scenarios)
- [[**10m**] **TT**: Avoiding Disaster](#10m-tt-avoiding-disaster)
  - [Command Line Tools](#command-line-tools)
- [[**10m**] **TT**: Automating Uptime Monitoring](#10m-tt-automating-uptime-monitoring)
- [[**10m**] **Activity**: Set Up an Uptime Monitor](#10m-activity-set-up-an-uptime-monitor)
- [[**10m**] **Activity**: Send and Test an Alert](#10m-activity-send-and-test-an-alert)
- [[**10m**] **TT**: Status Pages](#10m-tt-status-pages)
- [[**10m**] **Activity**: Set Up a Status Page](#10m-activity-set-up-a-status-page)
- [[**15m**] **Break**](#15m-break)
- [[**10m**] **TT**: Resource Monitoring](#10m-tt-resource-monitoring)
  - [Introducing Netdata](#introducing-netdata)
  - [Industry Use](#industry-use)
  - [What Netdata Monitors](#what-netdata-monitors)
- [[**10m**] **Activity**: Working With Netdata](#10m-activity-working-with-netdata)
  - [Step 1: Enable Netdata](#step-1-enable-netdata)
  - [Step 2: Enable Alerts](#step-2-enable-alerts)
  - [Step 3: Discover the Dashboard](#step-3-discover-the-dashboard)
- [[**10m**] **Wrap Up**: Poll / Q+A](#10m-wrap-up-poll--qa)

<!-- > -->

## [**10m**] **Warm Up**: Disaster Scenarios

In breakout groups of 4, discuss and record **every way** something could go wrong with a deployed application.

Example scenario: _The server lost power due to a severe thunderstorm._

<!-- > -->

## [**5m**] **Discuss**: Disaster Scenarios

Call on students and ask them to contribute what they discussed to a running list of disaster scenarios.

We'll be talking about how to mitigate many of these risks today!

<!-- > -->


## [**10m**] **TT**: Avoiding Disaster

Today, we'll cover several ways to know and be notified when you're site goes down.

First, let's learn how we can check manually using our terminal.

### Command Line Tools

<!-- > -->

#### Ping

Test the `ping` command using your server's IP address or hostname.

```sh
ping 111.111.111.111
```

```sh
ping captain.dev.YOURDOMAIN.COM
```

<!-- > -->

## [**10m**] **TT**: Automating Uptime Monitoring

These manual techniques work great when you're sitting in front of your computer. But what if you're AFK? How will we know the server is down?

**Scenario**: The hosted uptime checker your team relied on just shut down its free plan. Your lead wants a replacement running on the company PaaS by the end of the day, with alerts going somewhere the on-call engineer will actually see them.

You'll use [Uptime Kuma], a free, open-source, self-hosted monitoring tool. It can:

- Check HTTP(s), TCP, ping, DNS, Docker containers, and more, as often as every 20 seconds.
- Send notifications through 90+ services, including Email (SMTP), Discord, Slack, and Telegram.
- Publish public status pages.

CapRover ships an official **Uptime Kuma** one-click app, so you can deploy it in a few clicks.

> **BE AWARE**: A self-hosted monitor shares the fate of the server it runs on. If your droplet loses power, Uptime Kuma goes down with it, and nobody gets an alert. In production, run your monitor on a **different server and network** than the apps it watches.

> **ASK AUDIENCE:** You install Uptime Kuma on the same droplet as `first-app`. Which of the disaster scenarios from the warm up would it catch, and which would it miss?

<details>
<summary>Answer</summary>

It catches problems with the **app**: a crashed container, a bad deploy, a 5xx error, an expired certificate. It misses problems with the **server**: power loss, a full disk that takes down Docker, or a provider network outage. Uptime Kuma goes down too, so it can't send the alert. To cover those, run a second monitor somewhere else.

</details>

<!-- > -->

## [**10m**] **Activity**: Set Up an Uptime Monitor

**Deploy Uptime Kuma**:

1. Log in to CapRover at `https://captain.dev.YOURDOMAIN.COM`.
1. Choose **Apps** from the sidebar, then click **One-Click Apps/Databases**.
1. Search for `Uptime Kuma` and select it.
1. Enter `uptime` as the **App Name**.
1. Set **Uptime Kuma Version** to `2`, then click **Deploy**.
1. When the deploy finishes, open the `uptime` app, click **Enable HTTPS** on the **HTTP Settings** tab, then turn on **Force HTTPS**.

> **PROTIP**: The one-click app routes traffic to the container's port `3001`, turns on WebSocket support (Uptime Kuma needs it), and stores everything in a persistent volume mounted at `/app/data`. Your monitors survive restarts and updates. You'll learn how volumes work in the [Volumes](Volumes.md) lesson.

**Create your account and first monitor**:

1. Visit `https://uptime.dev.YOURDOMAIN.COM`.
1. If Uptime Kuma asks which database to use, choose **SQLite**.
1. Create your admin account. **Use a strong password**, since this dashboard is on the public internet.
1. Click **Add New Monitor** and enter:
   - **Monitor Type**: `HTTP(s)`
   - **Friendly Name**: `first-app`
   - **URL**: `https://first-app.dev.YOURDOMAIN.COM` (the app you deployed in [Your Personal PaaS](PaaS.md). If you deleted it, use any app you have running on CapRover.)
   - **Heartbeat Interval**: leave the default of `60` seconds.
1. Click **Save**. The monitor turns green once its first check passes.

<!-- > -->

## [**10m**] **Activity**: Send and Test an Alert

A monitor that nobody hears is just a dashboard. Send alerts somewhere you'll see them, then break something on purpose to prove it works.

**Set up a notification**:

1. In Uptime Kuma, open your `first-app` monitor and click **Edit**.
1. Under **Notifications**, click **Set Up Notification**.
1. Choose a **Notification Type** you can receive on your phone. **Discord** is the quickest: in a Discord server you own, open **Server Settings** > **Integrations** > **Webhooks**, create a webhook, then click **Copy Webhook URL** and paste it into the **Discord Webhook URL** field. ([Discord's webhook guide])
1. Check **Default enabled** and **Apply on all existing monitors**.
1. Click **Test**. You should get a test message within a few seconds.
1. Click **Save** on the notification, then **Save** on the monitor.

> **PROTIP**: Prefer email, Slack, or Telegram? Choose that **Notification Type** instead. Uptime Kuma supports [90+ notification services].

**Test the alert by stopping your app**:

1. In CapRover, open `first-app` and go to the **App Configs** tab.
1. Set **Instance Count** to `0`, then click **Save & Update**. Your app is now offline.
1. Watch Uptime Kuma. Within about a minute, `first-app` turns red and you get a **Down** notification.
1. Set **Instance Count** back to `1` and click **Save & Update**.
1. Wait for the monitor to turn green and for the **Up** notification.
1. Post a screenshot of your **Down** notification in the class channel.

> **ASK AUDIENCE:** You stopped the app, but the alert didn't fire right away. Why the delay? How could you make it faster, and what's the tradeoff?

<details>
<summary>Answer</summary>

Uptime Kuma checks once per **Heartbeat Interval** (60 seconds by default), so an outage can go unnoticed until the next check. Lowering the interval (down to 20 seconds) catches outages sooner but adds traffic and makes alerts noisier. Raising **Retries** above `0` does the opposite: fewer false alarms from one slow response, but a slower alert.

</details>

<!-- > -->

## [**10m**] **TT**: Status Pages

**Status pages**:

- Communicate incidents, scheduled maintenances and downtimes.
- Can either be public or private.
  - Private status pages communicate incidents with internal teams only
- Helps you build customer trust.
- Showcase the reliability of your platform
  - Historical uptime
  - Performance timeline of your service
- Control over how you communicate an incident
- Notify customers in real-time with updates to incidents and scheduled maintenance
- Reduce the number of phone calls or tickets when the site goes down.
- Should be hosted on a server other than the one(s) you're monitoring.
- Help you learn more about how fast you respond to downtime.
- Should always be hosted on a different server and network than the servers you're monitoring.

<!-- > -->

## [**10m**] **Activity**: Set Up a Status Page

1. In Uptime Kuma, click **Status Pages**, then **New Status Page**.
1. Enter a **Name** and a **Slug** (for example, `acs3220`), then click **Next**.
1. Under **Add a monitor**, select `first-app`, then click **Save**.
1. Paste the URL for your status page in chat. It looks like `https://uptime.dev.YOURDOMAIN.COM/status/acs3220`.

> **PROTIP**: Hang on to this URL. In the [Volumes](Volumes.md) lesson, you'll point `status.YOURDOMAIN.com` at it.

We will be adding additional alerting and status indicators when we discuss Docker Swarm.

<!-- > -->

## [**15m**] **Break**

<!-- > -->

## [**10m**] **TT**: Resource Monitoring

### Introducing Netdata

**Netdata is**:

1. A metrics collector for system and application metrics (including web servers, databases, containers, and much more)
1. A long-term metrics database that stores recent metrics in memory and "spills" historical metrics to disk for efficient long-term storage
1. A super fast, interactive, and modern metrics visualizer optimized for anomaly detection
1. An alarms notification engine for detecting performance and availability issues

See **[Why Netdata]** for more.

### Industry Use

<img src="https://user-images.githubusercontent.com/2662304/48305662-9de82980-e537-11e8-9f5b-aa1a60fbb82f.png">

Used by Amazon, DigitalOcean, Google, Groupon, Microsoft, NewRelic, Nvidia, TicketMaster, Vimeo, and more!

See [Netdata User Base] for the full list.

### What Netdata Monitors

- CPU
- Memory
- Disks
- Filesystems
- Networking
- Containers
- Log files

See the full list of [supported collectors].

Netdata's data collection is **extensible**, which means you can monitor anything you can get a metric for. You can even write a collector for your custom application!

<!-- > -->

## [**10m**] **Activity**: Working With Netdata

![](https://user-images.githubusercontent.com/1153921/80827388-b9fee100-8b98-11ea-8f60-0d7824667cd3.gif)

<!-- > -->

### Step 1: Enable Netdata

1. Log in to CapRover.
1. Choose **Monitoring** from the sidebar.
1. Click the `Start Netdata Engine` button on the middle of the page.

### Step 2: Enable Alerts

Send email alerts to your gmail account by scrolling down to Notification Settings and entering the following data in the **Email (SMTP)** section:

- **Recipient Email**: `youraccount@gmail.com`
- **Server Tag**: `CapRover`
- **SMTP Server**: `smtp.gmail.com`
- **SMTP Port**: `587`
- **Unsecure**: Leave unchecked
- **SMTP Username**: `youraccount@gmail.com`
- **SMTP Password**: `your_gmail_password` (will be hidden after submitting)

Click `Update Netdata` to finish.

<!-- > -->

### Step 3: Discover the Dashboard

Browse the dashboard by clicking `Open Netdata`!

<!-- > -->

## [**10m**] **Wrap Up**: Poll / Q+A

Present students with a Zoom poll to determine their progress for the day.

Use the remaining time to answer questions before dismissing class.


<!-- omit in toc -->
## Additional Resources

- [Uptime Kuma] on GitHub, plus its [wiki](https://github.com/louislam/uptime-kuma/wiki)
- Uptime Kuma [status page docs](https://github.com/louislam/uptime-kuma/wiki/Status-Page)
- CapRover [One-Click Apps](https://caprover.com/docs/one-click-apps.html)
- [Netdata] on GitHub

<!-- omit in toc -->
## For curriculum authors

<details>
<summary>Notes for instructors and curriculum authors</summary>

- This lab used FreshPing until FreshPing shut down free accounts on 2026-03-06. It switched to self-hosted Uptime Kuma in October 2026.
- Uptime Kuma runs from CapRover's official `uptime-kuma` one-click app: image `louislam/uptime-kuma`, container HTTP port `3001`, WebSocket support on, volume `$$cap_appname-data:/app/data`. As of October 2026, the one-click app's default version is `2.3.2`, while upstream's latest release is `2.5.6`. Students set the version to `2`, the upstream-recommended tag that tracks the latest v2 release.
- The "stop the app" step sets CapRover's **Instance Count** to `0`. CapRover's docs describe instance count but don't explicitly cover `0`, so do a dry run on a fresh droplet before class.
- The monitor shares a droplet with the apps it watches (see the BE AWARE callout). If time allows, have students add a second monitor from a different server, or from a teammate's Uptime Kuma, to show off-box monitoring.
- Discord is the suggested notification channel because it needs no SMTP credentials. Students without Discord can pick any other notification type.

</details>

<!-- do not edit below this line !-->
[Gradescope]: https://www.gradescope.com/courses/133579
[StatusPage.io]: https://statuspage.io
[Uptime Kuma]: https://github.com/louislam/uptime-kuma
[90+ notification services]: https://github.com/louislam/uptime-kuma/tree/master/server/notification-providers
[Discord's webhook guide]: https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks
[Resource Monitoring]: https://caprover.com/docs/resource-monitoring.html
[Netdata]: https://github.com/netdata/netdata
[Netdata User Base]: https://github.com/netdata/netdata#user-base
[Why Netdata]: https://github.com/netdata/netdata#why-netdata
[Supported Collectors]: https://github.com/netdata/netdata/blob/master/src/collectors/COLLECTORS.md
