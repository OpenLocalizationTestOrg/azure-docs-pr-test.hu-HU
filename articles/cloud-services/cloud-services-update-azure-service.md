---
title: "egy felhőalapú szolgáltatás aaaHow tooupdate |} Microsoft Docs"
description: "Ismerje meg, hogy miként tooupdate felhőalapú szolgáltatásokat az Azure-ban. Ismerje meg, hogyan egy frissítés egy felhőalapú szolgáltatás halad tooensure rendelkezésre állását."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c6a8b5e6-5c99-454c-9911-5c7ae8d1af63
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 7e4c8bd46e51a555b4309ea8927d120e8efcf0ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-a-cloud-service"></a>Hogyan tooupdate egy felhőalapú szolgáltatás

Frissítés egy felhőalapú szolgáltatás, beleértve a szerepkörök és a vendég operációs rendszer, akkor a három lépésben. Első lépésként hello bináris fájljait és konfigurációs fájljait hello új felhőalapú szolgáltatás, vagy fel kell tölteni, operációs rendszer verziója. A következő Azure számítási és hálózati erőforrásokat hello felhőszolgáltatás hello új felhőalapú szolgáltatás verzió hello követelmények alapján foglalja le. Végül Azure hajtja végre a működés közbeni frissítés tooincrementally frissítés hello bérlői toohello új verziója vagy a vendég operációs rendszer, a rendelkezésre állás megőrzése mellett. A cikk ismerteti ezen utolsó lépésében – működés közbeni frissítés hello hello részleteit.

## <a name="update-an-azure-service"></a>Frissítés az Azure-szolgáltatások
Azure rendezi a szerepkörpéldányok frissítési tartományok (UD) nevű logikai csoportosítással. Frissítési tartományok (UD), amelyeket logikai szerepkörpéldányt beállítani, hogy a csoport frissítése.  Az Azure töröl egy felhőalapú szolgáltatás egy UD egyszerre, így példányok más UDs toocontinue szolgál a forgalom.

