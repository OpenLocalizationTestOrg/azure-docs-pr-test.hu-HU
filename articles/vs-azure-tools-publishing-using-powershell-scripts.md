---
title: "aaaUsing Windows PowerShell-parancsfájlok tooPublish tooDev és a tesztkörnyezetek |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Windows PowerShell parancsfájl Visual Studio toopublish toodevelopment és tesztkörnyezetek."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5fff1301-5469-4d97-be88-c85c30f837c1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 491a058f96255576afa74f6156f20ae9559bb9f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-windows-powershell-scripts-toopublish-toodev-and-test-environments"></a>A Windows PowerShell parancsfájlok toopublish toodev és tesztkörnyezetek
A Visual Studio egy webalkalmazás létrehozásakor egy Windows PowerShell-parancsfájlt, hogy akkor használható a webhely tooAzure a későbbi tooautomate hello közzétételét egy webalkalmazást az Azure App Service vagy egy virtuális gép is létrehozhat. Szerkesztése és a Visual Studio-szerkesztőben toosuit hello hello Windows PowerShell-parancsfájl kiterjesztése a követelmények, vagy hello parancsfájl integrálható a meglévő build, a vizsgálati és a közzétételi parancsfájlok.

Az ezekhez a parancsfájlokhoz, megadhat testreszabott verziók (más néven a fejlesztési és tesztelési környezetben) a webhely ideiglenes használatra. Például akkor lehet, hogy állítsa be a webhely adott verziójának Azure virtuális gép vagy egy webhely toorun a tárolóhely átmeneti hello egy tesztcsomag, programhiba Reprodukálja, teszteléséhez hibajavítás, próbaverzió változtatást vagy bemutató vagy bemutató egyéni környezet beállítása. Olyan parancsfájlt, amely közzéteszi a projekt létrehozása után hozza létre újra az azonos környezetben újra hello parancsfájl futtatásával igény szerint, vagy a webes alkalmazás toocreate a saját build hello parancsfájl futtassa a teszteléshez egyéni környezet.

## <a name="what-you-need"></a>Mi szükséges
* Az Azure SDK 2.3-as vagy újabb. Lásd: [Visual Studio letöltések](http://go.microsoft.com/fwlink/?LinkID=624384) további információt.

Webes projektek hello Azure SDK toogenerate hello parancsfájlok nem szükséges. Ez a szolgáltatás webes projektek, a felhőalapú szolgáltatások nem webes szerepkörök szolgál.

* Az Azure PowerShell 0.7.4 vagy újabb. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) további információt.
* [A Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) vagy újabb.

