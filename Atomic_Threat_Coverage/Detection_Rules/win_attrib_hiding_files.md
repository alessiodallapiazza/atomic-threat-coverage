| Title                | Hiding files with attrib.exe                                                                                                                                                 |
|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description          | Detects usage of attrib.exe to hide files from users.                                                                                                                                           |
| ATT&amp;CK Tactic    | <ul><li>[TA0005: Defense Evasion](https://attack.mitre.org/tactics/TA0005)</li><li>[TA0003: Persistence](https://attack.mitre.org/tactics/TA0003)</li></ul>  |
| ATT&amp;CK Technique | <ul><li>[T1158: Hidden Files and Directories](https://attack.mitre.org/techniques/T1158)</li></ul>                             |
| Data Needed          | <ul><li>[DN_0003_1_windows_sysmon_process_creation](../Data_Needed/DN_0003_1_windows_sysmon_process_creation.md)</li></ul>                                                         |
| Trigger              | <ul><li>[T1158: Hidden Files and Directories](../Triggers/T1158.md)</li></ul>  |
| Severity Level       | low                                                                                                                                                 |
| False Positives      | <ul><li>igfxCUIService.exe hiding *.cui files via .bat script (attrib.exe a child of cmd.exe and igfxCUIService.exe is the parent of the cmd.exe)</li><li>msiexec.exe hiding desktop.ini</li></ul>                                                                  |
| Development Status   | experimental                                                                                                                                                |
| References           | <ul></ul>                                                          |
| Author               | Sami Ruohonen                                                                                                                                                |


## Detection Rules

### Sigma rule

```
title: Hiding files with attrib.exe
status: experimental
description: Detects usage of attrib.exe to hide files from users.
author: Sami Ruohonen
logsource:
    category: process_creation
    product: windows
detection:
    selection:
        Image: '*\attrib.exe'
        CommandLine: '* +h *'
    ini:
        CommandLine: '*\desktop.ini *'
    intel:
        ParentImage: '*\cmd.exe'
        CommandLine: +R +H +S +A \\*.cui
        ParentCommandLine: C:\WINDOWS\system32\\*.bat
    condition: selection and not (ini or intel)
fields:
    - CommandLine
    - ParentCommandLine
    - User
tags:
    - attack.defense_evasion
    - attack.persistence
    - attack.t1158
falsepositives:
    - igfxCUIService.exe hiding *.cui files via .bat script (attrib.exe a child of cmd.exe and igfxCUIService.exe is the parent of the cmd.exe)
    - msiexec.exe hiding desktop.ini
level: low

```





### es-qs
    
```

```


### xpack-watcher
    
```

```


### graylog
    
```
((Image:"*\\\\attrib.exe" AND CommandLine:"* \\+h *") AND NOT ((CommandLine:"*\\\\desktop.ini *" OR (ParentImage:"*\\\\cmd.exe" AND CommandLine:"\\+R \\+H \\+S \\+A \\\\*.cui" AND ParentCommandLine:"C\\:\\\\WINDOWS\\\\system32\\\\*.bat"))))
```


### splunk
    
```

```


### logpoint
    
```

```


### grep
    
```
grep -P '^(?:.*(?=.*(?:.*(?=.*.*\\attrib\\.exe)(?=.*.* \\+h .*)))(?=.*(?!.*(?:.*(?:.*(?:.*.*\\desktop\\.ini .*|.*(?:.*(?=.*.*\\cmd\\.exe)(?=.*\\+R \\+H \\+S \\+A \\\\.*\\.cui)(?=.*C:\\WINDOWS\\system32\\\\.*\\.bat))))))))'
```



