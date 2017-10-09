
Microsoft Azure-felhőszolgáltatásban problémák diagnosztizálása szükséges virtuális gépeken hello szolgáltatás-naplófájl összegyűjtése a hello hibák bekövetkezésekor. Hello AzureLogCollector bővítmény igény szerinti tooperfom egyszeri gyűjtemény naplók egy vagy több Cloud Service virtuális gépeken (a egyaránt webes és feldolgozói szerepkörök), és átviteli hello gyűjtött fájlok tooan Azure storage-fiókok – az összes távoli bejelentkezés nélkül is használhatja a virtuális gépek hello tooany.

> [!NOTE]
> A legtöbb hello naplózott adatok leírása http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp helyen találhatók meg.
> 
> 

Gyűjtemény két módja van hello típusú összegyűjtött fájlok toobe függ.

* Az Azure Vendég ügynök naplók csak (GA). A gyűjtemény üzemmódja összes hello naplók kapcsolódó tooAzure vendégügynökök és egyéb Azure összetevőket tartalmaz.
* Összes napló (teljes). A gyűjtemény üzemmódja összegyűjti az összes fájl plusz GA módban:
  
  * rendszer- és eseménynaplók
  * A HTTP hibanaplókat.
  * IIS-napló
  * Telepítési naplók
  * egyéb rendszer naplóit

Mindkét gyűjtemény módban további adatok gyűjteménymappához struktúra a következő hello gyűjteménye használatával adhatók meg:

* **Név**: hello hello gyűjtemény nevét, hello neveként almappa hello zip-fájl toobe belül használt gyűjti.
* **Hely**: hello elérési toohello mappa hello virtuális gépen fájl hova legyenek összegyűjtve.
* **SearchPattern**: gyűjtött fájlok toobe hello nevei hello mintáját. Alapértelmezett érték a "*"
* **Rekurzív**: Ha hello fájlok lesznek hello mappa alatt gyűjtött rekurzív módon.

