---
title: "végpont kulcs elforgatás és naplózás mentése az Azure Key Vault aaaSet |} Microsoft Docs"
description: "A kulcs elforgatás és figyelési kulcstároló naplóit beolvasása beállítása hogyan-tootoohelp használja."
services: key-vault
documentationcenter: 
author: swgriffith
manager: mbaldwin
tags: 
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: jodehavi;stgriffi
ms.openlocfilehash: e0c393873077e3b91adc9fa7f39128bc1b6abe26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-azure-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Az Azure Key Vault beállítása végpontok közötti kulcsforgatással és auditálással
## <a name="introduction"></a>Bevezetés
Miután létrehozta a kulcstároló, a tároló toostore használ a kulcsok és titkos képes toostart fogja. Az alkalmazások toopersist már nem szükséges, a kulcsok vagy titkos, de ahelyett, hogy fog igényelni őket hello kulcs tárolóból igény szerint. Ez lehetővé teszi tooupdate kulcsok és titkos hello viselkedést az alkalmazás, így akár a kulcs és a titkos felügyeleti lehetőségek széles választékát befolyásolása nélkül.

Ez a cikk útmutatást keresztül az Azure Key Vault toostore használatának példája egy titkos kulcsot, ebben az esetben egy Azure Storage-fiók kulcsot, amely egy alkalmazás használja. Is egy ütemezett elforgatási szögét a tárfiók kulcsa végrehajtását mutatja be. Végül azt végigvezeti az egyes hogyan toomonitor hello kulcstároló naplókat, és riasztást, ha a nem várt kérelmeket.

> [!NOTE]
> Ebben az oktatóanyagban nincs tervezett tooexplain részletes hello kezdeti telepítése a kulcstartót. Ezekről a [Get started with Azure Key Vault](key-vault-get-started.md) (Bevezetés az Azure Key Vault használatába) című cikkben találhat információt. Platformfüggetlen parancssori felületre vonatkozó utasításokat lásd: [kezelése Key Vault parancssori felület használatával](key-vault-manage-with-cli2.md).
>
>

## <a name="set-up-key-vault"></a>A Key Vault beállítása
egy alkalmazás titkos kulcs tooretrieve tooenable a kulcstároló, először hello titkos kulcs létrehozása, és töltse fel az tooyour tárolóban. Ehhez az Azure PowerShell-munkamenet indítása, aláírást tooyour az Azure-fiók a hello a következő parancsot:

```powershell
Login-AzureRmAccount
```

Hello előugró böngészőablakban adja meg a Azure-fiók felhasználói nevét és jelszavát. PowerShell beolvassa az ehhez a fiókhoz társított összes hello-előfizetést. PowerShell használ hello alapértelmezés szerint az elsőt.

Ha több előfizetéssel rendelkezik, lehetséges, hogy a kulcstartót hello egyet, de a használt toocreate toospecify. Adja meg a fiókhoz toosee hello előfizetések a következő hello:

```powershell
Get-AzureRmSubscription
```

Adja meg a toospecify hello előfizetés meg fogja naplózás, hello kulcstároló társított:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

Mivel ez a cikk bemutatja, hogy a tárfiók kulcsára, titkos kulcs tárolása, ha előbb telepítik azokra a tárfiók kulcsára.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

A titkos kulcsot (ebben az esetben a tárfiók kulcsára) beolvasása, után kell alakítani, hogy tooa biztonságos karakterláncot, és majd titkos kulcs létrehozása ezt az értéket a key vaultban lévő.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
A következő beolvasása hello URI létrehozott hello titkos kulcsot. Ez szolgál egy későbbi lépésben a titkos kód hello kulcstároló tooretrieve hívásakor. Futtassa a következő PowerShell-paranccsal hello, és jegyezze fel a hello azonosító értéke, amely hello titkos kulcs URI:

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-hello-application"></a>Hello alkalmazás beállítása
Most, hogy a titkos kulcs tárolása, kód tooretrieve használja, és használja azt. Van néhány lépést szükséges tooachieve ez. hello első és legfontosabb lépés az alkalmazás regisztrálása az Azure Active Directoryban, majd közli az alkalmazással kapcsolatos adatok Key Vault, így engedélyezheti, hogy az alkalmazás érkező kérelmeket.

