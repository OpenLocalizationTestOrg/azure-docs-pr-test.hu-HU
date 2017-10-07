---
title: "Automation DSC folyamatos üzembe helyezés Chocolatey aaaAzure |} Microsoft Docs"
description: "DevOps folyamatos üzembe helyezés Azure Automation DSC és Chocolatey Csomagkezelő használatával.  Példa a teljes JSON ARM-sablon és a PowerShell-forrás."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: c0baa411-eb76-4f91-8d14-68f68b4805b6
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/29/2016
ms.author: golive
ms.openlocfilehash: 60af52af5f834fd48e3a0dc4677919397b53f0f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="usage-example-continuous-deployment-toovirtual-machines-using-automation-dsc-and-chocolatey"></a>Példa: A folyamatos üzembe helyezés tooVirtual Automation DSC és Chocolatey használó számítógépek
DevOps világában nincsenek sok eszközök tooassist hello folyamatos integrációt feldolgozási folyamat különböző pontokkal.  Azure Automation kívánt állapot konfigurációs szolgáltatása (DSC) egy üdvözlő új hozzáadása toohello beállításokat DevOps csoportokat is alkalmaz.  Ez a cikk bemutatja a beállítás mentése folyamatos Deployment (CD) a Windows-számítógépen.  Könnyen kiterjesztheti hello technika tooinclude tetszőleges számú, Windows rendszerű számítógépek hello szerepkörben (egy webhely, például), és ott tooadditional a szükséges szerepkörök is.

![Infrastruktúra-szolgáltatási virtuális gépek folyamatos üzembe helyezés](./media/automation-dsc-cd-chocolatey/cdforiaasvm.png)

## <a name="at-a-high-level"></a>Magas szinten
Igen, felhasználó ide látogatott bit, de Szerencsére az is lehet osztani két fő folyamatok: 

* Kód írása tesztelés, létrehozása és közzététele telepítőcsomagok hello rendszer fő- és alverzió verzióit. 
* Létrehozása, és a virtuális gépek, amelyek telepítése és hello kód végrehajtása hello csomagok kezelése.  

Ha mindkét alapvető eljárás helyen, akkor a rövid lépés tooautomatically hello csomag bármely adott virtuális gép fut, az új verzió létrehozása és telepítése.

