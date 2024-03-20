---
title: Synching Kindle Scribe Notes to Obsidian
feed: show
date: 19-03-2024
---

There is something about a pen and paper. If I have an idea to nurture or a problem to solve - that is where I start. 

I now use the Kindle Scribe for this (other paper-like digital note taking tools are available. Well,  [one](https://remarkable.com/store/remarkable-2)). Stylus on e-ink isn't quite like pen on paper, but is comes close enough to compromise. The ability to organise and share notes digitally bridges the gap.

Once I am ready to progress an idea digitally, I use [Obsidian](https://obsidian.md/). It has allowed my to create an organised library of my ideas - to which the original hand-written notes are an important addition. They provide glimpses of the original inspiration that are sometimes lost in the depths of solving a hard problem. 

I sync my Obsidian notes between my laptop and phone using Google Drive (other cloud-based file storage platforms are available). I prefer the flexibility over using the paid Obsidian Sync option. There are many guides out there on how to do this. Here is [one](https://www.makeuseof.com/sync-obsidian-vault-across-devices/).

Kindle Scribe provides the ability to share a notebook via email - as a PDF or converted text. Unfortunately, my handwriting is questionable - too questionable for an algorithm to reliably convert. As a result, I prefer the PDF option. Once you receive the email, the file can be downloaded and added to Obsidian manually. Although this works ok, it is a bit cumbersome. 

With a bit of playing, I have automated this process using [Make.com](https://make.com) - an integration / automation platform. It's free tier provides 1000 events a month - which more than covers this use case. It looks something like this:

1. Create a free account with Make
2. Create a new 'Scenario'
3. Select 'Webhook' as the trigger and then 'Custom Mailhook'. This provides a designated email address. When an email is sent to the address, the automation is triggered
4. In your email tool of choice configure a filter rule. The rule should trigger when an email is received where the subject contains - 'from your kindle'
5. Configure the rule to forward email matching the filter to the address provided by Make. Anytime an email is received from the Scribe, it is forwarded to Make and triggers the automation. You may need to approve the Make email as a forwarding address (this is a requirement for Gmail). To do this, access the approval email in Make by navigating to your scenario --> incomplete executions. Find the approval link in the email and paste it in your browser
6. For the next scenario node, choose 'Text Parser' and 'Get Elements from HTML'. Choose 'Link' for the element type and 'HTML Content' under the HTML input. This isolates all the links in the email sent from Scribe - including the PDF download link
7. Add a filter to the text parser:
	1. For the 'Condition' option, select 'href'.
	2. Select 'Contains' for the text operator and enter 'kindle-content-requests'. This filters the links from the Scribe email to isolate the PDF download
8. Next, add a 'HTTP Get File' node. For the 'URL' field, choose 'href' (the PDF download link isolated in the previous step). This will access the link and download the PDF notebook
9. Last, add a 'Google Drive' node and use the 'Upload a File' option. You will need to authenticate Google Drive, approving that Make can access and upload files. Make provides a guide for this [here](https://www.make.com/en/help/app/google-drive#create-and-configure-a-google-cloud-console-project-for-google-drive)
10. Once authenticated, map the PDF to the correct folder in Google Drive. This must be a folder inside your Obsidian vault. For 'File' choose the 'HTTP - Get a file' option (the downloaded PDF notebook from the previous step). This will add the PDF file downloaded from the Scribe link to your Obsidian vault

![Alt Text](/assets/img/make_sync_scribe.png "Make Scenario")

That's it! When you next share a Scribe notebook, the automation will be triggered and the PDF will become available in your Obsidian vault.