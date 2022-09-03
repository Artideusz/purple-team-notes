# Useful Commands

## Powershell

- `[Environment]::OSVersion` - Check Windows OS Version (Useful to check patches)

## Cmd

- `winver` - Check Windows OS Version.
- `net user <username> <password> /add` - Create a new user
- `net user <username> /logonpasswordchg:yes` - Set user to change password on logon.
- `net user <username> /passwordchg:no` - Disable password change access
- `net user <username> /times:<time_format>` - Disable access variable by time
    - time_format examples:
        - Mon-Fri,10-22
- `net user <username> /expires:<expiry_date>` - Set expiry date for user.