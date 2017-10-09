---
title: "aaaBest eljárások a vállalatok tooAzure áthelyezése |} Microsoft Docs"
description: "Ismerteti, hogy a vállalatok számára a biztonságos és kezelhető környezet tooensure használhatja egy scaffold."
services: azure-resource-manager
documentationcenter: na
author: rdendtler
manager: timlt
editor: tysonn
ms.assetid: 8692f37e-4d33-4100-b472-a8da37ce628f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: rodend;karlku;tomfitz
ms.openlocfilehash: d1402cf21d0cf740e44c03fc345ecd39a6e1680c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Az Azure enterprise scaffold - részletes utasításokkal megadott előfizetés-irányítás
A vállalatok egyre vannak bevezetése hello nyilvános felhőbe az agilitást és a rugalmasságot. Azokat a rendszer okhoz hello felhő szintjeiről toogenerate bevétel, vagy hello vállalati erőforrások optimalizálására. A Microsoft Azure kínál számos, hogy a vállalatok összeállíthat például építőelemeket tooaddress munkaterheléseket és alkalmazásokat az széles köréről. 

Azonban hogy tudnák, toobegin esetén gyakran nehéz. Amikor toouse Azure eldöntötte, néhány kérdést gyakran merülhetnek fel:

* "Hogyan megfelelnek a törvény írja elő az adatok közös joghatóság alá, az egyes országokban?"
* "Hogyan do I győződjön meg arról, hogy valaki nem módosíthatja a kritikus rendszer?"
* "Honnan tudhatom, hogy mi minden erőforrás támogat, I is fiókot használja, és pontosan vissza számlázási?"

az üres előfizetés nincs védőkorlátokat a hello potenciális ijesztőnek. Az üres helyet a move tooAzure is akadályozhatják.

Ez a cikk műszaki szakemberek tooaddress hello igényének a helyes cégirányítás és a hello szükséges agilitást egyenleg kiindulási pontot biztosít. Egy vállalati scaffold, amely végigvezeti a szervezetek megvalósítása és kezelése az Azure-előfizetések hello fogalma okozna. 

## <a name="need-for-governance"></a>Az irányítás szükséges
Amikor tooAzure helyezi át, meg kell oldania hello témakör irányítás korai tooensure hello sikeres alkalmazásának hello felhő hello vállalaton belül. Sajnos hello idő és egy átfogó irányítás rendszer létrehozásának bürokrácia azt jelenti, hogy egyes üzleti csoportok lépjen közvetlenül toovendors vállalati informatikai bevonása nélkül. Ez a megközelítés hello vállalati nyitott toovulnerabilities hagyhatja, ha a hello erőforrásokat nem megfelelően kezeli. toobusiness csoportok tooquickly igénylő ügyfelek (belső és külső) hello kielégítése hello nyilvánosfelhő - agilitást, a rugalmasságot és fogyasztás alapján árképzési - hello jellemzői fontosak. De a nagyvállalati informatikai tooensure adatok és rendszerek hatékonyan védelmének biztosításához szükséges.

A valós életben állványok alapja használt toocreate hello hello struktúra. hello scaffold útmutatói hello általános elvet követik, és szerkesztési pontokat biztosít több állandó rendszerek toobe csatlakoztatva. Egy vállalati scaffold van hello ugyanaz: rugalmas vezérlők és az Azure-képességek, amelyek struktúra toohello környezetben, és a központi jellegűek biztosítanak hello nyilvános felhő épülő szolgáltatások készlete. Hello szerkesztők biztosít (informatikai és üzleti csoportok) egy foundation toocreate, és csatlakoztassa az új szolgáltatásokat.

hello scaffold eljárások azt összegyűjtötte a különböző méretű ügyfelek sok bevonására alapul. Ezek az ügyfelek közé kapcsolatos megoldások hello felhő tooFortune 500 vállalatok és a független szoftvergyártók áttelepítése és a megoldások hello felhőben kisebb szervezetek. hello vállalati scaffold "erre a célra kialakított" toobe rugalmas toosupport hagyományos informatikai munkaterhelések és a gyors munkaterhelések; például a fejlesztők szoftver,--szolgáltatás (SaaS) alkalmazások létrehozása Azure-képességek alapján.

