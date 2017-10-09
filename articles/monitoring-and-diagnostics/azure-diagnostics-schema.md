---
title: "Diagnosztika aaaAzure kiterjesztés konfigurációs séma verziója és az előzmények |} Microsoft Docs"
description: "Az Azure virtuális gépek, a Virtuálisgép-méretezési készlet, a Service Fabric és a Felhőszolgáltatások vonatkozó toocollecting teljesítményszámlálói."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/16/2017
ms.author: robb
ms.openlocfilehash: 854ad118f660810aa38703670284794d658142c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-extention-configuration-schema-versions-and-history"></a>Az Azure Diagnostics kiterjesztés konfigurációs séma verziók és előzményei
A lap indexek Azure Diagnostics bővítmény séma verziók rendszerrel szállított, Microsoft Azure SDK hello részeként.  

> [!NOTE]
> a következő hello Azure Diagnostics bővítmény hello összetevő használt toocollect teljesítményszámlálók és más statisztikáit:
> - Azure-alapú virtuális gépek 
> - Virtual Machine Scale Sets
> - Service Fabric 
> - Cloud Services 
> - Network Security Groups (Hálózati biztonsági csoportok)
> 
> Ezen a lapon csak fontos, ha ilyen szolgáltatást használ.

más Microsoft-diagnosztika termékek, például Azure figyelő, az Application Insights és Naplóelemzési hello Azure Diagnostics bővítmény használatos. További információ: [figyelés eszközök áttekintése a Microsoft](monitoring-overview.md).

## <a name="azure-sdk-and-diagnostics-versions-shipping-chart"></a>A diagram szállítási Azure SDK-t és a diagnosztika verziók  

|Az Azure SDK-verzió | Diagnosztika kiterjesztés verziója | Modell|  
|------------------|-------------------------------|------|  
|1.x               |1.0                            |beépülő modul|  
|2.0 - 2.4         |1.0                            |beépülő modul|  
|2.5               |1.2                            |Bővítmény|  
|2.6               |1.3                            |"|  
|2.7               |1.4                            |"|  
|2.8               |1.5                            |"|  
|2.9               |1.6                            |"|
|2.96              |1.7                            |"|
|2.96              |1.8                            |"|
|2.96              |1.8.1                          |"|
|2.96              |1.9                            |"|



 A beépülő modul modelljének – ami azt jelenti, hogy hello Azure SDK telepítésekor portáltól rendszerrel szállított vele az Azure diagnostics hello verziója először szállítani Azure Diagnostics 1.0-s verziója.  

 Tooan bővítmény modell Azure diagnostics (diagnosztika 1.2-es verzió), SDK 2.5 kezdve merült fel. hello eszközök tooutilize új funkciók csak Azure SDK-k újabb érhető el, de bármely szolgáltatás használata az Azure diagnostics volna átvételéhez hello szállítási legfrissebb közvetlenül az Azure-ból. Például SDK 2.5 továbbra is felhasználójánál volna tölthet hello hello előző táblázatban látható, attól függetlenül történik hello újabb funkciók használata legújabb verziójára.  

## <a name="schemas-index"></a>Sémák index  
Az Azure diagnostics különböző verziói különböző konfigurációs sémák használja. 

[Diagnosztika 1.0 konfigurációs séma](azure-diagnostics-schema-1dot0.md)  

[Diagnosztika 1.2-es konfigurációs séma](azure-diagnostics-schema-1dot2.md)  

[1.3 diagnosztikai és a későbbi konfigurációs séma](azure-diagnostics-schema-1dot3-and-later.md)  

## <a name="version-history"></a>Verzióelőzmények


### <a name="diagnostics-extension-19"></a>Diagnosztika bővítmény 1.9 
Docker támogatja.


### <a name="diagnostics-extension-181"></a>1.8.1 diagnosztika bővítmény 
Megadhat egy SAS-token helyett a tárfiók kulcsa hello titkos config. Ha egy SAS-jogkivonat van megadva, a rendszer figyelmen kívül hagyja hello tárfiók kulcsa.


```json
{
    "storageAccountName": "diagstorageaccount",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    }
}
```

```xml
<PrivateConfig>
    <StorageAccount name="diagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    <SecondaryStorageAccounts>
        <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
</PrivateConfig>
```


### <a name="diagnostics-extension-18"></a>Diagnosztika bővítmény 1.8 
A hozzáadott tárolótípus tooPublicConfig. StorageType lehet *tábla*, *Blob*, *TableAndBlob*. *Tábla* hello alapértelmezett.


