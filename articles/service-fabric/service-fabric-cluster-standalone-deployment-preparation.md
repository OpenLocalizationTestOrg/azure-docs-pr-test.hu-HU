---
title: "Service Fabric önálló fürt telepítés előkészítése aaaAzure |} Microsoft Docs"
description: "Dokumentáció toopreparing hello környezet és a kapcsolódó hello fürt konfigurációjának létrehozása toobe figyelembe veendő előzetes toodeploying egy fürt egy üzemi terhelés kezelésére szolgál."
services: service-fabric
documentationcenter: .net
author: maburlik
manager: timlt
editor: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/17/2017
ms.author: maburlik;chackdan
ms.openlocfilehash: e503c61a64b408af3f22bd75ab02f1c34ac9f380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
<a id="preparemachines"></a>

## <a name="plan-and-prepare-your-service-fabric-standalone-cluster-deployment"></a>Tervezze meg és készítse elő a Service Fabric önálló fürttelepítés
Hajtsa végre a következő lépéseket a fürt létrehozása előtt hello.

### <a name="step-1-plan-your-cluster-infrastructure"></a>1. lépés: A fürt-infrastruktúra megtervezése
Arra készül toocreate a Service Fabric-fürt saját gépeken, eldöntheti, milyen típusú hibákat kívánt fürt toosurvive hello. Például külön kiemelt sorok kell, vagy internetes kapcsolatok megadott toothese gépek? Ezenkívül fontolja meg a fizikai biztonsági hello ezeknek a gépeknek. Hello gépek helyét, és hozzáférést toothem kell? Ezek a döntések után logikailag leképezheti hello gépek toohello különböző tartalék tartományok (lásd a 4. lépés). hello infrastruktúra megtervezésének éles fürtök bonyolultabb, mint a tesztfürtökön.

### <a name="step-2-prepare-hello-machines-toomeet-hello-prerequisites"></a>2. lépés: Felkészülés hello gépek toomeet hello Előfeltételek
Előfeltételek az egyes gépek megjeleníteni kívánt tooadd toohello fürt:

* Egy legalább 16 GB RAM használata ajánlott.
* Ajánlott legalább 40 GB szabad lemezterület.
* A 4 mag, vagy nagyobb Processzor ajánlott.
* Tooa biztonságos hálózati kapcsolat és hálózati az összes számítógépen.
* Windows Server 2012 R2 vagy Windows Server 2016. 
* [.NET-keretrendszer 4.5.1-es vagy újabb](https://www.microsoft.com/download/details.aspx?id=40773), teljes verzióként.
* [A Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
* Hello [RemoteRegistry szolgáltatás](https://technet.microsoft.com/library/cc754820) minden hello gépen kell futnia.

hello Fürtfelügyelő központi telepítését és konfigurálását hello fürt rendelkeznie kell [rendszergazdai jogosultságokkal](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) egyes hello gépek. A Service Fabric tartományvezérlőn nem telepíthető.

### <a name="step-3-determine-hello-initial-cluster-size"></a>3. lépés: Hello a fürtcsomópontok kezdeti mérete határozza meg.
Különálló Service Fabric-fürt minden csomópontja rendelkezik hello Service Fabric futásidejű hatályba, és hello fürt tagja. Tipikus éles üzembe helyezése esetén nincs operációsrendszer-példányonként egy csomópont (fizikai vagy virtuális). hello foglalásiegység-méret az üzleti igényeknek megfelelően határozza meg. Azonban rendelkeznie kell egy minimális fürtméret három csomópontok (gépek vagy virtuális gépeken).
Fejlesztési célokra szolgál hogy az adott számítógépen egynél több csomópont. Éles környezetben a Service Fabric támogatja az egyes fizikai vagy virtuális gép csak egy csomópont.

### <a name="step-4-determine-hello-number-of-fault-domains-and-upgrade-domains"></a>4. lépés: Hello tartalék tartományainak számát határozza meg, és a tartományok frissítése
A *tartalék tartomány* (FD) olyan fizikai egység a hiba, és közvetlenül kapcsolódó toohello fizikai infrastruktúra hello adatközpontokban. A tartalék tartomány hardver összetevőből áll (számítógépek, kapcsolók, hálózatok és több), amelyek a hibaérzékeny pontok kialakulását. Bár a tartalék tartományok és állványok között nincs 1:1 leképezés, lazán beszéd, minden állványban tekinthető tartalék tartomány. Annak eldöntéséhez, hogy a fürtben található csomópontok hello, javasoljuk, hogy hello csomópontokat kiosztani legalább három tartalék tartományok között.

FDs művelet ad meg, beállíthatja minden FD hello nevét. A Service Fabric hierarchikus FDs támogatja, így a tükrözhetők az infrastruktúra-topológiát a bennük foglalt.  Például a következő FDs hello érvényesek:

* "faultDomain": "fd: / Room1/Rack1/gép1"
* "faultDomain": "fd: / FD1"
* "faultDomain": "fd: / Room1/Rack1/PDU1/M1"

Egy *frissítési tartomány* (UD) a csomópont egy logikai egységet. Frissítéskor a Service Fabric vezénylését (egy alkalmazás frissítése vagy a fürt frissítése) egy UD összes csomópontján vannak leállítaná tooperform hello frissítés közben egyéb UDs csomópontjának, továbbra is elérhető tooserve kérelmek. hello hajt végre a gépek belső vezérlőprogram frissítése nem fogadják el UDs, ezért tegye őket egy számítógép egyszerre.

hello legegyszerűbb módja toothink kapcsolatos ezekről a fogalmakról tooconsider FDs egységként hello nem tervezett hiba és UDs tervezett karbantartás hello egységként.

UDs művelet ad meg, beállíthatja minden UD hello nevét. Például a következő neveket hello érvényesek:

* "upgradeDomain": "UD0"
* "upgradeDomain": "UD1A"
* "upgradeDomain": "DomainRed"
* "upgradeDomain": "Blue"

Részletesebb információ a frissítési tartományok és a tartalék tartományok, lásd: [a Service Fabric-fürt leíró](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="step-5-download-hello-service-fabric-standalone-package-for-windows-server"></a>5. lépés: A Windows Server hello Service Fabric önálló csomag letöltése
[Töltse le a Windows Server - szolgáltatás háló önálló csomag - hivatkozás](http://go.microsoft.com/fwlink/?LinkId=730690) és csomagolja ki a csomag hello, vagy tooa telepítési számítógép, amely nem hello fürt része, vagy tooone hello gépek, amely a fürt része lesz.

### <a name="step-6-modify-cluster-configuration"></a>6. lépés: A fürt konfigurációjának módosítása
toocreate egy önálló fürtön toocreate önálló fürt konfigurációs művelet fájllal rendelkezik, amely hello fürt hello megadását ismerteti. Hello hivatkozáson található hello sablonok hello konfigurációs fájlt létrehozhatja. <br>
[Önálló fürtkonfigurációk](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

Hello részeiben ezt a fájlt a részletekért lásd: [önálló Windows-fürt konfigurációs beállításainak](service-fabric-cluster-manifest.md).

Nyissa meg az egyik hello művelet fájlokat a letöltött hello csomagból, és hello a következő beállításokat módosíthatja:
| **Konfigurációs beállítás** | **Leírás** |
| --- | --- |
| **A NodeType tulajdonságok értéke** |Csomóponttípusok tooseparate lehetővé teszik a fürtcsomópontok különböző csoportokba. A fürt legalább egy NodeType kell rendelkeznie. A csoport összes csomópontjának hello következő közös jellemzőkkel rendelkezik: <br> **Név** -Ez az hello csomóponttípus. <br>**Végpontportokat** – ezek különböző megnevezett végpontok (portok) Ez a csomóponttípus társított. Használhat bármilyen által kívánja, mindaddig, amíg azok nem ütköznek a bármi más, a jegyzékfájlban, és még nem hello számítógép vagy virtuális gépen más alkalmazás által használt port számát. <br> **Elhelyezési tulajdonságok** -ezek tulajdonságai csomópont placement Constraints korlátozásokat, a használt hello rendszerszolgáltatások vagy a szolgáltatások leírása. Ezek a Tulajdonságok felhasználó által definiált kulcs/érték párok, adja meg egy adott csomópont extra metaadatai. Csomópont tulajdonságai példái lenne, rendelkezik-e hello csomópont a a merevlemez-meghajtóról vagy grafikus kártya, a merevlemez-meghajtóról, maggal és más fizikai tulajdonságok forgórészek hello száma. <br> **Kapacitások** -csomópont-kapacitás hello nevét és egy adott erőforráshoz mennyisége megadása, hogy egy adott csomópont felhasználható rendelkezik-e. Meghatározhatja például, egy csomópont lehet, hogy rendelkezik-e olyan metrikajelentés "MemoryInMb" nevű kapacitás, és arról, hogy rendelkezik-e elérhető 2048 MB-ot alapértelmezés szerint. A telepítést, hogy adott mennyiségű erőforrást igénylő szolgáltatások kerülnek hello csomópontján, hogy ezeket az erőforrásokat hello elérhető szükséges mennyiségű futásidejű tooensure vannak használni.<br>**IsPrimary** – Ha több mint egy meghatározott NodeType győződjön meg arról, hogy csak az egyik értéke hello értékű tooprimary *igaz*, amely, ahol a hello rendszerszolgáltatások futtassa. Minden más csomóponttípusok toohello értéket kell megadni *hamis* |
| **Csomópontok** |Ezek az egyes hello csomópontok (típusú csomópont, a csomópont neve, IP-cím, tartalék tartományt, és frissítési hello csomópont) hello fürt részét képező hello részletek érhetők el. hello gépek kívánt hello fürt toobe létrehozni a szükséges toobe ebben a listában szerepel az IP-címét. <br> Ha azonos IP-címet az összes hello csomópontjára, majd egy beépített fürt jön létre, hello, tesztelési célokra használható. Ne használjon egy beépített fürtök üzembe helyezése a termelési számítási feladatokhoz. |

Miután hello fürtkonfiguráció minden konfigurált beállítások toohello környezetben volt, az tesztelhető elleni hello fürtkörnyezet (7. lépés).

<a id="environmentsetup"></a>

### <a name="step-7-environment-setup"></a>7. lépés A környezet beállítása

A fürt rendszergazdája állítja be, különálló Service Fabric-fürt, hello környezet esetén meg kell toobe állítsa be a következő feltételek hello: <br>
1. hello fürtöt hoz létre hello felhasználó biztonsági rendszergazdai jogosultságokkal tooall gépek csomópontok hello fürt konfigurációs fájlban felsorolt kell rendelkeznie.
2. Mely hello a fürt létrehozását számítógép, valamint minden fürt csomópont gép kell:
* El kell távolítani a Service Fabric SDK
* Rendelkezik a Service Fabric-futtatókörnyezet eltávolítása 
* Hello Windows tűzfalszolgáltatást (mpssvc) engedélyezett
* Engedélyezte a távoli beállításjegyzék szolgáltatás (remoteregistry) hello
* Fájl megosztási (SMB) engedélyezve van
* Rendelkezik a szükséges portok nyitva, a fürt konfigurációs portok alapján
* Rendelkezik a Windows az SMB és a távoli beállításjegyzék szolgáltatás számára szükséges portok: 135-ös, 137, 138, 139 és 445-ös
* Hálózati kapcsolat tooone van egy másik
3. Egy tartományvezérlő legyen hello fürt csomópont gépek egyike.
4. Ha hello fürt toobe telepített egy biztonságos fürt, ellenőrzi az előfeltételeket helyezze el, és megfelelően van konfigurálva elleni hello konfigurációs hello szükséges biztonsági.
5. Hello fürtbeli gépeken nincsenek internetről elérhető, ha a készlet hello következő hello fürt konfigurációs:
* Telemetria letiltása: az *tulajdonságok* beállítása *"enableTelemetry": hamis*
* Automatikus háló verzió letöltése & hello aktuális fürt verzió közelít végéhez értesítések letiltása: az *tulajdonságok* beállítása *"fabricClusterAutoupgradeEnabled": hamis*
* Másik lehetőségként toowhite felsorolt tartományokhoz hálózati internet-hozzáférés esetén hello tartományok az alábbi az automatikus frissítéséhez: go.microsoft.com jövőben a Microsoft

6. Állítsa be a megfelelő Service Fabric víruskereső kizárásokat:

| **Víruskereső kizárt könyvtárak** |
| --- |
| Program Files\Microsoft a Service Fabric |
| A FabricDataRoot (a fürtkonfiguráció) |
| (A fürtkonfiguráció) fabriclogroot mappában |

| **Víruskereső kizárt folyamatok** |
| --- |
| Fabric.exe |
| FabricHost.exe |
| FabricInstallerService.exe |
| FabricSetup.exe |
| FabricDeployer.exe |
| ImageBuilder.exe |
| FabricGateway.exe |
| FabricDCA.exe |
| FabricFAS.exe |
| FabricUOS.exe |
| FabricRM.exe |
| FileStoreService.exe |

### <a name="step-8-validate-environment-using-testconfiguration-script"></a>8. lépés. Ellenőrizze a környezet TestConfiguration parancsfájl használatával
hello TestConfiguration.ps1 parancsfájl hello önálló csomagban található. Ez egy ajánlott eljárásokat elemző eszköz toovalidate használt néhány hello feltétel fenti és egy megerősítést ellenőrzés toovalidate, használ kell, hogy a fürt telepíthető egy adott környezetben. Minden hiba esetén tekintse meg a listában toohello [környezetben való telepítés](service-fabric-cluster-standalone-deployment-preparation.md) hibaelhárításhoz. 

Ezt a parancsfájlt, amely rendelkezik rendszergazdai hozzáférési tooall hello gépek csomópontok hello fürt konfigurációs fájlban felsorolt gépi. Ezt a parancsfájlt futtató hello számítógép nem rendelkezik toobe hello fürt része.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
Trace folder already exists. Traces will be written tooexisting trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 : True
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
Passed                     : True
```

Jelenleg ez a konfiguráció tesztelési modul nem felel meg hello biztonsági konfiguráció így ez toobe egymástól függetlenül történik.  

> [!NOTE]
> Folyamatosan hajtunk fejlesztései toomake Ez a modul robusztusabb, így ha egy hibás vagy hiányzik egy eset, ami azt feltételezi nem jelenleg által észlelt TestConfiguration, tudassa velünk keresztül a [támogatnak](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).   
> 
> 

## <a name="next-steps"></a>Következő lépések
* [A Windows Server rendszert futtató önálló fürt létrehozása](service-fabric-cluster-creation-for-windows-server.md)