hello vállalati scaffold tervezett toobe hello foundation minden új előfizetés Azure belül. Segítségével a rendszergazdák tooensure munkaterhelések igazodhat hello minimális irányítási követelmények vonatkoznak a szervezet anélkül, hogy megakadályozza az üzleti csoportok és a fejlesztők gyorsan teljesíti a saját kitűzött célokat.

> [!IMPORTANT]
> Cégirányítási Azure kritikus fontosságú toohello sikerességének. Ez a cikk egy vállalati scaffold műszaki végrehajtásának hello, de csak koppint hello szélesebb körű folyamat és hello összetevői közötti kapcsolatok. Házirend irányítási hello felülről lefelé zajló kommunikációról, és határozza meg, milyen hello által az üzleti szeretne tooachieve. Természetesen hello irányítás-modell Azure foglal magában képviselőinek informatikai, de ennél is fontosabb rendelkeznie kell az üzleti csoport vezetők és adatvédelmi és kockázatkezelési management erős ábrázolását. A hello végén egy vállalati scaffold információ üzleti kockázati toofacilitate egy szervezet céljaira és célok van.
> 
> 

a következő kép hello hello scaffold hello összetevőit mutatja be. hello foundation egy teli tervet a részlegek, fiókok és -előfizetések támaszkodik. hello oszlopok erőforrás-kezelő házirendek és az erős elnevezési szabályai állnak. hello scaffold hello többi Azure-képességek core származik, és szolgáltatásokat, hogy engedélyezze a biztonságos és kezelhető környezet.

![scaffold összetevők](./media/resource-manager-subscription-governance/components.png)

> [!NOTE]
> Azure gyors nőtt 2008 bevezetése óta. A növekedési szükséges csapatának toorethink azok megközelítés kezelése és szolgáltatások telepítése a Microsoft mérnöki csapathoz. hello Azure Resource Manager modellt az 2014 lett bevezetve, és a felváltja az hello klasszikus üzembe helyezési modellben. Erőforrás-kezelő lehetővé teszi a szervezetek toomore egyszerű üzembe helyezését, rendezheti, és szabályozhatja az Azure-erőforrások. Erőforrás-kezelő tartalmazza a párhuzamos folyamatkezelést biztosítja, összetett, egymástól kölcsönösen függnek megoldások gyorsabb telepítése erőforrások létrehozásakor. A részletes hozzáférés-vezérlés és hello képességét tootag erőforrások metaadatokkal is tartalmaz. A Microsoft azt javasolja, hogy minden erőforrás hello erőforrás-kezelő használatával hozzon létre. hello vállalati scaffold explicit módon végzi a hello Resource Manager modellt.
> 
> 

## <a name="define-your-hierarchy"></a>Adja meg a hierarchiában
hello scaffold hello rálátással hello Azure nagyvállalati beléptetés (és az Enterprise Portal hello). a nagyvállalati beléptetés hello hello alakzat meghatározása és használó Azure-szolgáltatásokat a vállalaton belül, hello core irányítási szerkezete. Belül hello nagyvállalati szerződés, az ügyfelek képesek toofurther tovább hello környezet részlegek, fiókok, és végül előfizetések. Az Azure-előfizetésre hello alapvető egysége, ahol minden erőforrás található. Egyúttal meghatározza, milyen több határokon belül Azure, például a magok, erőforrások stb.

![hierarchia](./media/resource-manager-subscription-governance/agreement.png)

Minden vállalati más, és az előző ábrán hello hello hierarchia lehetővé teszi, hogy hogyan vannak rendszerezve Azure hello vállalaton belül jelentős rugalmasságot. Előtt hello útmutatást a dokumentumban található, a hierarchiában a modell és megismerheti a számlázási, erőforrás-hozzáférés és összetettsége hello hatását.

három közös minták hello Azure regisztrációkat a következők:

* Hello **működési** minta
  
    ![funkcionális](./media/resource-manager-subscription-governance/functional.png)
* Hello **részleg** minta 
  
    ![vállalata számára](./media/resource-manager-subscription-governance/business.png)
* Hello **földrajzi** minta
  
    ![földrajzi](./media/resource-manager-subscription-governance/geographic.png)

Hello scaffold: hello előfizetés szintű tooextend hello cégirányítási követelmények hello Enterprise hello előfizetéssé telepítését.

