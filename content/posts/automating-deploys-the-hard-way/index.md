---
title: Automating Deployments The Hard Way
date: 2021-07-27T01:30:07.396Z
description: Shipping legacy code without SSH or SFTP
---

Automation is great. I remember when I started my first job a million years ago we were shipping code on demand using sftp from Dreamweaver ***shudder***. We've come so far since then. The proliferation of great services like Travis CI, Circle CI, Jenkins, LaunchDarkly, etc. have propelled us into a new era of wonderful efficiencies. Fast forward to 2021 and my current position and.....we're still shipping code manually? Yes. When I arrived our team owned some _legacy_ infrastructure. This isn't necessarily a bad thing, in fact it's a common reality for a lot of companies. Living with legacy code is one thing but living with antiquated and _manual_ delivery of said code is not something I wanted to continue to deal with.

## Some Informal Background

My first task in onboarding myself into this position was to get the lay of the land. Where are the servers? Where's the code? How does code move around? You get the point. Thankfully I got plenty of help from an existing teammember. After sorting out where all the respositories lived and getting myself up and running with a suitable local development environment my attention turned to 'how to ship code?'. I was able to observe the same helpful teammember deplying code into our existing staging environment. The process was as follows:

1. RDP into staging server
2. Manually create an archive copy of app to be deployed
3. Locally run build
4. Copy local build DLLs and other pertinent files to some folder you own on staging server
5. Copy the files you just copied to the appropriate IIS directory (because permissions)
6. If necessary flush IIS cache by restarting site or recycling app pool
7. Inwardly sob because of all the time you've spent cutting and pasting things

Now, you may look at this and say "hey, come on that doesn't sound that bad"; and sure, if you're a veteran who'd done that process a million times you wouldn't bat an eyelash. As someone who was not a veteran and had done it zero times I could only think "what happens if.....when.....I mess this up". Rolling back was just a process of recopying the backed up files but this whole process was archaic and slow. If you were fast about it you'd probably have things deployed in 5-10 minutes; if you're new or unsure or a healthy amount of paranoid it could take double that. For a small team looking to ship code multiple times a day that's too much sunk time just pushing code around manually.

## Firmly Tying Both Hands Behind Your Back

There was an extra hitch in this whole set up. If you're astute you may have looked at the list in the section above and said "why RDP?" I think I mentioned it already but, we don't own or operate the servers we are targeting. Specifically, that means we don't have control over things like setting up SSH, FTP, or opening ports in a firewall. This was problimatic - my past experiences with services like Circle CI were great because I could easily set up an SSH key and get code from one service to the server very quickly. But, how can we automate moving code to a server that didn't allow for this?

My first thought was to change the IIS site path(s) from local directories to a SMB mount to an Azure file share. This _is_ possible and it should work. Unfortunately, I wasn't ever able to get this to work. I made sure to mount the remote drive as a local user, add that local user to the IIS_USRS group, and ensured that IIS knew which user to try to access the folder as. Nothing I did seemed to shake this loose so I went back to the drawing board.

My second approach would certainly _work_ but it felt less than ideal. I decided I would set up a powershell script to robocopy newer files from the SMB share to the local directory that IIS was serving the app from. Then I could just use the task scheduler to essentially long poll for changes. This I got to work pretty quickly but I thought I could do better.


## Automating Deploys with Azure File Sync

My final approach was to use the Azure File Sync service. This was sold by the Microsoft documentation as 'a way to keep your on premesis files in sync with your Azure file shares'. That sounded like _exactly_ what I needed. There might be other better ways but this worked for us....kind of.

To Microsoft's credit the setup for this was really easy. You basically just need an Azure File Share set up in a storage container. You can then set up a sync service in the Azure portal that works to keep the file share and on premesis server in sync. The final step was to install the server agent on the on premesis server and register it with the Azure sync service. And, VOILA, after a handful of satisfying green checkmarks we were ready to go.

I made a copy of the staging site files and moved them to the location I was syncing with the Azure file share and after a few seconds I was able to see the files in the Azure portal. Wow! Great - so now I just needed to do that in reverse.

## Pipeline or Pipedream?

At this point we have a couple of CI/CD pipelines set up in Azure Devops for newer applications so I was very familiar how to get an automation pipeline set up. As an aside all credit to Microsoft for making pipelines _really_ easy to get set up in Devops. Anyway - after getting a basic pipeline set up to run the build (I need to write a full post on the absurdity of trying to track down msbuild documentation) I was able to get a release set up to copy my build artifact to my Azure file share. I was so close; I could taste the glory; I felt the efficiency surging through my veins. I went ahead and ran my full pipeline and after a few moments my build files were queued for release to my Azure file share and I could close the door on manual deployments (of this app) forever. After my release succeeded I checked my file share in the Azure portal just to ensure the files had been update and they had! Ok, now I just need to verify the local server sync'd. To my great dismay my target folder on the local server remained empty (I had removed the files from my previous test). What the heck was going on? Maybe it's just slow? After waiting five, ten, then fifteen minutes I ruled out slow.

Back to the documentation I went in search of where I made my mistake. But, dear reader, I am happy to report I hadn't made a mistake (besides not reading well enough). Looking at the troubleshooting documentation I came across this:

> Changes made to the Azure file share by using the Azure portal or SMB are not immediately detected and replicated like changes to the server endpoint. Azure Files does not yet have change notifications or journaling, so there's no way to automatically initiate a sync session when files are changed.

Oh man, that's disappointing. Well, if it's not immediate how long? The answer was even more disappointing - 24 hours. Yes, twenty four freaking hours before your files will be shipped back from the Azure file share back to the on premesis server. This would've been something great to know before I even started down this path. My question is - how is this even useful? I mean I guess it serves some purpose becuase it exists but I can't imagine too many use cases where syncing servers can wait a full day. There's actually a pretty lengthy forum post covering the subject [here](https://feedback.azure.com/forums/217298-storage/suggestions/33072151-enable-immediate-sync-after-changes-on-the-azure-f).

Thankfully there _is_ a workaround and it was very easy to implement. There is a powershell cmdlet that was released as a bandaid for just this type of situation. You can just run the Invoke-AzStorageSyncChangeDetection with all the necessary arguments and it should fire the sync from your file share back to the on premesis server - [documentation here](https://docs.microsoft.com/en-us/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection?view=azps-6.2.1). I'm fairly certain this cmdlet is just a wrapper that sends an http request to the storage endpoint and just kicks the filesystem in the rear. But, whatever - it worked! Hurray!

## Conclusion

There's nothing definitive to draw from this experience. The longer I work within the Microsoft toolset I find a lot of thoughtful and useful services and a lot more headscratchers like this one. Hopefully Microsoft keeps pushing in the direction of it's customers and leaves behind these half baked solutions.
