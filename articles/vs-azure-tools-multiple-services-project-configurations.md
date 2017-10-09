---
title: "aaaConfiguring az Azure-projekt használatával több szolgáltatáskonfiguráció |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure az Azure felhőalapú hello ServiceDefinition.csdef és ServiceConfiguration.cscfg fájlok módosításával projektet."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a4fb79ed-384f-4183-9f74-5cac257206b9
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 14222266093eb876db0ac9ce8d3d17a04c65d1a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>A több szolgáltatáskonfiguráció használata Azure-projekt konfigurálása
Egy Azure-felhőszolgáltatás-projekt tartalmazza a két konfigurációs fájlok: ServiceDefinition.csdef és ServiceConfiguration.cscfg. Ezeket a fájlokat az Azure cloud service alkalmazással vannak csomagolva, és tooAzure telepíthetők.

* Hello **ServiceDefinition.csdef** fájl hello hello követelményeinek a felhő szolgáltatásalkalmazás, milyen szerepkörök tartalmaz, beleértve az Azure-környezetéhez szükséges hello metaadatokat tartalmaz. Ez a fájl is tooall példányok vonatkozó konfigurációs beállításokat tartalmaz. Ezeket a konfigurációs beállításokat futásidőben hello Azure szolgáltatást üzemeltető futásidejű API használatával olvasható. Ez a fájl nem frissíthető, miközben fut a szolgáltatás az Azure-ban.
* Hello **ServiceConfiguration.cscfg** fájl értékek beállítása hello szolgáltatásdefiníciós fájlban meghatározott hello beállításokat, és az egyes szerepkörökhöz példányok toorun hello számát adja meg. Ez a fájl frissíthető az Azure-ban a felhőalapú szolgáltatás futása közben.

