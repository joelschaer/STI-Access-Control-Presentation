https://192.168.75.131/bWAPP/aim.php

## Vertical Privilege Escalation

# Settings admin
http://192.168.75.131/bWAPP/admin/
https://192.168.75.131/bWAPP/admin/phpinfo.php

## Horizontal Privilege Escalation 

# Directory Traversal - Directories
http://192.168.75.131/bWAPP/directory_traversal_2.php?directory=documents
http://192.168.75.131/bWAPP/directory_traversal_2.php?directory=admin

# Directory Traversal - Files
http://192.168.75.131/bWAPP/directory_traversal_1.php?page=message.txt
http://192.168.75.131/bWAPP/directory_traversal_1.php?page=admin/settings.php


## Business logic exploitation 

# Session Mgmt. - Administrative Portals
http://192.168.75.131/bWAPP/smgmt_admin_portal.php?admin=0
Changed admin=0 to 1 

# Session Mgmt. - Strong Sessions
https://192.168.75.131/bWAPP/smgmt_strong_sessions.php
Changed in cookie security_level from 0 to 2
and changed the URL by passing trhought https://

# Broken Auth. - Logout Management
http://192.168.75.131/bWAPP/ba_logout.php
Suffit de logout et de retourner en arrière

# Restrict Device Access
http://192.168.75.131/bWAPP/restrict_device_access.php

utiliser burp et changer dans le header le périhpérique par 
Mozilla/5.0(iPhone;U;CPUiPhoneOS4_0likeMacOSX;en
-
us)AppleWebKit/532.9(KHTML,likeGecko)
Version/4.0.5Mobile/8A293Safari/6531.22.7

# Restrict Folder Access
http://192.168.75.131/bWAPP/restrict_folder_access.php
just logout and go on 
http://192.168.75.131/bWAPP/documents/
