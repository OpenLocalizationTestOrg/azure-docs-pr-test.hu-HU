---
title: "aaaUse csomag rögzítési toodo proaktív hálózati figyelés a riasztások és az Azure Functions |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate riasztást kiváltott csomagrögzítéssel rendelkező Azure hálózati figyelőt"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 4722a831f3a9d5537c0e6f53daba4dfc35d0cf24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a>Proaktív hálózatfigyelési riasztások és az Azure Functions csomagrögzítéssel használata

Hálózati figyelő csomagrögzítéssel rögzítési munkamenetek tootrack forgalom mindkét virtuális gépeket hoz létre. hello rögzítési fájl lehet egy szűrő definiált tootrack csak hello megjeleníteni kívánt toomonitor forgalmat. Ezeket az adatokat a tárolási blob vagy helyileg hello vendég gépen majd tárolja.

Ez a funkció más automatizálási esetekben, például az Azure Functions távolról is indítható. Csomag rögzítési ad meg hello funkció toorun proaktív rögzítésekre alapján definiált hálózati rendellenességek észlelését. Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, első hálózati behatolások, a hibakeresési ügyfél-kiszolgáló közötti kommunikációt, és további információkat gyűjt.

Az Azure rendszerbe telepítendő erőforrásokat 24/7 fut. Alkalmazottak nem aktívan figyeli a hello állapotát az összes erőforrás 24/7. Például mi történik, ha hiba fordul elő, hajnali 2 Órakor?

Használatával hálózati figyelőt, riasztások, és hello Azure ökoszisztémán belüli funkciókat proaktív reagálhat hello adatok és eszközök toosolve problémák a hálózaton.

![Forgatókönyv][scenario]

## <a name="prerequisites"></a>Előfeltételek

* hello legújabb verziójának [Azure PowerShell](/powershell/azure/install-azurerm-ps).
* Hálózati figyelőt meglévő példányát. Ha még nem rendelkezik egy, [hálózati figyelőt példányt létrehozni](network-watcher-create.md).
* Egy meglévő virtuális gép hello ugyanabban a régióban, hálózati figyelőt, hello [Windows bővítmény](../virtual-machines/windows/extensions-nwa.md) vagy [Linux virtuálisgép-bővítmény](../virtual-machines/linux/extensions-nwa.md).

## <a name="scenario"></a>Forgatókönyv

Ebben a példában a szokásosnál több TCP-szegmens küld, és azt szeretné, hogy toobe riasztást kap. TCP-szegmens az itt példaként szolgálnak, de a figyelmeztetési állapotot használhatja.

Amikor a rendszer megkérdezi, érdemes tooreceive csomagszintű adatok toounderstand miért kommunikációs növekedett. Majd lépéseket tooreturn hello virtuális gép tooregular kommunikációt.

Ez a forgatókönyv feltételezi, hogy rendelkezik-e hálózati figyelőt, és egy erőforráscsoport, érvényes virtuális gépet a meglévő példányát.

a következő lista hello a akkor történik meg a hello munkafolyamat nyújt áttekintést:

1. Figyelmeztetés jelenik meg a virtuális Gépen.
1. hello riasztás meghívja az Azure-függvény olyan webhook keresztül.
1. Az Azure-függvény hello riasztás feldolgozza, és hálózati figyelőt csomag rögzítési munkamenet indítása.
1. hello csomagrögzítéssel hello virtuális gép fut, és összegyűjti a forgalmat.
1. hello csomagok rögzítési fájl feltöltése tooa tárfiók áttekintésre és elemzés céljából.

tooautomate Ez a folyamat azt létrehozása és csatlakoznak egy riasztást a virtuális gép tootrigger hello esemény bekövetkezésekor. Azt is létrehozhat egy függvény toocall a hálózati figyelőt.

Ebben a forgatókönyvben hello a következő:

