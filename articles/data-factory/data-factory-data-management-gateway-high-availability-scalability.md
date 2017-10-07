---
title: "az adatkezelési átjáró az Azure Data Factory aaaHigh rendelkezésre állását |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan lehet horizontálisan adatkezelési átjárót további csomópontokat és skálázási hozzáadásával legfeljebb egy csomópont futtatható egyidejűleg futó feladatainak számát növeli."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: abnarain
ms.openlocfilehash: 925f63728e23596bca2655636f6535b509fce0b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-management-gateway---high-availability-and-scalability-preview"></a>Az adatkezelési átjáró - magas rendelkezésre állás és méretezhetőség (előzetes verzió)
Ez a cikk segítséget nyújt a magas rendelkezésre állás és méretezhetőség megoldás az adatkezelési átjáró konfigurálásához.    

> [!NOTE]
> Ez a cikk feltételezi, hogy Ön már ismeri az adatkezelési átjáró alapjait. Ha nem, lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md).

>**Az előzetes funkcióval hivatalosan az adatkezelési átjáró verziója 2.12.xxxx.x vagy újabb támogatott**. Ellenőrizze, hogy verzió 2.12.xxxx.x használ vagy újabb. Töltse le az adatkezelési átjáró legújabb verzióját hello [Itt](https://www.microsoft.com/download/details.aspx?id=39717).

## <a name="overview"></a>Áttekintés
Felügyeleti adatátjáró hello portálról egyetlen logikai átjáróval több helyszíni gépen telepített lehet társítani. Ezek a gépek nevezzük **csomópontok**. Lehet túl**négy csomópont** társított logikai átjáró. több csomópont (a telepített átjárót a helyszíni gépeket) rendelkező logikai átjáró hello előnyei a következők:  

- A helyszíni és a felhő közötti adatátvitel teljesítményének növelése a adattárolókhoz.  
- Ha valamilyen okból leáll valamelyik hello csomópontja, más csomópontok továbbra is elérhetők a hello adatok áthelyezését. 
- Hello csomópontok egyikét kell toobe karbantartás céljából offline állapotba, ha más csomópontok továbbra is elérhetők a hello adatok áthelyezését.

Beállíthatja úgy is hello száma **egyidejű adatátviteli feladatok** egy csomópont tooscale hello képességét a helyszíni és a felhő között megköveteli az adatok mentése futó adattárolókhoz. 

Hello Azure-portál használatával, figyelheti hello állapotának ezeket a csomópontokat, amely segít eldönteni, hogy tooadd vagy a csomópont eltávolítása hello logikai átjáró. 

## <a name="architecture"></a>Architektúra 
hello következő ábra nyújt hello architektúrájának áttekintése méretezhetőség és a rendelkezésre állás szolgáltatással az adatkezelési átjáró hello: 

![Az adatkezelési átjáró - magas rendelkezésre állás és méretezhetőség](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-high-availability-and-scalability.png)

A **logikai átjáró** hello átjáró tooa adat-előállító hello Azure-portálon kell létrehoznia. Korábban a helyszíni csak egy Windows-számítógép sikerült társítani az adatkezelési átjáró egy logikai átjáró telepítve. Ez a helyszíni átjáró számítógépén nevezzük. Mostantól társíthat mentése túl**négy fizikai csomópontok** logikai átjáróként. Egy logikai átjárójának több csomópont neve egy **többcsomópontos átjáró**.  

Ezek a csomópontok vannak **aktív**. Adatok feladatok toomove adat a helyszínen és a felhő közötti összes tud feldolgozni adattárolókhoz. Hello csomópontok egyikét kézbesítő és munkavégző is jár el. Más csomópontok hello csoportokban munkavégző csomópontokhoz. A **kézbesítő** csomópont lekéri az adatátviteli feladatok/feladatok hello felhőalapú szolgáltatás, és kiszállítja őket tooworker csomópontok (beleértve magát). A **munkavégző** csomópont végrehajtja az adatok feladatok toomove adat a helyszínen és a felhő között adattárolókhoz. Minden csomópont munkavállalók. Csak egy csomópont lehet a küldő és a munkavégző.    

Általában egy csomópont lehet, hogy kezdjen és **horizontális felskálázás** tooadd csomópontjait, meglévő csomópont hello vannak túlterhelik a hello adatok mozgása terhelés. Emellett **vertikális felskálázás** adatok mozgása funkció egy átjáró csomópont hello növelésével hello toorun engedélyezett hello csomóponton egyidejűleg futó feladatainak számát. Ez a funkció érhető el egy egy csomópontos átjáró (még akkor is, ha hello méretezhetőséget és a rendelkezésre állási funkció nincs engedélyezve). 

Egy átjárót a több csomópont tartja hello adattárolóhoz használandó hitelesítő adatok szinkronban összes csomópont. Ha egy csomópont-csomópont hálózati probléma, hello hitelesítő adatok szinkronban lehet. Ha úgy állítja be a hitelesítő adatokat a helyszíni adatokat tároló egy átjárót használó, hello kézbesítő/munkavégző csomóponton menti hitelesítő adatokat. hello kézbesítő csomópont szinkronizál más munkavégző csomópontokhoz. Ezt a folyamatot nevezik **hitelesítő adatok szinkronizálása**. csomópontok közötti kommunikációs csatorna számára hello lehet **titkosított** egy nyilvános SSL/TLS-tanúsítvány. 

## <a name="set-up-a-multi-node-gateway"></a>Több csomópontos átjárót a beállítása
Ez a szakasz azt feltételezi, hogy a következő két cikkek vagy ezek a cikkek jellemzői az ismerős hello lezajlott: 

- [Az adatkezelési átjáró](data-factory-data-management-gateway.md) -hello átjáró részletes áttekintést nyújt.
- [Adatok áthelyezése között a helyszíni és felhőalapú adattároló](data-factory-move-data-between-onprem-and-cloud.md) -egy bemutató részletes utasításokat egy átjáró egyetlen csomóponttal rendelkező tartalmazza.  

> [!NOTE]
> Mielőtt adatkezelési átjárót egy a helyi Windows-számítógépen telepíti, olvassa el a szakaszában ismertetett előfeltételeknek [hello fő cikk](data-factory-data-management-gateway.md#prerequisites).

1. A hello [forgatókönyv](data-factory-move-data-between-onprem-and-cloud.md#create-gateway), logikai átjáró létrehozásakor engedélyezése hello **magas rendelkezésre állás és méretezhetőség** szolgáltatás. 

    ![Az adatkezelési átjáró - enable magas rendelkezésre állás és méretezhetőség](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-enable-high-availability-scalability.png)
2. A hello **konfigurálása** lapján választhatja **Express telepítő** vagy **manuális telepítés** tooinstall egy átjáró hello első csomóponton (egy a helyszíni Windows-számítógép) hivatkozásra.

    ![Az adatkezelési átjáró - kifejezett vagy manuális telepítés](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-express-manual-setup.png)

    > [!NOTE]
    > Hello Gyorstelepítés beállítás használatakor a hello csomópontok kommunikáció titkosítás nélkül történik. hello csomópont neve legyen, mint hello gép neve. Használja a manuális telepítési módra, ha hello csomópontok kommunikációs toobe titkosítani kell, vagy azt szeretné, hogy a csomópont nevét az Ön által választott toospecify. Csomópont nevének később nem szerkeszthető.
3. Ha úgy dönt, **Gyorstelepítés**
    1. Hello hello átjáró sikeres telepítése után a következő üzenet jelenik meg:

        ![Az adatkezelési átjáró - gyors telepítés sikeres](media/data-factory-data-management-gateway-high-availability-scalability/express-setup-success.png)
    2. Indítsa el az adatkezelési konfigurációkezelőjének hello átjáró következő [ezeket az utasításokat](data-factory-data-management-gateway.md#configuration-manager). Megjelenik a hello átjáró neve, a csomópont neve, állapota és stb.

        ![Az adatkezelési átjáró - sikeresen telepítve](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)
4. Ha úgy dönt, **manuális telepítési módra**:
    1. A Microsoft Download Center hello hello-csomagjának letöltése, futtatáshoz tooinstall átjáró a számítógépre.
    2. Használjon hello **hitelesítési kulcs** a hello **konfigurálása** lap tooregister hello átjáró.
    
        ![Az adatkezelési átjáró - sikeresen telepítve](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-authentication-key.png)
    3. A hello **új átjárócsomópont** lapon megadhat egyéni **neve** toohello átjárócsomópont. Alapértelmezés szerint a csomópont neve legyen, mint hello gép neve.    

        ![Az adatkezelési átjáró - nevének megadása](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-name.png)
    4. A következő oldalon hello, dönthet úgy, hogy túl**engedélyezheti a titkosítást csomópontok kommunikációhoz**. Kattintson a **kihagyása** toodisable titkosítás (alapértelmezett).

        ![Az adatkezelési átjáró - titkosítás engedélyezése](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-node-encryption.png)  
    
        > [!NOTE]
        > A titkosítási mód módosítása csak támogatott, ha az átjáró egyetlen csomópont már hello logikai átjáró. toochange hello titkosítási módra, amikor egy átjáró van több csomópont, a következő lépéseket hello: egy csomópont kivételével minden hello csomópontot törlése, módosítása hello titkosítási mód és majd újból vegye fel hello csomópontok.
        > 
        > Lásd: [a TLS/SSL-tanúsítványokkal szemben támasztott követelmények](#tlsssl-certificate-requirements) szakasz listáját egy TLS/SSL-tanúsítvány használatára vonatkozó követelményeket. 
    5. Hello átjáró sikeres telepítése után kattintson a Configuration Manager indítása:
    
        ![Manuális telepítés - indítsa el a configuration manager](media/data-factory-data-management-gateway-high-availability-scalability/manual-setup-launch-configuration-manager.png)   
    6. Az adatkezelési átjáró konfigurációkezelőjének hello csomópont (a helyszíni Windows számítógép), amely csatlakozási állapotát jeleníti meg, hogy **átjárónevet**, és **csomópontnév**.  

        ![Az adatkezelési átjáró - sikeresen telepítve](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)

        > [!NOTE]
        > Ha egy Azure virtuális gépen hello átjáró kiépíteni, [a Githubon az Azure Resource Manager sablon](https://github.com/xiaoyingLJ/vms-with-multiple-data-management-gateway). Ezt a parancsfájlt hoz létre egy logikai átjárót az adatkezelési átjáró szoftverrel rendelkező virtuális gépek beállítja és hello logikai átjáró regisztrálja őket. 
6. Azure-portálon, indítsa el a hello **átjáró** lap: 
    1. A hello data factory kezdőlapján hello portálon kattintson **összekapcsolt szolgáltatások**.
    
        ![Data factory kezdőlap](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-home-page.png)
    2. Válassza ki a hello **átjáró** toosee hello **átjáró** lap:
    
        ![Data factory kezdőlap](media/data-factory-data-management-gateway-high-availability-scalability/linked-services-gateway.png)
    4. Megjelenik a hello **átjáró** lap:   

        ![Egyetlen csomópont megtekintése átjáró](media/data-factory-data-management-gateway-high-availability-scalability/gateway-first-node-portal-view.png) 
7. Kattintson a **csomópont hozzáadása** a hello eszköztár tooadd csomópont toohello logikai átjáró. Ha azt tervezi, toouse Gyorstelepítés, ehhez a lépéshez a csomópont toohello átjáróként hozzáadott hello a helyi számítógépen. 

    ![Az adatkezelési átjáró - csomópont menü hozzáadása](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)
8. Lépésekre hasonló toosetting hello első csomópont. a Configuration Manager felhasználói felületén hello lehetővé teszi a hello csomópont nevét állítja be, ha hello manuális telepítés lehetőséget választja: 

    ![Configuration Manager - telepítés második átjáró](media/data-factory-data-management-gateway-high-availability-scalability/install-second-gateway.png)
9. Hello átjáró telepítése után sikeresen hello csomóponton hello Configuration Manager eszközt a következő képernyő hello jeleníti meg:  

    ![Configuration Manager - telepítés második átjáró sikeres](media/data-factory-data-management-gateway-high-availability-scalability/second-gateway-installation-successful.png)
10. Ha hello **átjáró** lap hello portal látható két átjárócsomópontok most: 

    ![A két csomópont hello portálon átjáró](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)
11. toodelete egy átjáró csomópontot, kattintson a **törlése csomópont** hello eszköztáron válassza a kívánt toodelete, és kattintson a hello csomópont **törlése** hello eszköztáron. Ez a művelet törli a kijelölt csomópont hello hello csoportból. Vegye figyelembe, hogy ez a művelet nem távolítja el hello adatok felügyeleti átjáró szoftverének hello csomópont (a helyi Windows-számítógépen). Használjon **programok telepítése és törlése** a Vezérlőpulton a hello helyszíni toouninstall hello átjáró. Átjáró hello csomópontról eltávolításakor a rendszer automatikusan törli a hello portál.   

## <a name="upgrade-an-existing-gateway"></a>Egy meglévő átjáró frissítése
Frissíthet egy meglévő átjáró toouse hello magas rendelkezésre állás és méretezhetőség szolgáltatás. Ez a szolgáltatás működik, csak a csomópontok hello az adatkezelési átjáró verzió > = 2.12.xxxx. Láthatja, hogy az adatkezelési átjáró hello a gépre telepítve hello verziójának **súgó** hello az adatkezelési átjáró konfigurációkezelőjének lapján. 

1. Frissítés hello átjáró hello a helyi gép toohello legújabb verziójában következő Ha letölti és futtatja az MSI-telepítő csomag hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Lásd: [telepítési](data-factory-data-management-gateway.md#installation) című szakaszban talál információt.  
2. Keresse meg a toohello Azure-portálon. Indítsa el a hello **adat-előállító lap** az a data factory. Kattintson a társított szolgáltatások csempe toolaunch hello **összekapcsolt szolgáltatások lap**. Jelölje be hello átjáró toolaunch hello **átjáró lap**. Kattintson a gombra, majd engedélyezze **előzetes verziójú funkciók** látható hello kép a következő módon: 

    ![Az adatkezelési átjáró - enable előzetes verziójú funkciók](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-existing-gateway-enable-high-availability.png)   
2. Hello portálon hello előzetes funkció engedélyezése után zárja be az összes lapot. Nyissa meg újra a hello **átjáró lap** toosee hello új előzetes felhasználói felület (UI).
 
    ![Az adatkezelési átjáró - enable előzetes funkció sikeres](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview-success.png)

    ![Az adatkezelési átjáró – tekintse meg a felhasználói felület](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview.png)

    > [!NOTE]
    > Hello a frissítés során hello első csomópontjának hello gép hello neve. 
3. Ezután adja hozzá egy csomópont. A hello **átjáró** kattintson **csomópont hozzáadása**.  

    ![Az adatkezelési átjáró - csomópont menü hozzáadása](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)

    Kövesse az utasításokat az előző szakaszban tooset hello hello csomópont. 

### <a name="installation-best-practices"></a>Gyakorlati tanácsok a telepítéshez

- Energiaséma konfigurálása, hello gazdagépen hello átjáró számára, hogy hello gép hibernálásra nem. Ha hello gazdaszámítógépen hibernálás hello átjáró toodata kérelmek nem válaszol.
- Készítsen biztonsági másolatot hello átjáró társított hello tanúsítvány.
- Győződjön meg róla minden csomópont (ajánlott) ideális teljesítmény hasonló konfigurációt. 
- Vegyen fel legalább két csomóponttal tooensure magas rendelkezésre állású.  

### <a name="tlsssl-certificate-requirements"></a>A TLS/SSL-tanúsítványokkal szemben támasztott követelmények
Átjáró csomópontok közötti kommunikáció tehető biztonságossá használt hello TLS/SSL-tanúsítvány hello követelményei a következők:

- hello tanúsítványnak kell lennie egy nyilvánosan megbízható X509 v3 tanúsítvány.
- Minden átjáró csomópontnak kell minősíteniük ezt a tanúsítványt. 
- Azt javasoljuk, hogy a nyilvános (külső) hitelesítésszolgáltató (CA) által kiállított tanúsítványokat használ.
- Támogatja az SSL-tanúsítványok Windows Server 2012 R2 által támogatott bármely kulcs mérete.
- Nem támogatja a CNG-kulccsal használó tanúsítványokat.
- Helyettesítő tanúsítványokat támogatottak. 


## <a name="monitor-a-multi-node-gateway"></a>Több csomópontos átjáró figyelője
### <a name="multi-node-gateway-monitoring"></a>Több csomópontos átjáró figyelése
Hello Azure-portálon minden csomóponton együtt átjáró csomópontok állapotok megtekintheti közel valós idejű pillanatképe erőforrás-használat (Processzor, memória, network(in/out), stb.). 

![Az adatkezelési átjáró - több csomópont figyelése](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)

Engedélyezheti a **speciális beállítások** a hello **átjáró** toosee metrikák például Speciális lapon **hálózati**(in/out), **szerepkör & hitelesítő adatok állapot**, ez segíthet átjáró hibáinak feltárására és **egyidejűleg futó feladatainak** (futtató / Limit) amely is lehet módosított / megváltozott ennek megfelelően során teljesítményhangolás. hello alábbi táblázat mutatja az oszlopok hello **Átjárócsomópontok** listája:  

Figyelési tulajdonság | Leírás
:------------------ | :---------- 
Név | Hello logikai átjáró és hello átjáróhoz társított csomópont neve.  
status | Hello logikai átjáró és hello átjárócsomópontok állapotát. Példa: Online/Offline/korlátozott/stb. A fenti állapotok megjelenése kapcsolatos információkért lásd: [az átjáró állapotának](#gateway-status) szakasz. 
Verzió | Hello logikai átjáró hello verziója és minden átjáró csomópont jeleníti meg. hello logikai átjáró hello verziója hello csoportban lévő csomópontok többsége verziója alapján határozza meg. Csomópontok eltérő verziójú hello logikai átjáró beállítása esetén csak hello csomópont megfelelően hello azonos verziószámú hello logikai átjáró funkciót. Mások hello korlátozott módban van, és toobe (csak abban az esetben az automatikus frissítés nem sikerül) manuálisan frissíteni kell. 
Rendelkezésre álló memória | Rendelkezésre álló memória egy átjáró-csomóponton. Ez az érték közel valós idejű pillanatképet. 
CPU-felhasználás | Egy átjáró csomópont CPU-felhasználását. Ez az érték közel valós idejű pillanatképet. 
Hálózatkezelés (In/Out) | Hálózathasználat egy átjáró csomópont. Ez az érték közel valós idejű pillanatképet. 
Egyidejűleg futó feladatainak (futtató / Limit) | Feladatok vagy minden egyes csomóponton futó feladatok száma. Ez az érték közel valós idejű pillanatképet. Korlát azt jelzi, hogy az egyes csomópontok egyidejű feladatok maximális hello. Ez az érték hello mérete alapján van definiálva. Növelheti a hello korlát tooscale be egyidejű feladatok végrehajtásának speciális forgatókönyvekhez, ahol Processzor / memória / hálózati alatt szükség, de a tevékenységek vannak időtúllépés miatt. Ez a funkció érhető el egy egy csomópontos átjáró (még akkor is, ha hello méretezhetőséget és a rendelkezésre állási funkció nincs engedélyezve). További információkért lásd: [szempontok méretezése](#scale-considerations) szakasz. 
Szerepkör | A szerepkörök – kézbesítő és munkavégző két típusa van. Az összes csomópontja a dolgozók, ami azt jelenti, hogy az összes lehetnek használt tooexecute feladatok. A kézbesítő csak egy csomópont, amely feladatok használt toopull feladatok a felhőalapú szolgáltatások, és mennyi őket toodifferent munkavégző csomópontokhoz (beleértve magát) van. 

![Az adatkezelési átjáró - speciális több csomópont figyelése](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-advanced.png)

### <a name="gateway-status"></a>Az átjáró állapotának

hello következő táblázat a lehetséges állapotok egy **átjárócsomópont**: 

status  | Megjegyzések/forgatókönyvek
:------- | :------------------
Online | Csomópont tooData Factory szolgáltatásnak kapcsolódik.
Kapcsolat nélküli módban | Csomópontja offline állapotban.
Frissítése | hello csomópont automatikus frissítése folyamatban van.
Korlátozott | TooConnectivity probléma miatt. TooHTTP port 8050 problémát, a service bus kapcsolati probléma vagy a hitelesítő adatok szinkronizálási problémája miatt lehet. 
Inaktív | Csomópont van egy másik hello konfigurációból egyéb többsége csomópontok konfigurációban.<br/><br/> A csomópont inaktív lehet, amikor tooother csomópontok nem tud kapcsolódni. 


hello következő táblázat a lehetséges állapotok egy **logikai átjáró**. az átjáró állapotának hello hello átjáró csomópontok állapotok függ. 

status | Megjegyzések
:----- | :-------
Regisztrálnia kell az Adatátjárót | Nincs csomópont még regisztrált toothis logikai átjáró
Online | Átjáró csomópontja online állapotban.
Kapcsolat nélküli módban | Nincs csomópontja online állapotát.
Korlátozott | Ez az átjáró nem minden csomópontja kifogástalan állapotban vannak. Ez az állapot nem figyelmezteti rá, hogy néhány csomópont esetleg nem működik! <br/><br/>A kézbesítő/munkavégző csomóponton toocredential szinkronizálási problémája miatt lehet. 

### <a name="pipeline-activities-monitoring"></a>Feldolgozási sor / tevékenységek figyelése
hello Azure-portálon biztosít egy folyamat figyelésének lehetőségével részletes csomópont szintű adatokkal. Például azt jeleníti meg, mely tevékenységek futtatott melyik csomópontján. Lehet, hogy ezek az információk teljesítményproblémákat egy adott csomópont toonetwork szabályozás miatt mondja ki a hasznos információkat. 

![Az adatkezelési átjáró - folyamatok figyelését több csomópont](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-pipelines.png)

![Az adatkezelési átjáró - feldolgozási folyamat részletei](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-pipeline-details.png)

## <a name="scale-considerations"></a>Skála kapcsolatos szempontok

### <a name="scale-out"></a>Horizontális felskálázás
Ha hello **elérhető memória pedig kevés** és hello **CPU-használata túl magas**, új csomópont hozzáadása segíti a hello terhelés kibővítési gépek között. Tevékenységek miatt nem kapcsolódott tootime kibővített vagy az átjáró csomópont nem működnek, ha segít, ha csomópont toohello átjáró hozzáadása.
 
### <a name="scale-up"></a>Vertikális felskálázás
Amikor hello rendelkezésre álló memória és CPU nem használhatók is, de hello üresjárati kapacitás 0, érdemes méretezni legfeljebb egy csomópont futtatható egyidejűleg futó feladatainak számát hello növelésével. Is érdemes lehet tooscale mentése során a tevékenységek időtúllépés miatt, mivel hello átjáró túl van terhelve. Hello kép a következő ábrán is növelheti hello maximális csomópont. Javasoljuk, hogy így dupla toostart együtt.  

![Az adatkezelési átjáró - skálázási kapcsolatos szempontok](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-scale-considerations.png)


## <a name="known-issuesbreaking-changes"></a>Ismert problémák/legfrissebb módosítása

- Jelenleg is fel fizikai átjárócsomópontok toofour egyetlen logikai átjáró. Ha négynél több teljesítményének javítására van szüksége, küldjön egy e-mailt túl[DMGHelp@microsoft.com](mailto:DMGHelp@microsoft.com).
- Egy átjáró csomópont újra hello hitelesítési kulccsal történő egy másik logikai átjáró tooswitch hello aktuális logikai átjáró nem regisztrálható. toore regisztrációját, hello átjáró eltávolítása hello csomópont, telepítse újra hello átjárót, és regisztrálhatja azt az hello hitelesítési kulcs hello más logikai átjáró. 
- Ha a HTTP-proxy szükség az átjáró összes csomóponthoz, hello proxy diahost.exe.config és diawp.exe.config, majd hello server manager toomake meg arról, hogy minden csomópontja rendelkezik hello azonos diahost.exe.config és diawip.exe.config. Lásd: [konfigurálja a proxybeállításokat](data-factory-data-management-gateway.md#configure-proxy-server-settings) című szakaszban talál információt. 
- toochange titkosítási mód csomópontok kommunikáció az átjáró Konfigurációkezelőjében, hello portálon egy kivételével az összes hello csomópontok törlése. Adja hozzá a csomópontok hello titkosítási mód megváltoztatása után vissza.
- Hivatalos SSL-tanúsítvány használatára, ha úgy dönt, hogy tooencrypt hello csomópontok kommunikációs csatornát. Önaláírt tanúsítvány kapcsolat hibákat okozhat, ugyanazt a tanúsítványt nem lesz megbízható a más számítógépeken tanúsító listáján hello. 
- Egy csomópont tooa logikai átjárók nem lehet regisztrálni, ha hello csomópont verziószáma kisebb mint hello logikai átjáró verziója. Hello logikai átjáró összes csomópontja törlése a portálról, hogy egy alacsonyabb verziójú node(downgrade) regisztrálhatja azt. Ha töröl egy logikai átjáró összes csomópontja, manuálisan telepíti, és regisztrálja az új csomópontok toothat logikai átjáró. A gyorstelepítés nem támogatott ebben az esetben.
- Nem használhatja a gyorstelepítés tooinstall csomópontok tooan meglévő logikai átjárón, amely felhő hitelesítő adatai továbbra is használja. Ellenőrizheti, hogy az átjáró konfigurációkezelőjének hello hello-beállítások lapon hello hitelesítő adatok tárolására.
- A gyorstelepítés tooinstall csomópontok tooan meglévő logikai átjárón, amely rendelkezik a csomópontok titkosítás engedélyezése nem használható. Beállítás hello titkosítási mód magában foglalja a tanúsítványok manuális hozzáadásával, mivel az Expressz telepítés lehetőség nincs több. 
- A fájl másolatát a helyszíni környezetből, ne használjon \\localhost vagy C:\files többé óta localhost vagy a helyi meghajtó előfordulhat, hogy nem kell minden csomópont keresztül érhető el. Ehelyett használjon \\ServerName\files toospecify fájlok helyét.


## <a name="rolling-back-from-hello-preview"></a>Hello Preview visszaállítása 
tooroll újból az hello előzetes törlése kívül egy csomópont összes csomópontján. Nem számít, törlése, de ellenőrizze, hogy legalább egy csomópont az hello logikai átjáró csomópontok. Törölheti a csomópont hello gépen-átjáró eltávolítása vagy hello Azure-portál használatával. Az Azure portálon hello hello **adat-előállító** lapján kattintson a társított szolgáltatások toolaunch hello **összekapcsolt szolgáltatások** lap. Jelölje be hello átjáró toolaunch hello **átjáró** lap. A hello átjáró lapján látható hello átjáróhoz társított hello csomópontok. hello lap lehetővé teszi a csomópont hello átjáró törlése.
 
Ha töröl, kattintson a **az előzetes funkciók** hello ugyanazt az Azure portálon, és tiltsa le a hello előzetes funkció. Az átjáró tooone csomópont GA (általánosan rendelkezésre álló) átjáró visszaállítása.


## <a name="next-steps"></a>Következő lépések
Tekintse át a következő cikkek hello:
- [Az adatkezelési átjáró](data-factory-data-management-gateway.md) -hello átjáró részletes áttekintést nyújt.
- [Adatok áthelyezése között a helyszíni és felhőalapú adattároló](data-factory-move-data-between-onprem-and-cloud.md) -egy bemutató részletes utasításokat egy átjáró egyetlen csomóponttal rendelkező tartalmazza. 
