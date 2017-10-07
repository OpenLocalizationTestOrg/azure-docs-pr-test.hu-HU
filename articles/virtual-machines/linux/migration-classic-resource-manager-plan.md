---
title: "IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítése aaaPlanning |} Microsoft Docs"
description: "IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének tervezése"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 78492a2c-2694-4023-a7b8-c97d3708dcb7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/01/2017
ms.author: kasing
ms.openlocfilehash: 53c6f640425b69cae2ef10afb8c92b8ac4394267
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="planning-for-migration-of-iaas-resources-from-classic-tooazure-resource-manager"></a>IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének tervezése
Azure Resource Manager nagy mennyiségű elképesztő funkciókat kínál, de napjainkban ki az áttelepítési út toomake sure dolgot zökkenőmentesek kritikus tooplan. Tervezési idő költségeik biztosítja, hogy nem tapasztal, az áttelepítési tevékenységek végrehajtása során. 

> [!NOTE] 
> a következő útmutatást hello fokozottan átadott tooby hello Azure felhasználói Ügyféltanácsadói csapatának és ügyfelek áttelepítése nagy enviornments használata felhőalapú megoldás fejlesztők volt. Ez a dokumentum használja, így továbbra is frissült az új mintáinak sikeres merülnek fel, újból az idő tootime toosee Igen ellenőrzés ha vannak új javaslatokkal tooget.

Hello áttelepítési út négy általános fázisból áll:

![Áttelepítési fázis](../media/virtual-machines-windows-migration-classic-resource-manager/plan-labtest-migrate-beyond.png)

## <a name="plan"></a>Felkészülés

### <a name="technical-considerations-and-tradeoffs"></a>Technikai szempontokat és kompromisszumot

Attól függően, hogy a műszaki követelményeiben méret, földrajzi és üzemeltetési eljárások érdemes lehet tooconsider:

1. Miért szükséges a szervezet Azure Resource Manager?  Mik azok a hello üzleti okokból az áttelepítéshez?
2. Mik azok a hello technikai okokból az Azure Resource Manager?  Mi (ha van ilyen) további Azure-szolgáltatások szeretné tooleverage?
3. Melyik alkalmazás (vagy a virtuális gépek beállítása) része a hello áttelepítési?
4. Hello áttelepítési API mely forgatókönyvek támogatottak?  Felülvizsgálati hello [funkció és konfigurációs beállítás nem támogatott](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations).
5. Az operatív adapterekhez most támogatja alkalmazások/virtuális gépek Azure Resource Manager és klasszikus?
6. Hogyan (ha egyáltalán) Azure Resource Manager változik a virtuális gép telepítési, kezelési, figyelési és jelentési folyamatok?  Az üzembe helyezési parancsfájlok kell frissíteni toobe?
7. Mi az az hello kommunikáció tervezése a tooalert érdekelt felek (a végfelhasználók alkalmazástulajdonosok és infrastruktúrák tulajdonosai)?
8. Attól függően, hogy hello környezet hello összetettségét kell a karbantartási időszak hello alkalmazás esetén nem érhető el tooend felhasználók és tooapplication tulajdonosok?  Ha igen, mennyi ideig?
9. Mi az az hello képzési terv tooensure érdekelt felek tájékozott és az Azure Resource Manager jártassággal?
10. Mi az a hello program felügyeleti vagy felügyeleti projektterv hello áttelepítésre?
11. Mik azok a hello ütemtervek hello Azure Resource Manager áttelepítési és egyéb kapcsolódó technológia közúti maps?  Ezek optimális igazított?

### <a name="patterns-of-success"></a>Sikeres mintái

Sikeres ügyfél terveket, ahol hello a fenti kérdések tárgyalt, dokumentált és szabályozott részletesen.  Győződjön meg arról hello áttelepítési terveket körben közölt toosponsors és az érdekelt felekkel.  Az áttelepítési beállítások; ismereteket ellátására saját kezűleg egész megadása az alábbi áttelepítési dokumentumban ajánlott.