## <a name="naming-standards"></a>Elnevezési szabályai
hello első pillar a hello scaffold van elnevezési szabványoknak. Tetszetős elnevezési szabályai tooidentify erőforrások hello portálon, a számlázási és parancsfájlban engedélyezése. Nagy valószínűséggel már szabványok elnevezési a helyszíni infrastruktúrát. Azure tooyour környezet hozzáadásakor kell nyúlnia a elnevezési szabványok tooyour Azure-erőforrások. Szabványos elnevezési megkönnyítése hello környezet minden szinten hatékonyabb felügyeletét.

> [!TIP]
> Az elnevezési konvenciókat:
> * Tekintse át, és lehetőség szerint elfogadja hello [minták és gyakorlatok útmutatást](../guidance/guidance-naming-conventions.md). Ez az útmutató segítséget nyújt a adja meg egy beszédes elnevezési szabványnak.
> * Használjon camelCasing erőforrások (például a contoso.com és vnetNetworkName) nevét. Megjegyzés: Vannak bizonyos erőforrások, például a storage-fiókok, ahol hello egyetlen lehetősége: toouse kisbetű (és más különleges karaktereket).
> * Fontolja meg szabványos elnevezési Azure Resource Manager (hello következő szakaszban leírt) házirendek tooenforce használatát.
> 
> hello előző tippek segítségével megvalósíthatja egy egységes elnevezési konvenciót.

## <a name="policies-and-auditing"></a>Házirendek és a naplózás
hello második pillar a hello scaffold magában foglalja [Azure Resource Manager házirendek](resource-manager-policy.md) és [hello tevékenységnapló naplózás](resource-group-audit.md). Erőforrás-kezelő házirendek biztosítják azokat hello képességét toomanage kockázat az Azure-ban. Megadhatja, amelyek biztosítják az adatok közös joghatóság alá korlátozása, kényszerítése, vagy bizonyos műveletek naplózási házirendeket. 

* A házirend az alapértelmezett **engedélyezése** rendszer. Műveletek és hozzárendelése, megtagadása vagy a műveleteket az egyes erőforrások naplózási házirendek tooresources szabályozhatja.
* Egy házirend adatdefiníciós nyelv (if-majd feltételek) a házirend-definíciók alapján házirendek ismerteti.
* Létrehozhat olyan formázott fájlok JSON (Javascript Object Notation) a házirendeket. Meghatározza a házirendet, miután hozzárendelte tooa adott hatókör: előfizetés, az erőforráscsoportot, vagy az erőforrás.

Házirendek, amelyek lehetővé teszik a minden részletre kiterjedő megközelítés tooyour forgatókönyvek több műveletet kell végrehajtani. hello műveletek a következők:

* **Megtagadási**: blokkok hello erőforrás-kérelem
* **Naplózási**: lehetővé teszi, hogy hello kérelem azonban hozzáadja egy sor toohello tevékenységnapló (amely használt tooprovide riasztások vagy tootrigger runbookok)
* **Hozzáfűzendő**: hozzáadja a megadott információkat toohello erőforrás. Például ha nem a "CostCenter" címke erőforráson, vegye fel a címkét az alapértelmezett értéket.

### <a name="common-uses-of-resource-manager-policies"></a>Gyakori használati módjai erőforrás-kezelő házirendek
Az Azure Resource Manager-házirendek olyan hello Azure eszközkészlet egy hatékony eszköz. Lehetővé teszik a tooavoid váratlan költségek, tooidentify költségekkel center erőforrások címkézés és a tooensure segítségével, hogy teljesülnek-előírásainak. Házirendek hello beépített naplózási szolgáltatásai vannak együtt, akkor is fashion összetett és rugalmas megoldásokat. Házirendek lehetővé teszik a vállalatok tooprovide funkciók munkaterhelések "Hagyományos informatikai" és "Agile" munkaterhelések; például az ügyfél-alkalmazások fejlesztésével. hello leggyakoribb minták házirendek látható a következők:

