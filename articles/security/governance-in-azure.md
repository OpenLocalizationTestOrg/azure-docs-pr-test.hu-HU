---
title: az Azure-ban aaaGovernance |} Microsoft Docs
description: "További információk a felhőalapú számítási szolgáltatás, amely tartalmazza a számítási példányokért széles kijelölt & szolgáltatások automatikusan toomeet hello igényeinek az alkalmazás vagy a vállalati fel és le is méretezhető."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/01/2017
ms.author: TomSh
ms.openlocfilehash: 956e82e92f4232c24069bdb79fed5315f1d6486f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="governance-in-azure"></a>Az Azure-irányítás

Tudjuk, hogy biztonsági-e a feladat hello felhőben, és hogy mennyire fontos, hogy található-e az Azure biztonsági pontos és időben információt. Hello legjobb okok toouse Azure az alkalmazások és szolgáltatások egyike annak a biztonsági eszközök és képességek széles köréről tootake előnyeit. Ezekkel az eszközökkel és képességek segítségével könnyebben lehetséges toocreate biztonságos megoldások hello biztonságos Azure platformon.

a Microsoft operations perspektívák, ez a cikk "Irányítás az Azure" nevével, amely biztosít olyan átfogó hello, és mindkét hello ügyfél Microsoft Azure-ban végrehajtott Cégirányítási vezérlők hello tömbje jobban megismerte toohelp A Microsoft Azure-ban elérhető irányítás szolgáltatások.

## <a name="azure-platform"></a>Azure-platform

Azure egy nyilvános felhőszolgáltatási platform, amely támogatja az operációs rendszerek, programozási nyelvek, keretrendszerek, eszközök, adatbázisok és eszközök széles körű kijelölés. Linux-tárolók futtatható kikötõmunkások integrálása; alkalmazások, JavaScript, Python, .NET, PHP, Java és Node.js; build vissza-végpontok az iOS, Android és Windows eszközökhöz. Nyilvános Azure felhőszolgáltatások hello azonos technológiák több millió fejlesztők és informatikai szakemberek számára már alapulnak, és megbízható támogatja.

Létrehozása vagy áttelepítése informatikai eszközök, egy nyilvános felhőszolgáltatóhoz függő az adott szervezet képességek tooprotect az alkalmazások és adatok hello szolgáltatásaival és hello vezérlők biztosítanak a felhőalapú toomanage hello biztonsága eszközök.

Azure infrastruktúrája a hello létesítmény tooapplications felhasználók millióit egyidejűleg üzemeltetéséhez készült, és egy megbízható foundation, amelyre vállalatok megfelel a biztonsági követelményeknek biztosít. Emellett Azure biztosít számos biztonsági beállítások és hello képességét toocontrol őket, hogy a biztonsági toomeet hello egyéni követelményeinek a szervezet központi telepítések szabhatja testre.

Ez a dokumentum segítenek megérteni, hogyan Azure irányítás képességek segítségével ezek a követelmények teljesítéséhez.

## <a name="abstract"></a>Absztrakt

Microsoft Azure felhőbe irányítás biztosít egy integrált naplózási és áttekintését, és hogy tanácsadást hello Azure platformon a felhasználási szervezet tanácsadási megközelítést. A Microsoft Azure felhőbe irányítás hivatkozik toohello döntéshozó folyamatainak, a feltételek és a házirendek érintett hello tervezési, architektúra, beszerzési, telepítési, műveletet és egy felhőalapú felügyeleti számítástechnikai.

Microsoft Azure felhőbe irányítás tervének toocreate, kell tootake hello személyeket, folyamatok és a meglévő technológiák jelenleg részletes pillantást, és a majd build keretrendszert, amely megkönnyíti a informatikai tooconsistently támogatja az üzleti igények ugyanakkor biztosítható a záró a felhasználók hello rugalmasságot toouse hello hatékony szolgáltatásokat a Microsoft Azure.

A dokumentum azt ismerteti, hogyan érhet el egy emelt szintű szintet, az irányítás az informatikai erőforrások a Microsoft Azure-ban. A dokumentum azt segítenek megérteni a beépített tooMicrosoft Azure hello biztonsági és irányítási szolgáltatások.

Az alábbiakban hello fő hello irányítás tárgyalt e dokumentumban szereplő:

- Házirendek, folyamatok és eljárások szerint szervezeti célok végrehajtására.

- Biztonság és a szervezet szabványoknak való megfelelés folyamatos

- Riasztások és figyelése

## <a name="implementation-of-policies-processes-and-procedures"></a>Házirendek, folyamatok és eljárások végrehajtása 

Felügyeleti szerepkörök és felelősségek toooversee végrehajtásának hello információk biztonsági házirendet, és folyamatos működésének létesített Azure között. A Microsoft Azure felügyeleti biztonsági és a megfelelő csapat (többek között a következőket harmadik felek), és elősegítsék való megfelelés biztonsági házirendek, folyamatok és szabványok belül folytonossági eljárások felügyeletéért felelős.

Az alábbiakban továbbfejlődtek hello tényezőket:

- Fiók kiépítése

- Előfizetés vezérlők

- Szerepkör alapján hozzáférés-vezérlést

- Erőforrás-kezelés

- Erőforrás-követés

- Kritikus fontosságú erőforrás-vezérlő

- API-hozzáférés tooBilling információk

- Hálózati vezérlő

## <a name="account-provisioning"></a>Fiók kiépítése

Fiók hierarchia egy fő lépés toouse és struktúra Azure-szolgáltatásokat a vállalaton belül, és hello core irányítási szerkezete meghatározása. Hello nagyvállalati szerződéssel rendelkező ügyfelek esetén az ügyfelek további tovább lehet hello környezet részlegek, fiókok, és végül előfizetések.

