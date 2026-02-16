# QNA for basic Widnows users

Q:	I have 'Open "Program" with' everyday at 3AM. What's that?

A:	This may be because of bad MS Office 2016 (or other versions) or other programs scheduled tasks. Check the possible tasks at that time by this PowerShell command:
```
Get-ScheduledTask | ForEach-Object {
    $task = $_
    $info = Get-ScheduledTaskInfo -TaskName $task.TaskName -TaskPath $task.TaskPath
    if ($info.LastRunTime.Hour -eq 3) {  # Change this if needed: "... -eq 3...", where 3 for last run at 3:00 AM
        [PSCustomObject]@{
            TaskName     = $task.TaskName
            TaskPath     = $task.TaskPath
            LastRunTime  = $info.LastRunTime
            LastResult   = $info.LastTaskResult
        }
    }
} | Format-Table -AutoSize
```

Q: My Windows install works slow. What I can do to speed it up?

A: There's a few options to do, whenewer you experience low FPS in games or total system unresponciveness - reboot. Sometimes you just need to reboot (properly) your system (and not turn off and on since this is not a full reboot)