* **Földrajzi-megfelelőségi/adatok közös joghatóság alá** -Azure az régiók közötti hello world teszi lehetővé. A vállalatok gyakran kívánja a toocontrol ahol erőforrások jönnek létre (hogy tooensure adatok közös joghatóság alá vagy csak tooensure erőforrások Bezárás toohello end fogyasztók hello erőforrások jönnek létre).
* **Költségkezelésére** -egy Azure-előfizetés számos típusok és a skála erőforrásai szerepelhetnek. Vállalatok gyakran kívánja tooensure, hogy standard előfizetések kerülje a feleslegesen erőforrásokat, amelyek is költségeket, de ténylegesen el egy hónapban, vagy több száz.
* **Alapértelmezett irányítás szükséges címkéket keresztül** -címkék használata hello leggyakoribb és magas kívánt funkciók valamelyike. Azure Resource Manager házirendek segítségével a vállalatok egyre jobban tudja tooensure erőforrás címkéznie. hello leggyakoribb címke található: részleg, az erőforrás tulajdonosa és a környezet típusát (például - éles, teszt, fejlesztési)

**Példák**

"Hagyományos informatikai" előfizetés-üzletági alkalmazások

* Az összes erőforrás részleg és a tulajdonos címkék kényszerítése
* Erőforrás létrehozása toohello Észak-amerikai régió korlátozása
* Hello képességét toocreate G sorozatú virtuális gépek és a HDInsight-fürtök korlátozása

A felhőalapú alkalmazások létrehozása részleg "Gyors" környezet

* adatok közös joghatóság alá követelmények toomeet, az erőforrások hello létrehozásának engedélyezése a csak egy adott régióban.
* Az összes erőforrás környezetcímke kényszerítése. Egy erőforrást egy címke nélkül jön létre, ha hozzáfűzése hello **környezet: ismeretlen** toohello erőforrás címkét.
* Naplózási erőforrások Észak-Amerikában kívül jönnek létre, de nem akadályozzák meg.
* Naplózási nagy költségű erőforrások létrehozásakor.

> [!TIP]
> hello leggyakrabban használt erőforrás-kezelő házirendek szervezet használata toocontrol *ahol* erőforrásokat lehet létrehozni és *mi* típusú erőforrás hozhatók létre. Ezenkívül tooproviding vezérlők *ahol* és *mi*, sok vállalatok használata házirendek tooensure erőforrások rendelkezik hello megfelelő metaadatok toobill vissza a felhasználásához. Azt javasoljuk, hogy a hello előfizetés szintjén házirendek alkalmazása:
> 
> * Földrajzi-megfelelőségi/adatok közös joghatóság alá
> * Költségkezelés
> * Szükséges címkéket (üzleti igény, például a BillTo, az alkalmazás tulajdonosa azzal)
> 
> Hatókör alacsonyabb szinteken további házirendeket is alkalmazhat.
> 
> 

### <a name="audit---what-happened"></a>Naplózási – Mi történt?
Hogyan működik-e a környezet tooview, meg kell tooaudit felhasználói tevékenység. A legtöbb erőforrástípusok esetében az Azure diagnosztikai naplók, hogy a napló eszköz vagy az Azure Operations Management Suite szolgáltatásban is elemezheti létrehozása. A részlegszintű több előfizetések tooprovide keresztül tevékenységi naplóit vagy vállalati nézet gyűjthet. Naplózási bejegyzések fontos diagnosztikai eszköz, mind a kritikus fontosságú mechanizmus tootrigger események hello Azure környezetben.

A Resource Manager üzembe helyezések tevékenységi naplóit lehetővé teszik a toodetermine hello **műveletek** , amely tartott helye és végző felhasználók listáját. Tevékenységi naplóit is, majd összesíti a Naplóelemzési hasonló eszközökkel.

## <a name="resource-tags"></a>Az erőforráscímkék
Mivel a szervezet felhasználói erőforrások toohello előfizetés hozzáadásához egyre fontosabb tooassociate erőforrások hello megfelelő részleg, az ügyfél és a környezet válik. Csatolhat a metaadatok tooresources keresztül [címkék](resource-group-using-tags.md). Címkék tooprovide információ hello erőforrás vagy hello tulajdonosa használhatja. Címkék csak összesített toonot és erőforrások számos módon lehetővé teszik, de az adatokat a jóváírási hello alkalmazásában. Jelölheti meg too15 kulcs: érték párok fel az erőforrások. 

Az erőforráscímkék rugalmas és csatolt toomost erőforrásokat kell lenniük. Közös erőforráscímkék többek között:

* BillTo
* Szervezeti egység (vagy üzleti egység)
* Környezet (éles ponton fejlesztés)
* Réteg (webes réteg, alkalmazás szint)
* Alkalmazás tulajdonosa
* Projektnév

