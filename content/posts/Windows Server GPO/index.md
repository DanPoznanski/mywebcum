











```
Computer Configuration
└── Policies
    ├── Administrative Templates
    │   └── Windows Components
    │       └── Remote Desktop Services
    │           └── Remote Desktop Session Host
    │               └── Connections
    │                   └── Allow users to connect remotely using Remote Desktop Services → Enabled
    │
    ├── System
    │   └── Remote Assistance → Configure Offer Remote Assistance → Disabled (if you need)
    │
    └── Windows Settings
        ├── Security Settings
        │   ├── Local Policies
        │   │   └── User Rights Assignment
        │   │       └── Allow log on through Remote Desktop Services → Добавь:
        │   │             - Administrators
        │   │             - Remote Desktop Users
        │   │
        │   └── Windows Firewall with Advanced Security
        │       └── Inbound Rules → Включи:
        │             - Remote Desktop - User Mode (TCP-In)
        │             - Remote Desktop - User Mode (UDP-In)
```

```
Computer Configuration
└── Preferences
    └── Control Panel Settings
        └── Local Users and Groups
            └── New → Local Group
Action: Update

Group name: Remote Desktop Users

Add: dan\dan
```


```
gpresult /h C:\gpo-report.html
start C:\gpo-report.html

```