* Egy Azure függvény, amely elindítja a csomagrögzítéssel hoz létre.
* Egy riasztási szabályt hoz létre a virtuális gépen, és konfigurálja a hello riasztási szabály toocall hello Azure függvény.

## <a name="create-an-azure-function"></a>Egy Azure-függvény létrehozása

első lépés hello toocreate Azure függvény tooprocess hello riasztást, és hozzon létre egy csomagrögzítéssel.

1. A hello [Azure-portálon](https://portal.azure.com), jelölje be **új** > **számítási** > **függvény App**.

    ![A függvény-alkalmazás létrehozása][1-1]

2. A hello **függvény App** panelen adja meg a következő értékek hello, és válassza ki **OK** toocreate hello alkalmazást:

    |**Beállítás** | **Érték** | **Részletek** |
    |---|---|---|
    |**Alkalmazás neve**|PacketCaptureExample|hello függvény alkalmazás hello nevét.|
    |**Előfizetés**|[Az előfizetés] hello melyik toocreate hello függvény alkalmazás-előfizetés.||
    |**Erőforráscsoport**|PacketCaptureRG|hello erőforrás csoport toocontain hello függvény alkalmazást.|
    |**Üzemeltetési terv**|Felhasználás terv| hello típusú tervezze meg a függvény alkalmazás használ. Ezek fogyasztás vagy az Azure App Service-csomag. |
    |**Hely**|USA középső régiója| hello régióban mely toocreate hello függvény alkalmazásban.|
    |**Tárfiók**|{automatikusan létrehozott}| hello storage-fiók, amelyet az Azure Functions általános célú tárfiókok esetében.|

3. A hello **PacketCaptureExample függvény alkalmazások** panelen válassza **funkciók** > **egyéni függvény**  >  **+**.

4. Válassza ki **HttpTrigger-Powershell**, majd adja meg a fennmaradó információk hello. Végezetül toocreate hello függvény, jelölje be **létrehozása**.

    |**Beállítás** | **Érték** | **Részletek** |
    |---|---|---|
    |**A forgatókönyv**|Kísérleti|Forgatókönyv típusa|
    |**A függvény neve**|AlertPacketCapturePowerShell|Hello függvény neve|
    |**Jogosultsági szint**|Függvény|Jogosultsági szint hello függvényhez|

![Funkciók – példa][functions1]

> [!NOTE]
> hello PowerShell sablon kísérleti, és nem rendelkezik teljes körű támogatása.

Testreszabások szükséges ehhez a példához, és a lépéseket követve hello szakaszban találhatók.

### <a name="add-modules"></a>Modulok hozzáadása

Hálózati figyelő PowerShell-parancsmagjainak toouse hello legújabb PowerShell modul toohello függvény alkalmazás feltöltése.

1. A hello legújabb Azure PowerShell-modulok telepítése a helyi gépén futtassa a következő PowerShell-paranccsal hello:

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    A példa tesz lehetővé, hogy hello helyi elérési útja az Azure PowerShell-modulok. Ezek a mappák egy későbbi lépésben használt. hello ebben a forgatókönyvben használt modulokra:

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

    ![PowerShell mappák][functions5]

1. Válassza ki **Alkalmazásbeállítások működéséhez** > **nyissa meg a szolgáltatás szerkesztő tooApp**.

    ![Függvény-Alkalmazásbeállítások][functions2]

1. Kattintson a jobb gombbal hello **AlertPacketCapturePowershell** mappát, majd hozzon létre egy nevű és **azuremodules**. 

4. Hozzon létre egy almappát minden modul, amelyekre szüksége van.

    ![Mappa- és almappákkal együtt][functions3]

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

1. Kattintson a jobb gombbal hello **AzureRM.Network** almappát, és válassza **fájl feltöltése**. 

6. Nyissa meg tooyour Azure modulok. Hello helyi **AzureRM.Network** mappát, válassza ki az összes hello fájlok hello mappában. Válassza ki **OK**. 

7. Ismételje meg ezeket a lépéseket **AzureRM.Profile** és **AzureRM.Resources**.

    ![Fájlok feltöltése][functions6]

1. Miután elkészült, minden egyes hello PowerShell modul fájlok a helyi számítógépről kell rendelkeznie.

    ![PowerShell-fájlok][functions7]

### <a name="authentication"></a>Authentication

toouse hello PowerShell-parancsmagok, hitelesítenie kell. Hitelesítés konfigurálása a hello függvény alkalmazásban. tooconfigure hitelesítési kell konfigurálnia a környezeti változók és egy titkosított fájl toohello függvény alkalmazás feltöltéséhez.

> [!NOTE]
> Ebben a forgatókönyvben csak egy példa bemutatja, hogyan nyújt az Azure Functions tooimplement hitelesítés. Nincsenek más módokon toodo ez.

#### <a name="encrypted-credentials"></a>Titkosított hitelesítő adatokat

a következő PowerShell-parancsfájl hello létrehoz egy kulcs nevű **PassEncryptKey.key**. Egy megadott hello jelszó titkosított verzióját is tartalmazza. Ez a jelszó nem hello ugyanazt a jelszót, amely a hitelesítéshez használt hello Azure Active Directory-alkalmazás van definiálva.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

A hello App Service-szerkesztő hello függvény alkalmazás, hozzon létre egy nevű **kulcsok** alatt **AlertPacketCapturePowerShell**. Majd feltölteni hello **PassEncryptKey.key** hello előző PowerShell minta létrehozott fájlt.

![Funkciók kulcs][functions8]

### <a name="retrieve-values-for-environment-variables"></a>A környezeti változók értékeit beolvasása

hello végső vonatkozó követelmény akkor tooset hello környezeti változókat, amelyek a hitelesítéshez szükséges tooaccess hello értékek fel. hello alábbi lista mutatja azokat létrehozott hello környezeti változók:

* AzureClientID

* AzureTenant

* AzureCredPassword


#### <a name="azureclientid"></a>AzureClientID

hello ügyfél-azonosító hello egy alkalmazás az Azure Active Directoryban.

1. Ha még nem rendelkezik az alkalmazás toouse, futtassa a következő példa toocreate hello egy alkalmazást.

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > hello jelszó használni, amikor hello alkalmazás létrehozása hello kell ugyanazt a jelszót, amely a korábban létrehozott hello kulcsfájl mentése során.

1. Hello Azure-portálon, válassza ki **előfizetések**. Hello előfizetés toouse, majd válassza ki és **hozzáférés-vezérlés (IAM)**.

    ![IAM funkciók][functions9]

1. Válassza ki a hello fiók toouse, majd válassza ki **tulajdonságok**. Másolás hello azonosítót.

    ![Funkciók Alkalmazásazonosító][functions10]

#### <a name="azuretenant"></a>AzureTenant

A következő PowerShell-példa hello futtatásával beszerezni hello bérlő azonosítója:

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a>AzureCredPassword

hello hello AzureCredPassword környezeti változó értéke hello értéket kapott a következő PowerShell-példa hello futtatását. Ez a példa hello van, hogy megjelenik-e a fenti hello ugyanaz **titkosított hitelesítő adatok** szakasz. érték, amely szükséges hello hello hello kimenete `$Encryptedpassword` változó.  Ez a hello szolgáltatás egyszerű jelszót, amely korábban titkosított hello PowerShell parancsfájl használatával.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

### <a name="store-hello-environment-variables"></a>Tároló hello környezeti változók

1. Nyissa meg a toohello függvény alkalmazást. Válassza ki **Alkalmazásbeállítások működéséhez** > **Alkalmazásbeállítások konfigurálása**.

    ![Alkalmazásbeállítások konfigurálása][functions11]

1. Adja hozzá a hello környezeti változók és értékek toohello alkalmazás beállításai, és válassza ki **mentése**.

    ![Alkalmazásbeállítások][functions12]

### <a name="add-powershell-toohello-function"></a>PowerShell toohello függvény hozzáadása

Most idő toomake hívja be a hálózati figyelőt hello Azure függvény belül van. Ez a függvény végrehajtása hello hello követelményeitől függően eltérőek lehetnek. Azonban hello általános folyamata hello-szabályzat a következőképpen történik:

1. Folyamat-bemeneti paraméterekhez.
2. Meglévő csomagot tooverify korlátok rögzíti, és a névütközések feloldásához.
3. Hozzon létre egy csomagrögzítéssel megfelelő paraméterekkel.
4. A lekérdezési csomag rögzítése rendszeres időközönként, amíg nem fejeződik be.
5. Hello felhasználó értesítése, hogy hello csomagok rögzítési munkamenet befejeződik.

hello következő példa egy PowerShell-kódjába hello függvényben használható. Érték helyére toobe igénylő **subscriptionId**, **resourceGroupName**, és **storageAccountName**.

```powershell
            #Import Azure PowerShell modules required toomake calls tooNetwork Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID toosave captures in
            $storageaccountid = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}"

            #Packet capture vars
            $packetcapturename = "PSAzureFunction"
            $packetCaptureLimit = 10
            $packetCaptureDuration = 10

            #Credentials
            $tenant = $env:AzureTenant
            $pw = $env:AzureCredPassword
            $clientid = $env:AzureClientId
            $keypath = "D:\home\site\wwwroot\AlertPacketCapturePowerShell\keys\PassEncryptKey.key"

            #Authentication
            $secpassword = $pw | ConvertTo-SecureString -Key (Get-Content $keypath)
            $credential = New-Object System.Management.Automation.PSCredential ($clientid, $secpassword)
            Add-AzureRMAccount -ServicePrincipal -Tenant $tenant -Credential $credential #-WarningAction SilentlyContinue | out-null


            #Get hello VM that fired hello alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get hello Network Watcher in hello VM's region
                $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by hello function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on hello VM that fired hello alert
                if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-hello-function-url"></a>Hello függvény URL-cím beolvasása 
1. Miután létrehozta a funkciót, a riasztás toocall hello URL-CÍMEK konfigurálása hello függvény társított. tooget ezt az értéket, hello függvény URL-CÍMÉT a függvény alkalmazás.

    ![Hello függvény URL-cím keresése][functions13]

2. A függvény alkalmazás hello függvény URL-Címének másolása.

    ![Hello függvény URL-cím másolása][2]

Ha egyéni tulajdonságok hello webhook POST kérelem hasznos hello van szüksége, tekintse meg a túl[olyan webhook konfigurálása Azure metrika riasztást](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="configure-an-alert-on-a-vm"></a>Olyan riasztást konfigurálhat a virtuális gép

A riasztások lehetnek konfigurált toonotify egyéni felhasználók számára, egy adott metrika ebbe a küszöbérték, amely hozzá van rendelve tooit. Ebben a példában hello riasztás hello a TCP-szegmens küldött, de hello is lehet figyelmeztetés, ha sok más metrikákkal. Ebben a példában a riasztás konfigurált toocall webhook toocall hello függvény.

### <a name="create-hello-alert-rule"></a>Hello riasztási szabály létrehozása

Nyissa meg tooan meglévő virtuális gép, és adja hozzá a riasztási szabályt. Riasztások konfigurálása vonatkozó részletes dokumentációt a helyen találhatók [hozzon létre riasztásokat Azure figyelése az Azure-szolgáltatások - Azure-portálon](../monitoring-and-diagnostics/insights-alerts-portal.md). Adja meg a következő értékek hello hello **riasztási szabály** panelt, és válassza **OK**.

  |**Beállítás** | **Érték** | **Részletek** |
  |---|---|---|
  |**Name (Név)**|TCP_Segments_Sent_Exceeded|Hello riasztási szabály neve.|
  |**Leírás**|TCP-szegmens küldött meghaladja a küszöbértéket|riasztási szabály hello hello leírása.||
  |**Metrika**|Küldött TCP-szegmens| hello metrika toouse tootrigger hello riasztás. |
  |**Az állapot**|Nagyobb mint| hello feltétel toouse hello metrika kiértékelése során.|
  |**Küszöbérték**|100| hello hello mérőszám hello riasztást kiváltó érték. Ezt az értéket meg kell tooa érvényes értéket a környezetében.|
  |**Időtartam**|Hello utolsó öt perc alatt| Meghatározza, hogy a hello metrika hello küszöb hello mely toolook időszak.|
  |**Webhook**|[webhook URL-CÍMÉT függvény alkalmazásból]| hello webhook URL-CÍMÉT alkalmazásból hello függvény hello előző lépésekben létrehozott.|

> [!NOTE]
> hello TCP szegmensek metrika alapértelmezés szerint nincs engedélyezve. További tudnivalók tooenable további metrikák ellátogatva [figyelés engedélyezésekor és diagnosztikai](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).

## <a name="review-hello-results"></a>Hello eredményeinek áttekintése

Hello riasztási eseményindítók hello feltételek, miután a csomagrögzítéssel jön létre. Nyissa meg tooNetwork megfigyelő, és válassza ki **csomagrögzítéssel**. Ezen a lapon kiválaszthatja a hello csomagok rögzítési fájl hivatkozás toodownload hello csomagrögzítéssel.

![A csomagrögzítéssel megtekintése][functions14]

Ha hello rögzítési fájlt helyileg van tárolva, a bejelentkezéssel toohello virtuális gép kérheti le.

Az Azure storage-fiókok fájlok letöltésére vonatkozó utasításokért lásd: [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Használhat egy másik eszköz [Tártallózó](http://storageexplorer.com/).

A rögzítési letöltése után tekintheti által olvasható eszközzel egy **.cap** fájlt. Az alábbiakban ezen eszközök hivatkozások tootwo:

- [Microsoft Message Analyzert](https://technet.microsoft.com/library/jj649776.aspx)
- [WireShark](https://www.wireshark.org/)

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan tooview a csomag rögzíti ellátogatva [csomag eseményrögzítés elemzésének rendelkező Wireshark](network-watcher-deep-packet-inspection.md).


[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[1-1]: ./media/network-watcher-alert-triggered-packet-capture/figure1-1.png
[2]: ./media/network-watcher-alert-triggered-packet-capture/figure2.png
[3]: ./media/network-watcher-alert-triggered-packet-capture/figure3.png
[functions1]:./media/network-watcher-alert-triggered-packet-capture/functions1.png
[functions2]:./media/network-watcher-alert-triggered-packet-capture/functions2.png
[functions3]:./media/network-watcher-alert-triggered-packet-capture/functions3.png
[functions4]:./media/network-watcher-alert-triggered-packet-capture/functions4.png
[functions5]:./media/network-watcher-alert-triggered-packet-capture/functions5.png
[functions6]:./media/network-watcher-alert-triggered-packet-capture/functions6.png
[functions7]:./media/network-watcher-alert-triggered-packet-capture/functions7.png
[functions8]:./media/network-watcher-alert-triggered-packet-capture/functions8.png
[functions9]:./media/network-watcher-alert-triggered-packet-capture/functions9.png
[functions10]:./media/network-watcher-alert-triggered-packet-capture/functions10.png
[functions11]:./media/network-watcher-alert-triggered-packet-capture/functions11.png
[functions12]:./media/network-watcher-alert-triggered-packet-capture/functions12.png
[functions13]:./media/network-watcher-alert-triggered-packet-capture/functions13.png
[functions14]:./media/network-watcher-alert-triggered-packet-capture/functions14.png
[scenario]:./media/network-watcher-alert-triggered-packet-capture/scenario.png