* [IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a platform által támogatott áttelepítésének áttekintése](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének tervezése](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [PowerShell toomigrate IaaS erőforrásainak klasszikus tooAzure erőforrás-kezelő használata](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Parancssori felület toomigrate IaaS erőforrásainak klasszikus tooAzure erőforrás-kezelő használata](migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének védelmével kapcsolatos közösségi eszközök](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [A leggyakoribb áttelepítési hibák áttekintése](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Felülvizsgálati hello legtöbb kapcsolatos gyakori kérdések a klasszikus tooAzure erőforrás-kezelő áttelepítése IaaS-erőforrásokra](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="pitfalls-tooavoid"></a>Nehézségek tooavoid

- Hiba tooplan.  az áttelepítés hello technológia lépésein bizonyítottan és hello eredménye előre jelezhető.
- Feltételezve, hogy a hello támogatott platform áttelepítési API fog fiók minden forgatókönyve. Olvasási hello [funkció és konfigurációs beállítás nem támogatott](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations) toounderstand milyen forgatókönyvek is támogatottak.
- Nem lehetséges alkalmazás kimaradás tervezése végfelhasználók számára.  Tervezze meg elegendő puffer tooadequately figyelmeztetés a végfelhasználók számára a potenciálisan nem érhető el alkalmazás idő.


## <a name="lab-test"></a>Tesztelési labor 

**A felhasználásához replikálja, és hajtsa végre a test-áttelepítés**
  > [!NOTE]
  > A meglévő környezet replikációs végrehajtása hivatalosan nem támogatott Microsoft Support közösségi hozzájárultak eszköz használatával. Emiatt egy **választható** hello legjobb módja toofind miatt a termelési környezetben érintése nélkül, de a lépést. Közösségi hozzájárultak eszköz használatával lehetőség nem érhető el, ha majd olvassa el az alábbi ellenőrzése/Prepare/megszakítási próbafuttatást javaslat hello.
  >
  
  Hello legjobb módja tooensure zökkenőmentes áttelepítés végrehajtása egy laboratóriumi tesztelése a pontos forgatókönyv (számítási, hálózati és tárolási). Ezzel biztosítja:

  - Egy teljesen elkülönített labor- vagy egy meglévő, nem éles környezetben tootest. Azt javasoljuk, hogy egy teljesen különálló tesztkörnyezetet, mely ismételten telepíthető át, és a korábbi módosítható.  Parancsfájlok toocollect/hidrát metaadatok hello valós előfizetések listája látható.
  - Egy jó ötlet toocreate hello labor a különálló előfizetést is. hello oka az, hogy hello labor fog kell bontva ismételten, és hogy egy különálló, elkülönített előfizetés csökkenti hello alkalommal, hogy valami valós kap véletlenül törölt.

  Ez az hello AsmMetadataParser eszköz használatával valósítható meg. [További tudnivalók az eszköz itt](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

### <a name="patterns-of-success"></a>Sikeres mintái

hello következő számos olyan hello nagyobb áttelepítések felfedezett volt. Ez nem teljesnek és olvassa el az toohello [funkció és konfigurációs beállítás nem támogatott](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations) további részletek. Lehet, vagy nem a műszaki problémák merülhetnek, de ha így tesz megoldása a migrálás megkísérlése előtt fog gördülékenyebb élményt nyújtsanak.

- **Ellenőrzés/Prepare/megszakítási próbafuttatást tegye** – Ez lehet, hogy a hello legfontosabb lépés tooensure klasszikus tooAzure erőforrás-kezelő áttelepítése sikeres. hello áttelepítési API rendelkezik három fő lépésből: Ellenőrizze, előkészítése, és véglegesítse. Ellenőrizni fogja a klasszikus környezet hello állapotadatokat olvasni, és térjen vissza az összes probléma eredménye. Azonban mivel bizonyos problémák előfordulhat, hogy létezik a hello Azure Resource Manager-készletben, érvényesítése nem képes mindent. áttelepítési folyamat következő lépésének hello, előkészítési segítségével teszi közzé a ismertetünk. Készítse elő lesz áthelyezése hello metaadatai a klasszikus tooAzure erőforrás-kezelő, de fog nem véglegesítési hello áthelyezése, és nem eltávolítása vagy a klasszikus ügyféloldali hello bármit módosíthat. hello próbafuttatást magában foglalja a hello áttelepítésének előkészítése, majd megszakítása (**nem véglegesítése**) hello áttelepítésének előkészítése. hello ellenőrzése/előkészítése/megszakítási próbafuttatást célja toosee összes hello metaadatok hello Azure Resource Manager-készletben, vizsgálja meg (*programozott módon, vagy a portálon*), és győződjön meg arról, hogy minden helyesen áttelepíti, és a munka – technikai problémák.  Azt is ad bizonyos értelemben az áttelepítés időtartamát, tervezéséhez állásidejét ennek megfelelően.  A validate/előkészítése/abort nem okoz felhasználói leállási; ezért nem zavaró tooapplication használati.
  - hello elemeket kell toobe hello próbafuttatást előtt lehet megoldani, de próbafuttatást teszt is nyugodtan kiüríti azokat a előkészítő lépések, ha azok kimaradt. Vállalati az áttelepítés során is észleltünk hello próbafuttatást toobe egy biztonságos és hasznos információt módon tooensure áttelepítésre való készenlét biztosítására.
  - Ha előkészítése fut, hello vezérlő vezérlősík (az Azure felügyeleti műveletek) zárolva lesz hello teljes virtuális hálózat, ezért nem módosítható tooVM metaadatok ellenőrzése/előkészítése/megszakítás során.  De egyébként bármely alkalmazás függvény (távoli asztali Munkamenetgazda, a virtuális gép kihasználtsága, stb.) nem érinti.  Felhasználók hello virtuális gépek nem fogja tudni, hogy hello próbafuttatást végrehajtása zajlik.

- **Express Route Kapcsolatcsoportok és a VPN-**. Jelenleg Express Route átjárók engedélyezési hivatkozásokkal állásidő nélkül nem telepíthetők át. Hello megkerülő megoldás, lásd: [áttelepítése ExpressRoute áramkörök és társított virtuális hálózatokat hello klasszikus toohello Resource Manager üzembe helyezési modellben](../../expressroute/expressroute-migration-classic-resource-manager.md).

- **Virtuálisgép-bővítmények** -virtuálisgép-bővítmények számára hello futó virtuális gépek legnagyobb roadblocks toomigrating egyikét. Virtuálisgép-bővítmények szervizelésének sikerült upwards of 1-2 nap igénybe, ezért ennek megfelelően tervezheti.  Egy működő Azure-ügynök a szükséges tooreport hátsó Virtuálisgép-bővítmény állapotának futó virtuális gépek. Ha hello állapota hibás ismét elérhető lesz a futó virtuális gépek, megáll, hogy áttelepítés. hello megbízott maga nem üzemkész tooenable áttelepítés toobe szükséges, de ha léteznek bővítmények hello VM, akkor mindkét egy működő ügynök AND (DNS) a kimenő internetkapcsolat szükséges áttelepítési toomove előre.
  - Ha a kapcsolat tooa DNS-kiszolgáló összes Virtuálisgép-bővítmények BGInfo v1 kivéve az áttelepítés során elvész. \* kell toofirst eltávolítja minden virtuális gép áttelepítésének előkészítése előtt, és ezt követően újból hozzá hátsó toohello VM Azure Resource Manager az áttelepítés után.  **Ez csak futó virtuális gépek esetén.**  Virtuális gépek hello felszabadított leállnak, ha Virtuálisgép-bővítmények nem kell toobe eltávolítva. **Megjegyzés:** maguk sok bővítmények, például az Azure diagnostics és a security center figyelési lesz telepítse újra az áttelepítést követően, távolítsa el nincs probléma.
  - Emellett gondoskodjon arról, hogy hálózati biztonsági csoportok nem kimenő internet-hozzáférés korlátozása. Ez akkor fordulhat elő, az egyes hálózati biztonsági csoportok konfigurációk. Kimenő internet-hozzáférés (és a DNS-) van szükség a Virtuálisgép-bővítmények toobe tooAzure erőforrás-kezelő áttelepítése. 
  - Hello BGInfo bővítményt két verziója van: v1 és v2.  Hello VM hello klasszikus portál vagy a PowerShell használatával hozták létre, ha a virtuális gép hello valószínűleg hello v1 kiterjesztéssel rendelkeznek a rajta. A bővítmény nem kell eltávolítani toobe, és a rendszer kihagyja (áttelepítése nem) hello áttelepítéssel API használatával. Azonban ha hello klasszikus virtuális gép hello új Azure-portálon hozták létre, azt fogja valószínűleg hello JSON-alapú v2 verziója van BGInfo, amely lehet áttelepített tooAzure erőforrás-kezelő hello ügynök működik-e, és a kimenő internet-hozzáférés (és a DNS-) van megadva. 
  - **1. javítási lehetőséget**. Ha tudja, hogy a virtuális gépek nem fognak hozzáférni, egy működő DNS-szolgáltatás, és használata az Azure ügynökök hello virtuális gépeken, majd el kell távolítani az összes Virtuálisgép-bővítmények előkészítése előtt hello áttelepítés részeként kimenő internet, majd újra kell telepítenie hello Virtuálisgép-bővítmények az áttelepítés után. 
  - **2. lehetőség szervizelési**. Ha a Virtuálisgép-bővítmények túl nagy, egy a küszöbértéket, a másik lehetőség is tooshutdown/felszabadítani minden virtuális gép áttelepítése előtt. Hello nincs lefoglalva a virtuális gépek áttelepítése, majd indítsa újra őket a hello Azure Resource Manager oldalán. hello itt előnye, hogy a Virtuálisgép-bővítmények lesz áttelepítve. hello hátránya az, hogy az összes nyilvánosan elérhető a virtuális IP-cím el fog veszni (ez nem alapszintű lehet), és nyilvánvalóan a hello virtuális gépek egy sokkal nagyobb hatással vannak a működő alkalmazások, amely le fog állni.

    > [!NOTE] 
    > Ha az Azure Security Center házirend hello fut az áttelepítés alatt álló virtuális gépek elleni van konfigurálva, hello biztonsági házirendnek kell toobe kiterjesztések eltávolítása előtt leállt, ellenkező esetben a monitorozási bővítményt újra lesz telepítve automatikusan hello biztonsági hello után VM eltávolítása.

- **Rendelkezésre állási készletek** - egy virtuális hálózathoz (vNet) toobe az áttelepített erőforrás-kezelő tooAzure, hello Classic deployment (azaz a felhőalapú szolgáltatás) található virtuális gépek kell lenniük egy rendelkezésre állási csoport vagy hello virtuális gépek összes nem lehet a rendelkezésre állási csoportot. Egynél több rendelkezésre állási hello felhőszolgáltatás csoport rendelkezik nem kompatibilis az Azure Resource Manager és megáll, és áttelepítési.  Emellett nem lehet néhány virtuális gépek rendelkezésre állási csoportba, és egyes virtuális gépek rendelkezésre állási csoport nem található. tooresolve, fog kell tooremediate vagy átütemezésével a felhőalapú szolgáltatás.  Tervezze meg ennek megfelelően, mert ez időigényes lehet. 

- **Webes vagy feldolgozói szerepkör telepítéseket** -webes és feldolgozói szerepköröket tartalmazó Felhőszolgáltatások tooAzure erőforrás-kezelő nem tud áttelepíteni. hello webes vagy feldolgozói szerepköröket először el kell távolítani a virtuális hálózati hello áttelepítés megkezdése előtt.  Egy tipikus megoldás toojust áthelyezés webes/munkavégző szerepkör példányok tooa külön klasszikus virtuális hálózatot, amely is csatolt tooan ExpressRoute-kapcsolatcsoportot vagy toomigrate hello kód toonewer PaaS alkalmazásszolgáltatások (ismertető nem ez a dokumentum hello terjed). A hello volt eset újratelepíteni, hozzon létre egy új klasszikus virtuális hálózatot, áthelyezés/helyezze üzembe újra hello webes vagy feldolgozói szerepkörök toothat új virtuális hálózat, majd hello központi telepítések törlése hello áthelyezett virtuális hálózati. Nem szükséges kódmódosításokat. új hello [virtuális hálózati társviszony-létesítés](../../virtual-network/virtual-network-peering-overview.md) funkció használt toopeer együtt hello klasszikus virtuális hálózatot hello webes vagy feldolgozói szerepköröket tartalmazó, valamint az egyéb virtuális hálózatok hello azonos Azure-régiót például hello virtuális hálózati folyamatban áttelepített (**, nem telepíthetők át a virtuális hálózatok társítottak, virtuális hálózati áttelepítés befejezése után**), ezért hello azonos képességek biztosítása a teljesítmény adatvesztés nélkül és a késés/sávszélesség büntetést nem. Mivel hello [virtuális hálózati társviszony-létesítés](../../virtual-network/virtual-network-peering-overview.md), webes vagy feldolgozói szerepkör telepítéseket mostantól egyszerűen kivédhető, és nem blokkolja a hello áttelepítési tooAzure erőforrás-kezelő.

- **Az Azure erőforrás-kezelő kvótái** -Azure-régiók Azure Resource Manager és klasszikus külön kvóták/korlátokkal rendelkeznek. Annak ellenére, hogy az áttelepítési forgatókönyvben új hardver nem feldolgozottként *(azt a meglévő virtuális gépek a klasszikus tooAzure erőforrás-kezelő most csere)*, továbbra is meg kell toobe helyen elegendő kapacitással, mielőtt Azure erőforrás-kezelő kvótái áttelepítési elindíthatja. Az alábbiakban hello jelentős korlátokat is láttuk problémákhoz.  Nyissa meg a kvóta támogatási jegy tooraise hello korlátozza. 

    > [!NOTE]
    > Ezek a korlátozások kell keletkezés toobe hello ugyanabban a régióban, mint a jelenlegi felhasználásához toobe át.
    >

    - Hálózati illesztők
    - Terheléselosztók
    - Nyilvános IP-címek
    - Statikus nyilvános IP-címek
    - Processzormagok
    - Network Security Groups (Hálózati biztonsági csoportok)
    - Útvonaltáblák

    A jelenlegi Azure erőforrás-kezelő kvótái, a következő parancsokat az Azure CLI 2.0 hello legújabb verziójával hello segítségével ellenőrizheti.

    **Számítási** *(mag, Avaiability beállítása)*

    ```bash
    az vm list-usage -l <azure-region> -o jsonc 
    ```

    **Hálózati** *(virtuális hálózatok, a statikus nyilvános IP-címek, a nyilvános IP-címek, hálózati biztonsági csoportok, hálózati adapter áll rendelkezésükre, Terheléselosztók, Útvonaltábláit)*
    
    ```bash
    az network list-usages -l <azure-region> -o jsonc
    ```

    **Tárolási** *(Tárfiók)*
    
    ```bash
    az storage account show-usage
    ```

- **Az Azure Resource Manager API szabályozás korlátok** – Ha a elég nagy környezetekben (például) > 400 virtuális gépek a VNETEN belül), előfordulhat, hogy kattint az hello alapértelmezett API korlátozása korlátozza az írási műveletek (jelenleg **1200 írások óránként**) az Azure Resource Manager. Áttelepítés megkezdése előtt meg kell egy támogatási jegy tooincrease szolgálattól a korlát növelését az előfizetéséhez.

- **Előkészítési virtuális gép állapotát túllépte az időkorlátot** – Ha a virtuális gép állapota hello **kiépítése túllépte az időkorlátot**, ez igények toobe áttelepítés előtti feloldva. hello csak úgy toodo ezen nem állásidővel megszüntetés/reprovisioning hello virtuális gép (törlés, hello lemez megőrzése, és hozza létre újra a virtuális gép hello). 

- **RoleStateUnknown virtuális gép állapotát** – Ha az áttelepítés miatt leáll tooa **szerepkör állapota ismeretlen** hiba jelenik meg, vizsgálja meg hello VM hello portál használatával, és ellenőrizze, hogy fut-e. Ez a hiba általában eltűnik majd a (nem kötelező szervizelési) tulajdonosai néhány perc múlva, és gyakran egy átmeneti típus gyakran látható során a virtuális gép **start**, **leállítása**, **újraindítása** műveletek. **Ajánlott eljárás:** próbálja meg újból az áttelepítési újra néhány perc múlva. 

- **Fabric-fürt nem létezik** – néhány esetben egyes virtuális gépek nem telepíthetők át a különböző páratlan okokból. Ezekben az esetekben ismert egyik Ha hello virtuális gép (belül hello múlt héten vagy így) nemrég lett létrehozva, és egy Azure-fürttel, amely még nem rendelkezik Azure Resource Manager munkaterhelésekhez tooland történt.  Egy hiba, amely szerint jelenik **fabric-fürt nem létezik** és hello virtuális gép nem telepíthető át. Néhány nap Várakozás fog általában adott probléma megoldásához, hello fürt hamarosan megkapja az Azure Resource Manager engedélyezve van. Azonban egy azonnali megoldás túl,`stop-deallocate` hello a virtuális gép, majd áttelepítés előre folytatásához és start hello virtuális gép biztonsági mentése az Azure Resource Manager áttelepítése után.

### <a name="pitfalls-tooavoid"></a>Nehézségek tooavoid

- Parancsikonok igénybe vehet, és ne hagyja el a hello ellenőrzése/előkészítése/megszakítási próbafuttatást áttelepítéseket.
- Legtöbb, ha nem, a potenciális problémák fog surface hello ellenőrzése/előkészítése/megszakítási lépések során.

## <a name="migration"></a>Migrálás

### <a name="technical-considerations-and-tradeoffs"></a>Technikai szempontokat és kompromisszumot

Most már készen áll, mert a környezet kapcsolatos ismert problémák hello keresztül dolgozott.

Hello valós áttelepítések érdemes lehet tooconsider:

1. Tervezze meg, és ütemezés szerinti hello virtuális hálózat (áttelepítés a legkisebb egység) prioritás növelése.  Először hello egyszerű virtuális hálózatok, és további hello a folyamat bonyolult virtuális hálózatok.
2. A legtöbb ügyfél nem éles és éles környezetben fog rendelkezni.  Éles utolsó ütemezni.
3. **(VÁLASZTHATÓ)**  Tervezze egy karbantartási bőven puffer abban az esetben, ha váratlan problémák merülhetnek fel.
4. Kommunikálni, és a támogatási csoportokkal igazodnak, abban az esetben, ha problémák merülnek fel.

### <a name="patterns-of-success"></a>Sikeres mintái

hello műszaki útmutató a Lab Test szakasz fenti hello érdemes figyelembe venni, és a korábbi tooa valós áttelepítési problémák elhárításáról.  A megfelelő tesztek hello áttelepítési az ténylegesen nem esemény.  Éles környezetben lehet hasznos toohave további támogatást, például egy megbízható Microsoft-partnerével vagy Microsoft Premier szolgáltatások.

### <a name="pitfalls-tooavoid"></a>Nehézségek tooavoid

Nincs teljesen tesztelése problémákat okozhat, és hello áttelepítés késleltetés.  

## <a name="beyond-migration"></a>Áttelepítési túl

### <a name="technical-considerations-and-tradeoffs"></a>Technikai szempontokat és kompromisszumot

Most, hogy az Azure Resource Manager, maximalizálja hello platform.  Olvasási hello [áttekintése Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) toofind kimenő kapcsolatos további előnyökkel is jár.

Dolgot tooconsider:

- Kötegelése hello áttelepítési más tevékenységek.  A legtöbb ügyfél opt egy alkalmazás karbantartási időszak.  Ha igen, érdemes lehet toouse az állásidő tooenable egyéb Azure Resource Manager funkciói, például titkosítást és áttelepítési tooManaged lemezek.
- Le újra hello műszaki és üzleti okokból az Azure Resource Manager; hello további szolgáltatások engedélyezése csak az Azure Resource Manager érhető el, amelyek érvényesek a tooyour környezetben.
- A környezet PaaS szolgáltatásokkal korszerűsítésére.

### <a name="patterns-of-success"></a>Sikeres mintái

Kell a most kívánja tooenable az Azure Resource Manager szolgáltatásokról, szándékos.  Sok ügyfél hello vonatkozóan az Azure környezetben alatt található:

- [Szerepköralapú hozzáférés-vezérlés](../../azure-resource-manager/resource-group-overview.md#access-control).
- [Az Azure Resource Manager-sablonok egyszerűbbé és több ellenőrzött telepítéshez](../../azure-resource-manager/resource-group-overview.md#template-deployment).
- [Címkék](../../azure-resource-manager/resource-group-using-tags.md).
- [Tevékenység-vezérlő](../../azure-resource-manager/resource-group-audit.md)
- [Erőforrás-házirendekkel](../../azure-resource-manager/resource-manager-policy.md)

### <a name="pitfalls-tooavoid"></a>Nehézségek tooavoid

Ne feledje, hogy miért használatba a klasszikus tooAzure erőforrás-kezelő áttelepítési út.  Mi volt a hello eredeti üzleti okokból? Sikerült elérni hello üzleti okokból?


## <a name="next-steps"></a>Következő lépések

* [IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a platform által támogatott áttelepítésének áttekintése](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének tervezése](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [PowerShell toomigrate IaaS erőforrásainak klasszikus tooAzure erőforrás-kezelő használata](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének védelmével kapcsolatos közösségi eszközök](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [A leggyakoribb áttelepítési hibák áttekintése](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Felülvizsgálati hello legtöbb kapcsolatos gyakori kérdések a klasszikus tooAzure erőforrás-kezelő áttelepítése IaaS-erőforrásokra](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
