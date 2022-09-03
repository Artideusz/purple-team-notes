# Windows Registry

Windows registry is a large database containing settings and configurations of the OS. Folders, called "hives" contain rows of key-value data responsible for changing the OS and application behaviour. The *keys have to have a case insensitive name without backslashes*.

There are 7 root keys (root hives), I will attempt to explain every purpose the root keys one by one:

## 1. HKEY_LOCAL_MACHINE (HKLM)
This registry is responsible for storing local computer settings, system-wise in other sense. Applications can't create any additional subkeys here and the root key is maintained in memory, rather than stored on disk. It contains 4 subkeys on Windows NT:
- SAM
    - Security Accounts Manager
- SECURITY
- SYSTEM
- SOFTWARE


## 2. HKEY_CURRENT_CONFIG (HKCC)

## 3. HKEY_CLASSES_ROOT (HKCR)

## 4. HKEY_CURRENT_USER (HKCU)

## 5. HKEY_USERS (HKU)

## 6. (EXTRAS) HKEY_PERFORMANCE_DATA (HKPD)
This registry is used only on Windows NT, and it is invisible on other windows versions.

## 7. (EXTRAS) HKEY_DYN_DATA (HKDD)
This registry is only used in Windows 9x, and is still visible on Windows 10.