![tags](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Címkék további példákért lásd [Azure-erőforrások elnevezési szabályai ajánlott](../guidance/guidance-naming-conventions.md).

> [!TIP]
> Vegye figyelembe, hogy így egy egykiszolgálós használatát írja elő, a címkézés házirendet:
> 
> * Erőforráscsoportok
> * Storage
> * Virtuális gépek
> * Alkalmazáskiszolgálók környezetek vagy webes szolgáltatás
> 
> A címkézési stratégia azonosítja az előfizetések között, milyen metaadatok hello üzleti, a pénzügyi, a biztonsági, a kockázatkezelés és a hello környezet általános felügyeleti van szükség. 

## <a name="resource-group"></a>Erőforráscsoport
Erőforrás-kezelő lehetővé teszi tooput erőforrások kezelése, számlázási vagy természetes affinitás jelentéssel bíró csoportokba. Amint azt korábban említettük, az Azure két üzembe helyezési modellel rendelkezik. Hello korábbi Klasszikus modell, felügyeleti hello alapvető egysége hello előfizetés volt. Nehéz toobreak előfizetés, amely a kereslet az olyan előfizetések nagy mennyiségű toohello létrehozását erőforrások le volt. Hello Resource Manager modellt látott erőforráscsoportok hello bevezetése. Erőforráscsoportok olyan tárolók, az erőforrások, amelyek egy közös életciklussal vagy megosztani egy attribútum, például az "összes SQL-kiszolgáló" vagy "Alkalmazás A".

Erőforrás-csoportok nem lehetnek benne egymáshoz, és erőforrások csak tartozhatnak tooone erőforráscsoportot. Bizonyos műveleteket egy erőforráscsoportban található összes erőforrást is alkalmazhat. Például erőforráscsoport törlése eltávolítja az összes erőforrásokat hello erőforráscsoporton belül. Általában helyez egy teljes alkalmazás vagy a kapcsolódó rendszer hello ugyanabban az erőforráscsoportban. Például egy Contoso webalkalmazás nevű három réteg alkalmazás tartalmazná hello webkiszolgáló, alkalmazás vagy az SQL server a hello ugyanabban az erőforráscsoportban.

> [!TIP]
> Az erőforráscsoportok elrendezése eltérőek lehetnek "Hagyományos informatikai" munkaterhelések túl "Gyors informatikai" munkaterhelések:
> 
> * "Hagyományos informatikai" munkaterhelések leggyakrabban csoportosítási elemek hello belül azonos életciklussal, például egy alkalmazás. Csoportosítás alkalmazás lehetővé teszi, hogy az egyes Alkalmazáskezelés.
> * "Gyors informatikai" munkaterhelések általában toofocus a külső ügyfélkapcsolati felhőalapú alkalmazásokhoz. hello erőforráscsoportok tükröznie kell hello rétegek (például a webes réteg, App réteget) központi telepítés és kezelés.
> 
> A számítási feladatok ismertetése segít erőforrás csoport stratégia kidolgozása.

## <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés
Valószínűleg arra utasítja a saját kezűleg "hozzáférési tooresources rendelkező kell?" és "hogyan kezelheti a hozzáférést?" Így vagy letiltva a hozzáférés toohello Azure-portálon, és hozzáférési tooresources hello portálon szabályozása elengedhetetlen. 

Azure jelent, ha hozzáférést vezérlők tooa előfizetés voltak: rendszergazdának vagy társadminisztrátornak. Hozzáférés az tooa előfizetéshez hello Klasszikus modell hallgatólagos tooall hello az Azure-erőforrások hello portálon. Ez részletesebb vezérlést hiánya egy Azure regisztrálásra előfizetések tooprovide egy szint a megfelelő hozzáférés-vezérlés a toohello elterjedése vezetett.

Ez a előfizetések elterjedése már nem szükséges. Szerepköralapú hozzáférés-vezérlés, a felhasználók toostandard szerepköröket rendelhet (például közös "olvasó" és "író" típusok szerepkörök). Egyéni szerepkörök is meghatározhat.

> [!TIP]
> tooimplement szerepköralapú hozzáférés-vezérlés:
> * Csatlakozás a vállalati azonosító (leggyakrabban az Active Directory) tároló tooAzure Active Directory használatával hello AD Connect eszközzel.
> * Vezérlő hello rendszergazdai vagy Társadminisztrátori felügyelt identitásával előfizetés. **Nem** hozzárendelése rendszergazdai vagy társadminisztrátori tooa új előfizetés tulajdonosa. Ehelyett használja az RBAC-szerepkörök tooprovide **tulajdonos** jogok tooa csoport vagy személy.
> * Adja hozzá az Azure-felhasználók tooa csoportot (például X Alkalmazástulajdonosok) az Active Directoryban. Hello szinkronizált csoport tooprovide csoport tagjai hello megfelelő jogosultságokkal toomanage hello tartalmazó erőforráscsoport hello alkalmazás használja.
> * Hajtsa végre a hello elve hello megadása **legalacsonyabb jogosultsági szint** szükséges toodo hello munkahelyi várt. Példa:
>   * Üzembe helyezési csoport: Egy csoportot, amely csak képes toodeploy erőforrás.
>   * A virtuális gépek kezelése: A csoport, amely képes toorestart virtuális gépek (műveleteihez)
> 
> Ezek a tippek segítségével felügyelheti a felhasználók hozzáférését az előfizetésből.

## <a name="azure-resource-locks"></a>Azure-erőforrás zárolások feloldása
Mivel a szervezet alapvető szolgáltatások toohello előfizetés hozzáadása, egyre fontosabb, hogy ezek a szolgáltatások legyenek-e a rendelkezésre álló tooavoid üzleti megszűnésének tooensure válik. [Erőforrás zárolások](resource-group-lock-resources.md) lehetővé teszik a toorestrict műveletek az értékes erőforrások ahol módosítsa vagy törölje azokat az alkalmazások és a felhőalapú infrastruktúra jelentős hatással van. Zárolások tooa előfizetés, az erőforráscsoportot, vagy az erőforrás is alkalmazhatja. Általában a zárolások toofoundational erőforrások, például a virtuális hálózatok, az átjárók és a storage-fiókok telepítését. 

Erőforrás zárolások jelenleg két értéket támogatja: CanNotDelete és csak olvasható. CanNotDelete azt jelenti, hogy a felhasználói (hello megfelelő jogosultságokkal) is még olvasni vagy módosítani az erőforrást, de nem lehet törölni. Csak olvasható azt jelenti, hogy a jogosult felhasználók nem lehet törölni vagy módosítani az erőforrást.

toocreate vagy delete felügyeleti zárolás, hozzá kell férnie túl`Microsoft.Authorization/*` vagy `Microsoft.Authorization/locks/*` műveletek.
Hello beépített szerepkörök csak a tulajdonos és a felhasználói hozzáférés adminisztrátora kapnak ezeket a műveleteket.

> [!TIP]
> Alapvető hálózati beállítások zárral kell védeni. Az átjáró telephelyek közötti VPN véletlen törlésének lenne katasztrofális tooan Azure-előfizetés. Azure nem engedélyezi, hogy toodelete virtuális hálózat használatban van, de további korlátozásokat alkalmaz hasznos elővigyázatosságból. 
> 
> * Virtuális hálózat: CanNotDelete
> * Hálózati biztonsági csoport: CanNotDelete
> * Házirendek: CanNotDelete
> 
> Szabályzatok a megfelelő ellenőrzés kritikus fontosságú toohello karbantartási egyaránt. Azt javasoljuk, hogy alkalmazza a **CanNotDelete** használt toopolices zárolása.

## <a name="core-networking-resources"></a>Alapvető hálózati erőforrások
Lehet, hogy hozzáférési tooresources (belül hello hálózatán) belső vagy külső (keresztül hello internet). A szervezet tooinadvertently put hello helytelen helyszíni erőforrások a felhasználók könnyen, és potenciálisan Megnyitásukhoz toomalicious hozzáférést. Csakúgy, mint a helyszíni eszközök, a vállalatok megfelelő vezérlők tooensure, hogy a felhasználók Azure döntéseket hello jobb kell hozzáadnia. A előfizetés irányítás azt határozza meg, amely biztosítja a alapvető hozzáférés-vezérlést alapvető erőforrásai. hello alapvető erőforrások foglalják magukban:

* **Virtuális hálózatok** alhálózatok tároló objektumok. Bár nem feltétlenül szükséges, gyakran használják, ha van csatlakozás alkalmazások toointernal vállalati erőforrásokhoz.
* **Hálózati biztonsági csoportok** hasonló tooa tűzfal, és adjon meg szabályokat a hogyan erőforrás "működik" hello hálózaton keresztül. Adja meg az útmutató részletes szabályozását, /, ha egy alhálózat (vagy a virtuális gép) kapcsolódhatnak toohello Internet vagy a más alhálózatokon hello azonos virtuális hálózaton.

![hálózati szolgáltatásmag](./media/resource-manager-subscription-governance/core-network.png)

> [!TIP]
> Hálózati:
> * Hozzon létre virtuális hálózatok dedikált tooexternal irányuló munkaterhelések és a belső hálózati terhelés. Ez a megközelítés csökkenti a véletlenül helyezi el a virtuális gépek esetén, amelyeket külső irányuló térben belső munkaterhelések hello esélyét.
> * Hálózati biztonsági csoportok toolimit hozzáférés konfigurálása. Legalább a blokkolja a hozzáférést toohello internet belső virtuális hálózatok és a blokk hozzáférés toohello vállalati hálózathoz a külső virtuális hálózatok.
> 
> Ezek a tippek segítségével megvalósíthatja a biztonságos hálózati erőforrásokhoz.

### <a name="automation"></a>Automatizálás
Erőforrások kezelése külön-külön csak időigényes és potenciálisan nagyon eséllyel fordulnak elő bizonyos műveletek hiba. Azure Azure Automation, a Logic Apps és az Azure Functions különböző automatizálási képességeket nyújt. [Azure Automation szolgáltatásbeli](../automation/automation-intro.md) lehetővé teszi a rendszergazdák toocreate, és határozza meg a runbookok toohandle általános feladatok az erőforrások kezelése. A runbookok vagy egy PowerShell-kód szerkesztővel, vagy egy grafikus szerkesztő segítségével hoz létre. Összetett többlépcsős munkafolyamatokat hozhat létre. Azure Automation szolgáltatásbeli gyakran használt toohandle gyakori feladatokat, mint a nem használt erőforrások leállítása vagy az erőforrások létrehozása a válasz tooa meghatározott eseményindító emberi beavatkozás nélkül.

> [!TIP]
> Az automatizáláshoz:
> * Egy Azure Automation-fiók létrehozása, és tekintse át a hello rendelkezésre álló runbookok (grafikus és a parancs sor) hello elérhető [forgatókönyvek](../automation/automation-runbook-gallery.md).
> * Importálja és testre szabhatja a legfontosabb forgatókönyvek saját használatra.
> 
> Egy általános forgatókönyv hello képességét tooStart/leállítást virtuális gépek ütemezés szerint. Példa a runbookok által biztosított hello gyűjteménye, amely ebben a forgatókönyvben kezelni és szól, hogyan van tooexpand azt.
> 
> 

## <a name="azure-security-center"></a>Azure Security Center
Lehet, hogy egy hello legnagyobb blockers toocloud elfogadása volt hello aggályokat biztonsági. Informatikai kockázat vezetők és biztonsági osztályok kell, hogy az Azure-erőforrások biztonságosabbak tooensure. 

Hello [az Azure Security Center](../security-center/security-center-intro.md) központi hello hello előfizetések erőforrások biztonsági állapotát jeleníti meg, és ajánlásokat, amelyekkel megakadályozható a fertőzött erőforrásokat. Engedélyezheti, hogy a részletesebb házirendek (például alkalmazása házirendek toospecific erőforráscsoportok, amelyek lehetővé teszik a hello vállalati tootailor azok címzési előírások toohello kockázat). Végül az Azure Security Center nyitott platform, amely lehetővé teszi a Microsoft-partnereknek, és független szállítók toocreate szoftver csatlakozik az Azure Security Center tooenhance képességeit. 

> [!TIP]
> Az Azure Security Center az egyes előfizetések alapértelmezés szerint engedélyezve van. Azonban kell az ügynök engedélyezése a virtuális gépek tooallow az Azure Security Center tooinstall adatok gyűjtése, és megkezdi az adatgyűjtést.
> 
> ![Adatok gyűjtése](./media/resource-manager-subscription-governance/data-collection.png)
> 
> 

## <a name="next-steps"></a>Következő lépések
* Most, hogy az előfizetés irányítás megismerte idő toosee ezek az ajánlások a gyakorlatban. Lásd: [Azure-előfizetés irányítás implementációi](resource-manager-subscription-examples.md).