```json
{
    "WadCfg": {
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```xml
<PublicConfig>
    <WadCfg />
    <StorageAccount>diagstorageaccount</StorageAccount>
    <StorageType>TableAndBlob</StorageType>
</PublicConfig>
```


### <a name="diagnostics-extension-17"></a>Diagnosztika bővítmény 1.7 
A hozzáadott hello képességét tooroute tooEventHub.

### <a name="diagnostics-extension-15"></a>1.5-ös diagnosztika bővítmény
Hello fogadók esetében elem és hello képességét toosend diagnosztikai adatainak túl hozzáadott[Application Insights](../application-insights/app-insights-cloudservices.md) így egyszerűbb toodiagnose problémák az alkalmazás, valamint a hello rendszer és az infrastruktúra szint között.

### <a name="azure-sdk-26-and-diagnostics-extension-13"></a>Az Azure SDK 2.6 és diagnosztika bővítmény 1.3 
A Visual Studio Felhőszolgáltatás-projektek hello a következő változások történtek. (Ezek is a módosítások Azure SDK toolater verziói.)

* hello helyi emulátor mostantól támogatja a diagnosztika. Ez azt jelenti, hogy diagnosztikai adatokat gyűjteni, és győződjön meg arról, az alkalmazás hoz létre a hello jobb nyomkövetési adatokat, amíg Ön fejlesztése és tesztelése a Visual Studio. kapcsolati karakterlánc hello `UseDevelopmentStorage=true` diagnosztikai adatok gyűjtésének lehetővé teszi a futtatása közben a felhőszolgáltatási projektet a Visual Studio hello az Azure storage emulator használatával. Minden diagnosztikai adatgyűjtés hello (fejlesztési tároló) tárfiókban.
* hello diagnosztika tárolási fiók kapcsolati karakterlánc (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) ismét hello szolgáltatás konfigurációs (.cscfg) fájljában tárolódik. Az Azure SDK 2.5 hello diagnosztikai tárfiók hello diagnostics.wadcfgx fájl van megadva.

Bizonyos különbségek vannak a figyelmet a jelentősebb hogyan hello kapcsolati karakterláncot az Azure SDK 2.4 és a korábbi munkaidő-e, és hogyan működik az Azure SDK 2.6-os és újabb verziók között.

* Azure SDK 2.4 és korábbi hello kapcsolati karakterlánc volt megadva a futtatókörnyezet által hello diagnosztika beépülő modul tooget hello tárfiókadatok átvitelére a diagnosztikai naplókat.
* Azure SDK 2.6 vagy újabb rendszerben hello diagnosztika kapcsolati karakterláncot a Visual Studio tooconfigure hello diagnosztika bővítmény hello megfelelő tárfiókadatok közzététele során használja. hello kapcsolati karakterláncot adja meg a Visual Studio fogja használni, amikor a közzétételi különböző szolgáltatáskonfiguráció eltérő tárfiókokból teszi lehetővé. Azonban hello diagnosztika beépülő modul (az Azure SDK 2.5) után már nem érhető el, mert hello .cscfg fájl önmagában hello diagnosztika bővítmény nem engedélyezhető. Hogy tooenable hello bővítmény külön eszközöket, például a Visual Studio vagy a PowerShell használatával.
* toosimplify hello folyamatán hello diagnosztika bővítmény a PowerShell-lel, hello csomag kimenetét a Visual Studio hello nyilvános konfigurációs XML-t az egyes szerepkörökhöz hello diagnosztika bővítmény is tartalmaz. A Visual Studio hello diagnosztika kapcsolati karakterlánc toopopulate hello tárfiókadatok hello nyilvános konfigurációs szerepel használja. hello nyilvános konfigurációs fájljainak hello bővítmények mappában jönnek létre, és kövesse a hello mintát PaaSDiagnostics. <RoleName>. PubConfig.xml. A PowerShell-alapú telepítések használhatja ezt a mintát toomap minden konfigurációs tooa szerepkör.
* hello hello .cscfg fájlban kapcsolati karakterláncot is használják hello Azure portál tooaccess hello diagnosztikai adatokat, a hello szerepelhet **figyelés** külön-külön hello kapcsolati karakterlánc szükséges tooconfigure hello szolgáltatás tooshow részletes: figyelési adatok hello portálon.

#### <a name="migrating-projects-tooazure-sdk-26-and-later"></a>Áttelepítése projektek tooAzure SDK 2.6-os és újabb verziók
Amikor áttelepítése az Azure SDK 2.5 tooAzure SDK 2.6 vagy újabb, ha egy diagnosztikai tárfiók hello .wadcfgx fájlban megadott majd marad van. eltérőek a tárolási konfigurációk fiókok tootake előnyeit hello rugalmasan különböző tárolót használ, a toomanually hello kapcsolati karakterlánc tooyour projekt hozzáadása. Ha az Azure SDK 2.4 vagy korábbi tooAzure SDK 2.6, amelybe migrálna egy projektet, majd hello diagnosztika kapcsolati karakterláncok megmaradnak. Vegye azonban figyelembe hello változásait hogyan kapcsolati karakterláncok számít az Azure SDK 2.6 megadott hello előző szakaszban.

#### <a name="how-visual-studio-determines-hello-diagnostics-storage-account"></a>Hogyan határozza meg a Visual Studio hello diagnosztikai tárfiók
* Ha az hello .cscfg fájlban megadott diagnosztika kapcsolati karakterláncot, a Visual Studio használja tooconfigure hello diagnosztika bővítmény közzétételekor, valamint hogy mikor hello nyilvános konfigurációs XML-fájlok generálása csomagolás során.
* Nincs diagnosztika kapcsolódási karakterlánc esetén hello .cscfg fájlban, majd a Visual Studio visszavált toousing hello hello .wadcfgx tooconfigure hello diagnosztika kiterjesztés közzététele, és hello nyilvános generálása a megadott tárfiók konfigurációs XML-fájlokat, ha a csomagban.
* hello diagnosztika kapcsolati karakterlánc hello .cscfg fájlban élvez hello tárfiók hello .wadcfgx fájlban. Ha a diagnosztika a kapcsolati karakterlánc hello .cscfg fájlban megadott majd Visual Studio használ, és figyelmen kívül hagyja .wadcfgx hello storage-fiókot.

#### <a name="what-does-hello-update-development-storage-connection-strings-checkbox-do"></a>What does hello "Fejlesztési tárolási kapcsolati karakterláncok frissítése..." jelölőnégyzet elvégezni?
hello jelölőnégyzetét **fejlesztési tárolási kapcsolati karakterláncok frissítése a diagnosztika és gyorsítótárazását a Microsoft Azure storage-fiók hitelesítő adataival tooMicrosoft Azure közzétételekor** lehetővé teszi egy kényelmes módszert arra tooupdate minden fejlesztési tárfiók kapcsolati karakterláncainak közzététele során megadott hello Azure storage-fiók.

Tegyük fel például, a bejelöli ezt a jelölőnégyzetet, és hello diagnosztika kapcsolati karakterlánc határoz meg `UseDevelopmentStorage=true`. Hello projekt tooAzure közzétételekor a Visual Studio automatikusan frissíti hello diagnosztika kapcsolati karakterlánc hello közzététel varázslóban megadott hello tárfiók. Azonban ha egy valódi tárfiók hello diagnosztika kapcsolati karakterláncként megadva, majd, hogy a fiókot használja.

### <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnosztikai funkciók eltérései Azure SDK 2.4 és a korábbi és az Azure SDK 2.5-ös és újabb verziók
Ha a frissítés során a projekthez az Azure SDK 2.4 tooAzure SDK 2.5-ös vagy újabb kell szerepelnie a következő diagnosztikai funkciót különbségek szem előtt tartva hello.

* **Konfigurációs API-k elavultak** – programozható konfigurációja diagnosztikai Azure SDK 2.4 vagy korábbi verzió érhető el, de az Azure SDK 2.5-ös vagy újabb elavult. Ha a diagnosztika jelenleg definiált kódban, szüksége lesz tooreconfigure ezeket a beállításokat a hello áttelepített projektben ahhoz, hogy diagnosztika tookeep működő teljesen. hello diagnosztika konfigurációs fájl az Azure SDK 2.4 diagnostics.wadcfg, de diagnostics.wadcfgx az Azure SDK 2.5 és újabb verzióihoz.
* **Diagnosztika szolgáltatás felhőalkalmazásokhoz csak szinten hello szerepkör nem hello példány szintjén konfigurálható.**
* **Minden alkalommal, amikor az alkalmazás telepítéséhez hello diagnosztikai konfigurációja frissül** – Ha a Server Explorer a diagnosztika konfigurációjának módosítása, és ezután telepítse újra az alkalmazást paritású problémák léphetnek.
* **Azure SDK 2.5-ös vagy újabb rendszerben összeomlási memóriaképek hello diagnosztika konfigurációs fájlban, nem a kódban vannak-e konfigurálva** – Ha összeomlási memóriaképek kódban konfigurálva van, akkor kell toomanually átviteli hello konfigurációs kódot toohello konfigurációs fájlból, mivel hello összeomlási memóriaképek hello áttelepítési tooAzure SDK 2.6 során nem kerülnek.

