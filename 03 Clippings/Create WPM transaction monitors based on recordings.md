---
title: Create WPM transaction monitors based on recordings
source: https://documentation.solarwinds.com/en/success_center/wpm/content/orionseumagtransactionscreating.htm
author: 
published: 
created: 2024-12-17
description: How to create a WPM transaction monitor by assigning a WPM recording to a playback location.
tags:
  - clippings
  - LogicMonitor
---
After creating a WPM recording, you can use the Add Transaction wizard to create a WPM transaction monitor by assigning the recording to a WPM Player hosted on a system at a specific location, known as a [transaction location](https://documentation.solarwinds.com/en/success_center/wpm/content/orionseumagplaybacklocationsmanaging.htm) or "playback location." You can also assign a single recording to different transaction locations to create multiple transaction monitors that run the same steps.

Each WPM transaction monitor requires:

- A recording, and
- An assigned transaction location.

## Create a transaction monitor with the Add Transaction wizard

To create a transaction monitor, select a recording and assign it to a playback location:

1. Click Settings > All Settings > WPM Settings.
2. Click Add a Transaction Monitor to open the Add Transaction wizard.

You can also open the wizard from the [Discovery Central](http://www.solarwinds.com/documentation/en/flarehelp/npm/content/npm-discovery-central-sw88.htm) page.
3. Select a recording and click Next.

To import a recording, click Import, browse to the file, and click Import.
4. Select the location where you want to play the recording, and then click Next.

To add a new location, see [Add transaction locations in WPM](https://documentation.solarwinds.com/en/success_center/wpm/content/orionwpmagaddingalocation.htm).

After you assign a recording to a location, define the properties for the transaction monitor.

1. Enter a Description.
2. Select a Playback interval to specify how frequently you want to play the transaction.
3. Select [Set thresholds in WPM](https://documentation.solarwinds.com/en/success_center/wpm/content/orionseumagtransactionsthresholds.htm) for each step in the transaction.
4. (Optional) To use a proxy server, click Advanced, and enter the server address in the Proxy URL field. For details, see [Configure proxies for WPM transaction locations and transactions](https://documentation.solarwinds.com/en/success_center/wpm/content/orionseumphproxy.htm).
5. To enable screen captures, click Advanced, and enable that option.
6. Click Save Monitor.

To improve transaction troubleshooting, set dependencies on transaction steps as well as transactions, providing an increased level of granularity to isolate dependencies at the level of single browser actions. See [Configure WPM transaction dependencies](https://documentation.solarwinds.com/en/success_center/wpm/content/orioncoreagmanagingdependencies.htm).