## <a name="prerequisites"></a>Előfeltételek
* Generált toosave zip bővítményfájlok toohave tárfiók van szükség.
* Meg kell győződnie arról, hogy azok be Azure PowerShell-parancsmagok V0.8.0 vagy újabb. További információkért lásd: [Azure letölti](https://azure.microsoft.com/downloads/).

## <a name="add-hello-extension"></a>Hello-bővítmény hozzáadása
Használhat [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) parancsmagok vagy [Service Management REST API-k](https://msdn.microsoft.com/library/ee460799.aspx) tooadd hello AzureLogCollector bővítmény.

A Felhőszolgáltatások hello a meglévő Azure Powershell-parancsmag **Set-AzureServiceExtension**, felhőalapú szolgáltatás szerepkörpéldányokat használt tooenable hello kiterjesztés lehet. Minden alkalommal, amikor a bővítmény engedélyezve van ez a parancsmag használatával, naplógyűjtést akkor váltódik ki, a kiválasztott szerepkörök hello kijelölt szerepkör példánya.

A virtuális gépek, a meglévő Azure Powershell-parancsmag hello **Set-AzureVMExtension**, használt tooenable hello bővítményt a virtuális gépeken is lehet. Minden alkalommal, amikor a bővítmény engedélyezve van a hello parancsmagokon keresztül, az egyes példányok naplógyűjtést váltja ki.

Ehhez a kiterjesztéshez belső JSON-alapú PublicConfiguration hello és PrivateConfiguration használja. hello hello elrendezés egy minta JSON nyilvános és titkos konfiguráció látható.

### <a name="publicconfiguration"></a>PublicConfiguration
    {
        "Instances":  "*",
        "Mode":  "Full",
        "SasUri":  "SasUri tooyour storage account with sp=wl",
        "AdditionalData":
        [
          {
                  "Name":  "StorageData",
                  "Location":  "%roleroot%storage",
                  "SearchPattern":  "*.*",
                  "Recursive":  "true"
          },
          {
                "Name":  "CustomDataFolder2",
                "Location":  "c:\customFolder",
                "SearchPattern":  "*.log",
                "Recursive":  "false"
          },
        ]
    }

### <a name="privateconfiguration"></a>PrivateConfiguration
    {

    }

> [!NOTE]
> Ehhez a bővítményhez nem szükséges **privateConfiguration**. Csak egy üres struktúra biztosíthatja hello **– PrivateConfiguration** argumentum.
> 
> 

Hello két kövesse a következő lépéseket tooadd hello AzureLogCollector tooone vagy több példány egy felhőalapú szolgáltatás vagy a virtuális gép a kiválasztott szerepkörök, mely eseményindítók minden virtuális gép toorun gyűjteményeit hello és tooAzure fiók hello gyűjtött fájlok küldése a megadott.

## <a name="adding-as-a-service-extension"></a>Egy bővítmény hozzáadása
1. Hajtsa végre a hello utasításokat tooconnect Azure PowerShell tooyour előfizetés.
2. Adja meg a hello szolgáltatás neve, a tárhely, a szerepkörök és a szerepkör példányok toowhich szeretné, hogy tooadd és hello AzureLogCollector bővítmény engedélyezéséhez.
   
        #Specify your cloud service name
        $ServiceName = 'extensiontest2'
   
        #Specify hello slot. 'Production' or 'Staging'
        $slot = 'Production'
   
        #Specified hello roles on which hello extension will be installed and enabled
        $roles = @("WorkerRole1","WebRole1")
   
        #Specify hello instances on which extension will be installed and enabled.  Use wildcard * for all instances
        $instances = @("*")
   
        #Specify hello collection mode, "Full" or "GA"
        $mode = "GA"
3. Adja meg a fájlok gyűjtenek hello további adatok mappa (Ez a lépés nem kötelező).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
   
   > [!NOTE]
   > Használhatja a token `%roleroot%` toospecify hello szerepkör gyökérmeghajtóján óta nem használ rögzített meghajtóra.
   > 
   > 
4. Adja meg a hello az Azure storage-fiók nevét és a kulcs toowhich gyűjtött fájlok lesz feltöltve.
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
5. Hello (hello hello cikk végén található) SetAzureServiceLogCollector.ps1 követi tooenable hello AzureLogCollector bővítményként kérjen egy felhőalapú szolgáltatás. Ha hello végrehajtása befejeződött, a hello feltöltött fájl található`https://YouareStorageAccountName.blob.core.windows.net/vmlogs`
   
        .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList

hello hello átadott toohello parancsfájl hello definíciója látható. (Ez másolódik alatt is.)

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
      [Parameter(Mandatory=$true)]
    [string]   $ServiceName,

    [Parameter(Mandatory=$false)]
    [string[]] $Roles ,

    [Parameter(Mandatory=$false)]
    [string[]] $Instances,

    [Parameter(Mandatory=$false)]
    [string]   $Slot = 'Production',

    [Parameter(Mandatory=$false)]
    [string]   $Mode = 'Full',

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountName,

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountKey,

    [Parameter(Mandatory=$false)]
    [PSObject[]] $AdditionDataLocationList = $null
    )

* *Szolgáltatásnév*: A felhőszolgáltatás neve.
* *Szerepkörök*: a szerepköröket, például "WebRole1" vagy "WorkerRole1" listáját.
* *Példányok*: hello nevei a szerepkörpéldányok vesszővel elválasztott – hello helyettesítő karakterlánc ("*") tartozó összes szerepkörpéldányt.
* *Tárolóhely*: tárhely neve. "Éles" vagy "Tesztelés".
* *Mód*: gyűjtemény módja. "Teljes" vagy "GA".
* *StorageAccountName*: neve az Azure storage-fiók tárolására összegyűjtött adatokat.
* *StorageAccountKey*: neve az Azure tárfiók kulcsa.
* *AdditionalDataLocationList*: a következő struktúra hello listáját:
  
      {
      String Name,
      String Location,
      String SearchPattern,
      Bool   Recursive
      }

## <a name="adding-as-a-vm-extension"></a>A Virtuálisgép-bővítmény hozzáadása
Hajtsa végre a hello utasításokat tooconnect Azure PowerShell tooyour előfizetés.

1. Adja meg a hello szolgáltatás neve, a virtuális gép és a hello gyűjtemény módja.
   
        #Specify your cloud service name
        $ServiceName = 'YourCloudServiceName'
   
        #Specify hello VM name
        $VMName = "'YourVMName'"
   
        #Specify hello collection mode, "Full" or "GA"
        $mode = "GA"
   
        Specify hello additional data folder for which files will be collected (this step is optional).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
2. Adja meg a hello az Azure storage-fiók nevét és a kulcs toowhich gyűjtött fájlok lesz feltöltve.
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
3. Hello (hello hello cikk végén található) SetAzureVMLogCollector.ps1 követi tooenable hello AzureLogCollector bővítményként kérjen egy felhőalapú szolgáltatás. Ha hello végrehajtása befejeződött, a https://YouareStorageAccountName.blob.core.windows.net/vmlogs hello feltöltött fájl található

hello hello átadott toohello parancsfájl hello definíciója látható. (Ez másolódik alatt is.)

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
        [Parameter(Mandatory=$true)]
      [string]   $ServiceName,

      [Parameter(Mandatory=$false)]
      [string] $VMName ,

        [Parameter(Mandatory=$false)]
      [string]   $Mode = 'Full',

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountName,

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountKey,

      [Parameter(Mandatory=$false)]
      [PSObject[]] $AdditionDataLocationList = $null
      )

