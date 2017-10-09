---
title: "aaaContinuous kézbesítési felhőalapú szolgáltatások és a TFS az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset be folyamatos kézbesítését az Azure felhőalapú alkalmazásokba. Kódminták MSBuild parancssori utasításokat és a PowerShell-parancsfájlokat."
services: cloud-services
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4f3c93c6-5c82-4779-9d19-7404a01e82ca
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/12/2017
ms.author: kraigb
ms.openlocfilehash: c0e5e72ffbd3c05b84ce1733068e92c528bcc4b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Azure felhőszolgáltatások folyamatos kézbesítés
hello ebben a cikkben leírt eljárás bemutatja, hogyan tooset be folyamatos kézbesítését az Azure felhőalapú alkalmazásokat. Ez a folyamat automatikusan a csomagok létrehozását és telepítését a hello csomag tooAzure után minden kód be teszi lehetővé. hello csomag felépítési folyamat cikkben leírt egyenértékű toohello **csomag** parancs a Visual Studio és a közzétételi lépéseket is egyenértékű toohello **közzététel** a Visual Studio parancsot.
hello cikk magában foglalja az hello módszert szeretné használni toocreate build kiszolgáló MSBuild parancssori utasításokat és a Windows PowerShell-parancsfájlok, és azt is bemutatja, hogyan konfigurálja az toooptionally a Visual Studio Team Foundation Server - definíciók csoport létrehozása toouse hello MSBuild parancsok és a PowerShell-parancsfájlokat. hello testre szabható a összeállító környezetet és az Azure környezetekben történik.

Is használhatja a Visual Studio Team Services, egy, a TFS verziójának Azure szolgáltatásban üzemeltetett, toodo ez könnyebben. 

Kezdés előtt kell közzé tenni az alkalmazást a Visual Studio eszközből.
Ezzel biztosíthatja, hogy minden hello erőforrás elérhető-e és inicializálva-e amikor megpróbál tooautomate hello kiadvány folyamat.

## <a name="1-configure-hello-build-server"></a>1: hello Build kiszolgáló konfigurálása
Egy Azure-csomag létrehozása MSBuild használatával, előtt telepítenie kell hello szükséges szoftverek és eszközök hello build kiszolgálón.

A Visual Studio nem szükséges toobe hello build kiszolgálón telepítve. Toouse Team Foundation Build Service toomanage build kiszolgálóját, hajtsa végre hello hello utasításait [Team Foundation Build Service] [ Team Foundation Build Service] dokumentációját.