frissítési tartományok száma hello alapértelmezett érték 5. Hello upgradeDomainCount attribútum belefoglalja hello szolgáltatás definíciós fájl (.csdef) egy másik hány frissítési tartományt is megadhat. További információ a hello upgradeDomainCount attribútum: [webrole típusról séma](https://msdn.microsoft.com/library/azure/gg557553.aspx) vagy [WorkerRole séma](https://msdn.microsoft.com/library/azure/gg557552.aspx).

A szolgáltatás egy vagy több szerepkör helybeni frissítést hajt végre, amikor Azure frissítések beállítása szerint toohello frissítési tartomány toowhich tartoznak szerepkörpéldányt beállítani. Azure online – összes hello-példány a megadott frissítési tartomány – leállítása, frissítése, állapotba hozza őket a biztonsági frissítések telepítését a következő tartomány hello helyezi. Az aktuális hello futtató csak hello példányait leállításával frissítési tartomány, Azure gondoskodik arról, hogy egy frissítés hello szolgáltatást futtató lehető legkisebb hatással toohello történik. További információkért lásd: [hogyan hello frissítés halad](#howanupgradeproceeds) című cikkben.

> [!NOTE]
> Miközben hello feltételek **frissítése** és **frissítése** némileg eltérő jelentése hello Azure környezetben, akkor felcserélhetők a hello folyamatokat és az ebben a dokumentumban hello szolgáltatások leírása.
>
>

A szolgáltatás definiálnia kell egy adott szerepkör frissített toobe helyben állásidő nélkül szerepet legalább két példánya. Ha hello szolgáltatást egy szerepkörnek csak egy példánya, a szolgáltatás nem lehet hello helybeni frissítése befejeződött.

Ez a témakör ismerteti a hello Azure frissítésével kapcsolatos információkat a következő:

* [A szolgáltatás módosítása engedélyezett frissítése közben](#AllowedChanges)
* [Hogyan egy frissítés előrehalad az](#howanupgradeproceeds)
* [Frissítés visszaállítása](#RollbackofanUpdate)
* [Az egy folyamatban lévő telepítés több mutating művelet kezdeményezése](#multiplemutatingoperations)
* [Kapcsolódó szerepkörök frissítési tartományok között](#distributiondfroles)

<a name="AllowedChanges"></a>

## <a name="allowed-service-changes-during-an-update"></a>A szolgáltatás módosítása engedélyezett frissítése közben
hello következő táblázatban hello engedélyezett módosítások tooa szolgáltatás frissítése közben:

| Engedélyezi a módosításokat, toohosting, szolgáltatások és szerepkörök | Frissítés helyben | Előkészített (virtuális IP-címcsere) | Törölje és hozza létre |
| --- | --- | --- | --- |
| Operációs rendszer verziója |Igen |Igen |Igen |
| .NET megbízhatósági szintjét. |Igen |Igen |Igen |
| Virtuálisgép-méret<sup>1</sup> |Igen<sup>2</sup> |Igen |Igen |
| Helyi tárolási beállításai |Csak növelése<sup>2</sup> |Igen |Igen |
| Hozzáadása vagy eltávolítása a szerepkörök szolgáltatás |Igen |Igen |Igen |
| Egy adott szerepkör példányainak száma |Igen |Igen |Igen |
| Vagy a szolgáltatás végpontjainak típus |Igen<sup>2</sup> |Nem |Igen |
| Neveit és értékeit a konfigurációs beállítások |Igen |Igen |Igen |
| Értékek (de nem nevek) a konfigurációs beállítások |Igen |Igen |Igen |
| Hozzáadhat új tanúsítványokat |Igen |Igen |Igen |
| Meglévő tanúsítványok módosítása |Igen |Igen |Igen |
| Új kód telepítése |Igen |Igen |Igen |

<sup>1</sup> méretének módosítása korlátozott hello felhőszolgáltatás elérhető méretek toohello részhalmaza.

<sup>2</sup> szükséges Azure SDK 1.5 vagy újabb verzió.

> [!WARNING]
> Hello virtuális gép méretének módosítása törlődnek a helyi adatokat.
>
>

a következő elemek hello frissítés közben nem támogatott:

* Egy szerepkör hello nevét módosítani. Távolítsa el, és adja hozzá a következő új néven hello hello szerepkör.
* Hello frissítési tartományok számának módosításához.
* A helyi erőforrások hello hello méretének csökkentése.

Ha más frissítések tooyour szolgáltatás definíciós, például a helyi erőforrás hello méretének csökkentése ehelyett végezze el a VIP-csere frissítése. További információkért lásd: [felcserélése telepítési](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>

## <a name="how-an-upgrade-proceeds"></a>Hogyan egy frissítés előrehalad az
Eldöntheti, hogy kívánja-e tooupdate valamennyi hello szerepkörök a szolgáltatás és egyetlen szerepkör hello szolgáltatásban. Mindkét esetben minden frissítés alatt áll, és ugyanahhoz az első frissítési tartomány toohello szerepkör példányainak leállt, frissítése, és automatikusan visszaáll online állapotba. Ha azok ismét online, hello hello második frissítési tartomány-példány leállt, frissítése, és kerül ismét online állapotba. Egy felhőalapú szolgáltatás lehet aktív egyszerre legfeljebb egy frissítés. hello frissítés mindig hello hello felhőalapú szolgáltatás legújabb verziója alapján történik.

hello következő diagram azt ábrázolja, hogyan hello frissítés előrehalad, ha frissíti az összes hello szerepkörök hello szolgáltatásban:

![Service frissítése](media/cloud-services-update-azure-service/IC345879.png "service frissítése")

A következő diagram azt ábrázolja, hogyan lehet hello frissítés, hogy csak egy szerepkör frissíti:

![Frissítési szerepkör](media/cloud-services-update-azure-service/IC345880.png "frissítési szerepkör")  

Az automatikus frissítése közben hello Azure Fabric Controller rendszeresen kiértékeli hello cloud service toodetermine hello állapotát, amikor az biztonságos toowalk hello következő UD. Az állapot kiértékelésekor szerepkör alapon történik, és úgy ítéli meg, csak a példányok hello legújabb verzió (azaz példányok, amely már telefonon UDs). Ellenőrzi, hogy a szerepkörpéldányok, az egyes szerepkörökhöz minimális száma elérte a kielégítő terminál állapotba.

### <a name="role-instance-start-timeout"></a>Szerepkör példány Start időtúllépés
hello Fabric Controller minden egyes szerepkör-példány tooreach elindítva állapotának 30 percet várakozik. Ha hello időkorlát tartama eltelte után hello Fabric Controller továbbra is tovább szerepkörpéldányt toohello érdekében.

### <a name="impact-toodrive-data-during-cloud-service-upgrades"></a>A felhőalapú szolgáltatás frissítéskor toodrive adatait

A szolgáltatás egy egypéldányos toomultiple példányokból frissítésekor a szolgáltatás kerül a hello frissítés végrehajtása miatt toohello módon Azure szolgáltatás frissítése közben. hello szolgáltatás szolgáltatásiszint-szerződés garanciavállaló szolgáltatás rendelkezésre állása csak akkor, ha egynél több példánya telepített tooservices. hello alábbi lista ismerteti hogyan befolyásolják az egyes Azure-szolgáltatások frissítési forgatókönyv minden olyan meghajtó hello adatokat:

|Forgatókönyv|C meghajtó|D meghajtó|E meghajtó|
|--------|-------|-------|-------|
|Virtuális gép újraindítása|Megőrzi|Megőrzi|Megőrzi|
|Portál újraindítás|Megőrzi|Megőrzi|Megsemmisül|
|Lemezkép-visszaállítási portál|Megőrzi|Megsemmisül|Megsemmisül|
|Frissítés helyben|Megőrzi|Megőrzi|Megsemmisül|
|Csomópont áttelepítése|Megsemmisül|Megsemmisül|Megsemmisül|

Vegye figyelembe, hogy, a fenti listában hello, hello E: meghajtó hello szerepkör gyökérmeghajtóján jelöli, nem változtatható lehet. Ehelyett használja a hello **RoleRoot %** környezeti változó toorepresent hello meghajtó.

toominimize hello állásidő egy egypéldányos szolgáltatás frissítésekor központi telepítése egy új többpéldányos szolgáltatás toohello átmeneti kiszolgálón, és végezze el a virtuális IP-címcsere.

<a name="RollbackofanUpdate"></a>

## <a name="rollback-of-an-update"></a>Frissítés visszaállítása
Azure services kezelése frissítés közben azáltal, hogy egy szolgáltatás, további műveleteket kezdeményezheti által hello Azure Fabric Controller hello kezdeti frissítési kérelem elfogadása után rugalmasságot biztosít. Csak akkor hajtható végre a visszaállítást, ha egy frissítés (konfigurációváltozás), vagy frissítés hello **folyamatban** állapot hello üzemelő példányon. Egy frissítést, illetve a mindaddig, amíg nincs hello szolgáltatást, amely még nem frissített toohello új verziója legalább egy példánya tekinthető a toobe folyamatban. tootest, hogy visszaállítás történt engedélyezett, ellenőrizze hello RollbackAllowed jelző, által visszaadott hello értékének [első központi telepítési](https://msdn.microsoft.com/library/azure/ee460804.aspx) és [felhőalapú szolgáltatás tulajdonságainak beolvasása](https://msdn.microsoft.com/library/azure/ee460806.aspx) műveletek, tootrue van beállítva.

> [!NOTE]
> A csak teszi logika toocall visszaállítási egy **helyben** frissítési vagy verziófrissítési, mert a VIP-csere frissítéseket tartalmaz, amely a szolgáltatás egy teljes futó példányát cseréje egy másik.
>
>

A folyamatban lévő frissítés visszaállításának követő hatások hello üzemelő példányon hello rendelkezik:

* Bármely szerepkörpéldányok, amelyen korábban még nem frissített vagy frissített toohello új verziója nem frissített vagy frissítve, mert azokat a példányokat már fut a célverzió hello hello szolgáltatást.
* A szerepkör példánya, amelyek már frissültek vagy frissített toohello hello szolgáltatás csomag új verziója (\*.cspkg) fájl vagy hello szolgáltatás konfigurációs (\*.cscfg) fájl (mind) visszatért toohello frissítés előtti verziója Ezeket a fájlokat.

Ez gyakorlatilag által biztosított hello a következő funkciókat:

* Hello [visszaállítási módosítása vagy frissítése](https://msdn.microsoft.com/library/azure/hh403977.aspx) művelet, amely nem hívható meg a konfiguráció frissítése a (meghívásával indított [telepítési konfiguráció módosítása](https://msdn.microsoft.com/library/azure/ee460809.aspx)) vagy a verziófrissítésre (meghívásával indított [ Frissítés telepítési](https://msdn.microsoft.com/library/azure/ee460793.aspx)), amíg nincs legalább egy példány hello szolgáltatásban amely még nincs frissítve toohello új verziója.
* Zárolt elem és hello RollbackAllowed elem, hello adott válasz törzsének hello részeként visszaküldött hello [első központi telepítési](https://msdn.microsoft.com/library/azure/ee460804.aspx) és [felhőalapú szolgáltatás tulajdonságainak beolvasása](https://msdn.microsoft.com/library/azure/ee460806.aspx) műveletek:

  1. hello zárolt elem lehetővé teszi toodetect mutating művelet a megadott központi telepítés keletkezett.
  2. hello RollbackAllowed elem lehetővé teszi a toodetect, amikor hello [visszaállítás frissítés vagy frissítési](https://msdn.microsoft.com/library/azure/hh403977.aspx) művelet nem hívható meg egy adott központi telepítés a.

  A sorrend visszaállítás tooperform nincs toocheck hello zárolt, mind az RollbackAllowed elemek hello. Elegendő, ha tooconfirm, hogy RollbackAllowed tootrue van beállítva. Ezek az elemek csak adott vissza, ha ezek a módszerek hívják hello kérelem fejléce túl állítsa be a "x-ms-version: 2011-10-01" vagy egy újabb verziója. Versioning fejlécek kapcsolatos további információkért lásd: [Service Management Versioning](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Vannak olyan helyzetek, ahol egy frissítést a visszaállítás vagy a frissítés nem támogatott, ezek a következők:

* Helyi erőforrások - csökkenése Ha hello frissítés növekszik hello helyi erőforrások szerepkör hello Azure platform nem engedélyezi a visszaállítása.
* Sablonkérelem -, ha hello frissítés volt a művelet már nem lehet vertikális rendelkezik-e elegendő számítási kvóta toocomplete hello visszaállítási művelet. Minden Azure-előfizetésre vonatkozó kvóta hello mag, amely toothat előfizetéshez tartozó összes üzemeltetett szolgáltatás által igénybe vehető maximális számát megadó rendelkezik. A visszaállítás egy adott frissítés végrehajtása után kerülne az előfizetés keresztül kvóta, majd a visszaállítás nem engedélyezhető.
* Versenyhelyzet - Ha hello kezdeti frissítése befejeződött, a visszaállítás nincs lehetőség.

Ha frissítési hello visszaállítási hasznosak lehetnek példa: Ha hello használ [frissítés telepítési](https://msdn.microsoft.com/library/azure/ee460793.aspx) manuális mód toocontrol hello sebesség, amellyel egy fő helybeni frissítés tooyour Azure üzemeltetett szolgáltatásban futó művelete megkezdődött.

Hello bevezetés hello frissítés alatt hívható [frissítés telepítési](https://msdn.microsoft.com/library/azure/ee460793.aspx) manuális üzemmódban és a frissítési tartományok toowalk megkezdéséhez. Bármikor, ahogy figyeli hello frissítését, vegye figyelembe hello első frissítési tartományt, amely megvizsgálja az egyes szerepkörpéldányokat válaszol, ha hívása hello [visszaállítás frissítés vagy frissítési](https://msdn.microsoft.com/library/azure/hh403977.aspx) művelet hello üzemelő példányon, amely hagyja változatlanul hello példányok kellett még nincs frissítve, és volt-e a visszavonás-példányok toohello előző szolgáltatáscsomag és konfiguráció frissítése.

<a name="multiplemutatingoperations"></a>

## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Az egy folyamatban lévő telepítés több mutating művelet kezdeményezése
Bizonyos esetekben érdemes lehet tooinitiate több egyidejű mutating műveletek egy folyamatban lévő telepítés. Például előfordulhat, hogy hajtsa végre a szolgáltatás frissítése és a szolgáltatásban, ez a frissítés van megkezdődött, amíg meg toomake néhány változás, például tooroll hello biztonsági frissítés, egy másik frissítés vagy hello is törli. Egy esetet, ahol erre akkor lehet szükség, ha a szolgáltatás frissítése egy frissített szerepkör példány toorepeatedly összeomlási hatására buggy kódot tartalmazza. Ebben az esetben hello Azure Fabric Controller nem lesz képes toomake folyamatban van az alkalmazásában, amelyet frissíteni, mivel a példányok hello a frissített tartományban elegendő számú kifogástalan. Ebben az állapotban a hivatkozott tooas egy *elakadt a központi telepítési*. Hello telepítési visszaállítása hello frissítés, vagy egy friss frissítés alkalmazása sikertelen egy hello tetején keresztül is követően.

Miután hello kezdeti kérés tooupdate vagy frissítési hello szolgáltatás hello Azure Fabric Controller kapott, elindíthatja a későbbi szótárból elemeinek módosítása műveletek. Ez azt jelenti, hogy nincs a hello kezdeti művelet toocomplete toowait egy másik mutating művelet megkezdése előtt.

A második-frissítési művelet kezdeményezése, amíg hello első frissítés folyamatban lévő hasonló toohello visszaállítási művelet fogja elvégezni. Ha hello második frissítés automatikus üzemmódban, hello első frissítési tartomány azonnal frissíti, akkor esetleg nem kapcsolódik a hello több frissítési tartományt vezető tooinstances azonos időpontra történő.

hello szótárból elemeinek módosítása műveletek a következők: [telepítési konfiguráció módosítása](https://msdn.microsoft.com/library/azure/ee460809.aspx), [frissítés telepítési](https://msdn.microsoft.com/library/azure/ee460793.aspx), [központi telepítésének állapota](https://msdn.microsoft.com/library/azure/ee460808.aspx), [törlése Központi telepítési](https://msdn.microsoft.com/library/azure/ee460815.aspx), és [visszaállítási vagy Verziófrissítésekor](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Két műveletek [első központi telepítési](https://msdn.microsoft.com/library/azure/ee460804.aspx) és [felhőalapú szolgáltatás tulajdonságainak beolvasása](https://msdn.microsoft.com/library/azure/ee460806.aspx), térjen vissza a hello zárolt jelzőt, amely vizsgálni toodetermine is lehet, hogy mutating művelet a megadott központi telepítés is elindítható.

Az order toocall hello verzióban az alábbi módszerek hello zárolt jelző adja vissza, amely be kell kérelem fejléce túl "x-ms-version: 2011-10-01" vagy egy újabb verziója. Versioning fejlécek kapcsolatos további információkért lásd: [Service Management Versioning](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>

## <a name="distribution-of-roles-across-upgrade-domains"></a>Kapcsolódó szerepkörök frissítési tartományok között
Azure egyenletesen elosztja a szerepkör példánya beállítása a hello szolgáltatás definíciós (.csdef) fájl részeként konfigurálhatók frissítési tartományok száma. frissítési tartományok maximális száma hello 20 és hello alapértelmezett érték 5. További információ a hogyan toomodify hello a szolgáltatásdefiníciós fájlban: [Azure szolgáltatás definíciós séma (.csdef fájl)](cloud-services-model-and-package.md#csdef).

Például ha a szerepkör tíz példánnyal rendelkezik, alapértelmezés szerint minden frissítési tartomány két példányait tartalmazza. Ha a szerepkör 14 példánnyal rendelkezik, majd négy hello frissítési tartományok három példányokat tartalmazhatják, és ötödik tartomány két tartalmazza.

Frissítési tartományok azonosítják a nulla alapú indexét: hello első frissítési tartomány 0 Azonosítót tartalmaz, és a második frissítési tartomány hello Azonosítót 1, és így tovább.

hello következő ábra bemutatja hogyan két szerepköröket tartalmazza, mint szolgáltatás terjessze hello szolgáltatás két frissítési tartományt határoz meg. hello szolgáltatás fut hello webes szerepkör nyolc példánya és a feldolgozói szerepkör hello kilenc példányai.

![A frissítési tartományok terjesztési](media/cloud-services-update-azure-service/IC345533.png "frissítési tartományok terjesztése")

> [!NOTE]
> Vegye figyelembe, hogy az Azure vezérlők példányok frissítési tartományok közötti elosztását vezérli. Már nem lehetséges toospecify előfordulások toowhich tartomány foglal le.
>
>

## <a name="next-steps"></a>Következő lépések
[TooManage Cloud Services](cloud-services-how-to-manage.md)  
[TooMonitor Cloud Services](cloud-services-how-to-monitor.md)  
[TooConfigure Cloud Services](cloud-services-how-to-configure.md)  