* Szolgáltatásnév: A felhőszolgáltatás neve.
* Virtuális gép hello VMName hello neve.
* Mód: Gyűjtemény mód. "Teljes" vagy "GA".
* StorageAccountName: A gyűjtött adatok tárolásához Azure storage-fiókjának neve.
* StorageAccountKey: Azure storage-fiók kulcs neve.
* AdditionalDataLocationList: Hello struktúra a következő listája:

```
      {
        String Name,
        String Location,
        String SearchPattern,
        Bool   Recursive
      }
```

## <a name="extention-powershell-script-files"></a>Kiterjesztés PowerShell parancsfájlok
SetAzureServiceLogCollector.ps1

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Roles ,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Instances = '*',

                  [Parameter(Mandatory=$false)]
                  [string]   $Slot = 'Production',

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject

    if ($Instances -ne $null -and $Instances.Count -gt 0)  #Instances should be seperated by ,
    {
        $instanceText = $Instances[0]
        for ($i = 1;$i -lt $Instances.Count;$i++)
        {
              $instanceText = $instanceText+ "," + $Instances[$i]
          }
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value $instanceText
    }
    else  #For all instances if not specified.  hello value should be a space or *
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value " "
    }

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need tooget hello Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData toocollect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it tooJSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json
    "publicConfig is:  $publicConfigJSON"

    #we just provide a empty privateConfig object
    $privateconfig = "{
    }"

    if ($Roles -ne $null)
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot -Role $Roles -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }
    else
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot  -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }

    #
    #This is an optional step: generate a sasUri toohello container so it can be shared with other people if nened
    #
    $SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
    $SasUri = $SasUri + "&restype=container&comp=list"
    Write-Output "hello container for uploaded file can be accessed using this link:`r`n$sasuri"


SetAzureVMLogCollector.ps1

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string] $VMName ,

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value "*"

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need tooget hello Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(90).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData toocollect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it tooJSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json

    Write-Output "PublicConfigurtion is: \r\n$publicConfigJSON"

    #
    #we just provide a empty privateConfig object
    #
    $privateconfig = "{
    }"

    if ($VMName -ne $null )
    {
          $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
          $VM.VM.OSVirtualHardDisk.OS

          if ($VM.VM.OSVirtualHardDisk.OS -like '*Windows*')
          {
                Set-AzureVMExtension -VM $VM -ExtensionName "AzureLogCollector" -Publisher Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.* | Update-AzureVM -Verbose

                #
                #We will check hello VM status toofind if operation by extension has been completed or not. hello completion of hello operation,either succeed or fail, can be indicated by
                #hello presence of SubstatusList field.
                #
                $Completed = $false
                while ($Completed -ne $true)
                {
                        $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
                        $status = $VM.ResourceExtensionStatusList | Where-Object {$_.HandlerName -eq "Microsoft.WindowsAzure.Compute.AzureLogCollector"}

                        if ( ($status.Code -ne 0) -and ($status.Status -like '*error*'))
                        {
                            Write-Output "Error status is returned: $($Status.ExtensionSettingStatus.FormattedMessage.Message)."
                              $Completed = $true
                        }
                        elseif (($status.ExtensionSettingStatus.SubstatusList -eq $null -or $status.ExtensionSettingStatus.SubstatusList.Count -lt 1))
                        {
                              $Completed = $false
                              Write-Output "Waiting for operation toocomplete..."
                        }
                        else
                        {
                              $Completed = $true
                              Write-Output "Operation completed."

                        $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                              $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                      #
                            # This is an optional step:  For easier access toohello file, we can generate a read-only SasUri directly toohello file
                              #
                              $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                              $ReadSasUri = New-AzureStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                            Write-Output "hello uploaded file can be accessed using this link: $ReadSasUri"

                              #
                              #This is an optional step:  Remove hello extension after we are done
                              #
                              Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                        }
                        Start-Sleep -s 5
                }
          }
          else
          {
              Write-Output "VM OS Type is not Windows, hello extension cannot be enabled"
          }

    }
    else
    {
      Write-Output "VM name is not specified, hello extension cannot be enabled"
    }

## <a name="next-steps"></a>Következő lépések
Most vizsgálja meg, vagy a naplók másolása egy nagyon egyszerű helyről.

