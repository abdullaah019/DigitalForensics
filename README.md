<h1>Windows Investigation 1 - 2 Lab </h1>


<h2>Description</h2>
This lesson will focus on artifacts that can be gathered from systems running Windows, specifically focusing on artifacts related to the use of applications and programs on the system. 
<br />


<h2>artifacts</h2>

- <b>LNK Files</b> 
- <b>Prefetch Files</b>
- <b>Jump List Files</b> 

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)

<h2>Lab walk-through:</h2>

 What is the full file name of the PlagueRat file?
Open Windows File Analyzer by right-clicking it and selecting ‘Run as Administrator’ (C:\\Users\\BTLOTest\\Desktop\\Windows Investigation One\\WFA.exe), then choose File > Analyze Shortcuts... and choose C:\\Users\\BTLOTest\\Desktop\\Windows Investigation One\\Shortcuts folder.
Next, look a the row (marked in red) and look at the file name (marked in yellow).
We can see that ‘Plaguerat' is found in the Filename column. Looking along the row we find lots of useful information.

 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/464a8b2d-76ef-41b8-9688-4097bf849b28)


What is the name of the ZIP file that PlagueRat was found in?

 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/5bb709c2-bf2a-48b6-bbe7-289d5f91fca8)

 Analyze the .LNK files in the Shortcuts folder using Windows File Analyzer - What other file or files were in the ZIP file?
 
 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/5f3e48e0-d26f-4392-b73b-56c9a2b7431f)

- Analyze the Prefetch files using PECmd.exe - What is the full file name of the PlagueRat file in CMD.EXE-89305D47.pf?
Open a command prompt and move to the location of PECmd.exe using cd "C:\Users\BTLOTest\Desktop\Windows Investigation One\PECmd\" , then run the following command:
PECmd.exe -f "C:\Users\BTLOTest\Desktop\Windows Investigation One\Prefetch\CMD.EXE-89305D47.pf"
 
 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/88692327-9bba-45b7-a149-293ed07fb555)

Look for PLAGUERAT in the output on Line 13, where the file is found with the extension ‘.bat’ - a batch script file.![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/fd826d8b-1725-4837-aef2-c1ab5cfe762e)


Analyze the Prefetch files using PECmd.exe - What is the full file path of the PlagueRat file in CMD.EXE-89305D47.pf? (Starting with \USERS\)![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/106fb5ad-337e-4901-a4de-a2b2be022d00)

Using the above screenshot from Q4 we can see the full file path listed on Line 13.

Analyze the Prefetch files using PECmd.exe - Open all .pf files by pointing PECmd at the /prefetch/ directory. Add the following to your command -k "plaguerat.ps1" to highlight rows that mention this string in red - What two applications were used to open PlagueRat.ps1?
Open a command prompt and move to the location of PECmd.exe, then run the following command:
PECmd.exe -k “plaguerat.ps1” -d "C:\Users\BTLOTest\Desktop\Windows Investigation One\Prefetch\"
Because we've used the -k flag to highlight string matches (PECmd.exe will also match certain strings by default, so not every red-highlighted line will be relevant to us) we can look for red lines to see if they contain ‘plaguerat.ps1’ or not. Going from the bottom of the output upwards we can find our first match:
 
 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/f7c0bdb6-2f73-43cb-8089-163ca2bc54a0)

Scrolling up, we find what prefetch file, and therefore the executable name, that interacted with our suspicious file:![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/0e6d1292-1215-40a9-b658-fd70dfc8c5b6)

 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/092ee0e8-a426-4c32-9516-50c68c138183)

Going up again, we find another result:

 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/87bc048f-592f-4994-9f28-1bdbad23ddeb)

 Finding the top of this section, we can find the executable that ran it:

  ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/9aa9f725-ed13-4414-9038-dbac4c82ad71)

Analyze the Jump List using JumpList Explorer.exe - Find the website that was accessed by the user. What is the domain they visited?

Since we are looking for a website, the best thing to look for is a browser. There are two entries for Microsoft Edge. In the bottom left panel we can see that a visited website is listed.

 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/386aa3b4-166e-48f6-b28f-09cd49fa4ac6)

 Question 8 - Analyze the Jump List using JumpList Explorer.exe - What browser was used to access this site?
Based on the previous question, we know this was Microsoft Edge, or Edge.
![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/e403a54d-d5de-4ba1-9386-58e0a65e5ff5)



<h2>Lab walk-through Part 2:</h2>

Question 1 - What web browsers have been used by the employee, listed in alphabetical order? (Use simplified browser names)

Firstly, we'll open Browser History Viewer (BHV) inside the Windows Investigation Two folder on the Desktop. Once it has loaded, we'll go to File > Load History.

  ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/f2cbd5e9-fcf5-4277-ad42-88996fe5228c)



 

When we're prompted to load history capture, we'll click on the “…” button, and find the Capture folder.

  ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/d1b265d1-b061-48a8-beb4-724e7f6dd2c4)



 

Once BHV has imported the data, we can begin answering the questions!

On the first screen we can see the column on the far-right is titled ‘Web Browser (Profile)’. Using this we can identify the web browsers that have been used on the system. We simply need to read through the list and take note of the browser names. It's very important to note that there are 4 pages, which can be navigated using the arrows on the left-hand side of BHV. As the question is asking for simplified names, instead of ‘Edge Legacy’ we would enter ‘Edge’. We're also asked to provide them in alphabetical order, so we'll make sure we enter them correctly.

  ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/4e3b3da6-3d55-4458-91db-0cbf2f3d0bb4)



 

 

 