![Fiók kiépítése](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Ha nem rendelkezik nagyvállalati szerződéssel, érdemes lehet [Azure címkék](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) : előfizetés szintű toodefine hierarchia. Az Azure-előfizetésre hello alapvető egysége, ahol minden erőforrás található. Egyúttal meghatározza, milyen több határokon belül Azure, például a magok, erőforrások stb. Előfizetések tartalmazhat [erőforráscsoportok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), amely erőforrásai szerepelhetnek. [Az RBAC](https://docs.microsoft.com/azure/api-management/api-management-role-based-access-control) alapelvek a ezen három szinten.

Minden vállalati más, és az Azure-címkék használata esetén nem vállalati ügyfelek hello hierarchia lehetővé teszi, hogy hogyan vannak rendszerezve Azure hello vállalaton belül jelentős rugalmasságot. A Microsoft Azure erőforrásait telepítés megkezdése előtt hierarchia modell és megismerheti a számlázási, erőforrás-hozzáférés és összetettsége hello hatását.

## <a name="subscription-controls"></a>Előfizetés vezérlők

Előfizetési erőforrások használatának jelentett és számlázható hogyan szabályozza. Előfizetések a külön számlázási és a fizetési állíthatnak be. Említett korábbi egy Azure-fiókot, hogy több előfizetéssel is rendelkezhet. Előfizetések használt toodetermine hello Azure-Erőforrás kihasználtsága a vállalat több szervezeti egységek lehet.

Például, ha a vállalat informatikai, HR és Marketing szervezeti és ezen osztályok futtató különböző projekt van. Például a virtuális gépek Azure-erőforrások hello-használat alapján minden részleg ezek számlázható ennek megfelelően. Ez minden részleg hello pénzügyi szabályozhatja azt.

Azure-előfizetések létrehozása három paramétert:

- egy egyedi előfizető azonosítója

- a számlázási helye

- Rendelkezésre álló erőforrások csoportja

Egy adott, amely tartalmazza a Microsoft-fiók egy azonosító, a szolgáltatások a hitelkártya száma és hello teljes készlete Azure – bár Microsoft fogyasztás korlátok hello előfizetés típusától függően érvénybe lépteti.

Az Azure regisztrációs hierarchiák határozza meg, hogyan szolgáltatások felépítése nagyvállalati szerződéssel belül. hello vállalati portál lehetővé teszi az ügyfelek toodivide tooAzure erőforrások eléréséhez kapcsolódó nagyvállalati szerződéssel rugalmas hierarchiák testre szabható tooan szervezet egyedi igényeinek megfelelően. hello hierarchia mintát meg kell felelnie a szervezetek felügyeleti és földrajzi struktúra, hogy hello tartozó számlázási és erőforrás-hozzáférés pontosan számítani lehet.

hello három magas szintű minták működési, üzleti egység, és olyan felügyeleti szerkezetet a földrajzi, a szervezeti egységek fiók csoportosításokban. Minden részleg fiókokhoz rendelhetők hozzá előfizetések, amelyek a számlázási és számos kulcsfontosságú korlátok silók létrehozása az Azure (például virtuális gépek számát storage-fiókok stb.).

![Előfizetés vezérlők](./media/governance-in-azure/security-governance-in-azure-fig2.png)


A nagyvállalati szerződéssel rendelkező szervezetek számára Azure-előfizetések hierarchiája egy negyedik szintű:

- Vállalati alkalmazásbeléptetési rendszergazda

- szervezeti egység rendszergazdája

- fiók tulajdonosának

- Szolgáltatás-rendszergazda

Ezt a hierarchiát irányító hello következő:

- Számlázási kapcsolatban

- Felügyeleti fiók

- Szerepkör alapú hozzáférés-vezérlés (RBAC) tooartifacts

- Határok/korlátok

- Határok

  - Használati és elszámolási (díjszabási kártyát ajánlat számok alapján)

  - Korlátok

  - Virtual Network

- Too1 AAD csatlakoztatott (1 AAD társítható sok előfizetések)

- Kapcsolódó tooan vállalati beléptetési fiók

## <a name="role-based-access-controls"></a>Szerepköralapú hozzáférés-vezérlést

Azure jelent, ha hozzáférést vezérlők tooa előfizetés voltak: rendszergazdának vagy társadminisztrátornak. Hozzáférés az tooa előfizetéshez hello Klasszikus modell hallgatólagos tooall hello az Azure-erőforrások hello portálon. Ez részletesebb vezérlést hiánya egy Azure regisztrálásra előfizetések tooprovide egy szint a megfelelő hozzáférés-vezérlés a toohello elterjedése vezetett.

![Szerepköralapú hozzáférés-vezérlést](./media/governance-in-azure/security-governance-in-azure-fig3.png)

Ez a előfizetések elterjedése már nem szükséges. Szerepköralapú hozzáférés-vezérlés, a felhasználók toostandard szerepköröket rendelhet (például közös "olvasó" és "író" típusok szerepkörök). Egyéni szerepkörök is meghatározhat.

[Azure szerepköralapú hozzáférés-vezérlés (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) lehetővé teszi a részletes hozzáféréskezelést az Azure-bA. Az RBAC használata, biztosíthat csak hello mértékű hozzáférést a felhasználóknak frissíteniük kell tooperform a munkájukat. A vállalat biztonsági célú hello pontos engedélyeket szükségük ad az alkalmazottak kell összpontosítania. Túl sok engedélyek egy fiók tooattackers tenni. Túl kevés engedélyek jelenti azt, hogy az alkalmazottak nem munkavégzéséhez hatékony. Azure szerepköralapú hozzáférés-vezérlés (RBAC) segít a részletes hozzáféréskezelést az Azure felajánlásával oldja meg a problémát. Segít az RBAC toosegregate azon belül a csapat és a támogatás csak hello hozzáférés toousers, hogy be kell tooperform a munkájukat. Jogosultságot ad a Mindenki helyett korlátozás engedélyek az Ön Azure-előfizetés vagy erőforrásokhoz, engedélyezheti a csak bizonyos műveleteket.

Például használja az RBAC toolet egy alkalmazott egy előfizetésben található virtuális gépek kezeléséhez, miközben egy másik kezelhető SQL-adatbázisok hello belül azonos előfizetéssel.

Az Azure RBAC három alapvető szerepkörökhöz tooall erőforrástípusok rendelkezik:

- **Tulajdonos** rendelkezik teljes körű hozzáférési tooall erőforrások, például az hello jobb toodelegate hozzáférés tooothers.

- **A közreműködői** is létrehozása és kezelése az Azure-erőforrások minden típusú, de nem tudja engedélyezni a hozzáférést tooothers.

- **Olvasó** megtekintheti a meglévő Azure-erőforrások.

hello rest hello RBAC-szerepkörök az Azure-ban az adott Azure-erőforrások felügyelet lehetővé tételéhez. Virtuális gép közreműködő szerepkört hello például lehetővé teszi, hogy a felhasználó toocreate hello, és virtuális gépeket. Azt nem kapnak hozzáférést toohello virtuális hálózat vagy, amely a virtuális gép hello hello alhálózathoz csatlakozik.

[Beépített RBAC-szerepkörök](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) listák hello szerepkörök az Azure-ban érhető el. Hello műveletek és, hogy minden beépített szerepkörök toousers hatókört határoz meg.

Adjon hozzáférést rendel hello megfelelő RBAC szerepkör toousers, csoportok és alkalmazások egy adott hatókörben. szerepkör-hozzárendelés hello hatóköre lehet előfizetés, egy erőforráscsoport vagy egy erőforrást. Egy szülő hatókörben szerepkörrel is hozzáférést biztosít az abban szereplő toohello gyermekei.

Például tooa erőforráscsoport hozzáféréssel rendelkező felhasználó az összes hello erőforrást tartalmaz, például webhelyekhez, virtuális gépek és alhálózatok kezelheti.

Az Azure RBAC csak támogatja a felügyeleti műveleteket, az Azure-erőforrások hello hello Azure-portál és az Azure Resource Manager API-k. Nem lehet engedélyezni, az összes adat szintű műveletet Azure-erőforrások. Például valaki a BLOB Storage-fiókok toomanage, de nem toohello vagy egy Tárfiókon belül táblák nem engedélyezheti. Hasonlóképpen SQL-adatbázis kezelheti, de nem hello táblák belül.

Ha további részleteket szeretne arról, hogy hogyan segít az RBAC a hozzáférések kezelésében, tekintse meg [a szerepköralapú hozzáférés-vezérlést](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) ismertető szakaszt.

Emellett [hozzon létre egy egyéni biztonsági szerepkört](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) a átruházásához hozzáférés-vezérlés (RBAC) hogy hello beépített szerepkörök egyike sem felel meg a kívánt hozzáférés szükséges. Egyéni szerepkörök segítségével hozhatók létre [Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell), [Azure parancssori felület (CLI)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli), és hello [REST API](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-rest). Hasonlóan a beépített szerepkörök egyéni szerepkörök rendelhetők hozzá toousers, csoportok és alkalmazások előfizetés, erőforráscsoport és erőforrás-hatókörök.

Minden előfizetésen belül biztosíthat a szerepkör-hozzárendelések too2000 fel.

## <a name="resource-management"></a>Erőforrás-kezelés

Azure eredetileg csak a hello klasszikus üzembe helyezési modellben megadott. Ebben a modellben minden erőforrás már létezett a egymástól függetlenül; nem lehetett toogroup kapcsolódó erőforrások együtt. Ehelyett toomanually nyomon követése áll a megoldás vagy az alkalmazás mely erőforrásokat, és ne feledje toomanage kellett a összehangolt őket.

toodeploy megoldást, tooeither hozzon létre az egyes erőforrások külön-külön hello klasszikus portálon keresztül, vagy hozzon létre olyan parancsfájlt, amely minden hello erőforrások hello megfelelő sorrendben telepítve volt. toodelete megoldást, kellett toodelete az egyes erőforrások külön-külön. Akkor lehetett könnyen nem vonatkoznak, és kapcsolódó erőforrások hozzáférés-vezérlési házirendek frissítése. Végül akkor nem alkalmazható a következő címkék tooresources toolabel azokat a feltételeket, amelyek segítenek az erőforrások figyelése és kezelése.

A 2014-re az Azure erőforrás-kezelő, amely erőforráscsoport hello fogalma hozzá vezette be. Erőforráscsoport egy olyan tároló, amelyek egy közös életciklussal erőforrások. hello Resource Manager üzembe helyezési modellben számos előnyt kínál:

- Központi telepítése, kezelése és figyelése a megoldás összes hello szolgáltatást egy csoportot, hanem külön-külön kezelése ezeket a szolgáltatásokat.

- Ismételten a teljes életciklus megoldás üzembe helyezése, és lehet abban, hogy az erőforrások telepítése konzisztens lesz.

- Access control tooall erőforrások az erőforráscsoportban alkalmazhat, és ezek a házirendek automatikusan alkalmazzák, amikor új erőforrásokat ad toohello erőforráscsoport.

- Címkékkel láthatja tooresources toologically rendszerezése összes hello erőforrást az előfizetésében.

- A megoldás JavaScript Object Notation (JSON) toodefine hello infrastruktúra is használhatja. a Resource Manager sablonként ismert hello JSON-fájlt.

- Megadhatja, hogy hello függőségek között erőforrásokat, hogy azok hello megfelelő sorrendben legyenek telepítve.

![Erőforrás-kezelés](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Erőforrás-kezelő lehetővé teszi tooput erőforrások kezelése, számlázási vagy természetes affinitás jelentéssel bíró csoportokba. Amint azt korábban említettük, az Azure két üzembe helyezési modellel rendelkezik. A korábbi hello [Klasszikus modell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model), felügyeleti hello alapvető egysége hello előfizetés volt. Nehéz toobreak előfizetés, amely a kereslet az olyan előfizetések nagy mennyiségű toohello létrehozását erőforrások le volt. Hello Resource Manager modellt látott erőforráscsoportok hello bevezetése.

Egy erőforráscsoport egy olyan tároló, amely egy Azure megoldás kapcsolódó erőforrásokat tárol. [hello erőforráscsoport](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) hello megoldás összes hello erőforrásait, és csak azokat az erőforrásokat, amelyet az egy csoportként toomanage tartalmazhatnak. Úgy dönt, hogy hogyan tooallocate erőforrások tooresource csoportok alapján hasznossá hello a legtöbb, a szervezet számára legjobb.

A sablonokra vonatkozó javaslatokat talál a [Best practices for creating Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) (Az Azure Resource Manager-sablonok létrehozásának ajánlott eljárásai) című cikkben.

Az Azure Resource Manager tooensure erőforrások jönnek létre a megfelelő sorrendben hello függőségek elemzésével. Ha egy erőforrás egy másik erőforráshoz tartozó értéket használ fel (például egy virtuális gép, amely egy tárfiókot igényel a lemezekhez), akkor beállíthat egy függőséget.

>[!Note]
>További információ: [Függőségek meghatározása az Azure Resource Manager sablonokban](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies).

Hello sablon frissítések toohello infrastruktúra is használható. Például erőforrás tooyour megoldás hozzáadása, és hozzáadhat konfigurációs szabályokat a már telepített hello erőforrások. Ha hello sablont létrehoznia egy erőforrást, de az erőforrás már létezik, az Azure Resource Manager egy új eszköz létrehozása helyett frissítést hajt végre. Az Azure Resource Manager frissítések hello meglévő eszköz toohello azonos, mert állapot mintha új lenne.

Erőforrás-kezelő bővítményeket biztosít olyan forgatókönyvek amikor további műveletek, például a Szoftvertelepítés, amely nem része a hello beállítása van szüksége.

## <a name="resource-tracking"></a>Erőforrás-követés

Mivel a szervezet felhasználói erőforrások toohello előfizetés hozzáadásához egyre fontosabb tooassociate erőforrások hello megfelelő részleg, az ügyfél és a környezet válik. Metaadatok tooresources címkék használatával csatolható. Használhat [címkék](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) hello erőforrás vagy hello tulajdonos tooprovide információt. Címkék lehetővé teszik a csak összesített toonot és erőforrások többféle módon, de az adatokat a jóváírási hello alkalmazásában.

Címkék használata, ha összetett erőforráscsoport-sablonok és erőforrások több tartalmaz, és kell toovisualize hello módon hello legtöbb logika tooyou az eszközöket. Például elláthat címkével olyan erőforrásokat, amelyek hasonló szerepet töltenek be a szervezet vagy toohello tartozik részleghez.

Nélkül hozhat létre a címkék, a szervezet felhasználói több erőforrást, amely lehet, hogy nehéz toolater azonosításához és kezeléséhez. Például előfordulhat, hogy kívánja toodelete projekt összes hello erőforrást. Ha a források nem hello projekt címkével rendelkeznek, manuálisan találnia őket. Címkézés lehet egy módja a felesleges költségek tooreduce az előfizetésben.

Az erőforrásoknak nem kell a hello tooreside azonos erőforrás csoport tooshare egy címkét. Létrehozhat saját címke besorolás tooensure, hogy a szervezete összes felhasználója közös címkék helyett használjon felhasználók ne használjanak véletlenül némileg eltérő címkéket (például "részleg" helyett "osztály").

Erőforrás-házirendek lehetővé teszik a szervezet alapértelmezett szabályai toocreate. Amelyek biztosítják az erőforrások címkézett hello megfelelő értékekkel házirendeket is létrehozhat.

> [!Note]
> További információkért lásd: [címkék erőforrás házirendek alkalmazását](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags).

Címkézett erőforrásoknak hello Azure-portálon is megtekintheti.

Hello [használati jelentés](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) számára az előfizetés tartalmazza címke neveit és értékeit, ami lehetővé teszi a címkék alapján toobreak kimenő költségek.

> [!Note]
> Címkékkel kapcsolatos további információkért lásd: [Using címkéket tooorganize az Azure-erőforrások](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags).

a következő korlátozások hello tootags vonatkoznak:

- Minden erőforrás vagy erőforráscsoport legfeljebb 15 címke kulcs/érték párok rendelkezhet. Ez a korlátozás csak akkor érvényes, közvetlenül alkalmazható tootags toohello erőforráscsoportba vagy erőforrás. Erőforráscsoport 15 címke kulcs/érték párok tartalmazó sok erőforrást tartalmazhat.

- hello címke név hossza legfeljebb too512 karaktert.

- hello címke értéke korlátozott too256 karaktereket.

- Az erőforráscsoport hello erőforrások nem örökli alkalmazott címkék toohello erőforráscsoportot.

Ha több mint 15 értékeket, hogy kell-e tooassociate erőforrással, használja a JSON karakterláncnak hello címke értéke. hello JSON-karakterláncban sok alkalmazott tooa egyetlen címke kulcs értékeket tartalmazhat.

### <a name="tags-and-billing"></a>Címkék és a számlázás

Címkék lehetővé teszik az Ön toogroup a számlázási adatok. Például ha a másik szervezet számára több virtuális gép fut, használja hello címkék toogroup használati költségközpont által. Is használhatja címkék toocategorize költségek futtatókörnyezetben; például a hello számlázási használati éles környezetben futó virtuális gépeket.

Visszaállíthatja az keresztül hello címkékkel kapcsolatos információkat is [Azure erőforrás-használat és RateCard API-k](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) vagy hello használati vesszővel tagolt (CSV) fájl. Hello használati fájl letöltését hello [fiókok az Azure portál](https://account.windowsazure.com/) vagy [EA portal](https://ea.azure.com/).

>[!Note]
> Programozott hozzáférés toobilling információk kapcsolatos további információkért lásd: [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). A REST API-műveleteket, lásd: [Azure számlázási REST API-referencia](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Hello használata CSV számlázási címkék támogató szolgáltatások letöltésekor hello címkék hello címkék oszlopban jelennek meg.

## <a name="critical-resource-controls"></a>Kritikus fontosságú erőforrás-vezérlők

Mivel a szervezet alapvető szolgáltatások toohello előfizetés hozzáadása, egyre fontosabb, hogy ezek a szolgáltatások legyenek-e a rendelkezésre álló tooavoid üzleti megszűnésének tooensure válik. [Erőforrás zárolások](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) lehetővé teszik a toorestrict műveletek az értékes erőforrások ahol módosítsa vagy törölje azokat az alkalmazások és a felhőalapú infrastruktúra jelentős hatással van. Zárolások tooa előfizetés, az erőforráscsoportot, vagy az erőforrás is alkalmazhatja. Általában a zárolások toofoundational erőforrások, például a virtuális hálózatok, az átjárók és a storage-fiókok telepítését.

Erőforrás zárolások jelenleg két értéket támogatja: CanNotDelete és csak olvasható. CanNotDelete azt jelenti, hogy a felhasználói (hello megfelelő jogosultságokkal) is még olvasni vagy módosítani az erőforrást, de nem lehet törölni. Csak olvasható azt jelenti, hogy a jogosult felhasználók nem lehet törölni vagy módosítani az erőforrást.

Erőforrás-kezelő zárolások alkalmazása csak a fordul elő hello felügyeleti vezérlősík, amely túl elküldött műveletek toooperations<https://management.azure.com>. hello zárolások korlátozza, hogy milyen erőforrásokat a saját funkció végrehajtására. Erőforrás módosítások korlátozott, de erőforrás műveletek nem korlátozottak. Például egy SQL-adatbázis csak olvasható zárolást megakadályozza, hogy törli vagy módosítása hello adatbázis, de nem akadályozza meg a létrehozása, frissítése vagy törlése hello adatbázis adatait.

Alkalmazása **ReadOnly** toounexpected eredmények vezethet, mivel bizonyos műveleteket, amelyek az adatok, például olvasási műveletek további műveleteket igényelnek. Például, ha kialakít egy **ReadOnly** zárolását egy tárfiókot minden felhasználót megakadályoz hello kulcsainak listázása. hello listában kulcsok művelet segítségével egy POST kérést kezel, mert hello vissza kulcsok elérhetők az írási műveletek.

![Kritikus fontosságú erőforrás-vezérlők](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Például egy App Service-erőforrást egy csak olvasható zárolási helyezését megakadályozza, hogy a Visual Studio Server Explorer hello erőforrás fájl jelenik meg, mert az adott interakció írási hozzáféréssel kell rendelkeznie.

Szerepköralapú hozzáférés-vezérlés, ellentétben a felügyeleti zárolás tooapply korlátozás összes felhasználók és szerepkörök használhat. toolearn vonatkozó engedélyek beállítása a felhasználók és szerepkörök, lásd: [Azure szerepköralapú hozzáférés-vezérlés](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

A szülő hatókörben zárolást alkalmazásakor hatókörön belüli összes erőforrás öröklése hello azonos zárolása. Később fel még akkor is, erőforrások örökölt hello zárolási hello. hello szigorúbb zárolást hello öröklési lép érvénybe.

toocreate vagy delete felügyeleti zárolás, rendelkeznie kell hozzáféréssel tooMicrosoft.Authorization/ _vagy Microsoft.Authorization/locks/_ műveletek. Hello beépített szerepkörök, csak **tulajdonos** és **felhasználói hozzáférés adminisztrátora** kapnak a csatolási műveleteket.

## <a name="api-access-toobilling-information"></a>API-hozzáférés toobilling információk

Azure számlázási API-k toopull használati és erőforrás-adatok használja azokat az elsődleges adatok elemzésére szolgáló eszközöket. hello Azure erőforrás-használat és RateCard API-k segítségével pontosan előre jelezni, és a költségek kezelésére. hello API-k által hello Azure Resource Manager API-k hello operációsrendszer-család része és erőforrás-szolgáltató valósíthatók meg.

### <a name="azure-resource-usage-api-preview"></a>Az Azure erőforrás-használat API (előzetes verzió)

Használjon hello Azure [erőforrás-használati API](https://msdn.microsoft.com/library/azure/mt219003) tooget a becsült Azure fogyasztási adatokhoz. hello API tartalmazza:

- **Szerepköralapú hozzáférés-vezérlés az Azure** -hozzáférés konfigurálása hello házirendek [Azure-portálon](https://portal.azure.com/) vagy [Azure PowerShell-parancsmagok](https://docs.microsoft.com/powershell/azure/overview) mely felhasználók vagy alkalmazások ingyenesen juthatnak az toospecify toohello előfizetés használati adatok. Hívóknak kell használnia szabványos Azure Active Directory-jogkivonatokat a hitelesítéshez. Hello hívó tooeither hello számlázási olvasó, olvasó, tulajdonosi vagy közreműködői szerepkör tooget hozzáférés toohello használati adatok hozzáadása egy adott Azure-előfizetéséhez.

- **Óránkénti vagy a napi aggregáció** - hívóknak adhat meg, hogy azok szeretné-e az Azure használati adatokat óránkénti időszakok vagy napi időszakok. hello alapértelmezett érték a napi.

- **(Erőforrás-címkét tartalmaz) példány metaadatok** – például teljesen minősített erőforrás uri hello példányszintű részletek beszerzése (/subscriptions/ {előfizetés-azonosító} /..), erőforrás csoport adatai, és az erőforráscímkék hello. A metaadatok deterministically segít, és programokon keresztül oszt használati által hello címkék, például a kereszt-díjszabási használati eseteket.

- **Erőforrás-metaadatok** -erőforrás részletei például hello mérő nevét, a mérési kategória, a mérő az alkategória, a egység és a régió adjon hello hívó szeretné jobban megismerni mi felhasznált. Tooalign erőforrás metaadatok terminológia hello Azure-portál, Azure használati CSV, EA CSV, számlázási és egyéb nyilvánosan elérhető lép, akkor összefüggéseket adatok között lép toolet is dolgozunk.

- **Minden használati kínálnak típusok** – használati adatok érhető el az összes kínálnak, például a használatalapú fizetés, MSDN, pénzügyi kötelezettségvállalást, pénzügyi kreditet és EA.

**Azure-erőforrás RateCard API (előzetes verzió)**

Használjon hello Azure Resource RateCard API tooget hello elérhető Azure-erőforrások listája, és minden árakról becsült. hello API tartalmazza:

- **Szerepköralapú hozzáférés-vezérlés az Azure** – hello Azure-portálon a hozzáférési házirendek konfigurálásához, vagy Azure PowerShell parancsmagok toospecify, amely a felhasználók vagy alkalmazások kérheti le a hozzáférési toohello RateCard adatokat. Hívóknak kell használnia szabványos Azure Active Directory-jogkivonatokat a hitelesítéshez. Hello hívó tooeither hello olvasó, a tulajdonos vagy közreműködő szerepkört tooget hozzáférés toohello használati adatok hozzáadása egy adott Azure-előfizetés.

- **Használatalapú fizetés, az MSDN, a pénzügyi kötelezettségvállalást a és a pénzügyi kreditet biztosít a (EA nem támogatott)** -Ez az API felület Azure ajánlat szintű arány információkat nyújt. Ez az API felület védőfalkezelőbe hello hello ajánlat tooget erőforrás adatai és a díjszabás meg kell felelnie. Mert EA ajánlatok testreszabott beléptetési mértékek a rendszer jelenleg nem tudja tooprovide EA sebességét. Az alábbiakban néhány lehetséges hello használati és hello RateCard API-k hello kombinációjával rendelkező végrehajtott hello forgatókönyv:

- **Azure töltött hello hónapban** -hello használati és RateCard API-k tooget jobb betekintést a felhő használatát hello együttes töltenek hello hónap során. Hello óránkénti elemezheti és használati és kell fizetni napi gyűjtők becslése.

- **Riasztások beállítása** – hello használata, illetve hello RateCard API-k tooget felhő használat és díjak becsült, és erőforrás-alapú vagy pénzügyi-alapú értesítések beállítása.

- **Számlázási előrejelzése** – Get a becsült felhasználás és a felhő kiadásokat, és gépi tanulási algoritmusok toopredict milyen hello számlázási számlázási ciklus hello hello végén lehet alkalmazni.

- **Elemzés előtti fogyasztás** – hello RateCard API toopredict mekkora a számlázási okozna a várható használati helyezi át a munkaterheléseket tooAzure használja. Ha meglévő alkalmazások más felhők vagy a privát felhők, is hozzárendelheti a felhasználást a hello Azure díjszabás tooget Azure jobb becslése televíziózással töltenek. Ez a becslés lehetőséget biztosít hello képességét toopivot kínálnak, és közötti hello különböző típusú túl használatalapú fizetés, hasonlítsa össze, például pénzügyi kötelezettségvállalást a és a pénzügyi kreditet. hello API is lehetővé teszi az hello képességét toosee költség régiónként különbségek attól függnek, és lehetővé teszi egy központi telepítési döntések lehetőségelemzések költség elemzés toohelp toodo.

- **Elemzési** -meghatározhatja, hogy-e további költséghatékony toorun munkaterhelések egy másik régióban, vagy egy másik hello Azure-erőforrás-konfigurációban. Hello használata az Azure-régió alapján Azure-erőforrás költségek eltérőek lehetnek.

- Azt is meghatározhatja, hogy ha egy másik Azure-ajánlat típusa a nagyobb mértékben nyújt egy Azure-erőforrás.

## <a name="networking-controls"></a>Hálózati vezérlő

Lehet, hogy hozzáférési tooresources (belül hello hálózatán) belső vagy külső (keresztül hello internet). A szervezet tooinadvertently put hello helytelen helyszíni erőforrások a felhasználók könnyen, és potenciálisan Megnyitásukhoz toomalicious hozzáférést. Például a helyszíni eszközök, a vállalatok kell hozzáadnia, hogy a felhasználók Azure döntéseket hello jobb megfelelő vezérlők tooensure /.

![Hálózati vezérlő](./media/governance-in-azure/security-governance-in-azure-fig6.png)

A előfizetés irányítás azt határozza meg, amely biztosítja a alapvető hozzáférés-vezérlést alapvető erőforrásai. hello alapvető erőforrások foglalják magukban:

### <a name="network-connectivity"></a>Hálózati kapcsolat

[Virtuális hálózatok](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) alhálózatok tároló objektumok. Bár nem feltétlenül szükséges, gyakran használják, ha van csatlakozás alkalmazások toointernal vállalati erőforrásokhoz. hello Azure Virtual Network szolgáltatás lehetővé teszi, hogy Ön toosecurely Csatlakozás más Azure-erőforrások tooeach virtuális hálózatokról (Vnetekről).

A virtuális hálózat a saját hálózati hello felhőben megjelenítése. A virtuális hálózat hello Azure felhőben dedikált tooyour előfizetés logikai elkülönítése. Vnetek tooyour a helyszíni hálózathoz is csatlakoztathatja.

Azure virtuális hálózatok képességei a következők:

- **Elkülönítési**: Vnetek el különítve egymástól. Fejlesztési, tesztelési és éles külön Vnetek adott használata hello is létrehozhat ugyanahhoz a CIDR címblokkokat. Ezzel szemben, amelyek különböző CIDR címblokkokat és összekapcsolhatja az hálózatok több Vnetek is létrehozhat. Egy VNet érdekében több alhálózatra is szegmentálhatja. Azure belső névfeloldást biztosít a virtuális gépek és Felhőszolgáltatások szerepkörpéldányokat csatlakoztatott tooa virtuális hálózat. Konfigurálhatja a virtuális hálózat toouse saját DNS-kiszolgálókat, az Azure belső névfeloldást használata helyett.

- **Internetkapcsolat**: minden Azure virtuális gépek (VM) és a Felhőszolgáltatások szerepkörpéldányokat csatlakoztatott tooa VNet rendelkezik hozzáférési toohello Internet, alapértelmezés szerint. Bejövő hozzáférést toospecific erőforrások, igény szerint is engedélyezheti.

- **Az Azure erőforrás-kapcsolat**: például a Cloud Services és a virtuális gépek Azure-erőforrások lehetnek csatlakoztatott toohello ugyanazt a virtuális hálózatot. hello erőforrások képesek-e csatlakozni más tooeach privát IP-címek, még akkor is, ha külön alhálózatokon vannak. Azure biztosít alapértelmezett között alhálózatok, a Vnetek és a helyszíni hálózatokhoz, így nem rendelkezik tooconfigure és útvonalak kezelése.

- **VNet-kapcsolatot**: Vnetek lehet a más csatlakoztatott tooeach, erőforrások engedélyezése tooany VNet toocommunicate kapcsolódnak bármilyen olyan erőforrás bármely más virtuális hálózaton.

- **A helyi kapcsolat**: Vnetek csatlakoztatott tooon helyi hálózatok magánhálózati kapcsolatok a hálózati és az Azure között, vagy egy pont-pont VPN-kapcsolaton keresztül lehet hello interneten keresztül.

- **Forgalomszűrést végez**: virtuális gép és a felhőalapú szolgáltatások szerepkör példányok hálózati forgalom bejövő és kimenő alapján szűrhetők forrás IP-cím és port, cél IP-cím és port és protokoll.

- **Útválasztás**: opcionálisan felülbírálhatja Azure alapértelmezett útválasztási saját útvonalak konfigurálása, vagy használja a BGP-útvonalakat hálózati átjárón keresztül.

## <a name="network-access-controls"></a>Hálózati hozzáférés-vezérlést

[Hálózati biztonsági csoportok](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) például egy tűzfal, és adjon meg szabályokat a hogyan erőforrás "működik" hello hálózaton keresztül. Adja meg az útmutató részletes szabályozását, /, ha egy alhálózat (vagy a virtuális gép) kapcsolódhatnak toohello Internet vagy a más alhálózatokon hello azonos virtuális hálózaton.

A hálózati biztonsági csoport (NSG) felsorolja azokat a szabályokat, amelyek engedélyezik vagy megtagadják a hálózati forgalom tooresources csatlakoztatott tooAzure virtuális hálózatot (VNet). NSG-ket lehet társított toosubnets, az egyes virtuális gépeken (klasszikus), vagy az egyes hálózati adapterek (NIC) kapcsolódó tooVMs (Resource Manager).

Amikor egy NSG társított tooa alhálózati, hello szabályokat alkalmazni tooall erőforrások csatlakoztatott toohello alhálózat. Forgalom tovább korlátozhatja is társításával egy NSG tooa VM vagy a hálózati adapterhez.

## <a name="security-and-continuous-compliance-with-organizational-standards"></a>Biztonság és a vállalati szabványoknak való megfelelés folyamatos

Minden üzleti különböző igények rendelkezik, és minden üzleti élvezzék rendszer által nyújtott megoldások különböző előnyeit. Továbbra is, az ügyfeleknek mindenfajta rendelkezik hello toohello felhő áthelyezését ugyanazon alapvető kétségei. Az adatok feletti ellenőrzési tooretain szeretnék, és szeretnék, hogy biztonságban marad, az összes megőrzésével átláthatóság és megfelelőségi adatokat toobe.

Mi az ügyfelek szeretné, hogy a szolgáltatók van:

- **Az adatok biztonságos** közben, hogy képes-e biztosítani hello felhő igazolása nőtt adatok biztonsági és felügyeleti szabályozást, informatikai vezetők továbbra is van szó, hogy áttelepítése toohello felhő hagyja őket a jelenlegi sebezhetőbbé toohackers belső fejlesztésű megoldások.

- **Az adatok megőrzése** titkos felhőszolgáltatások előléptetése egyedi adatvédelmi kapcsolatban felmerülő kihívások vállalatok számára. A vállalatok toohello felhő toosave meg az infrastruktúra használati díjait, és javítja a nagyon hatékony eszközök, azokat is aggódni vezérelhető, hogy az adatok tárolására, ki fér hozzá, és felhasználásáról lekérdezi.

- **Vezérlő biztosítják** akár kihasználják a hello felhő toodeploy további innovatív megoldások, vállalatok nagyon fontos az adatok feletti ellenőrzési elveszíti tartoznak. legutóbbi felfedett hello el az ügyféladatokat, mind a jogi, mind a extra jogi azt jelenti, állami intézményekhez győződjön néhány igazgatók óvatos hello felhő az adatok tárolását.

- **Átláthatóságának elősegítése** módban biztonsági, adatvédelmi és vezérlő fontos toobusiness döntéshozók, akkor is érdemes hello képességét tooindependently ellenőrizze, hogy hogyan a adatot tárol, és a biztonságos hozzáférés.

- **Megfelelőség fenntartásában** vállalatok bontsa ki a felhőalapú technológiái által nyújtott használatát, mivel a hello összetettségét és szabványok és köre tooevolve továbbra is. A vállalatok, amelyek teljesítik a megfelelőségi követelményeket, és változik, hogy a megfelelőségi szabályzat módosítása adott idő alatt, tooknow kell.

## <a name="security-configuration-monitoring-and-alerting"></a>Biztonsági beállítások, figyelés és riasztás

Az Azure-előfizetők több eszközről kezelhetik felhőkörnyezeteiket, például felügyeleti munkaállomásokról, fejlesztői PC-kről, és olyan jogosult végfelhasználói eszközökről is, amelyek feladatspecifikus engedélyekkel rendelkeznek. Bizonyos esetekben a felügyeleti funkciók webalapú konzolok például hello Azure-portálon keresztül történik. Más esetekben lehet közvetlen kapcsolatokon keresztül tooAzure a helyszíni rendszerekben a virtuális magánhálózatok (VPN), a Terminálszolgáltatások, ügyfél-alkalmazásprotokollokon, vagy (szoftveresen) hello Azure Service Management API (SMAPI) keresztül. Továbbá az ügyfél-végpontok lehetnek vagy tartományhoz csatlakoztatottak, vagy pedig elkülönítettek és felügyelet nélküliek, mint például a táblagépek vagy az okostelefonok.

Bár több hozzáférési és kezelési képesség a lehetőségek széles skáláját adja meg, a variancia jelentős kockázat tooa felhő üzembe helyezése adhat hozzá. Nehéz toomanage lehet követni, és rendszergazdai műveletek naplózhatók. A variancia is vezethet be a felhőszolgáltatások kezelésére használt ügyfélvégpontokhoz hozzáférés tooclient végpontok biztonsági fenyegetésekkel. Az általános vagy személyes munkaállomások fejlesztésre és infrastruktúra-kezelésre való használata olyan kiszámíthatatlan fenyegetési vektoroknak enged utat, mint például a webböngészés (pl. alapesetben megbízható weboldalak megfertőződése, ún. watering hole attack) vagy az e-mail (pl. pszichológiai manipuláció és adathalászat).

A megfigyelés és a naplózás biztosítják követésének és értelmezésének felügyeleti tevékenységek alapul, de nem mindig lehet megvalósítható tooaudit összes művelet befejezéséhez miatt keletkező toohello adatmennyiség részletei. Az ajánlott eljárás szerint azonban hello hello felügyeleti házirendek hatékonyságának naplózása.

Az Azure biztonsági irányítás az AD DS csoportházirend-objektumok toocontrol minden hello rendszergazda Windows felületeihez, mint például a fájlmegosztást. Terjessze ki a naplózási és megfigyelési folyamatokat a felügyeleti munkaállomásokra. Kövessen nyomon minden rendszergazdai és fejlesztői hozzáférést és tevékenységet.

### <a name="azure-security-center"></a>Az Azure security Centerben

Hello [az Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) központi hello hello előfizetések erőforrások biztonsági állapotát jeleníti meg, és ajánlásokat, amelyekkel megakadályozható a fertőzött erőforrásokat. Engedélyezheti, hogy a részletesebb házirendek (például alkalmazása házirendek toospecific erőforráscsoportok, amelyek lehetővé teszik a hello vállalati tootailor azok címzési előírások toohello kockázat).

![Azure Security Center](./media/governance-in-azure/security-governance-in-azure-fig7.png)

A Security Center itt integrált biztonsági monitorozást és kezelése az Azure-előfizetések között, megkönnyíti a nehezen észlelhető fenyegetések azonosítását szabályzatkezelést, és számos biztonsági megoldással együttműködik. Miután engedélyezte a [biztonsági házirendek](https://docs.microsoft.com/azure/security-center/security-center-policies) az előfizetéshez tartozó erőforrásokra, a Security Center elemzi az erőforrások tooidentify potenciális biztonsági réseket hello biztonságát. A hálózati konfigurációval kapcsolatos információk azonnal elérhetők.

Az Azure Security Center ajánlott eljárás elemzési és biztonsági házirendek kezelése Azure-előfizetés belül az összes erőforrás kombinációját jelenti. Az egyszerű és hatékony toouse eszköz lehetővé teszi, hogy a biztonsági csoportok és a kockázati tisztviselő tooprevent, észlelésében és kezelésében toosecurity fenyegetéseket, automatikusan gyűjti és elemzi az Azure-erőforrások, a hello hálózati és a partneri megoldások, például biztonsági adatait kártevőirtó programok és tűzfalak.

Ezenkívül az Azure Security Center bővített analitikát alkalmaz, beleértve a gépi tanulási és viselkedési elemzési során, ami a Microsoft termékei és szolgáltatásai hello Microsoft Digital Crimes Unit (DCU), hello Microsoft globális fenyegetésfelderítési adataival Biztonsági válasz Center (MSRC), valamint külső hírcsatornák. [Biztonsági irányítás](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) körben alkalmazható hello előfizetés szintjén vagy leszűkítheti toospecific, alkalmazza a részletes követelményeket tooindividual erőforrások házirenddefiníción keresztül.

Végül az Azure Security Center erőforrás biztonsági állapota ezek a házirendek alapján elemzi, és használja a tooprovide osztályon irányítópultok és a riasztás esemény, például a kártevő-észlelési vagy kártékony IP-kapcsolatot kísérletek.

>[!Note]
> További információ tooapply ajánlásokat, olvassa el [végrehajtási biztonsági javaslatok az Azure Security Centerben](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

A Security Center adatokat gyűjti össze a virtuális gépek tooassess biztonsági állapotukra, adja meg a biztonsági javaslatok és riasztást küldjön, toothreats. A Security Center első megnyitásakor adatgyűjtés engedélyezett az előfizetésében szereplő összes virtuális gépet. Adatgyűjtés ajánlott azonban akkor is lemondáshoz által [adatok gyűjtésének letiltása](https://docs.microsoft.com/azure/security-center/security-center-faq) a Security Center házirend hello.

Végül az Azure Security Center nyitott platform, amely lehetővé teszi a Microsoft-partnereknek, és független szállítók toocreate szoftver csatlakozik az Azure Security Center tooenhance képességeit.

Az Azure Security Center figyelők hello Azure-erőforrások a következő:

- Virtuális gépek (VM) (beleértve a Cloud Services)

- Azure virtuális hálózatok

- Az Azure SQL-szolgáltatás

- Webalkalmazási tűzfal virtuális gépeken és a például az Azure-előfizetésében integrált partnermegoldások [App Service Environment-környezet](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme).

### <a name="operations-management-suite"></a>Operations Management Suite

hello OMS szoftverfejlesztői és szolgáltatási csoport információ-biztonság és [cégirányítási program](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) támogatja az üzleti követelmények és toolaws és a szabályozásoknak megfelelő, részben ismertetett módon [Microsoft Azure Trust Center ](https://azure.microsoft.com/support/trust-center/) és [Microsoft adatvédelmi központ megfelelőségi](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). Hogyan OMS biztonsági követelmények meghatározásához, biztonsági vezérlők azonosítja, kezeli, és figyeli a kockázatok is létezik ismerteti. Évente, azt felülvizsgálati házirendeket, szabványokat, eljárásokra és útmutatást.

Minden egyes OMS fejlesztési csoport egy tagja megkapja a formális alkalmazás biztonsági képzés. A verziókezelő rendszer belsőleg, szoftverek fejlesztésére használjuk. Minden szoftver projekt hello verziókezelő rendszer által védett.

A Microsoft hogyan felügyeli, és értékeli az összes szolgáltatás a Microsoft biztonsági és megfelelőségi csoport rendelkezik. Biztonsági informatikusok hello team alkotják, és nincs társítva a műszaki osztály, amely a fejlesztés OMS részlegek hello. hello biztonsági tisztviselő saját felügyeleti lánc rendelkezik, és végezze el a termékek és szolgáltatások tooensure biztonsági és megfelelőségi független értékelése.

Az Operations Management Suite (OMS) hello felhőben hello indítás tervezett szolgáltatások gyűjteménye. Ahelyett, hogy telepíti, és a helyi erőforrások kezelése, OMS-összetevők teljes mértékben az Azure-ban tárolja. Minimális konfigurációt igényelnek, és akár percek alatt használatba vehetők.

![Az Operations Manager Suite](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Mivel a OMS-szolgáltatások futnak hello felhő nem jelenti azt, hogy azokat ténylegesen nem tudja kezelni a helyszíni környezetben.

Ügynök helyezhető el a Windows vagy Linux rendszerű számítógép az adatközpont, és elküld elemzés, ahol elemzése egyéb adatokkal együtt a felhőből vagy a helyi szolgáltatások gyűjtött adatok tooLog. Azure biztonsági mentési és az Azure Site Recovery tooleverage hello felhő használja a biztonsági mentési és magas rendelkezésre álláshoz tartozó a helyi erőforrásokat.

Runbookokat hello felhőben általában nem tud hozzáférni a helyszíni erőforrásokat, de egy vagy több számítógépet az ügynök telepíthető, amely túl runbookokat az Adatközpont fogja futtatni. Amikor elindít egy runbookot, akkor egyszerűen adja meg, hogy azt toorun hello felhő, vagy a helyi munkavégző.

az Azure-ban futó szolgáltatások OMS hello alapvető funkcióit biztosítja. Minden szolgáltatás egy adott felügyeleti funkciót biztosít, és kombinálhatja services tooachieve különböző felügyeleti lehetőségeket.

![Az Operations Manager Suite](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Azure-művelet manager bővíti a Funkciók, adja meg a felügyeleti megoldásokra. [Megoldások](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) logika előre csomagolt csoportja egy hasznosítja ki egy vagy több OMS-szolgáltatások felügyeleti forgatókönyv megvalósításához.

![Azure-művelet kezelése](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Más megoldások érhetők el, a Microsoft és a partnerek, hogy egyszerűen hozzáadhatja tooyour Azure-előfizetés tooincrease hello a befektetés értékét az OMS Szolgáltatáshoz.

Partnerként hozzon létre egy saját megoldások toosupport, az alkalmazások és szolgáltatások, és biztosítani nekik toousers keresztül hello Azure piactér vagy gyors üzembe helyezési sablonokat.

## <a name="performance-alerting-and-monitoring"></a>Teljesítményriasztások és figyelése

### <a name="alerting"></a>Riasztások kezelése

A riasztások jelezhetik az Azure-erőforrás metrikáit, eseményeket és naplók figyelésére szolgáló módszer, és értesítést kapjanak, ha a megadott feltétel teljesül.

**Riasztások a különböző Azure-szolgáltatások**

Különböző szolgáltatások, beleértve a különböző riasztások érhetők el:

- Az Application Insights: Lehetővé teszi a webalkalmazás-teszt és metrika riasztásokat.

>[!Note]
> Lásd: [riasztások beállítása a Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) és [figyelése rendelkezésre állásának és reakcióidőt, a webhely](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- Naplóelemzési (Operations Management Suite): Lehetővé teszi, hogy a tevékenység és a diagnosztikai naplók tooLog Analytics útválasztását hello. Az Operations Management Suite metrika, a napló és a más riasztástípusok ettől teszi lehetővé.

>[!Note]
> További információkért lásd: a riasztások [Naplóelemzési](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- Az Azure figyelő: Lehetővé teszi, hogy a riasztások metrika értékek és a tevékenység alkalmazásnapló-események alapján. Használhatja a hello [Azure figyelő REST API](https://msdn.microsoft.com/library/dn931943.aspx) toomanage riasztásokat.

>[!Note]
> További információkért lásd: [hello Azure-portálon, a PowerShell vagy a hello parancssori felület toocreate riasztások használatával](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>Figyelés

A cloud app teljesítményével kapcsolatos problémákat hatással lehet az üzleti. A több összekapcsolt összetevőkkel és gyakori kiadásokban degradations bármikor fordulhat elő. És ha az alkalmazást, a felhasználók általában felderítése a tesztjei során nem talált problémákat. Meg kell ezekről a problémákról értesülnek azonnal, eszközöket, és diagnosztizálására és hello problémák elhárításához. A Microsoft Azure tartománnyal rendelkező eszközök a problémák azonosításához.

**Hogyan figyelhetek az Azure felhőalapú alkalmazásokat?**

Nincs számos eszközt az Azure-alkalmazások és szolgáltatások figyelését. A funkciók némelyike átfedésben vannak. Ez egy korábbi okokból részben és részben miatt toohello fellazítja a fejlesztési és a művelet az alkalmazások között.

Az alábbiakban hello egyszerű eszközök:

- **Az Azure figyelő** alapvető eszköz Azure-on futó szolgáltatások figyelése. Biztosít egy szolgáltatás és a környezet körülvevő hello hello átviteli infrastruktúra szintű adatait. Ha kezeli az alkalmazások az összes Azure-ban, annak eldöntése, hogy tooscale felfelé vagy lefelé erőforrásokat, majd Azure a figyelő lehetővé teszi az valóban használt funkciókért toostart.

- **Az Application Insights** fejlesztési és egy üzemi felügyeleti megoldás is használható. Működését tekintve a csomag telepítése az alkalmazásba, és így lehetővé teszi több belső nézetét jelenít meg. Az adatok közé tartoznak a függőségi, kivétel nyomkövetési adatok, pillanatképeket, végrehajtási profilok hibakeresés válaszidejét. Az hatékony intelligens olyan eszközöket biztosít a telemetriai adatok elemzéséhez mindkét egy alkalmazás és megismerte a felhasználók tevékenységeit vele toohelp debug toohelp. Megállapítható, hogy egy csúcs az igények válaszidők esedékes toosomething egy alkalmazást vagy egy külső resourcing probléma. Ha a Visual Studio használata és hello alkalmazás hibás, akkor átvihető jobb toohello probléma sor kód, hogy kijavíthassuk.

- **Naplófájl Analytics** éles környezetben futó alkalmazások teljesítmény- és terv karbantartási tootune igény van. Az Azure-ban alapul. Gyűjti, és számos más forrásból, bár a késéssel too15 10 perc összesíti. Körű informatikai felügyeleti megoldást nyújt az Azure-ba, a helyszíni és a külső felhőalapú infrastruktúra (például az Amazon Web Services). Az tooanalyze adatok gazdagabb eszközöket biztosít több forrás, lehetővé teszi összetett lekérdezések összes napló és a megadott feltételek proaktív riasztást küldhet. Egyéni adatok azokat a központi tárházához lekérdezhetik és megjelenítheti azt, még akkor is gyűjtheti.

- **A System Center Operations Manager (SCOM)** van, felügyelheti és figyelheti a nagy felhőalapú telepítések. Valószínűleg már tisztában van, a felügyeleti eszköz a helyi Windows Server és Hyper-V alapú-felhők, de is integrálása és az Azure-alkalmazások kezelése. Többek között az Application Insights telepíthet a meglévő élő alkalmazásokra. Ha egy alkalmazás leáll, láthatja, másodpercben.


## <a name="next-steps"></a>Következő lépések

- [Ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

- [Azure-előfizetés irányítás implementációi](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-subscription-examples).

- [A Microsoft Azure Government](https://docs.microsoft.com/azure/azure-government/).
