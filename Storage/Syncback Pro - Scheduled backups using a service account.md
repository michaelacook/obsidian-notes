If you want to run a backup schedule using a dedicated service account, you need:
- The service account must have a local profile on the backup appliance
	- You can still disable interactive logon after seeding the profile on the backup appliance
- Backup profile settings must be moved from the default location to a location that can be accessed by all users on the system
	- Do this in **hamburger menu>Global Settings>Settings**
	- Recommended to make a folder in C:\ and add permissions to the folder
- Open task scheduler, locate the backup scheduled tasks, open Properties and change the user to your SA, check **Run whether user is logged on or not**

[Scheduled Tasks not running when using System or Service account](https://help.2brightsparks.com/support/solutions/articles/43000458452-scheduled-tasks-not-running-when-using-system-or-service-account)
[Moving where profile settings are stored](https://help.2brightsparks.com/support/solutions/articles/43000335966)