## <a name="additional-tools"></a>További eszközök
További eszközök és erőforrások használatához a PowerShell használatával a Visual Studio Azure fejlesztési érhetők el. Lásd: [PowerShell Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-hello-publish-scripts"></a>Parancsfájlok hello generálása közzététele
Hello is létrehozhat egy virtuális gép, amelyen a webhely, amikor követve hozzon létre egy új projektet parancsfájlok közzététele [ezeket az utasításokat](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Emellett [készítése az Azure App Service web Apps parancsfájlok közzététele](app-service-web/app-service-web-get-started-dotnet.md).

## <a name="scripts-that-visual-studio-generates"></a>Olyan parancsfájlok, amelyek a Visual Studio hoz létre
A Visual Studio megoldás szintű mappa állít elő **PublishScripts** két Windows PowerShell-fájlok, a virtuális gép vagy a webhely és a modult tartalmaz, amelyek segítségével is hello funkciók közzététel parancsfájlt tartalmazó parancsfájlok. A Visual Studio is létrehoz egy fájlt, amely meghatározza a hello projektet telepít hello részleteit hello JSON formátumban.

### <a name="windows-powershell-publish-script"></a>A Windows PowerShell parancsfájl közzététele
hello közzététele a parancsfájl tartalmazza a megadott közzététele tooa webhelyére vagy a virtuális gép telepítésének lépései. A Visual Studio biztosít a Windows PowerShell fejlesztési színátmenetekhez szintaxist. Súgó hello funkciók érhető el, és szabadon szerkeszthető hello parancsfájl toosuit hello funkciók a változó követelményeknek megfelelően.

### <a name="windows-powershell-module"></a>A Windows PowerShell-modul
a Windows PowerShell modul, amely a Visual Studio hoz létre olyan függvényeket tartalmaz, hello hello közzététele parancsfájlt használ. Azok az Azure PowerShell-funkciók és nem tervezett toobe módosítani. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) további információt.

### <a name="json-configuration-file"></a>JSON-konfigurációs fájlt
hello JSON-fájl jön létre hello **konfigurációk** mappa, és pontosan mely erőforrások toodeploy tooAzure megadó konfigurációs adatokat tartalmaz. hello hello fájl neve, amely állít elő, a Visual Studio projekt-neve-WAWS-dev.json Ha létrehozott egy webhely vagy a név-VM-dev.json projekt Ha létrehozott egy virtuális gép. Íme egy példa egy JSON-konfigurációs fájlt, amely jön létre, amikor egy webhely létrehozása. Hello értékek többsége magától értetődő. webhely neve hello Azure, hozza létre, akkor előfordulhat, hogy a projekt neve nem egyezik.

```json
{
    "environmentSettings": {
        "webSite": {
            "name": "WebApplication26632",
            "location": "West US"
        },
        "databases": [{
            "connectionStringName": "DefaultConnection",
            "databaseName": "WebApplication26632_db",
            "serverName": "YourDatabaseServerName",
            "user": "sqluser2",
            "password": "",
            "edition": "",
            "size": "",
            "collation": "",
            "location": "West US"
        }]
    }
}
```
A virtuális gép létrehozásakor hello JSON-konfigurációs fájl következőhöz hasonló toohello következő. Vegye figyelembe, hogy egy felhőalapú szolgáltatás hello virtuális gép elhelyezése jön létre. hello a virtuális gépben hello szokásos végpontok web Access HTTP és HTTPS használatával, valamint a Web Deploy, amely lehetővé teszi, hogy a helyi számítógépen, a távoli asztal és a Windows PowerShell toohello webhelyet közzéteheti végpontok.

```json
{
    "environmentSettings": {
        "cloudService": {
            "name": "myusernamevm1",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myusernamevm1",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [{
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [{
            "connectionStringName": "",
            "databaseName": "",
            "serverName": "",
            "user": "",
            "password": ""
        }],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Szerkesztheti hello JSON konfigurációs toochange mi történik, ha futtatja a hello parancsfájlok közzététele. Hello `cloudService` és `virtualMachine` szakasz használata kötelező, de törölheti hello `databases` szakaszban, ha nincs szüksége. hello tulajdonságokhoz üres hello alapértelmezett konfigurációs fájl, amely a Visual Studio létrehozza a rendszer kötelező megadni. értékek rendelkeznek hello alapértelmezett konfigurációs fájlban szükség.

Ha olyan webhelyet, ahol több telepítési környezetekben (más néven üzembe helyezési ponti) helyett egy egyetlen munkakörnyezeti helyet az Azure-ban, hello tárhely neve is megadhat a hello neve hello webhely hello JSON-konfigurációs fájlt. Például, ha rendelkezik nevű webhellyel **saját webhely** és tárhely, nevű **tesztelése** majd hello URI saját webhely-test.cloudapp.net, de hello megfelelő nevet toouse hello konfigurációs fájlban mysite(test) . Csak ehhez Ha hello webhely és a tárolóhelyek már létezik az előfizetés. Ha azok még nem léteznek, hello tárolóhely megadása nélkül hello parancsfájl futtatásával hello webhelyet hoz létre, majd hozzon létre a hello hello tárolóhely [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), ezt követően futtassa hello parancsfájl hello módosított webhely neve. A web Apps üzembe helyezési kapcsolatos további információkért lásd: [átmeneti környezet az Azure App Service web Apps beállítása](app-service-web/web-sites-staged-publishing.md).

## <a name="how-toorun-hello-publish-scripts"></a>Hogyan toorun hello közzététele parancsfájlok
Soha ne futtatása előtt egy Windows PowerShell-parancsfájlt, ha hello végrehajtási házirend tooenable parancsfájlok toorun először meg kell adnia. Ez az egy biztonsági funkció tooprevent felhasználók Windows PowerShell-parancsfájlok futtatását, ha sebezhető toomalware vagy a vírusok, például a parancsfájlok végrehajtása.

### <a name="run-hello-script"></a>Hello parancsfájl futtatása
1. A projekt hello Web Deploy csomagot hozhat létre. A Web Deploy csomag egy tömörített (.zip fájl), amelyet toocopy tooyour webhelyére vagy a virtuális gép fájlokat tartalmazó. A Visual Studio Web Deploy platformra alapuló csomagok bármely webalkalmazás hozhat létre.

![Webes hozható létre csomag telepítése](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

További információkért lásd: [Útmutató: webes telepítési csomag létrehozása a Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). Automatizálható a Web Deploy csomag létrehozása hello hello szakaszban leírt módon **testreszabása és hello kiterjesztése közzététele parancsfájlok** a témakör későbbi részében.

1. A **Megoldáskezelőben**, nyissa meg a helyi menü hello hello parancsfájlt, és válassza **nyissa meg a PowerShell ISE**.
2. Ha hello első alkalommal ezen a számítógépen már futtatta a Windows PowerShell-parancsfájlok, nyisson meg egy parancssori ablakot rendszergazdai jogosultságokkal és típus hello a következő parancsot:

    ```powershell
    Set-ExecutionPolicy RemoteSigned
    ```

3. Jelentkezzen be tooAzure hello a következő parancs használatával.

    ```powershell
    Add-AzureAccount
    ```

    Amikor a rendszer kéri, adja meg a felhasználónevét és jelszavát.

    Vegye figyelembe, hogy ha automatizálni hello parancsfájl, ez a módszer az Azure hitelesítő adatokat adjanak nem fognak működni. Ehelyett használjon hello .publishsettings fájlt tooprovide hitelesítő adatokat. Egy alkalommal csak, paranccsal hello **Get-AzurePublishSettingsFile** toodownload hello fájlt az Azure-ból, és ezt követően használható **Import-AzurePublishSettingsFile** tooimport hello fájlt. Részletes útmutatásért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

4. (Választható) Ha azt szeretné, hogy toocreate Azure erőforrások, például a hello virtuális gépet, az adatbázis és a webhely anélkül, hogy a webalkalmazás közzétételét hello használata **Publish-WebApplication.ps1** hello parancsot **-konfiguráció** argumentum kötelező értéke toohello JSON-konfigurációs fájlt. Ez a parancssor a mely erőforrások toocreate hello JSON-konfigurációs fájl toodetermine használja. A más parancssori argumentumok hello alapértelmezett beállításait használja, mert hoz létre hello erőforrásokat, de nem tegye közzé a webalkalmazást. hello – részletes kimenet lehetővé teszi az eseményekről további információt.

    ```powershell
    Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json
    ```

5. Használjon hello **Publish-WebApplication.ps1** látható módon egy hello példák tooinvoke hello parancsfájl a következő parancsot, és tegye közzé a webalkalmazást. Ha toooverride hello alapértelmezett beállítások hello bármely egyéb argumentumok hello előfizetés nevét, például közzététele a csomag neve, a virtuális gép hitelesítő adatait, vagy az adatbázis hitelesítő adatait, ezeket a paramétereket is megadhat. Használjon hello **– részletes** toosee további információt a közzétételi folyamat hello hello folyamatban lehetőséget.

    ```powershell
    Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
    –SubscriptionName Contoso `
    -WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
    -DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
    -Verbose
    ```

    Ha egy virtuális gépet hoz létre, a hello parancs következőhöz hasonló hello. Ez a példa azt is bemutatja, hogyan toospecify hello több adatbázis hitelesítő adatait. Ezek a parancsfájlok hozzon létre virtuális gépek hello hello SSL-tanúsítvány származik nem egy megbízható legfelső szintű hitelesítésszolgáltatóval. Ezért figyelembe kell toouse hello **– AllowUntrusted** lehetőséget.

    ```powershell
    Publish-WebApplication.ps1 `
    -Configuration C:\Path\ADVM-VM-test.json `
    -SubscriptionName Contoso `
    -WebDeployPackage C:\Path\ADVM.zip `
    -AllowUntrusted `
    -VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
    -DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
    -Verbose
    ```

    hello parancsfájl adatbázisokat hozhat létre, de az adatbázis-kiszolgálók nem hoz létre. Ha azt szeretné, hogy egy adatbázis-kiszolgáló toocreate, használhatja a hello **New-AzureSqlDatabaseServer** hello Azure modul függvényt.

## <a name="customizing-and-extending-hello-publish-scripts"></a>Hello bővítik a parancsfájlok közzététele
Testre szabhatja a hello teszik közzé a parancsfájl és a JSON-konfigurációs fájlt. Windows PowerShell-moduljának hello funkciók hello **AzureWebAppPublishModule.psm1** nincsenek tervezett toobe módosítani. Ha most szeretné, hogy egy másik adatbázishoz toospecify vagy néhány hello hello virtuális gép tulajdonságainak módosítása, szerkessze a hello JSON-konfigurációs fájlt. Ha azt szeretné, hogy hello parancsfájl tooautomate összeállításakor és tesztelésekor a projekt tooextend hello funkcióit, a függvény kódcsonkok Megvalósíthat **Publish-WebApplication.ps1**.

a projekt tooautomate túl MSBuild behívó kód hozzáadása`New-WebDeployPackage` a kód példában látható módon. hello elérési toohello MSBuild parancs nem egyezik a telepített Visual Studio hello verziójától függően. tooget hello a helyes elérési utat, hello funkció használata **Get-MSBuildCmd**, ebben a példában látható módon.

### <a name="tooautomate-building-your-project"></a>a projekt tooautomate
1. Adja hozzá a hello `$ProjectFile` paraméter hello globális param szakaszban.

    ```powershell
    [Parameter(Mandatory = $false)]
    [ValidateScript({Test-Path $_ -PathType Leaf})]
    [String]
    $ProjectFile,
    ```

2. Hello függvény másolni `Get-MSBuildCmd` a parancsfájl-fájlba.

    ```powershell
    function Get-MSBuildCmd
    {
            process
    {

                $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                    Sort-Object {[double]$_.PSChildName} -Descending |
                                    Select-Object -First 1 |
                                    Get-ItemProperty -Name MSBuildToolsPath |
                                    Select -ExpandProperty MSBuildToolsPath

                $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

            return Get-Item $path
        }
    }
    ```

3. Cserélje le `New-WebDeployPackage` hello az alábbi kód, és hello sor hozhat létre hello helyőrzőt cserélje le a `$msbuildCmd`. Ez a kód a Visual Studio 2015 van. Ha a Visual Studio 2013 használ, módosítsa a hello **VisualStudioVersion** az alábbi tulajdonság túl`12.0`.

    ```powershell
    function New-WebDeployPackage
    {
        #Write a function toobuild and package your web application
    ```

    toobuild a webalkalmazást, MsBuild.exe használja. Útmutatásért lásd: a MSBuild parancssori útmutatóban: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)

    ```powershell
    Write-VerboseWithTime 'Build-WebDeployPackage: Start'

    $msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory

    Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
    ```

### <a name="start-execution-of-hello-build-command"></a>Hello build parancs végrehajtásának indítása

```powershell
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru

if ($job.ExitCode -ne 0) {
    throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain hello project name
$projectName = (Get-Item $ProjectFile).BaseName

#Construct hello path tooweb deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName

#Get hello full path for hello web deploy zip package. This is required for MSDeploy toowork
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir

Write-VerboseWithTime 'Build-WebDeployPackage: End'

return $WebDeployPackage
}
```

1. Hello hívás `New-WebDeployPackage` függvény előtt a sor: `$Config = Read-ConfigFile $Configuration` web Apps vagy `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` virtuális gépekhez.

    ```powershell
    if($ProjectFile)
    {
    $WebDeployPackage = New-WebDeployPackage
    }
    ```

2. Testre szabott hello parancsfájl használata a sikeres hello parancssorból meghívása `$Project` argumentum, mint a következő példa parancssori hello.

    ```powershell
    .\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
    -ProjectFile ..\WebApplication5\WebApplication5.csproj `
    -VMPassword @{Name="VMUser";Password="Test.123"} `
    -AllowUntrusted `
    -Verbose
    ```

    az alkalmazás tesztelése tooautomate hozzáadása kód túl`Test-WebApplication`. Lehet, hogy toouncomment hello sorainak **Publish-WebApplication.ps1** ahol ezek a funkciók nevezzük. Ha nem ad meg megvalósítást, manuálisan hozhat létre a projektet a Visual Studio, és hogy a futási hello tegye közzé parancsfájl toopublish tooAzure.

## <a name="publishing-function-summary"></a>Közzétételi összesítő függvény
hello Windows PowerShell parancssorába, használható függvények tooget súgóját hello paranccsal `Get-Help function-name`. hello súgó paraméter Súgó és példákat tartalmaz. azonos súgószöveg egyben hello parancsfájl forrásfájlok hello **AzureWebAppPublishModule.psm1** és **Publish-WebApplication.ps1**. hello parancsfájl és súgó a Visual Studio nyelven honosítva vannak.

**AzureWebAppPublishModule**

| Függvény neve | Leírás |
| --- | --- |
| Adja hozzá AzureSQLDatabase |Létrehoz egy új Azure SQL-adatbázist. |
| Adja hozzá AzureSQLDatabases |Hello értékek hello JSON-konfigurációs fájlt, amely a Visual Studio létrehozza az Azure SQL adatbázisokat hoz létre. |
| Adja hozzá AzureVM |Létrehoz egy Azure virtuális gépet, és visszaadja a hello hello URL-CÍMÉT, központilag telepített virtuális gép. hello függvény állít be hello Előfeltételek és hívások hello **New-AzureVM** (Azure modul) toocreate egy új virtuális gép működik. |
| Adja hozzá AzureVMEndpoints |Hozzáadja a bemeneti végpontok tooa új virtuális gép, és a virtuális gép hello hello új végpont beolvasása. |
| Adja hozzá AzureVMStorage |Ebben az előfizetésben hello hoz létre egy új Azure storage-fiók. hello fiók hello neve kezdődik "devtest" egyedi alfanumerikus karakterlánc követ. hello függvény hello új tárfiók hello nevét adja vissza. Adjon meg egy helyet vagy affinitáscsoport hello új tárfiók. |
| Adja hozzá AzureWebsite |Hello megadott név és hely egy webhelyet hoz létre. Ez a funkció meghívja a hello **New-AzureWebsite** hello Azure modul függvényt. Ha hello előfizetés már nem tartalmaz egy webhely hello megadott névvel, a függvény hello webhelyet hoz létre, és egy webhely objektumot ad vissza. Ellenkező esetben az eredmény `$null`. |
| Backup-előfizetés |Menti az aktuális Azure-előfizetés a hello hello `$Script:originalSubscription` változó parancsfájl hatókörében. Ez a funkció menti hello jelenlegi Azure-előfizetés (módon nyert `Get-AzureSubscription -Current`) és a tárfiók, és hello előfizetés módosítja ezt a parancsfájlt (hello változó tárolja `$UserSpecifiedSubscription`) és a tárfiókot, a parancsfájl hatókörében. Úgy, hogy elmenti hello értékek, a függvény használható, például a `Restore-Subscription`, toorestore hello eredeti aktuális előfizetés és a tárolási fiók toocurrent állapota Ha hello aktuális állapota megváltozott. |
| Keresés – AzureVM |Lekérdezi hello Azure virtuális géphez megadott. |
| Formátum-DevTestMessageWithTime |Dátum és idő tooa üdvözlőüzenetére lefoglalja. Ez a funkció a toohello hiba és részletes adatfolyamok üzenetek szolgál. |
| Get-AzureSQLDatabaseConnectionString |Állítja össze a kapcsolati karakterlánc tooconnect tooan Azure SQL-adatbázis. |
| Get-AzureVMStorage |Beolvasása hello hello első tárfiók hello mintát neve "devtest*" (kis-és nagybetűket) hello megadott helyre, vagy a kapcsolat csoportban. Ha hello "devtest*" tárfiók nem egyezik a hello helye vagy affinitáscsoportja, hello függvény figyelmen kívül hagyja azt. Meg kell adnia, vagy olyan helyre, vagy affinitáscsoport. |
| Get-MSDeployCmd |A parancs toorun hello MsDeploy.exe eszköz adja vissza. |
| Új AzureVMEnvironment |Megállapítja, vagy létrehoz egy virtuális gépet hello az előfizetést, amely megfelel a hello értékek hello JSON-konfigurációs fájlban. |
| Közzététel WebPackage |Felhasználási MsDeploy.exe és a webes közzétenni a csomagot. A zip-fájl toodeploy erőforrások tooa webhelyet. Ez a függvény nem ad kimenetet. Ha nem sikerül hello hívás tooMSDeploy.exe, hello függvény kivételt vált. tooget részletesebb kimenet, használjon hello **-Verbose** lehetőséget. |
| Közzététel WebPackageToVM |Hello paraméterértékek ellenőrzi, és ekkor meghívja a hello **Publish-WebPackage** függvény. |
| Olvasási-ConfigFile |Hello JSON-konfigurációs fájl ellenőrzi, és egy kivonattáblát a kijelölt értékeket adja vissza. |
| Visszaállítás-előfizetés |Alaphelyzetbe állítja a hello a jelenlegi előfizetés toohello eredeti előfizetés. |
| Teszt-AzureModule |Beolvasása `$true` Ha telepített hello Azure modul verziószáma 0.7.4 vagy újabb. Beolvasása `$false` Ha hello modul nincs telepítve, vagy korábbi verziójú. Ez a funkció nem paraméterrel rendelkezik. |
| Teszt-AzureModuleVersion |Beolvasása `$true` Ha hello hello Azure modul verziószáma 0.7.4 vagy újabb. Beolvasása `$false` Ha hello modul nincs telepítve, vagy korábbi verziójú. Ez a funkció nem paraméterrel rendelkezik. |
| Teszt-HttpsUrl |Hello bemeneti URL-cím tooa System.Uri objektumnak alakítja. Beolvasása `$True` Ha hello URL-cím abszolút pedig a rendszer https. Beolvasása `$false` Ha hello URL-cím relatív az sémája nem HTTPS, vagy a hello bemeneti karakterlánc nem lehet konvertált tooa URL-címet. |
| Teszt-tag |Beolvasása `$true` Ha egy tulajdonság vagy metódus hello objektum tagja. Ellenkező esetben adja vissza `$false`. |
| Írási-ErrorWithTime |Írja az aktuális idő hello előtagként hibaüzenetet. Ez a funkció meghívja a hello **formátum-DevTestMessageWithTime** függvény tooprepend hello idő hello üzenet toohello hibafolyam írása előtt. |
| Írási-HostWithTime |Egy üzenet toohello állomás program ír (**Write-Host**) aktuális idő hello előtagként. hello hatása toohello állomás program írásakor függ. A legtöbb üzemeltető Windows PowerShell írási ezek az üzenetek toostandard kimeneti. |
| Írási-VerboseWithTime |Írja az aktuális idő hello előtagként egy részletes üzenetet. Mert meghívja **Write-Verbose**, hello üzenet jelenik meg, hogy csak a hello hello parancsprogram futtatásakor **részletes** paraméter, vagy ha hello **VerbosePreference** beállítás ki van Állítsa be a túl**Folytatás**. |

**Közzététele: webalkalmazás**

| Függvény neve | Leírás |
| --- | --- |
| Új AzureWebApplicationEnvironment |Azure-erőforrások, például a webhelyek vagy virtuális gépet hoz létre. |
| Új WebDeployPackage |Ez a funkció nincs megvalósítva. Adhat hozzá parancsok a függvény toobuild a projekthez. |
| Közzététel AzureWebApplication |A webes alkalmazás tooAzure közzéteszi. |
| Közzététele: webalkalmazás |Hoz létre, és a Web Apps, a virtuális gépek, a SQL-adatbázisok és a storage-fiókok a Visual Studio webes projektet telepít. |
| Teszt:-webalkalmazás |Ez a funkció nincs megvalósítva. Adhat hozzá parancsok a függvény tootest az alkalmazást. |

## <a name="next-steps"></a>Következő lépések
PowerShell parancsfájl-kezelési beolvasásával kapcsolatos további [a Windows PowerShell parancsfájlok](https://technet.microsoft.com/library/bb978526.aspx) és egyéb Azure PowerShell-parancsfájlok, hello [Script Center](https://azure.microsoft.com/documentation/scripts/).