> [!NOTE]
> Az alkalmazás kell létrehozni a hello ugyanaz, mint a kulcstárolót Azure Active Directory-bérlő.
>
>

Nyissa meg az Azure Active Directory hello alkalmazások lapon.

![Nyissa meg az alkalmazások az Azure Active Directoryban](./media/keyvault-keyrotation/AzureAD_Header.png)

Válasszon **hozzáadása** tooadd egy alkalmazás tooyour Azure Active Directoryban.

![A Hozzáadás gombra](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Alkalmazás típusú hello hagyja **WEB APPLICATION AND/OR WEB API** és nevezze el az alkalmazást.

![Nevű hello alkalmazás](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Adja meg az alkalmazás egy **SIGN-ON URL** és egy **APP ID URI**. Ebben a bemutatóban bármilyen is lehetnek, és azokat később módosítható, ha szükséges.

![Adja meg a szükséges URI-azonosítók](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Hello alkalmazás hozzáadása az Active Directory tooAzure után jut hello alkalmazás oldalhoz. Kattintson a hello **konfigurálása** lapon, majd keresse meg és hello másolása **ügyfél-azonosító** érték. Jegyezze fel az ügyfél-azonosító hello későbbi lépéseire.

Ezt követően az alkalmazás kulcs létrehozása, az Azure Active Directory kommunikálhat. Ez a hello hozhat létre **kulcsok** hello szakasz **konfigurációs** lapon. Jegyezze fel az újonnan létrehozott hello kulcs használható az Azure Active Directory-alkalmazás egy későbbi lépésben.

![Az Azure Active Directory-alkalmazás kulcsok](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Mielőtt bármely hívásokat az alkalmazásból történő hello kulcstároló létrehozása, a hello kulcstároló kérje meg az alkalmazásról és az engedélyeket. hello következő parancsnak hello tároló nevére és hello ügyfél-azonosító az Azure Active Directory-alkalmazás és biztosít **beolvasása** hozzáférés tooyour kulcstároló hello alkalmazáshoz.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Ekkor készen áll az alkalmazás hívások felépítése toostart áll. Az alkalmazás az Azure Key Vault és az Azure Active Directory hello NuGet-csomagok szükséges toointeract kell telepíteni. Hello Visual Studio Csomagkezelő konzolról adja meg a következő parancsok hello. : Ez a cikk hello írását, hello hello Azure Active Directory-csomag nem 3.10.305231913, így előfordulhat, hogy szeretné, hogy tooconfirm hello legújabb verzióját, és ennek megfelelően frissülnek.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Az alkalmazás kódjában hozzon létre egy osztály toohold hello hitelesítési módszer választása az Azure Active Directoryban. Ebben a példában az adott osztály neve **Utils**. Adja hozzá hello következő using utasítást:

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Ezután adja hozzá a hello metódus tooretrieve hello JWT jogkivonat követően az Azure Active Directoryból. A karbantartási követelmények érdemes lehet az toomove hello kódolt karakterlánc-értékek be a webhely vagy alkalmazás konfigurációjának.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed tooobtain hello JWT token");

    return result.AccessToken;
}
```

Hello szükséges kód toocall Key Vault hozzáadása, és a titkos érték beolvasása. Először hozzá kell adnia hello következő using utasítást:

```csharp
using Microsoft.Azure.KeyVault;
```

Hello metódus hívások tooinvoke Key Vault hozzáadása, és a titkos kulcs beolvasása. Ezzel a módszerrel hello titkos kulcs URI, amelyet az előző lépésben mentett adja meg. Vegye figyelembe a hello hello használata **GetToken** hello metódusnak **Utils** korábban létrehozott osztályt.

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Az alkalmazás futtatásakor meg kell most kell hitelesítő tooAzure Active Directory és majd a titkos értékének beolvasása az Azure Key Vault.

## <a name="key-rotation-using-azure-automation"></a>Azure Automation használatával kulcs Elforgatás
Az Azure Key Vault titkok, tárolt értékek Elforgatás stratégiája megvalósításának számos lehetőség áll rendelkezésre. Titkos kulcsok forgatható el kézi folyamat részeként, akkor lehetséges, hogy forgatható programozott módon API-hívásokkal, vagy előfordulhat, hogy forgatható vállalja egy automatizálási parancsfájl. Ez a cikk hello célokra fogja az Azure PowerShell együttesen Azure Automation toochange egy Azure Storage-fiók hozzáférési kulcsot. A kulcstároló titkos kulcsot az új kulcs majd frissíti.

tooallow Azure Automation tooset titkos értékek key vaultban lévő, ha előbb telepítik azokra hello ügyfél-azonosító AzureRunAsConnection, az Azure Automation-példányt létrejöttekor létrehozott nevű hello kapcsolathoz. Válassza ezt az Azonosítót található **eszközök** az Azure Automation-példányból. Ott, válasszon **kapcsolatok** , és válassza a hello **AzureRunAsConnection** szolgáltatás elvet. Jegyezze fel a hello **Alkalmazásazonosító**.

![Azure Automation ügyfél-azonosítója](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

A **eszközök**, válassza a **modulok**. A **modulok**, jelölje be **gyűjteménye**, majd keresse meg és **importálása** frissített verziói egyes hello modulok a következő:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> : Ez a cikk hello írását, csak hello szükséges korábban feljegyzett modulok toobe frissítve a következő parancsfájl hello. Ha talál meg, hogy az automation-feladat meghiúsul, győződjön meg arról, hogy importálta-e minden szükséges modulokat és függőségi viszonyaikat.
>
>

Az Azure Automation-kapcsolat beolvasása hello Alkalmazásazonosító, után kell közli a kulcstartót, hogy rendelkezik-e az alkalmazás hozzáférési tooupdate titkokat a tárolóban lévő állapottal. Ehhez a következő PowerShell-paranccsal hello:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Válassza ki, **Runbookok** az Azure Automation-példány, és válassza ki azt a **hozzáadása egy Runbook**. Kattintson a **Gyors létrehozás** gombra. A runbook neve, és válassza ki **PowerShell** hello runbook típusként. Hello beállítás tooadd tartozik leírás. Végezetül kattintson **létrehozása**.

![Runbook létrehozása](./media/keyvault-keyrotation/Create_Runbook.png)

Illessze be a következő PowerShell-parancsfájl hello szerkesztő ablaktáblában az új runbook hello:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get hello connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in tooAzure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set hello following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for hello storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Hello szerkesztő ablaktáblán válassza ki a **teszt ablaktábla** tootest a parancsfájlt. Miután hello parancsfájl hiba nélkül fut-e, kijelölheti a **közzététel**, és vissza a hello runbook konfigurációs ablaktáblán hello runbook ütemezés szerint alkalmazhatja.

## <a name="key-vault-auditing-pipeline"></a>Key Vault naplózási folyamat
Kulcstároló beállításakor bekapcsolása toocollect toohello kulcstároló hozzáférési kérelmet a naplófájlokat. Ezek a naplók a kijelölt Azure Storage-fiókban tárolt, és figyeli, és elemezni, lekért. hello alábbi forgatókönyvet használja az Azure functions, az Azure logic apps és kulcstároló naplózási naplók toocreate egy folyamat toosend egy e-mailt Ha egy alkalmazás, amely egyezik a hello Alkalmazásazonosító hello webalkalmazás olvas be a titkos kulcsok hello tárolóból.

Először engedélyeznie kell a kulcstartót bejelentkezni. Ez a következő PowerShell-parancsok hello keresztül végezhető (teljes részletei láthatók [kulcs-tároló-naplózás](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Miután engedélyezve van, az auditnaplókat a kijelölt tárfiókkal hello gyűjtése start. Ezek a naplók tartalmaz arról, hogyan és mikor érhetők el a kulcstárolót, és ki eseményeket.

> [!NOTE]
> Elérheti a naplóinformációkat hello kulcstároló műveletet követően 10 percet. Általában lesz gyorsabb, mint ez.
>
>

következő lépés hello túl van[hozzon létre egy Azure Service Bus-üzenetsorba](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Ez azért, ahol a kulcstartót naplók leküldött vannak. Ha hello naplózási üzenetek hello várólista, a hello logikai alkalmazás felveszi őket, és kezelje őket. Hozzon létre egy service bus hello a következő lépéseket:

1. Hozzon létre egy Service Bus-névtér (Ha már rendelkezik egy használni kívánt ehhez toouse kihagyása tooStep 2).
2. Toohello a service bus hello Azure-portálon, és jelölje be hello névtér toocreate hello várólista a Tallózás gombra.
3. Válassza ki **új** válassza **Service Bus > várólista** és írja be a szükséges hello adatait.
4. Hello névtér kiválasztásával, majd válassza ki a hello Service Bus kapcsolati információit **kapcsolatadatok**. A következő szakaszban hello szüksége lesz ezt az információt.

Ezt követően [egy Azure-függvény létrehozása](../azure-functions/functions-create-first-azure-function.md) toopoll kulcstároló naplóit belül hello tárfiók, és új események átvételéhez. Ez lesz az ütemezés szerint kiváltó függvényt.

Válasszon egy Azure függvény toocreate **új > függvény App** hello Azure-portálon a. A létrehozás során egy meglévő üzemeltetési terv használja, vagy hozzon létre egy újat. Sikerült is választhat dinamikus üzemeltetéséhez. További részleteket a beállításokat tartalmazó függvény található [hogyan tooscale Azure Functions](../azure-functions/functions-scale.md).

Hello Azure függvény létrehozásakor nyissa meg a tooit, és válasszon egy számlálót függvény és C\#. Kattintson a **Ez a függvény létrehozása**.

![Az Azure Functions lépések panelen](./media/keyvault-keyrotation/Azure_Functions_Start.png)

A hello **Develop** lapján hello következő hello run.csx kód cseréje:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging;
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log)
{
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting toonow.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required tooorder by time as they may not be in hello file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending tooServiceBus, use hello payloadStream and set keeporiginal tootrue
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate hello shared access signature on hello container, setting hello constraints directly on hello signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```


> [!NOTE]
> Győződjön meg arról, hogy tooreplace hello változók hello megelőző kód toopoint tooyour tárfiókot, ahol hello kulcstároló naplóit íródtak, korábban létrehozott hello a service bus, és a megadott elérési út toohello kulcstároló tárolási naplóit hello.
>
>

hello funkció szerzi be hello legújabb naplófájl hello tárfiókból ahol hello kulcstároló írja a naplókat, Markoló hello legújabb események a fájl, és a Service Bus-üzenetsorba tooa leküldéses értesítések. Mivel egyetlen fájl rendelkezhet több esemény, készítsen hello függvény is ellenőrzi, hogy az utolsó esemény hello mentése kivételezett toodetermine hello időbélyegzőjét sync.txt fájlt. Ez biztosítja, hogy nem leküldéses hello ugyanarra az eseményre több alkalommal. A sync.txt fájl tartalmaz egy Timestamp típusú hello utolsó észlelt esemény. hello naplókat, ha be van töltve, van rendezve toobe hello időbélyeg tooensure megfelelő sorrendben alapján.

Ennél a függvénynél néhány további szalagtár szerepel, amely nem használható az Azure Functions hello mezőben kívüli jelenleg hivatkozik. tooinclude ezeket, igazolnia kell, hogy az Azure Functions toopull őket NuGet segítségével. Válassza ki a hello **fájlok megtekintése** lehetőséget.

![Fájlok beállítás megtekintése](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

És adja hozzá a következő tartalommal project.json nevű fájlba:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Akkor **mentése**, az Azure Functions hello szükséges bináris fájlokat tölti le.

Váltás toohello **integráció** lapon és hello időzítő paraméter adjon meg egy kifejező nevet toouse hello függvényen belül. A kód megelőző hello, hello időzítő toobe nevű vár *myTimer*. Adjon meg egy [CRON-kifejezés](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) az alábbiak szerint: 0 \* \* \* \* \* hello időzítő, amely újraindítja a hello függvény toorun percenként egyszer.

A hello azonos **integráció** lapon maradva adja hozzá a hello típusú bemeneti **Azure Blob Storage**. Ez fog mutatni toohello sync.txt hello tekintett meg hello függvény utolsó esemény időbélyegzője hello tartalmazó fájlt. Ez lesz hello paraméter neve hello függvényen belül használható. A kód megelőző hello, hello Azure Blob Storage bemenetet vár hello paraméter neve toobe *inputBlob*. A tárfiók hello hello sync.txt fájl tároló választható (lehet hello ugyanabban vagy egy másik tárolási fiókot). Hello elérési út mezőbe adjon meg a hello elérési utat, ahol él, hello fájl hello formátum {container-name}/path/to/sync.txt.

Adja hozzá egy kimeneti hello típusú *Azure Blob Storage* kimeneti. Ez fog mutatni hello bemeneti megadott toohello sync.txt fájlt. Ez használható az hello függvény toowrite hello hello utolsó esemény időbélyegzője tekintett meg. hello előző kód várja a paraméter toobe nevű *outputBlob*.

Ezen a ponton hello függvény készen áll. Győződjön meg arról, hogy tooswitch hátsó toohello **Develop** lapra, és mentse a hello kódot. Hello kimeneti ablakban a fordítási hibákat, és ennek megfelelően javítsa ki azokat. Ha hello kód lefordításához, majd hello kódot kell most kell ellenőrzése hello kulcstároló naplóit percenként és bármely új események alakzatot hello küldését definiált Service Bus-üzenetsorba. Minden indításakor lefutnak hello függvény toohello napló ablakban kiírni naplózási információkat kell megjelennie.

### <a name="azure-logic-app"></a>Az Azure Logic Apps alkalmazást
Ezután létre kell hoznia egy Azure logikai alkalmazás, amely szerzi be, hogy hello függvény küldését toohello Service Bus-üzenetsorba, hello tartalom elemzi és elküld egy e-mailt az egyező feltétel alapján hello események.

[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md) címen túl**új > logikai alkalmazás**.

Hello logikai alkalmazás létrehozása után nyissa meg a tooit, és válassza a **szerkesztése**. Hello logic app szerkesztő választható **Service Bus-üzenetsorba** , és írja be a Service Bus-hitelesítő adatok tooconnect azt toohello várólista.

![Az Azure Logic App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Ezután válasszon **feltétel hozzáadása**. Hello állapotban speciális szerkesztő toohello váltson, és adja meg a következő kódot, APP_ID lecserélését hello hello webalkalmazás tényleges APP_ID:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Ebben a kifejezésben lényegében adja vissza **hamis** Ha hello *appid* hello a beérkező eseményhez (amely hello Service Bus üzenet törzsét hello) nem hello *appid* a hello alkalmazás.

Ezután hozzon létre egy műveletet a **Ha nem, nem történik semmi**.

![Az Azure Logic App művelet kiválasztását.](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Hello a művelethez, válassza ki a **Office 365 – e-mail küldése**. Töltse ki hello mezők toocreate egy e-mailek toosend hello meghatározva feltétel beolvasása **hamis**. Ha nem rendelkezik Office 365, sikerült tekinti meg alternatív tooachieve hello ugyanazokat az eredményeket.

Ezen a ponton rendelkezik egy záró tooend-feldolgozási folyamat percenként egyszer új kulcstartó naplók keresi. Ez a leküldéses értesítések tooa service bus-üzenetsorba megtalálja az új naplók. hello logikai alkalmazás lesz kiváltva, ha egy új üzenet fájljai a hello várólista. Ha hello *appid* belül hello esemény nem egyezik meg az alkalmazás hívása hello hello Alkalmazásazonosító, egy e-mailt küld.