## <a name="component-overview"></a>Az összetevő áttekintése
Például a csomag kezelők [apt get](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool) hello Linux world, de nem ennyire hello Windows világ közérthető jól ismert.  [Chocolatey](https://chocolatey.org/) egy ilyen dolog, és Scott Hanselman [blog](http://www.hanselman.com/blog/IsTheWindowsUserReadyForAptget.aspx) hello a témakör egy nagyszerű bevezetés.  A ez már a vég Chocolatey lehetővé teszi egy központi tárházban csomagok tooinstall-csomagok hello parancssorból Windows rendszerben.  Hozhat létre és kezelheti a saját tárház, és Chocolatey csomagok kijelölt adattárak tetszőleges számú telepíthetnek.

Szükségeskonfiguráció-konfiguráló (DSC) ([áttekintése](https://technet.microsoft.com/library/dn249912.aspx)), amely lehetővé teszi toodeclare hello konfigurációt, amelyet a gép PowerShell eszköz.  Feltételezzük például, hogy tudja, "Chocolatey telepíteni kívánt, kívánt telepített IIS-t, szeretném a 80-as port nyitva, kívánt 1.0.0 a webhely telepített verzió."  hello DSC helyi Configuration Manager (LCM) valósítja meg, hogy a konfigurálás. A DSC lekérni a kiszolgáló rendelkezik egy konfigurációinak a gépek-tárházat. hello LCM minden számítógépen ellenőrzi a rendszeres időközönként toosee konfigurációja megfelel hello tárolt konfigurációs. Az állapotjelentés, vagy kísérlet toobring hello gép hello tárolt konfigurációjával igazítása programba. Szerkesztheti a hello tárolt konfigurációs hello lekéréses kiszolgáló toocause egy gép vagy gépek toocome igazítását a módosított hello konfigurációjával kapcsolatban.

Azure Automation szolgáltatásbeli egy olyan felügyelt szolgáltatás, amely lehetővé teszi tooautomate különböző feladatok runbookok, csomópontok, hitelesítő adatokat, erőforrások és ütemezési és globális változókat eszközök használatával a Microsoft Azure-ban. Az Azure Automation DSC kibővíti az automatizálási funkció tooinclude PowerShell DSC eszközök.  Ez egy nagy [áttekintése](automation-dsc-overview.md).

A DSC-erőforrás egy modul kód, amely az adott funkciót, például a hálózat, az Active Directory és az SQL Servert kezelésére.  hello Chocolatey DSC erőforrás tudja, hogyan tooaccess a NuGet-kiszolgáló (többek között), töltse le a csomagok, csomagok és stb.  Nincsenek DSC sok egyéb erőforrások hello [PowerShell-galériában](http://www.powershellgallery.com/packages?q=dsc+resources&prerelease=&sortOrder=package-title).  Ezek a modulok telepíti azokat az Azure Automation DSC-lekérni a kiszolgáló (,), ezek a konfigurációk által használható.

Resource Manager-sablonok deklaratív teszik lehetővé a létrehozást, az infrastruktúra - többek között a hálózatok, az alhálózatok, a hálózati biztonsági és útválasztási, betölteni, terheléselosztók, hálózati adapterek, virtuális gépek és így tovább.  Íme egy [cikk](../azure-resource-manager/resource-manager-deployment-model.md) , hogy a szolgáltatás összehasonlítja Resource Manager üzembe helyezési modellben (deklaratív) hello hello Azure szolgáltatásfelügyelet (ASM vagy klasszikus) a központi telepítési modell (imperatív), és ismerteti hello alapvető erőforrás szolgáltatókat, a számítási, tárolási és hálózati.

Egy Resource Manager-sablon egy alapfunkciója esetén a képes tooinstall hello VM be egy Virtuálisgép-bővítmény, mert ki van építve.  A Virtuálisgép-bővítmény például egy egyéni parancsfájl futtatása, a víruskereső szoftver telepítése vagy a DSC-konfigurációs parancsprogram futtatása adott képességekkel rendelkezik.  Nincsenek számos más típusú Virtuálisgép-bővítmények.

## <a name="quick-trip-around-hello-diagram"></a>Gyors út körül hello diagramja
Hello felső-től kezdődően a kódot ír összeállítása és tesztelése, majd telepítőcsomag létrehozása.  Chocolatey kezelni tud a különféle telepítési csomagokat, például az MSI-fájl, MSU, ZIP.  És akkor áll hello teljes PowerShell toodo hello tényleges telepítés Ha Chocolatey tartozó natív képességek nem elég tooit fel.  Hello csomag üzembe valahol érhető el – a csomag tárházba.  A használati példában az egyik nyilvános mappájába egy Azure blob storage-fiók, de bárhol lehet.  Felügyeleti csomag metaadatok natív módon a NuGet-kiszolgálók és néhány más chocolatey működik.  [Ez a cikk](https://github.com/chocolatey/choco/wiki/How-To-Host-Feed) hello elérhető lehetőségeket ismerteti.  A használati példa NuGet.  Egy Nuspec a csomagok metaadatait.  hello Nuspec "lefordítva" NuPkg meg, és a NuGet-kiszolgáló tárolja.  Amikor a konfigurációs csomag kér név szerint, és egy NuGet kiszolgálóra hivatkozik, hello Chocolatey DSC-erőforrás (most a virtuális gép hello) grabs hello csomagot, és telepíti azt.  A csomag egy adott verziójához is kérheti.

Hello bal alsó részén hello kép nincs az Azure Resource Managerrel (ARM) sablont.  A használati példában hello Virtuálisgép-bővítmény regisztrálja hello VM hello Azure Automation DSC lekérési kiszolgálójával (Ez azt jelenti, hogy a lekérési kiszolgálójával) rendelkező csomópont.  hello konfigurációs hello lekérési kiszolgálójával tárolja.  Ténylegesen, tárolt kétszer: egyszer egyszerű szövegként, és miután összeállítani MOF-fájlként (az alábbiakhoz ilyen dolgot tudnia.)  Hello portálon hello MOF, a "csomópont-konfiguráció" (mint megakadályozását toosimply "konfiguráció").  Az hello összetevő, amely egy csomópont van társítva, ezért hello csomópont tudni fogják konfigurációjában.  Alább a részletekre bemutatják, hogyan tooassign hello konfigurációs toohello csomópontok.

Valószínűleg már végzett hello bit hello tetején, vagy azt a legtöbb.  Hello nuspec létrehozása, fordítása, és tárolja őket a NuGet-kiszolgáló egy kis is.  És már Ön által felügyelt virtuális gépek.  Hello tovább lépés toocontinuous telepítési véve szükséges hello lekéréses (egyszer) kiszolgálót állít be, a regisztráló a csomópontok (egyszer), és a létrehozása és a hello konfigurációs hiba (kezdetben) tárolja.  Csomagok frissítése, és központilag telepített toohello-tárházban, majd frissítse hello konfigurációs és a csomópont-konfiguráció hello lekérési kiszolgálójával (ismétlődő igény szerint).

Ha még nem kezdve egy ARM-sablon, ez is OK gombra.  A virtuális gépek hello lekérési kiszolgálójával, és az összes többi hello regisztrált PowerShell parancsmagkészleteket toohelp van. További részletekért lásd: Ez a cikk: [bevezetési gépeket Azure Automation DSC általi kezelésre](automation-dsc-onboarding.md)

## <a name="step-1-setting-up-hello-pull-server-and-automation-account"></a>1. lépés: Hello lekéréses kiszolgáló és az automation-fiók beállítása
Egy hitelesített (Add-AzureRmAccount) PowerShell parancssorból: (percet is igénybe vehet néhány közben hello lekéréses kiszolgáló be van állítva)

    New-AzureRmResourceGroup –Name MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES
    New-AzureRmAutomationAccount –ResourceGroupName MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES –Name MY-AUTOMATION-ACCOUNT 

Az automation-fiók minden régióban (más néven helye) a következő hello helyezhet el: USA keleti régiója 2, déli középső Régiójában, Velünk – (kormányzati) Virginia, Nyugat-Európában, Délkelet-Ázsiában, kelet-japán, közép-Indiában és Ausztrália délkeleti, Kanada központi Észak-Európa.

## <a name="step-2-vm-extension-tweaks-toohello-arm-template"></a>2. lépés: Virtuálisgép-bővítmény beállításokból állnak toohello ARM-sablon
VM-regisztrációk (hello PowerShell DSC Virtuálisgép-bővítmény) jelen részletei [Azure gyors üzembe helyezés sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/dsc-extension-azure-automation-pullserver).  Ez a lépés az új virtuális gép hello lekérési kiszolgálójával hello listában a DSC-csomópontok regisztrálja.  Ez a regisztráció részét hello konfigurációs alkalmazott toobe toohello csomópontok határozza meg.  Ez a csomópont-konfiguráció nem rendelkezik tooexist még hello lekérési kiszolgálójával, ezért ezt a OK, amely a 4. lépés, ha ez történik a hello első alkalommal.  De itt 2. lépésben kell toohave vállalat úgy döntött, hello hello csomópont neve és hello konfigurációs hello nevét.  A használati példában hello csomópont "isvbox" pedig hello konfigurációs "ISVBoxConfig".  Ezért a hello csomópont-konfiguráció neve (toobe DeploymentTemplate.json megadott) "ISVBoxConfig.isvbox".  

## <a name="step-3-adding-required-dsc-resources-toohello-pull-server"></a>3. lépés: A szükséges DSC erőforrások toohello lekéréses kiszolgáló hozzáadása
PowerShell-galériában hello felműszerezett tooinstall DSC erőforrások az Azure Automation-fiók be.  Keresse meg, majd kattintson a "Központi telepítés tooAzure Automation" gomb hello toohello erőforrás.

![PowerShell-galériában – példa](./media/automation-dsc-cd-chocolatey/xNetworking.PNG)

Egy másik módszerrel nemrégiben hozzáadott toohello Azure portál lehetővé teszi új modulok vagy frissítés meglévő modulok toopull. Kattintson a hello Automation-fiók erőforrás, hello eszközök csempe, és végül hello modulok csempe.  hello Tallózás gyűjteményelem ikon lehetővé teszi toosee hello listájához hello katalógusában, részletekbe menően tárhatják fel részleteit, és végül importálja az Automation-fiók. Ez egy nagy tookeep a modulok a idő tootime toodate fel van. És hello importálás funkció ellenőrzi az egyéb modulok tooensure semmi lekérdezi szinkronban függőségek.

Vagy nincs hello manuális módszerrel.  a PowerShell integrációs modul a Windows-számítógép hello mappaszerkezetét kissé eltér a hello gyökérmappa-szerkezetében hello Azure Automation által várt.  Ehhez egy kis tökéletesítse beavatkozást.  Azonban ez nem rögzített, és erőforrásonként csak egyszer elkészült (kivéve, ha azt szeretné, hogy tooupgrade azt a jövőben.)  A PowerShell integrációs modulok készítése további információkért lásd: Ez a cikk: [integrációs modulok készítése az Azure Automation](https://azure.microsoft.com/blog/authoring-integration-modules-for-azure-automation/)

* Telepítse a hello modul, amely a következőképpen kell a munkaállomáson:
  * Telepítés [Windows Management Framework v5](http://aka.ms/wmf5latest) (a Windows 10-hez nem szükséges)
  * `Install-Module –Name MODULE-NAME`< – Markoló hello modulnak a hello PowerShell-galériában 
* Másolás hello modul mappát `c:\Program Files\WindowsPowerShell\Modules\MODULE-NAME` tooa ideiglenes mappa létrehozása 
* Kódminták és dokumentáció hello fő mappa törlése 
* Zip hello mappáját, pontosan elnevezési hello ZIP-fájl hello azonos hello mappaként 
* Hello ZIP-fájl helyezze egy elérhető-e HTTP-helyre, például az Azure-Tárfiók a blob-tároló.
* Futtassa ezt a PowerShell:
  
      New-AzureRmAutomationModule `
          -ResourceGroupName MY-AUTOMATION-RG -AutomationAccountName MY-AUTOMATION-ACCOUNT `
          -Name MODULE-NAME –ContentLink "https://STORAGE-URI/CONTAINERNAME/MODULE-NAME.zip"

hello szereplő példa cChoco és xNetworking hajtja végre ezeket a lépéseket. Lásd: hello [megjegyzések](#notes) a cChoco a különleges kezelést.

## <a name="step-4-adding-hello-node-configuration-toohello-pull-server"></a>4. lépés: Hello csomópont konfigurációs toohello lekéréses kiszolgáló hozzáadása
Nincs szükség különleges hello kapcsolatos első alkalommal a konfigurációs importál hello lekérési kiszolgálójával és fordítási.  Az összes későbbi importálási/lefordítja a hello azonos konfigurációs megjelenését pontosan hello azonos.  Minden alkalommal, amikor frissíti a csomagot, és azt tooproduction meg ehhez a lépéshez győződjön meg arról hello konfigurációs fájl toopush kell helyesek – beleértve a csomag új verziója hello is.  Hello konfigurációs fájl és a PowerShell itt található:

ISVBoxConfig.ps1:

    Configuration ISVBoxConfig 
    { 
        Import-DscResource -ModuleName cChoco 
        Import-DscResource -ModuleName xNetworking

        Node "isvbox" {   

            cChocoInstaller installChoco 
            { 
                InstallDir = "C:\choco" 
            }

            WindowsFeature installIIS 
            { 
                Ensure="Present" 
                Name="Web-Server" 
            }

            xFirewall WebFirewallRule 
            { 
                Direction = "Inbound" 
                Name = "Web-Server-TCP-In" 
                DisplayName = "Web Server (TCP-In)" 
                Description = "IIS allow incoming web site traffic." 
                DisplayGroup = "IIS Incoming Traffic" 
                State = "Enabled" 
                Access = "Allow" 
                Protocol = "TCP" 
                LocalPort = "80" 
                Ensure = "Present" 
            }

            cChocoPackageInstaller trivialWeb 
            {            
                Name = "trivialweb" 
                Version = "1.0.0" 
                Source = “MY-NUGET-V2-SERVER-ADDRESS” 
                DependsOn = "[cChocoInstaller]installChoco", 
                "[WindowsFeature]installIIS" 
            } 
        }    
    }

Új-ConfigurationScript.ps1:

    Import-AzureRmAutomationDscConfiguration ` 
        -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT ` 
        -SourcePath C:\temp\AzureAutomationDsc\ISVBoxConfig.ps1 ` 
        -Published –Force

    $jobData = Start-AzureRmAutomationDscCompilationJob ` 
        -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT ` 
        -ConfigurationName ISVBoxConfig 

    $compilationJobId = $jobData.Id

    Get-AzureRmAutomationDscCompilationJob ` 
        -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT ` 
        -Id $compilationJobId

Ezek új csomópont-konfiguráció lépéseket eredményez a lekérési kiszolgálón hello helyezés "ISVBoxConfig.isvbox" nevű.  hello csomópont-konfiguráció neve "configurationName.nodeName" mint épül.

## <a name="step-5-creating-and-maintaining-package-metadata"></a>5. lépés: Létrehozása és karbantartása csomag metaadatai
Minden csomagot, hogy vannak-e történő hello csomag tárházba van szüksége egy nuspec, amely azt ismerteti.  Adott nuspec kell összeállítani, és a NuGet-kiszolgáló által tárolt. A műveletet [Itt](http://docs.nuget.org/create/creating-and-publishing-a-package).  MyGet.org NuGet-kiszolgálóként is használhatja.  Ez a szolgáltatás el, hanem alapszintű Termékváltozat, amely nem rendelkezik.  A NuGet.org saját NuGet-kiszolgáló a saját csomagok telepítésével kapcsolatos találhat.

## <a name="step-6-tying-it-all-together"></a>6. lépés: Az összes együtt Ön
Minden alkalommal, amikor egy verzió QA továbbítja, jóvá van hagyva központi telepítéséhez hello csomagot a létrehozása, nuspec és nupkg frissítése és toohello NuGet-kiszolgáló telepítése.  Ezenkívül hello (4. lépés a fenti) értéknek kell lennie frissített tooagree hello új verziószámmal.  Kell küldött toohello lekérési kiszolgálójával, és fordítva.  Ettől kezdve akkor toohello virtuális gépeinek konfigurációs toopull hello frissítést függ, és telepítse.  E frissítések egyszerű - csak egy vonal, vagy két PowerShell.  Visual Studio Team Services hello esetben némelyikük vannak ágyazva a build együtt elérése összeállítási feladatokat.  Ez [cikk](https://www.visualstudio.com/en-us/docs/alm-devops-feature-index#continuous-delivery) további részleteket tartalmaz.  Ez [GitHub-tárház](https://github.com/Microsoft/vso-agent-tasks) részletek hello különböző elérhető összeállítási feladatokat.

## <a name="notes"></a>Megjegyzések
Ez a példa egy virtuális Gépet az Azure katalógusában hello általános Windows Server 2012 R2 lemezképéről kezdődik.  Indítsa el a tárolt képet, és majd végeznünk a DSC-konfiguráció hello onnan.  Egy képre sütik-konfiguráció módosítása azonban sokkal nehezebb mint dinamikusan a DSC használata hello konfigurációjának frissítése.

Nincs toouse egy ARM-sablon és a Virtuálisgép-bővítmény toouse hello ezzel a módszerrel a virtuális gépekkel.  És a virtuális gépek nem vannak toobe CD felügyelete alatt álló Azure toobe.  Minden olyan akkor szükség, lehet, hogy Chocolatey telepítve van, és hello LCM konfigurálva a virtuális gép hello így az tudni fogja, ahol hello lekérési kiszolgálójával van.  

Természetesen frissít egy csomagot a virtuális gép, amely a termelésben, úgy kell tootake Elforgatás kívüli virtuális közben hello frissítés telepítve van.  Ennek módja széles körben változik.  Például egy virtuális Gépet egy Azure terheléselosztó mögött, hozzáadhat egy egyéni mintavételi.  Hello virtuális gép frissítésekor hello mintavételi végpont térjen vissza a 400 rendelkezik.  hello szükséges toocause végeznünk a módosítás lehet belül a konfigurációs is hello végeznünk tooswitch vissza tooreturning egy 200 hello frissítés végrehajtása után.

Ez a példa a teljes forrás van [a Visual Studio-projekt](https://github.com/sebastus/ARM/tree/master/CDIaaSVM) a Githubon.

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Azure Automation DSC – áttekintés](automation-dsc-overview.md)
* [Azure Automation DSC-parancsmagokkal](https://msdn.microsoft.com/library/mt244122.aspx)
* [Azure Automation DSC általi kezelésre bevezetési gépek](automation-dsc-onboarding.md)

