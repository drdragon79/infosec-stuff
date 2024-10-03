- *UAC* - User Account Control
- Was introduced in windows Vista.
- Allows users to run application with normal user rights, and elevate to administrator rights when required.
# UAC Virtualization
- If an application was written in the windows XP days to perform administrative task, it would not know about the UAC mechanism which was introduced later.
- UAC doesn't allow application to write user-specific data in global locations, or in the *HKLM\Software* hive.
- UAC virtualization, if enabled, automatically translate these global location and hives to local per user area:
	- `C:\Users\drdragon\AppData\Local\VirtualStore`
- For legacy application, virtualization is enabled by default. for newer application, it needs to be enabled. For system processes, applications can never be virtualized.
- The virtualization is also a part of [Access Token](Access%20Token.md).