hello Azure eszközök a Microsoft Visual Studio használható tooset konfigurációs beállításokat a fájlokban tárolt tulajdonságlapokat adja meg. tooaccess hello tulajdonságlapjain, kattintson duplán a hello szerepkör hivatkozás alatti hello Azure cloud service projektben a Megoldáskezelőre, vagy hello szerepkör hivatkozás gombbal és válassza **tulajdonságok**, ahogy az ábra a következő hello.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Az alapul szolgáló definiált hello szolgáltatás és konfigurációs fájlok sémái hello kapcsolatos információkért lásd: hello [Sémareferenciája](https://msdn.microsoft.com/library/azure/dd179398.aspx). Szolgáltatás konfigurációjával kapcsolatos további információkért lásd: [tooConfigure Cloud Services](cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Szerepkör tulajdonságainak konfigurálása
a webes és feldolgozói szerepkörök hello tulajdonságlapokat hasonlítanak, habár van néhány különbség, a következő részekben hello ki emelni.

A hello **gyorsítótárazását** lapon konfigurálhatja hello Azure gyorsítótárazás a szolgáltatásokat.

### <a name="configuration-page"></a>Konfiguráció lap
A hello **konfigurációs** lapon állíthatja be ezeket a tulajdonságokat:

**Példányok**

Set hello **példány** számolja tulajdonság toohello példányok hello szolgáltatást futtatni ehhez a szerepkörhöz.

Set hello **Virtuálisgép-méretet** tulajdonság túl**Extra Small**, **kis**, **Közepes**, **nagy**, vagy **Extra nagy**.  További információkért lásd: [Felhőszolgáltatások mérete](cloud-services/cloud-services-sizes-specs.md).

**Indítási műveletet** (csak webes szerepkör)

Állítsa be a tulajdonság toospecify, amely a Visual Studio kell elindítani a webböngészőt végpontok hello HTTP vagy HTTPS-végpontnak hello, vagy mindkét, ha a hibakeresés elindításához.

hello HTTPS-végpont lehetőség áll rendelkezésre, csak akkor, ha a HTTPS-végpont már meghatározta a szerepkör. Meghatározhatja a HTTPS-végpontnak a hello **végpontok** tulajdonságlapján.

Már felvett HTTPS-végpontnak, hello HTTPS-végpont beállítás alapértelmezés szerint engedélyezve van, és a Visual Studio fog elindít egy böngészőt, ezen a végponton, hibakeresés, továbbá a HTTP-végpont tooa böngésző indításakor. A parancs feltételezi, hogy engedélyezve vannak-e a mindkét indítási lehetőséget.

**Diagnosztika**

Hello webes szerepkör alapértelmezés szerint engedélyezve van a diagnosztika. Azure cloud service-projekt hello és a tárfiók toouse hello helyi storage emulator van beállítva. Amikor készen áll a toodeploy tooAzure, kiválaszthatja a hello jelentéskészítő gomb (**...** ) tooupdate hello tárolási fiók toouse hello felhőben az Azure storage. Hello diagnosztikai adatok toohello tárfiók vihetők át, az igény szerinti vagy automatikusan rendszeres időközönként. Az Azure diagnostics kapcsolatos további információkért lásd: [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Beállítások lap
A hello **beállítások** lap, a szolgáltatás konfigurációs beállítások is hozzáadhat. Konfigurációs beállítások olyan név-érték párok. Hello szerepkörben futó tudja olvasni a konfigurációs beállításokat futásidőben hello által biztosított osztályokat hello értékek [Azure felügyelt kódtár](http://go.microsoft.com/fwlink?LinkID=171026). Pontosabban hello [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) metódus egy elnevezett konfigurációs beállítás futásidőben hello értékét adja vissza.

### <a name="configuring-a-connection-string-tooa-storage-account"></a>A kapcsolati karakterlánc tooa storage-fiókok konfigurálása
A kapcsolati karakterlánc egy konfigurációs beállítás hello storage Emulator vagy az Azure-tárfiók kapcsolati és hitelesítési adatokat biztosító. Amikor a kódot kell érheti el az Azure storage szolgáltatások adatait – Ez azt jelenti, hogy a blob, a várólista vagy a tábla adatai – egy szerepkörben futó, hogy toodefine, hogy a tárfiók kapcsolati karakterláncot.

Egy kapcsolati karakterláncot, amely az Azure storage-fiók tooan mutat a definiált formátumot kell követnie. További információ toocreate kapcsolati karakterláncok, lásd: [konfigurálása Azure Storage kapcsolati karakterláncok](storage/common/storage-configure-connection-string.md).

Amikor a rendszer készen áll a tootest elleni hello Azure storage szolgáltatások a szolgáltatás, vagy ha kész toodeploy a felhőalapú szolgáltatás tooAzure, módosíthatja bármilyen kapcsolati karakterláncok toopoint tooyour Azure storage-fiók hello értékének. Válassza ki (**...** ) elemre, jelölje be **adja meg a tárfiók hitelesítő adatainak**. Adja meg a fiók adatait, amely tartalmazza a fiók nevét és a fiókkulcsot. A hello **tárolási fiók kapcsolati karakterlánc** párbeszédpanelen is jelezheti, hogy kívánja-e toouse hello alapértelmezett HTTPS-végpontnak (hello alapértelmezett beállítás), HTTP-végpontokról hello alapértelmezett vagy egyéni végpontokat. Dönthet úgy, hogy toouse egyéni végpontokat Ha egy egyéni tartománynevet a szolgáltatásra regisztrált leírtak [blob adatok egyéni tartománynév beállítása az Azure-tárfiók](storage/blobs/storage-custom-domain-name.md).

> [!IMPORTANT]
> A szolgáltatás telepítése előtt módosítania kell a kapcsolati karakterláncok toopoint tooan Azure storage-fiók. Toodo hiányában ez a szerepkör nem toostart azt okozhatja, vagy toocycle keresztül hello inicializálása, foglalt és leállítása állapotok.
> 
> 

## <a name="endpoints-page"></a>Végpontok lap
A feldolgozói szerepkör több HTTP, HTTPS vagy TCP-végpontok is rendelkezhetnek. Végpontok lehetnek a bemeneti végpontok, amelyek elérhető tooexternal ügyfelek, vagy a belső végpontok, amelyek elérhető tooother hello szolgáltatásban futó szerepkörök.

* toomake HTTP-végpont elérhető tooexternal ügyfelek és a böngészők, módosítsa a hello végpont típusa tooinput, és adjon meg egy nevet és egy nyilvános port számát.
* egy HTTPS toomake végpont elérhető tooexternal ügyfelek és a böngészők, módosítsa hello végponttípus túl**bemeneti**, és adjon meg egy nevet, egy nyilvános port számát és a felügyeleti tanúsítvány nevét.
  
    Vegye figyelembe, hogy előtt meg lehet adni egy felügyeleti tanúsítványt meg kell adni hello tanúsítványt a hello **tanúsítványok** tulajdonságlapján.
* más szerepkörök hello felhőalapú szolgáltatás, a belső hozzáféréshez elérhető végpontok toomake hello végpont típusa toointernal módosítsa, majd adjon meg egy nevet és lehetséges titkos portok ezen a végponton.

## <a name="local-storage-page"></a>Helyi tároló lap
Hello használható **helyi tároló** tulajdonság lap tooreserve egy vagy több helyi tároló-erőforrások szerepkör. A helyi tároló egyik erőforrásához egy fenntartott könyvtárat a hello fájlrendszerében hello Azure virtuális gép szerepkör példánya fut. a.

## <a name="certificates-page"></a>Tanúsítványok lap
A hello **tanúsítványok** lapon tanúsítványok a szerepkörhöz is hozzárendelhető. a HTTPS-végpontnak a hello lehet használt tooconfigure hozzáadott hello tanúsítványok **végpontok** tulajdonságlapján.

Hello **tanúsítványok** tulajdonságlapján hozzáadja a tanúsítványok tooyour szolgáltatás konfigurációjának adatait. Vegye figyelembe, hogy a tanúsítványok nem vannak csomagolva az szolgáltatással; a tanúsítványok külön-külön kell feltöltenie keresztül hello tooAzure [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).

a szerepkör tanúsítvány tooassociate nevezze el a hello tanúsítványt. A név toorefer toohello tanúsítványt használ, HTTPS-végpontnak hello konfigurálásakor **végpontok** tulajdonságlapján. Ezt követően adja meg, hogy hello tanúsítványtároló **helyi számítógép** vagy **aktuális felhasználó** és hello tároló hello neve. Végül adja meg a hello tanúsítvány ujjlenyomata. Ha hello tanúsítványt az aktuális User\Personal hello (a) tároló, a hello tanúsítvány ujjlenyomata hello tanúsítványt választja adatokat tartalmazó listát is megadhatja. Bármely más helyen található, adja meg kézzel hello ujjlenyomat értékét.

Tanúsítvány hello tanúsítványtárolóból hozzáadásakor összes köztes tanúsítvány automatikusan toohello konfigurációs beállításokat. A köztes tanúsítványok is fel kell tölteni a rendelés toocorrectly tooAzure konfigurálása a szolgáltatás az SSL-hez.

Bármely felügyeleti tanúsítványok társítani a szolgáltatás tooyour szolgáltatás alkalmazása, csak akkor, amikor fut hello felhőben. Ha a szolgáltatás hello helyi fejlesztési környezetben fut, az hello compute emulator által kezelt szabványos tanúsítványt használ.

## <a name="configuring-hello-azure-cloud-service-project"></a>Hello Azure cloud service-projekt konfigurálása
tooconfigure beállítások tooan teljes Azure-felhőszolgáltatás-projekt, először megnyitja hello helyi menü az adott projekt csomópont, és a majd tulajdonságok tooopen a tulajdonságait tartalmazó oldalakon. a következő táblázat hello tulajdonság weblapokat meglátogató jeleníti meg.

| Tulajdonságlap. | Leírás |
| --- | --- |
| Alkalmazás |Ezen a lapon a felhőszolgáltatás-projekt használó Azure eszközök hello verziójával kapcsolatos információk megjelenítése, és frissítheti a hello eszközök toohello jelenlegi verziójával. |
| Események létrehozása |Ezen a lapon megadhatja előtti és utáni események. |
| Fejlesztés |Ezen a lapon adhat meg build-konfigurációs utasításokat és hello feltételek, amely alatt a utáni eseményeket futnak. |
| Web |Ezen a lapon konfigurálhatja a webkiszolgáló toohello a szoftverközponthoz kapcsolódó beállításokat. |

