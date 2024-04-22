![Depression Center Logo](https://github.com/DepressionCenter/.github/blob/main/images/EFDCLogo_375w.png "depressioncenter.org")

# Sleep Data Automation

## Description
Automated sleep data cleanup and processing to harmonize Fitbit data obtained via Fitabase with self-reported sleep diary entries sent via SMS-to-Email. This project uses Microsoft Power Automate for processing sleep diary entries in a shared mailbox, and Excel / Power Query for loading and cleaning sleep stages and HRV data.

### What this automation does:
+ Parse sms-to-email messages to get a list of sleep markers (self-reported sleep/wake times) into a spreadsheet
+ Decipher the correct sleep/wake times based on sleep stages from Fitabase (because Fitbit includes time in bed before and after sleep sometimes)
+ Identify the main sleep episode and filter out naps, based on configurable settings (longest duration, latest sleep episode, etc.)
+ From that, get the correct total time asleep, time awake, time in bed, and other measures
+ Use the calculated sleep/wake times to get the correct HRV values falling only inside that time window, and calculate averages for RMSSD/HF/LF
+ Combine sleep stages, HRV and email markers (sleep diary)
+ Optionally, adjust the sleep/wake up times based on self-reported times automatically when actual vs self-reported differ by a certain number of minutes
+ Option to enter manual time adjustments in an override file, which will will override both Fitbit and self-reported times
+ Present everything in a single sheet with one row per participant, per day, per sleep episode
+ Show a weekly summary with a drop-down to select the study participant and the week


![Sleep Data Automation Output Screenshot](https://github.com/DepressionCenter/SleepDataAutomation/blob/main/images/SleepDataAutomation-Screenshot.png?raw=true "Sleep Data Automation Output Example")


![Sleep Data Automation Weekly Summary Screenshot](https://github.com/DepressionCenter/SleepDataAutomation/blob/main/images/SleepDataAutomation-WeeklySummary-Screenshot.png?raw=true "Sleep Data Automation Weekly Summary Example")



## Quick Start Guide
+ Download the Excel-PowerQuery directory
+ Save your Fitbit, sleep markers, and sleep override files to the CSV directory
+ Open CleanSleepData.xlsx, adjust the parameters in Power Query, and refresh all data
+ To use sms-to-email sleep markers, import the Power Automate zip file into your Power Automate environment, adjust the mailbox name and output location for OneDrive, and create the needed directories in the mailbox




## Documentation
Documentation for this project is available at the Eisenberg Family Depression Center's [Knowledge Base](https://teamdynamix.umich.edu/TDClient/210/DepressionCenter/KB/ArticleDet?ID=11822).

### Detailed Setup
+ Download the Excel-PowerQuery directory.
+ Login to Fitabase, and create a "merged" export that includes 30-second sleep stages and 5-minute HRV. Be sure to click the "merge" checkbox so all participants are included in one file per measure.
+ Download and unzip the export, and place the two CSV files in the Excel-PowerQuery\CSV sub-directory.
+ Open CleanSleepData.xlsx in Excel. Alternatively, load CleanSleepData-PowerQuery.odc in PowerBI Desktop.
+ In Excel, right-click on the data table and select Table -> Edit Query to open Power Query.
+ Find the CSV-Directory parameter and enter your local path to the CSV sub-directory.
+ Adjust the other optional parameters for automatically adjusting times based on sleep diary entries, how to identify main sleep vs naps, etc.
+ Click Refresh Preview. If there are no errors, click File -> Save and Load.
+ In Excel, under the Data ribbon, click Refresh All.
+ The data table will now have a row for every participant, per day, with measures such as sleep start/end times, sleep duration, and average RMMSD / HF / LF.
+ To use sleep markers, setup a shared mailbox to which participants can send an SMS/MMS message. Participants should send a text to that email address with the format "ParticipantID sleep" upon going to sleep, and "ParticipantID wake" upon waking up.
+ In the shared mailbox, create two top-level folders called "Data-Processed" and "Data-Failed". Power Automate will automatically move emails to one of these folders after processing. Anything under Data-Failed will need to be processed manually.
+ Login to OneDrive for Business (if you are a Michigan Medicine user, login using your level-2 credentials). Create a folder called EmailSleepMarkers or similar. In this project, we use a mailbox called "sleeptimes@....edu" so the folder is called EmailSleepMarkers-SleepTimesMailbox. Upload the sample EmailSleepMakers.xlsx spreadsheet to this folder, and clear all rows except the headers.
+ Login to [Power Automate](https://make.powerautomate.com) (if you are a Michigan Medicine user, login using your level-2 credentials). Go to My Flows -> Import -> Import Package (Legacy) and upload a copy of ParseMessagesInSleepTimesMailbox.zip (from the PowerAutomate\packages sub-dirctory). When prompted, enter your credentials to connect to OneDrive and Outlook.
+ Once imported, edit the flow and change the shared mailbox email address in the 2nd step, under "Set Shared Mailbox Address Here." Then scroll down to the last step, and change the path to your EmailSleepMarkers.xlsx spreadsheet (saved to OneDrive) under the step titled "Add a row into a table."
+ Enable the flow. By default it will run every 30 minutes and process 25 emails at a time. Do not run it more often than this to avoid being throttled by the Microsoft Graph API.
+ Emails will be processed and saved to the spreadsheet in OneDrive. Once a week, copy the spreadsheet from OneDrive and place it in the CSV sub-directory. Refresh the data, and the sleep markers will show alongside the sleep and HRV data.



## Additional Resources
+ Mobile Technologies KB Article - [Sleep Data Automation: Overview](https://teamdynamix.umich.edu/TDClient/210/DepressionCenter/KB/?CategoryID=847)
+ Mobile Data Expert Network (MDEN) - [MDEN Code Repository](https://github.com/DepressionCenter/MDEN)


## About the Team
The Mobile Technologies Core provides investigators across the University of Michigan the support and guidance needed to utilize mobile technologies and digital mental health measures in their studies. Experienced faculty and staff offer hands-on consultative services to researchers throughout the University – regardless of specialty or research focus.



## Contact
To get in touch, contact the individual developers in the check-in history.

If you need assistance identifying a contact person, email the EFDC's Mobile Technologies Core at: efdc-mobiletech@umich.edu.



## Credits
#### Contributors:
##### Eisenberg Family Depression Center [(@DepressionCenter)](https://github.com/DepressionCenter/):
+ [Dr. Cathy Goldstein, M.D., M.S.](https://depressioncenter.org/become-member/our-members/cathygo)
+ [Gabriel Mongefranco](https://gabriel.mongefranco.com/) [(@gabrielmongefranco)](https://github.com/gabrielmongefranco)
##### [Sleep and Circadian Research Laboratory](https://medicine.umich.edu/dept/psychiatry/programs/sleep/sleep-circadian-research-laboratory):
+ [Dr. Helen J. Burgess, Ph.D.](https://medicine.umich.edu/dept/psychiatry/helen-burgess-phd)
+ [Moony Rizvydeen](https://www.linkedin.com/in/muneer-rizvydeen-b3b26254?original_referer=https%3A%2F%2Fdepressioncenter.org)
+ [Zainab Fayyaz](https://mcommunity.umich.edu/person/fzainab) 



#### This work is based in part on the following projects, libraries and/or studies:
+ "IBD-Sleep: A Pilot Study Looking at Changes in Sleep Timing and IBD Symptoms." Principal investigator: [Helen J. Burgess, Ph.D.](https://medicine.umich.edu/dept/psychiatry/helen-burgess-phd)



## License
### Copyright Notice
Copyright © 2024 The Regents of the University of Michigan


### Software and Library License
This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program. If not, see <https://www.gnu.org/licenses/gpl-3.0-standalone.html>.


### Documentation License
Permission is granted to copy, distribute and/or modify this document 
under the terms of the GNU Free Documentation License, Version 1.3 
or any later version published by the Free Software Foundation; 
with no Invariant Sections, no Front-Cover Texts, and no Back-Cover Texts. 
You should have received a copy of the license included in the section entitled "GNU 
Free Documentation License". If not, see <https://www.gnu.org/licenses/fdl-1.3-standalone.html>



## Citation
If you find this repository, code or paper useful for your research, please cite it.  

>_Mongefranco, Gabriel; Rizvydeen, Moony; Fayyaz, Zainab; Burgess, Helen; Goldstein, Cathy (2024). Sleep Data Automation. University of Michigan. Software. https://michmed.org/sleepdata_  
​​​​​​​    >_DOI: 10.6084/m9.figshare.25669173.v1_



----

Copyright © 2024 The Regents of the University of Michigan
