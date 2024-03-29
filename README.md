# Powershell-Quickie: Quick Mailbox MoveRequests realtime monitoring console one-liner

It's sometime useful to real-time monitor your mailbox moves (OnPrem to OnPrem or OnPrem to Exchange Online or vice versa) - you can dedicate a PowerShell window to do this, potentially put that window on a separate screen (with other PowerShell monitoring windows, like perf monitoring for example), and do your other admin stuff on another Powershell window. An example of real-time mailbox move requests monitor PowerShell windows would look like this :

<center><img src=https://user-images.githubusercontent.com/33433229/176021663-5f0e90b3-cfa3-4fcd-9d49-fa4dc63bc43d.png height = "60%" width = "60%" ></center>


> ***Prerequisite** : The below one-liner PowerShell line has to be pasted on a PowerShell console with Exchange Management Tools loaded (either Exchange Online or Exchange 2016/2019/2022).*

Here is a single PowerShell command line, actually several instructions separated by semicolons, to run a sort of realtime monitoring console that shows all your Move Requests in progress, refreshed every 10 seconds (or choose the number of seconds you want for each refresh).

On an Exchange Management Shell console (aka a Powershell session in which you have Exchange management cmdlets loaded), just copy and paste the below one-liner PowerShell instructions to launch a periodic check poll (every 10 seconds) of your Mailbox Move requests:

```powershell
$UpdateDelay=10;while($true){$TimeStampCheck = Get-date;$NextCheck = $TimeStampCheck.AddSeconds(10);$NexStat = get-moverequest -ResultSize Unlimited| Get-MoveRequestStatistics;cls;Write-Host "Check:$TimeStampCheck";Write-Host "Next Check : $NextCheck";$NexStat | ft DisplayName, PercentComplete, SourceDatabase, TargetDatabase, TotalMailboxSize, StatusDetail -a;write-host "Seconds before new refresh: " -nonewline;For ($i=$UpdateDelay;$i -ge 0;$i--){Write-Host "$i" -ForegroundColor green -nonewline;if ($i -eq $($UpdateDelay - 1)){write-host "`b`b "-nonewline}else{write-host "`b" -nonewline};Sleep 1}}
```

The above command line will poll forever the Exchange Move Requests status every 10 seconds, until you interrupt the loop (CTRL+C). You can for example run the above line in a PowerShell session (with Exchange Management Tools loaded) in the background or on another screen with other monitoring stuff, and manage your migration requests and migration batches on another PowerShell session, and you'll see the output evolve in realtime (refreshed every 10 seconds).

The output will look like the below and will run forever, with a refresh every 10 seconds (there is a green count down at the end of the moves list), until you interrupt the process with CTRL+C or by simply closing the Powershell console:

![image](https://user-images.githubusercontent.com/33433229/176021663-5f0e90b3-cfa3-4fcd-9d49-fa4dc63bc43d.png)
