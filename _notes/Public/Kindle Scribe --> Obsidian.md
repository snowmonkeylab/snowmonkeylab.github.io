---
title: Synching Kindle Scribe Notes to Obsidian
feed: show
date: 19-03-2024
---
There is something about a pen and paper that kick-starts my brain. If I have an idea to nurture or a problem to solve - that is where I start. 

I now use the Kindle Scribe for this (other paper-like digital note taking tools are available. Well, one). Stylus on e-ink isn't quite like pen on paper, but is comes close enough to compromise. The ability to organise and share notes digitally bridges the gap.

Once I am ready to progress an idea digitally, I use Obsidian. It provides an organised library of ideas and my original hand-written notes are an important addition. They provide glimpses of the original inspiration that are sometimes lost in the depths of a hard problem. I sync my Obsidian notes between my laptop and phone using Google Drive (other cloud-based file storage platforms are available). There are many guides out there on how to do this.

Kindle Scribe provides the ability to share a notebook via email - as a PDF or converted text. Unfortunately, my handwriting is questionable - too questionable for an algorithm to properly convert. As a result, I use the PDF option. Once I receive the email, I download the file and add it to Obsidian manually. Although this works ok, it is a bit cumbersome. 

With a bit of playing I have worked out a way to automate the process using Make.com - an integration / automation platform. It's free tier provides 1000 events a month - which more than covers this use case:
1. Create a new 'Scenario'
2. Select 'Webhook' as the trigger and then 'Custom Mailhook'. A mailhook provides a designated email address. When an email is sent to this address, the automation is triggered
3. In your chosen email tool configure a filter rule. Personally, I use Gmail. For the rule, use the 'Includes the words' option and 'from your kindle' for the filter text. These words are in the subject of the email sent from Kindle Scribe
4. Then, select 'Forward it to' and use the email address provided by Make. Anytime an email is received from the Scribe, it is forwarded to Make and  triggers the Scenario
5. For the next scenario step, choose 'Test Parser' and 'Get Elements from HTML'. Choose 'Link' for the element type and 'HTML Content' under the HTML input. This isolates all the links in the email sent from Scribe
6. Add a filter to the text parser. For the 'Condition' option, select 'href'. Select 'Contains' for the text operator and enter 'kindle-content-requests'. This filters the links from the Scribe email to the file download
7. Next, add a 'HTTP Get File' node. For the 'URL', choose 'href'. This will download the PDF notebook
8. Last, add a 'Google Drive' node using the 'Upload a File' option. You will need to authenticate Google Drive as part of this step. This requires a bit of configuration. Make provides a guide for this here - https://www.make.com/en/help/app/google-drive#create-and-configure-a-google-cloud-console-project-for-google-drive
9. One authenticated, map the uploaded file to the correct folder in Google Drive. This must be a folder inside your Obsidian vault. Under 'File' choose the 'HTTP - Get a file' option. This will add the PDF file downloaded from the Scribe link to your Obsidian vault
![[Pasted image 20240319235911.png]]
That's it. When you next share a Scribe notebook via email, the automation will be triggered and the file will become available within your Obsidian vault.