Question 2 - The company has a policy against employees visiting social media on corporate devices. What sites has the employee visited, listed alphabetically?

To answer this question we need to focus on the ‘URL’ column, where we can see exactly what web resources have been visited by the user. This is a case of going through them all to identify the names of social media websites.

 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/e52defd1-b5d9-4f41-894e-5e4848cb8fb7)




 

If you're not sure whether a platform is considered social media or not, a quick Google search can help you understand. As an example:

  ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/729c9ed3-aa33-409a-ac1e-212654edf8c7)



 

To help you answer this question, there are 5 sites that we deem to be classed as social media. Make sure to put them in alphabetical order! We accept multiple variations for this answer.

 

 

 

Question 3 - Looking at Cached Images at (UTC) 19/06/2020 12:12:10 with the filename "everything-in-one-place-scenario-base[1].png", what is the subject line of the third email?

For this question we're provided a date to help us identify the file of interest. We have two ways to find this date, which is stored in the ‘Last Fetched’ column of the Cached Images tab of BHV.

Option One - Click on the header for Last Fetched to change the results to show in time ascending or time descending order. If the timestamps are in order, it'll be easier for us to find the correct timestamp.
Option Two - Use the ‘Filter by date’ section on the right-hand panel. For this lab all of the results are on the same day, however in a real-world scenario, there would most likely be days or weeks worth of data, so this feature would be useful to look at specific days of activity easily.
 
 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/af6e8695-3788-4182-842b-0f5cf29f8b14)


 

Using the Last Fetched column, we find the right time and file (based on the Filename column).

  ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/c7bf8125-1f89-46af-9a52-e4a80ed03f86)



 

Looking at the bottom panel of BHV, if we select a file in the table, it will highlight the associated image. We can double-click the image to show it full size in a new window. Here we can get the answer to this question.

 

 

 

Question 4 - What is the full URL of the music video the employee is listening to on Youtube?

For this question we're going to head back to the Website History tab in the top left. Instead of searching through all of the URLs to find YouTube ones, we can use the ‘Filter by keyword’ search box in the top-right. If we type in YouTube, we'll only see URLs or Titles that include the string ‘youtube’.

After searching for this, we can quickly find the answer to the question.

  ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/5b379f69-62fa-4973-b90e-7eb8559ed4c5)



 

 

 

Question 5 - What is the name of the malicious file that was downloaded by the employee?

To approach this question, we have three primary methods:

Option One - Manually review all URLs until we see URLs that end in a filename, highlighting a download URL (such as securityblue.team/downloads/file.exe). This method will be time-consuming depending on the volume of records, but you could also identify other activities of interest during the search.
Option Two - We could utilize the ‘Filter by keyword’ search box to look for file extensions, searching for phrases such as ‘.zip’, ‘.exe’, and others. These searches will reduce noise from records that don't contain file names, but we would need to go through every file type until we find what we're looking for. If we forget to search for a file type, we might miss the related activity.
Option Three - All downloaded files in BHV are prefixed with “file:///”. We can use this to see the name of the file, and where it was saved on the system. This is the quickest way, and once we have retrieved the filename, we can then use that in the search box to find where it was downloaded from!
 

Using option three and filtering by most recent Date Visited first, we find a ZIP file with an unusual name. This is definitely worth investigating further!

 
 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/443db849-b187-402a-8101-b7e7a6e13c00)


 

 

 

Question 6 - Based on the employee's browsing history around the time of the file download, what is the likely source of the malicious URL?

Now that we've found the suspicious file, we can start to look deeper. While the URL that's hosting the file is interesting to us, looking back further in time we can understand exactly how they got there. Searching for the filename we can find the hosting URL.

 
 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/142ef454-0985-4e7d-ab60-c39018fd11a0)


 

So we know that the user visited that URL and downloaded the file to their Desktop at 12:14:09. Let's clear all our filters by clicking on ‘Reset’ on the right-hand side. Next we'll use the Date Visited column to find the timestamp 19/06/2022 12:14:09, allowing us to see the records that occurred before this download event.

 
 ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/effff00f-6cab-447e-81fa-2079eea65777)


 

Based on the information above, we can infer that the following journey was taken by the user:

1) User goes to Outlook's login page.
2) User successfully logs in and goes to their email inbox.
3) User downloads the zip file, most likely from an email in their inbox. The file is downloaded to their Desktop.
 

It's worth mentioning that we cannot be 100% sure that this activity represents a URL within an email, without actually having access to their inbox to confirm this finding. It is completely possible that the user logs in to their email account and then manually types in the URL within a browser to visit it. However, based on the information available to us, and the timestamps that are all very close together, the most likely explanation is a link within an email.

Therefore, the answer for this question will be the domain shown in the highlighted section 2.

 

 

 

Question 7 - What is the domain name where the malware was downloaded from?

We already retrieved the answer for this question in Q6 above (shown in the highlighted section 3).

 

 

 

Question 8 - Search for the malware URL on https://urlhaus.abuse.ch (outside of the lab) to find out what type of malware has been downloaded

Visiting URLhaus and searching for the full download URL, we can see based on the tags that this is Quakbot malware!

  ![image](https://github.com/abdullaah019/DigitalForensics/assets/139023222/ecda0b88-c45e-4060-9449-e50de69e684c)



 

If you want to learn more about this URL, you can click on the hyperlinked URL in the search results (don't worry, this just takes you to a more detailed page).

 


 












 

 


 



<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