1. Hello build kiszolgálón telepítse a hello [.NET-keretrendszer 4.5.2-es][.NET Framework 4.5.2], mert az MSBuild tartalmazza.
2. Telepítse legújabb hello [.NET-keretrendszerhez készült Azure szerzői eszközök](https://azure.microsoft.com/develop/net/).
3. Telepítse a hello [.NET Azure-könyvtárakban](http://go.microsoft.com/fwlink/?LinkId=623519).
4. Másolás hello Microsoft.WebApplication.targets fájlt a Visual Studio telepítési toohello kiszolgáló létrehozása.

   A telepített Visual Studio számítógépen, a fájl C: hello könyvtárban található\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications. Másolja az toohello hello build kiszolgálón ugyanabban a könyvtárban.
5. Telepítse a hello [Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: MSBuild parancsokkal csomag létrehozása
Ez a szakasz ismerteti, hogyan tooconstruct az MSBuild parancsot, amely egy Azure-csomagot hoz létre. Futtassa ezt a lépést a hello build server tooverify, hogy minden helyesen van-e állítva, és hogy hello MSBuild parancs valóban azt toodo. Hello a következő szakaszban leírt módon a parancssor tooexisting fordítási parancsprogramot hello build kiszolgálón, vagy használhatja a TFS Build definícióban hello parancssori vagy hozzáadni. Parancssori paraméterek és MSBuild kapcsolatos további információkért lásd: [MSBuild parancssori hivatkozás](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1. Visual Studio hello build kiszolgálóra van telepítve, keresse meg és válassza a **Visual Studio parancssorból** a hello **Visual Studio eszközök** Windows-mappa.

   Ha a Visual Studio hello build kiszolgálón nincs telepítve, nyisson meg egy parancssort, és győződjön meg arról, hogy MSBuild.exe elérhető-e az elérési út. MSBuild telepítve van a .NET-keretrendszer hello hello elérési útja: % WINDIR %\\Microsoft.NET\\keretrendszer\\*verzió*. Például ha .NET Framework 4 telepítve van a MSBuild.exe toohello PATH környezeti változóba hozzáadásához írja be a hello parancs hello parancssorba a következő:

       set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"
2. Hello parancssorban keresse meg a toohello toobuild kívánt Azure-projekt tartalmazó mappára.
3. Futtassa a MSBuild hello/TARGET: Publish beállítást, mint például a következő hello:

       MSBuild /target:Publish

   Ez a beállítás rövidíthető /t: közzététele. MSBuild hello /t:Publish beállítást kell nem keverendő össze a hello közzététel parancsok, amely a Visual Studio Ha hello Azure SDK telepítve van. hello /t: Publish beállítást, csak a buildek hello Azure csomagok. Azt nem kell telepítenie hello csomagok, mint a Visual Studio hello közzététel parancsok.

   Szükség esetén megadhatja az MSBuild paraméter hello projekt nevét. Ha nincs megadva, az aktuális könyvtár hello szolgál. MSBuild parancssori beállításokkal kapcsolatos további információkért lásd: [MSBuild parancssori hivatkozás](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).
4. Keresse meg a hello kimeneti. Alapértelmezés szerint ez a parancs létrehoz egy könyvtárat kapcsolat toohello gyökérmappában hello projekthez, például a *ProjectDir*\\bin\\*konfigurációs* \\ App.publish\\. Azure-projekt összeállításakor létrehozhat két fájlt, a hello csomagfájl és a konfigurációs fájl kísérő hello:

   * Project.cspkg
   * ServiceConfiguration. *TargetProfile*.cscfg

   Alapértelmezés szerint minden Azure-projekt tartalmazza egy szolgáltatás konfigurációs fájljában (.cscfg fájl) (hibakereséshez) helyi buildek és egy másikat a felhő (átmeneti vagy üzemi) buildek, de adhat hozzá vagy távolítsa el a szolgáltatás konfigurációs fájlokat, igény szerint. Visual Studio csomag összeállításakor kérni fogja melyik szolgáltatás konfigurációs fájl tooinclude hello csomag mellett.
5. Adja meg a hello szolgáltatás konfigurációs fájljában. Ha egy csomag MSBuild hozhat létre, hello helyi szolgáltatás konfigurációs fájlja veszi fel alapértelmezés szerint. egy másik szolgáltatás konfigurációs fájlja tooinclude hello MSBuild parancs, mint például a következő hello TargetProfile tulajdonságának beállítása:

       MSBuild /t:Publish /p:TargetProfile=Cloud
6. Adjon meg hello kimeneti hello helyet. A /p:PublishDir használatával set hello elérési út =*Directory* \\ beállítást, többek között a záró perjelet elválasztó, mint például a következő hello hello:

       MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

   Amennyiben az előre összeállított és tesztelt egy megfelelő MSBuild sor toobuild a projektek parancsot, és összefogni azokat az Azure-csomag, a parancssor tooyour build parancsprogramokat is megadhat. Ha a build server egyéni parancsfájlokat használ, ez a folyamat az egyéni létrehozási folyamata során a mintaadatokról függ. Ha a TFS egy összeállító környezetet használ, majd kövesse hello tovább lépés tooadd hello Azure-csomag létrehozási tooyour felépítési folyamat hello utasításait.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: build a csomagot, a TFS-csoport létrehozása
Ha TFS létrehozási gépként beállítása, a build vezérlő és hello készítsen a kiszolgáló beállítása Team Foundation Server (TFS), majd igény szerint állíthatja be az automatikus létrehozási az Azure-csomagnak a. Hogyan tooset be és a Team Foundation server használatát a buildelési rendszer: kapcsolatos [a buildelési rendszer kibővítési][Scale out your build system]. Különösen a következő eljárás feltételezi, hogy konfigurálta a build kiszolgáló leírtak [központi telepítése és build kiszolgálók][Deploy and configure a build server], és létrehozott egy csapatprojekt létrehozott felhő Projekt hello csapatprojektben.

tooconfigure TFS toobuild Azure csomagok, hajtsa végre a következő lépéseket hello:

1. Válassza ki a Visual Studio a fejlesztési számítógépen hello Nézet menü **Team Explorer**, vagy válassza a Ctrl +\\, Ctrl + M. A csapat Explorer-ablakban bontsa ki a hello **buildek** csomópont, vagy válasszon hello **buildek** lapon, és válassza a **új Build Definition**.

   ![Új beállítás található definíció létrehozása][0]
2. Válassza ki a hello **eseményindító** lapot, és adja meg a hello szükséges feltételeit, ha azt szeretné hello beépített csomag toobe. Adja meg például **folyamatos integrációt** akkor fordul elő, amikor egy forrás szabályozása be toobuild hello csomag.
3. Válassza ki a hello **adatforrás-beállítások** lapot, és győződjön meg arról, hogy a projektmappa hello szerepel **vezérlő forrásmappa** oszlop, és hello állapota **Active**.
4. Válassza ki a hello **Build alapértelmezett** fülre, és a Build vezérlő, ellenőrizze a hello hello build kiszolgáló nevét.  Válassza ki, hello beállítás **másolási build kimeneti toohello következő drop mappa** , és adja meg a szükséges hello eldobott üzenetek helye.
5. Válassza ki a hello **folyamat** fülre. Hello folyamat lapon válassza a hello alapértelmezett sablon, a **Build**, hello projekt válassza, ha nincs bejelölve, és bontsa ki a hello **speciális** hello szakasz **Build**hello rács szakasza.
6. Válasszon **MSBuild-argumentumok**, és állítsa be a megfelelő MSBuild parancssori argumentumok hello fenti a 2. lépésben leírtak szerint. Adja meg például **/t: /p:PublishDir közzététele =\\\\myserver\\esik\\**  toobuild egy csomagot, és másolja hello csomag fájlok toohello hely \\ \\myserver\\esik\\:

   ![MSBuild-argumentumok][2]

   > [!NOTE]
   > Másolás hello fájlok tooa nyilvános megosztás teszi egyszerűbbé toomanually a fejlesztési számítógépen hello csomagok központi telepítése.
7. A build lépés hello sikeres teszteléséhez módosítása tooyour projektben ellenőrzésével, vagy egy új build sorba. mentése új buildverziót, a csapat Explorer tooqueue kattintson a jobb gombbal **összes Build definíciókat,** majd **várólista új Build**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: a PowerShell-parancsfájl használatával csomag közzététele
Ez a szakasz ismerteti, hogyan tooconstruct hello felhő alkalmazáscsomag közzétehető Windows PowerShell-parancsfájl kimeneti tooAzure opcionális paraméterek használatával. Ez a parancsfájl hello build a build egyéni automatizálás lépés után hívható. Azt a folyamatsablon munkafolyamat tevékenységei a Visual Studio TFS Team Build is hívható.

1. Telepítse a hello [Azure PowerShell-parancsmagok] [ Azure PowerShell cmdlets] (v0.6.1 vagy újabb).
   Hello parancsmag telepítési fázis során válassza tooinstall egy beépülő modulként. Vegye figyelembe, hogy a hivatalosan támogatott felváltja hello régebbi kínált a Codeplex webhelyen, bár a korábbi verziók hello volt számozott 2.x.x.
2. Indítsa el az Azure PowerShell hello Start menüből vagy a kezdőlapot. Ily módon indul el, ha hello Azure PowerShell-parancsmagok lesz betöltve.
3. Hello PowerShell-parancssorba, győződjön meg arról, hogy a PowerShell-parancsmagok hello hello részleges parancs megadásával vannak betöltve `Get-Azure` és hello majd nyomja le a Tab billentyű utasítás befejezésére.

   Ha ismételten hello Tab billentyű lenyomása, különböző Azure PowerShell-parancsokat kell megjelennie.
4. Győződjön meg arról, hogy Azure-előfizetés tooyour csatlakozhasson ehhez hello .publishsettings fájlt importálja az előfizetési adatokat.

   `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

   Majd adja meg a hello parancsot

   `Get-AzureSubscription`

   Ez az előfizetés információkat jeleníti meg. Győződjön meg arról, hogy minden rendben.
5. Ez a cikk a parancsfájlok mappába c: hello végén megadott hello parancsfájl sablon mentése\\parancsfájlok\\WindowsAzure\\**PublishCloudService.ps1**.
6. Tekintse át a hello paraméterek szakaszban hello parancsfájl. Adja hozzá, vagy módosítsa az alapértelmezett értékeket. Sikeres explicit paraméterek mindig felülbírálhatja ezeket az értékeket.
7. Győződjön meg arról, érvényes felhőalapú szolgáltatás, és az előfizetés, amely telepíthető hello által létrehozott tárfiókok közzététele parancsfájl. A tárfiók (blob-tároló) használt tooupload kell, és a központi telepítési csomag- és konfigurációs fájl hello ideiglenesen tárolja, a központi telepítés létrehozása közben.

   * új felhőalapú szolgáltatás toocreate, hívása a parancsfájl vagy használata hello [Azure-portálon](https://portal.azure.com). egy teljesen minősített tartománynevet az előtag hello felhőszolgáltatás neve lesz, és ezért egyedinek kell lennie.

         New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
   * új tárfiók toocreate, hívása a parancsfájl vagy használata hello [Azure-portálon](https://portal.azure.com). egy teljesen minősített tartománynevet az előtag hello tárfiók neve lesz, és ezért egyedinek kell lennie. Próbálja meg hello ugyanazt a nevet használja, mint a felhőalapú szolgáltatáshoz.

         New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
8. Hello parancsfájl hívása közvetlenül az Azure PowerShell, illetve a hello csomag build után be a parancsfájl tooyour állomás build automation toooccur vezetékes.

   > [!IMPORTANT]
   > hello parancsfájl mindig törölje, vagy cserélje le a meglévő telepítések alapértelmezés szerint, ha azokat. Ez akkor szükségesek, hogy lehetővé teszik a folyamatos Automation ahol nincs felhasználói adatkérésekhez lehetséges.
   >
   >

   **1. példa:** átmeneti környezetben a szolgáltatások folyamatos üzembe helyezés toohello:

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   Ez általában következnek teszt futtatásakor ellenőrzési és a virtuális IP-címcsere. hello VIP swap megteheti a hello [Azure-portálon](https://portal.azure.com) vagy hello áthelyezés telepítési parancsmag használatával.

   **2. példa:** folyamatos üzembe helyezés toohello éles környezetben található dedikált teszt szolgáltatás

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   **Távoli asztali:**

   Ha a távoli asztal engedélyezve van az Azure-projekt tooperform további egyszeri lépéseket tooensure hello felhőalapú szolgáltatás megfelelő tanúsítvány van feltöltve. Ez a parancsfájl által megcélzott tooall felhőszolgáltatások szüksége lesz.

   Keresse meg a szerepkörök által várt hello tanúsítvány ujjlenyomata értékeket. Az ujjlenyomat értékek láthatók a hello tanúsítványok szakasz a felhőalapú konfigurációs fájl (pl. ServiceConfiguration.Cloud.cscfg). Egyben hello távoli asztali konfigurálása párbeszédpanel a Visual Studio alkalmazásban látható beállítások megjelenítése és a nézet hello kiválasztott tanúsítvány.

       <Certificates>
             <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
       </Certificates>

   A távoli asztal-tanúsítványok feltöltésére használja a következő parancsmag parancsfájl hello egyszeri telepítő lépésként:

       Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

   Példa:

       Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

   Másik lehetőségként exportálhatja hello tanúsítványfájl PFX a titkos kulcs és a feltöltés tanúsítványok tooeach cél felhőalapú szolgáltatás használata a [Azure-portálon](https://portal.azure.com).

   <!---
   Fixing broken links for Azure content migration from ACOM tooDOCS. I'm unable toofind a replacement links, so I'm commenting out this reference for now. hello author can investigate in hello future. "Read hello following article toolearn more: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx.
   -->
   **Frissítés a központi telepítés vagy. Törölje a központi telepítés –\> új központi telepítés**

   hello parancsprogrammal alapértelmezés szerint végezheti az frissítés központi telepítése ($enableDeploymentUpgrade = 1) Ha nem paraméter átadott vagy explicit módon átadott 1 érték. Az egyetlen példány Ez azt az előnyt, teljes körű telepítésére rövidebb ideig tart. A példányok hello előnye, hogy bizonyos esetekben fut, míg mások is annak magas rendelkezésre állást igénylő frissítése (a frissítési tartomány érdekében), valamint a VIP nem lesz törölve.

   Frissítés telepítése is le kell tiltani a hello parancsfájl ($enableDeploymentUpgrade = 0), vagy úgy, hogy *- enableDeploymentUpgrade 0* paraméterként, amely megváltoztatja a parancsfájl viselkedés toofirst törölje a meglévő telepítést, és hozzon létre egy Új központi telepítést.

   > [!IMPORTANT]
   > hello parancsfájl mindig törölje, vagy cserélje le a meglévő telepítések alapértelmezés szerint, ha azokat. Ez az Automation folyamatos kézbesítési ahhoz szükséges, hogy nincs felhasználói/operátor kérdés esetén lehetséges.
   >
   >

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: közzétenni a csomagot, a TFS-csoport létrehozása
Ez az opcionális lépés csatlakozik a TFS-csoport létrehozása toohello parancsfájl a 4, amely kezeli a hello csomag build tooAzure közzétételét. Ez a módosítása hello használják a build definition hello végén lévő hello munkafolyamat futtatása egy közzétételi tevékenység folyamatsablon terjed ki. Közzététel tevékenység hello végrehajtja a hello build a paraméterek átadása a PowerShell-parancsot. Hello MSBuild célozza, és tegye közzé a parancsfájl kimenete fog kell adatcsatornán hello szabványos felépítési művelet kimenetében be.

1. Hello felelős Build definíciójának szerkesztése folyamatos központi telepítése.
2. Jelölje be hello **folyamat** fülre.
3. Hajtsa végre a [ezeket az utasításokat](http://msdn.microsoft.com/library/dd647551.aspx) tooadd hello tevékenység projektben folyamatsablon felépítéséhez, töltse le a hello alapértelmezett sablon, adja hozzá a projekthez hello és be. Adjon meg új nevet, például a AzureBuildProcessTemplate hello build folyamatsablon.
4. Térjen vissza a toohello **folyamat** lapot, és használja **részletek megjelenítése** tooshow érhető el összeállítási folyamat sablonok listájának. Válassza ki a hello **új...**  gombra, és keresse meg a most hozzáadott és be van jelölve, toohello projekt. Keresse meg az imént létrehozott hello sablont, és válassza a **OK**.
5. Nyissa meg hello folyamatsablon kijelölt szerkesztésre. Úgy is megnyithatja közvetlenül hello munkafolyamat-tervezőben vagy hello XML-szerkesztő toowork a hello XAML-kódot.
6. Adja hozzá a következő új argumentumok listájának hello argumentumok lapján hello munkafolyamat-Tervező külön sorként hello. Minden argumentum irányba kell rendelkeznie, és írja be a = = karakterlánc. Ezek mely majd get használt toocall hello közzététele parancsfájl hello munkafolyamatba hello build definícióból használt tooflow paraméterek lesz.

       SubscriptionName
       StorageAccountName
       CloudConfigLocation
       PackageLocation
       Environment
       SubscriptionDataFileLocation
       PublishScriptLocation
       ServiceName

   ![Argumentumok listájának megjelenítése][3]

   hello megfelelő XAML néz ki:

       <Activity  _ />
         <x:Members>
           <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
           <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
           <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
           <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
           <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
           <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
           <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
           <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
           <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
           <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
           <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
           <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
           <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
           <x:Property Name="GetVersion" Type="InArgument(x:String)" />
           <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
           <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
           <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
           <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
           <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
           <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
           <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
           <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
           <x:Property Name="Environment" Type="InArgument(x:String)" />
           <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
           <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
           <x:Property Name="ServiceName" Type="InArgument(x:String)" />
         </x:Members>

         <this:Process.MSBuildArguments>
7. Felvehet egy új sorozatot futtassa az ügynök hello végén:

   1. Először vegyen fel egy Ha utasítás tevékenység toocheck meg egy érvényes parancsfájlt. Hello feltétel toothis érték beállítása:

          Not String.IsNullOrEmpty(PublishScriptLocation)
   2. Hello majd hello Ha utasítás esetében adja hozzá egy új feladatütemezési tevékenységet. Set hello megjelenítési név too'Start közzététele "
   3. A Start közzététele a kijelölt feladatütemezési hello adja hozzá az alábbi listán szereplő új változók a munkafolyamat-Tervező hello lapján elemeinek külön sorban. Minden változót kell változótípus = karakterlánc és a hatókör = Start közzététele. Ezek a munkafolyamatba, mely majd get használt toocall hello közzététele parancsfájl hello build definícióból használt tooflow paraméterek lesz.

      * SubscriptionDataFilePath, karakterlánc típusú
      * PublishScriptFilePath, karakterlánc típusú

        ![Új változók][4]
   4. Ha a TFS 2012 használ, vagy korábbi, adjon hozzá egy ConvertWorkspaceItem tevékenységet a hello elején hello új feladatütemezési. Ha a TFS 2013, vagy később, adjon hozzá egy GetLocalPath tevékenységet hello új feladatütemezési hello elején. Egy ConvertWorkspaceItem beállítása hello tulajdonságok az alábbiak szerint: irányát ServerToLocal, DisplayName = = 'Convert közzététele parancsfájl fájlnév', bemeneti = "PublishScriptLocation", az eredmény = "PublishScriptFilePath", a munkaterület = "Munkaterület". GetLocalPath tevékenységhez, állítsa be a hello tulajdonság IncomingPath too'PublishScriptLocation ", és az eredmény too'PublishScriptFilePath hello". A tevékenység konvertálja hello elérési toohello közzététele TFS helyekről parancsfájl (ha van ilyen) tooa szabványos helyi lemez elérési útja.
   5. Ha a TFS 2012 használ, vagy korábbi, adjon hozzá egy másik ConvertWorkspaceItem tevékenységet, hello végén lévő hello új feladatütemezési. Irány ServerToLocal, DisplayName = = "Átalakításáról előfizetés fájlnév" bemeneti = "SubscriptionDataFileLocation", az eredmény = "SubscriptionDataFilePath" munkaterület = "Munkaterület". Ha a TFS 2013, vagy később, a másik GetLocalPath hozzáadása. IncomingPath = "SubscriptionDataFileLocation", és az eredmény = "SubscriptionDataFilePath."
   6. Hello hello végén InvokeProcess tevékenység hozzáadása új feladatütemezési.
      Build Definition hello által átadott e tevékenység hívások PowerShell.exe hello argumentumokkal.

      + Argumentumok = String.Format ("-""{0}" "- szolgáltatásnév {1} - storageAccountName {2} - packageLocation""{3}" "- cloudConfigLocation""{4}" "– subscriptionDataFile""{5}" "– selectedSubscription {6} fájlt-környezet""{7}" "",  PublishScriptFilePath, ServiceName, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, környezet)
      + DisplayName = Execute parancsfájl közzététele
      + FileName = "PowerShell" (a hello idézőjelek között)
      + OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)
   7. A hello **szabványos kimeneti kezelni** a InvokeProcess a szövegmező területen állítsa be a hello szövegmező érték too'data ". Ez az egy változó toostore hello szabványos kimeneti adatokat.
   8. Adjon hozzá egy WriteBuildMessage tevékenységet hello alatt **szabványos kimeneti kezelni** szakasz. Fontos hello beállítása = "Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High" és a hello Message = "data". Ez biztosítja a szabványos kimeneti hello parancsfájl fog írása toohello felépítési művelet kimenetében.
   9. A hello **kezelni hiba kimeneti** a InvokeProcess a szövegmező területen állítsa be a hello szövegmező érték too'data ". Ez az egy változó toostore hello standard hiba adatok.
   10. Adjon hozzá egy WriteBuildError tevékenységet hello alatt **kezelni hiba kimeneti** szakasz. Állítsa be az üdvözlő üzenet = "data". Ez biztosítja, hogy toohello build hiba kimeneti hello standard hibák hello parancsfájl fog írása.
   11. Javítsa ki a hibákat, kék felkiáltójel jelek jelölik. Mutasson a felkiáltójel jelek tooget hello hiba vonatkozó javaslat. Törölje a hibák hello munkafolyamat mentése.

   hello végeredményét hello munkafolyamat-tevékenységek fog kinézni hello Designer közzététele:

   ![Munkafolyamat-tevékenységek][5]

   hello végeredményét hello közzététele a munkafolyamat-tevékenységek fog kinézni XAML-kódban:

       <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
           <If.Then>
             <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
               <Sequence.Variables>
                 <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                 <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
               </Sequence.Variables>
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
               <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                 <mtbwa:InvokeProcess.ErrorDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.ErrorDataReceived>
                 <mtbwa:InvokeProcess.OutputDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.OutputDataReceived>
               </mtbwa:InvokeProcess>
             </Sequence>
           </If.Then>
         </If>
       </Sequence>
8. Hello létrehozási munkafolyamat sablont, és ellenőrizze a fájl mentéséhez.
9. Hello build definíciójának szerkesztése (lezárhatja, amennyiben már meg nyitva), és jelölje be hello **új** gombra kattint, ha még nem látja hello új sablon folyamatsablonok hello listájában.
10. Hello paraméter tulajdonságértékei állíthatók hello egyebek szakasz a következőképpen:

    1. CloudConfigLocation = "c:\\esik\\app.publish\\ServiceConfiguration.Cloud.cscfg" *ezt az értéket, amelyből származik: ($PublishDir)ServiceConfiguration.Cloud.cscfg*
    2. PackageLocation = "c:\\esik\\app.publish\\ContactManager.Azure.cspkg" *ezt az értéket, amelyből származik: ($PublishDir)($ProjectName) .cspkg*
    3. PublishScriptLocation = "c:\\parancsfájlok\\WindowsAzure\\PublishCloudService.ps1"
    4. ServiceName = "mycloudservicename" *használata hello megfelelő felhőszolgáltatás neve itt*
    5. Környezet = "Átmeneti"
    6. StorageAccountName = "mystorageaccountname" *használata hello megfelelő tárfióknév Itt*
    7. SubscriptionDataFileLocation = "c:\\parancsfájlok\\WindowsAzure\\Subscription.xml"
    8. SubscriptionName = "alapértelmezett"

    ![Tulajdonság értékei][6]
11. Hello módosítások toohello Build definíció mentése.
12. Feldolgozási sor egy Build tooexecute mindkét hello csomag létrehozási, és tegye közzé. Ha egy eseményindító tooContinuous integráció beállítása, végrehajtja a Ez a viselkedés a minden egyes bejelentkezéskor.

### <a name="publishcloudserviceps1-script-template"></a>PublishCloudService.ps1 parancsfájl sablon
```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy too$servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according too$alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress tooactivity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Következő lépések
tooenable távoli hibakeresés folyamatos kézbesítés használata esetén lásd: [folyamatos kézbesítési toopublish tooAzure használatakor távoli hibakeresésének engedélyezése](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

[Team Foundation Build Service]: https://msdn.microsoft.com/library/ee259687.aspx
[.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
[.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
[Scale out your build system]: https://msdn.microsoft.com/library/dd793166.aspx
[Deploy and configure a build server]: https://msdn.microsoft.com/library/ms181712.aspx
[Azure PowerShell cmdlets]: /powershell/azureps-cmdlets-docs
[hello .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
[0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
[2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
[3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
[4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
[5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
[6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
