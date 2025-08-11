### `C:\Users\Username\AppData`
- Each Windows profile has its own appdata folders at `C:\Users\Username\AppData`
- Storing extra data that programs use that aren't part of the original program files - config files, temp files, file caches, etc
- Allows each user having separate configs for a program
- More secure - can't change someone else's files and settings
- `C:\Program Files` requires admin privileges for write - so non-admin users couldn't have their configs and program data stored there
- programs <i>can</i> be installed for a single user by installing them to AppData and may be the default install directory for some programs and will be used by programs that give the option to install for a single user (just me)

### `AppData\Local` and `AppData\Roaming`
- meant for files that stay on one computer 
- as opposed to roaming means files follow a user in a domain environment no matter what computer on the domain they're logged into
	- roaming folder exists even if you aren't on a domain but will do the same thing the Local folder does
	- houses files that *would* be synced if you were on a domain 
- why use local vs roaming or both? compatibility and consistency
- who decides which to use? the developer
- some times large files will get put in local because they are too large and slow
- no hard rule
- some programs don't use AppData and put files in the `C:\Users\Username\Documents` - mRemoteNG does this
	- games often do this because they know people are dumb and won't look elsewhere

#### `AppData\LocalLow`
- same as local, but with low integrity level
- integrity level means trustworthiness of an object or program
- there are 4 integrity levels in Windows:
	- **System** - highest, root, can do anything 
	- **High** - administrator rights
	- **Medium** - regulate user writes
	- **Low** - very restricted in access to files and folders it can access and write to
	- **Untrusted** - lowest
	- **Installer** - special
- each integrity level can only access at it's own level and lower
- e.g., web browsers may run in low or untrusted integrity level for security reasons
	- can see this with Sysinternals - Process Explorer program
- low-integrity programs write to LocalLow

#### `C:\Program Data`
- settings folders for programs
- aren't part of base installation, but also aren't user-specific
- resources that will be used by the program and shared by all users
- ProgramData in Windows XP: `C:\documents and settings\All Users\Application Data`
	- AKA `C:\%HOMEPATH\%ALLUSERSPROFILE%\%APPDATA`
		- AKA `C:\$HomePath%\All Users`
			- AKA `C:\ProgramData`
- backwards compatibility

#### Shortcuts
- `%AppData%` - takes you to `C:\Users\Username\Appdata\Roaming`
	- default, developers tend to use this unless they have a reason to use another folder
- `%LocalAppData%` - takes you to `C:\Users\Username\AppData\Local`
- No shortcut to `C:\ProgramData` because no program should write there