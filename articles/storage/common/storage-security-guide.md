---
title: "aaaAzure tárolási biztonsági útmutatója |} Microsoft Docs"
description: "Részletek sok védelmének biztosítása az Azure Storage, ideértve, de nem kizárólagosan tooRBAC, Storage szolgáltatás titkosítási, ügyféloldali titkosítás, az SMB 3.0-s és Azure Disk Encryption hello."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 6f931d94-ef5a-44c6-b1d9-8a3c9c327fb2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 1f5a4e724e00ea6d16f5511b9120154f89441758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-security-guide"></a>Az Azure Storage biztonsági útmutató
## <a name="overview"></a>Áttekintés
Az Azure Storage a biztonságos alkalmazások toobuild biztonsági képességeket, amelyek együtt lehetővé teszik a fejlesztők széles választékát nyújtja. hello tárfiók magát a szerepköralapú hozzáférés-vezérlés és az Azure Active Directory használatával kell biztonságossá. Adatok védve legyenek az alkalmazás és az Azure közötti átvitel során használatával [ügyféloldali titkosítás](../storage-client-side-encryption.md), HTTPS és SMB 3.0-s. Adatok állíthat be, automatikus a titkosítás írásakor toobe tooAzure használatával [Storage Service Encryption (SSE)](storage-service-encryption.md). Virtuális gépek által használt operációsrendszer- és adatlemezek beállítható használatával titkosított toobe [Azure Disk Encryption](../../security/azure-security-disk-encryption.md). Delegált hozzáférést toohello adatobjektumainak az Azure Storage használatával engedélyezhetők [megosztott hozzáférési aláírásokkal](../storage-dotnet-shared-access-signature-part-1.md).

Ez a cikk nyújt áttekintést. az egyes szolgáltatások az Azure Storage használható. Egyes szolgáltatások részleteit, egyszerűen megteheti adják tooarticles hivatkozásokkal további vizsgálatra minden témakör.

Az alábbiakban a cikkben szereplő hello témakörök toobe:

* [Felügyeleti Vezérlősík biztonsági](#management-plane-security) – a Tárfiók védelmének biztosítása

  hello felügyeleti vezérlősík hello használt erőforrások toomanage áll a tárfiók. Ebben a szakaszban lesz döntésről bővebben hello Azure Resource Manager üzembe helyezési modellben, és hogyan férhetnek hozzá a toouse szerepköralapú hozzáférés-vezérlést (RBAC) toocontrol tooyour storage-fiókok. A tárfiók kulcsait kezelése is előadás és hogyan tooregenerate őket.
* [Adatbiztonság Vezérlősík](#data-plane-security) – hozzáférés biztonságossá tétele tooYour adatok

  Ebben a szakaszban megnézzük, hozzáférést toohello tényleges adatobjektumainak tárfiókba blobok, fájlok, üzenetsorokat és táblákat, például megosztott hozzáférési aláírásokkal és hozzáférési házirendek tárolja. Bemutatjuk a szolgáltatásiszint-SAS és a fiók szintű SAS. Is megtanulhatja, hogyan toolimit férnek hozzá az tooa adott IP-cím (vagy az IP-címek), hogyan toolimit hello protokoll tooHTTPS, és hogyan nem várja meg azt a közös hozzáférésű Jogosultságkód toorevoke tooexpire.
* [Titkosítás az átvitel során](#encryption-in-transit)

  Ez a szakasz ismerteti hogyan toosecure adatok átvitel során, vagy abból az Azure Storage. Lesz döntésről bővebben hello HTTPS és hello használja az Azure fájlmegosztások SMB 3.0-titkosítás ajánlott. Most elindítjuk egy pillantást ügyféloldali titkosítás, mely lehetővé teszi tooencrypt hello előtt a tároló ügyfél-alkalmazásokban, és toodecrypt hello adatok tárolási kivitt után is.
* [Titkosítás inaktív állapotban](#encryption-at-rest)

  Az előadás Storage Service Encryption (SSE), és hogyan engedélyezi azt egy tárfiókot, ami azt eredményezi, a blokkblobokat, lapblobokat, és hozzáfűző blobok, automatikus a titkosítás, ha a tárolási tooAzure ír. Hogyan használja az Azure Disk Encryption és felfedezés hello alapvető különbséget és az adatok titkosítása és SSE ügyféloldali titkosítás és esetek is fog keresni. Röviden: a FIPS előírásainak való megfelelést az USA fog keresni Kormánya számítógépek.
* Használatával [tárolási analitika](#storage-analytics) tooaudit hozzáférés az Azure Storage

  Ez a szakasz ismerteti, hogyan naplózza az toofind információk hello storage Analytics egy kérelemre vonatkozóan. Azt fogja vessen egy pillantást valós tárolási analitika naplóadatokat, és tekintse meg, hogyan toodiscern, hogy a kérelem hello tárolási kulcsot, a közös hozzáférésű jogosultságkód fiókot, vagy névtelen, illetve hogy az sikeres vagy sikertelen volt.
* [CORS használatával Fiókértékek ügyfelek engedélyezése](#Cross-Origin-Resource-Sharing-CORS)

  Ez a szakasz beszél hogyan tooallow eltérő eredetű erőforrások megosztása (CORS). Tartományok közötti elérést, és hogyan toohandle hello CORS képességek a beépített Azure Storage fogja döntésről.

## <a name="management-plane-security"></a>Felügyeleti Vezérlősík biztonsága
hello felügyeleti vezérlősík maga hello tárfiók-műveletek áll. Például akkor is hozzon létre vagy tárfiók törlése, storage-fiókok listájának beolvasása egy előfizetésben, hello tárfiókkulcsok beolvasni vagy hello tárfiókkulcsok újragenerálása.

Amikor létrehoz egy új tárfiókot, akkor klasszikus és Resource Manager telepítési modell kiválasztása. hello Klasszikus modell erőforrások létrehozása az Azure-ban csak lehetővé teszi, hogy mindent hozzáférés toohello előfizetés, és a tárfiók viszont hello.

Ez az útmutató hello Resource Manager modellt, ami azt jelenti, hogy a storage-fiókok létrehozásához ajánlott hello összpontosít. Hello erőforrás-kezelő storage-fiókok, ahelyett, hogy teljes előfizetés amely hozzáférési toohello szabályozhatja a hozzáférést a szerepköralapú hozzáférés-vezérlést (RBAC) használatával több véges szintű toohello felügyeleti vezérlősík.

### <a name="how-toosecure-your-storage-account-with-role-based-access-control-rbac"></a>Hogyan toosecure a tárolási fiók szerepköralapú hozzáférés-vezérlést (RBAC)
Most szolgáltatással kapcsolatban RBAC van, és hogyan használhatja azt. Minden Azure-előfizetés Azure Active Directoryval rendelkezik. Felhasználók, csoportok és alkalmazások tartalmazza a toomanage az Azure-erőforrások hello Azure-előfizetés hello Resource Manager üzembe helyezési modellben használó engedélyezhetők. Ez a hivatkozott tooas szerepköralapú hozzáférés-vezérlést (RBAC). toomanage ez fér hozzá, használhatja a hello [Azure-portálon](https://portal.azure.com/), hello [Azure CLI-eszközei](../../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), vagy hello [Azure Storage erőforrás szolgáltató REST API-k ](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Hello erőforrás-kezelő modellel helyezze hello tárfiók egy erőforrás csoport és a vezérlés hozzáférési toohello felügyeleti vezérlősík bizonyos tárolási fiók az Azure Active Directoryval. Például biztosíthat bizonyos felhasználók hello képességét tooaccess hello tárfiókok kulcsait, amíg más felhasználók hello tárfiók kapcsolatos információk is megtekinthetők, de nem hello tárfiókkulcsok.

#### <a name="granting-access"></a>Hozzáférés biztosítása
A hozzáférés hello megfelelő RBAC szerepkör toousers, csoportok és alkalmazások hello megfelelő hatókörben hozzárendelésével. toogrant hozzáférés toohello egész előfizetésre, hogy rendelhet hozzá egy szerepkört hello előfizetés szintjén. Hozzáférés tooall hello erőforrás egy erőforráscsoportban biztosíthat engedélyek toohello erőforráscsoport maga megadásával. Egyes szerepkörök toospecific erőforrások, például a storage-fiókok is hozzárendelhetők.

Az alábbiakban hello főbb pontjai, hogy kell-e tooknow RBAC tooaccess hello felügyeleti műveletek az Azure Storage-fiók használata:

* Ha hozzáférés hozzárendelése alapvetően egy szerepkör toohello fiókot, amelyet az toohave hozzáférés rendeljen hozzá. Megadhatja a hozzáférés toohello műveletek toomanage, hogy a tárfiókot, de nem toohello-adatobjektumok hello fiók. Például engedélyt adhat tooretrieve hello tulajdonságainak hello storage-fiók (például a redundancia érdekében), de nem tooa tároló vagy egy belül a Blob Storage tárolóban lévő adatok.
* Mások toohave engedély tooaccess hello hello tárfiókban lévő adatokat objektumok átadhatja nekik engedély tooread hello tárfiókkulcsok és, hogy a felhasználó használhatja ezeket kulcsok tooaccess hello BLOB, a várólisták, a táblák és a fájlokat.
* Szerepkörök rendelhetők hozzá tooa adott felhasználói fiók, a felhasználók vagy tooa az adott alkalmazást.
* Minden szerepkörhöz műveletek és a nem műveletek listáját. Például a hello virtuális gép közreműködő szerepkört "listKeys" művelettel rendelkezik, amely lehetővé teszi, hogy hello tárolási fiók kulcsok toobe olvasni. hello közreműködői "Nem műveletek" van például hello Active Directory a felhasználók hozzáférésének hello frissítése.
* Tárolási szerepkörei tartalmazzák (azonban nem csak) hello következő:

  * Tulajdonos – azok mindent felügyelhetnek, beleértve a hozzáférést.
  * Közreműködő – azok is végrehajthat hello tulajdonosa is, kivéve hozzáférés hozzárendelése. Ezzel a szerepkörrel rendelkező bármely személy megtekintheti és hello tárfiókkulcsok újragenerálása. Hello tárfiókok kulcsait, az hello adatobjektumainak eléréséhez.
  * Olvasó – hello tárfiók, kivéve a titkos kulcsok adatait is megtekinthetik. Például ha egy szerepkör hello tárolási fiók toosomeone olvasási engedélyekkel rendelkező, hello tulajdonságok hello tárfiók is megtekinthetik, de nem hajtsa végre a módosításokat toohello tulajdonságok vagy hello tárfiókkulcsok megtekintése.
  * Tárolási fiók közreműködői – kezelésére hello tárfiók – elolvasása hello előfizetés erőforráscsoportok és erőforrásokat, és hozzon létre és előfizetés erőforrás csoport központi telepítések felügyeletéhez szükséges. Hello tárfiókok kulcsait, ami viszont azt jelenti, hogy hozzáférhessenek a hello adatok vezérlősík is eléréséhez.
  * Felhasználói hozzáférés adminisztrátora – általa kezelhető a felhasználói hozzáférés toohello tárfiók. Például azok olvasó hozzáférés tooa adott felhasználó megadásához.
  * Virtuális gép közreműködő – kezelésére virtuális gépek, de nem hello tárolási fiók toowhich vannak csatlakoztatva. Ez a szerepkör készíthetünk hello tárfiókok kulcsait, ami azt jelenti, hogy ehhez a szerepkörhöz hozzárendelt felhasználói toowhom hello hello adatok vezérlősík frissítheti.

    Ahhoz, hogy egy felhasználó toocreate egy virtuális gépet toobe képes toocreate hello megfelelő VHD-fájlt olyan tárfiókban rendelkeznek. toodo, hogy be kell toobe képes tooretrieve hello tárolási fiók kulcs, és adja át toohello API hello virtuális gép létrehozása. Ezért így azok készíthetünk hello tárfiókkulcsok ezzel az engedéllyel kell rendelkezniük.
* hello képességét toodefine egyéni szerepkörök olyan szolgáltatás, amely lehetővé teszi a toocompose listájából az elérhető műveleteket, Azure-erőforrások hajtható végre műveletek egy csoportját.
* hello beállítása az Azure Active Directoryban, mielőtt egy szerepkör toothem toobe van.
* Hozzon létre egy jelentést, akik nyújtott/visszavont milyen típusú hozzáférést és a akinek, és milyen hatókörben PowerShell használatával, vagy az Azure parancssori felület hello.

#### <a name="resources"></a>Erőforrások
* [Azure Active Directory szerepköralapú hozzáférés-vezérlése](../../active-directory/role-based-access-control-configure.md)

  Ez a cikk ismerteti a hello Azure Active Directory szerepköralapú hozzáférés-vezérlés és annak működéséről.
* [RBAC: Beépített szerepkörök](../../active-directory/role-based-access-built-in-roles.md)

  Ez a cikk részletesen összes hello beépített szerepkör RBAC érhető el.
* [A Resource Manager-alapú és a klasszikus üzembe helyezés ismertetése](../../azure-resource-manager/resource-manager-deployment-model.md)

  Ez a cikk ismerteti a hello Resource Manager üzembe helyezési és a klasszikus üzembe helyezési modellel, és hello erőforrás-kezelő és az erőforrás-csoportok használata hello előnyeit ismerteti. Azt ismerteti, hogyan hello Azure számítási, hálózati és tárolási szolgáltatók működnek hello Resource Manager modellben.
* [Szerepköralapú hozzáférés-vezérlés a REST API hello kezelése](../../active-directory/role-based-access-control-manage-access-rest.md)

  Ez a cikk bemutatja, hogyan toouse hello REST API toomanage RBAC.
* [Az Azure Storage erőforrás szolgáltató REST API-referencia](https://msdn.microsoft.com/library/azure/mt163683.aspx)

  Ez az API-kat használhatja toomanage a tárfiók programozott módon hello hello referenciája.
* [Fejlesztői útmutató tooauth Azure Resource Manager API-hoz](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

  Ez a cikk bemutatja, hogyan tooauthenticate használatával hello Resource Manager API-k.
* [Szerepköralapú hozzáférés-vezérlés az Ignite-tól a Microsoft Azure számára](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

  Ez az a hivatkozás tooa a hello 2015-ös MS ignite-on konferencia a Channel 9 videót. Ebben a munkamenetben, azok szolgáltatással kapcsolatban hozzáférhet a felügyeleti és jelentéskészítési lehetőségeket az Azure-ban, és vizsgálja meg körül tooAzure előfizetések az Azure Active Directoryval hozzáférés biztosítása érdekében ajánlott eljárások.

### <a name="managing-your-storage-account-keys"></a>A Tárfiók kulcsait kezelése
Tárfiók kulcsokban 512 bites karakterláncok Azure által létrehozott, együtt hello storage-fiók neve, hello tárfiókot, például BLOB, tábla, üzenetsor-üzeneteket és az Azure File-megosztáson található fájlok belüli tárolt használt tooaccess hello adatok objektum lehet. Ellenőrző hozzáférés toohello tárolási fiók kulcsok szabályozza a toohello adatok vezérlősík tárolási fiók hozzáférést.

Minden tárfiók rendelkezik említett két kulcs tooas "Kulcsot 1" és "Kulcs 2" hello [Azure-portálon](http://portal.azure.com/) és a PowerShell-parancsmagok hello. Ezek helyreállíthatja segítségével manuálisan többféle módszer, ideértve, de nem kizárólagosan toousing hello [Azure-portálon](https://portal.azure.com/), PowerShell, hello Azure CLI vagy programozott módon .NET a Storage ügyféloldali kódtára hello vagy hello Azure Storage szolgáltatások REST API-t.

Számos bármely okból tooregenerate a tárfiók kulcsait.

* Előfordulhat, hogy generálja újra őket rendszeresen biztonsági okokból.
* A tárfiók kulcsait volna generálja újra, ha valaki toohack felügyelt egy alkalmazásba, és tooyour tárfiók teljes hozzáférési jogosultságot ad hello kulcs szoftveresen kötött vagy menteni a konfigurációs fájl beolvasása.
* Egy másik eset a kulcs újragenerálása, ha a csapatával használ egy Tártallózó alkalmazást, amely megőrzi hello tárfiók hívóbetűjét, és egyik hello csoport tagjai. hello alkalmazás toowork engedélyezése hozzáférés tooyour tárfiók után már nem fontosságúak folytatni. Ez oka ténylegesen hello elsődleges fiók szintű megosztott hozzáférési aláírásokkal hozza létre őket – használhat egy fiók szintű SAS helyett hello elérési kulcsok tárolása egy konfigurációs fájlban.

#### <a name="key-regeneration-plan"></a>Kulcs újragenerálása terv
Nem szeretné, hogy toojust újragenerálása hello kulcs néhány tervezés nélkül használ. Ha így tesz, sikerült levágási minden hozzáférési toothat storage-fiók, amely súlyos problémákat okozhat. Ezért két kulcs van. Egyszerre csak egy kulcs kell generálni.

Mielőtt újragenerálja a kulcsokat, lehet, hogy az összes függő hello tárfiók alkalmazásával, valamint használ az Azure-szolgáltatások listáját. Például ha a tárfiók függő Azure Media Services használ, újra kell szinkronizálnia hello hívóbetűk a médiaszolgáltatással után hello kulcs újragenerálása. Ha az alkalmazásokat, például a Tártallózó alkalmazással használ, szüksége lesz a tooprovide hello új kulcsok toothose alkalmazásokat is. Vegye figyelembe, hogy ha virtuális gépeket, amelyek VHD-fájlok hello tárfiók vannak tárolva, nem vonatkoznak rá által hello tárfiókkulcsok újragenerálása.

A kulcsok hello Azure-portálon állíthatja helyre. Miután kulcsok újragenerálása vannak is veszik too10 perc toobe tárolószolgáltatások szinkronizálását.

Ha elkészült, ez hello általános folyamata, és részletesen leírja, hogyan kell módosítani a kulcsot. Ebben az esetben hello feltételezése, hogy a jelenleg használt kulcs 1 és fog toochange mindent toouse kulcs 2 helyette.

1. Generálja újra, hogy a rendszer biztonságos kulcs 2 tooensure. Ehhez a hello Azure-portálon.
2. Az összes hello alkalmazások hello biztonságitár-kulcs tárolására módosítsa a hello tárolási kulcs toouse kulcs 2 új értéket. Tesztelje, és tegye közzé hello alkalmazást.
3. Ha minden hello az alkalmazások és szolgáltatások működik, és megfelelően fut-e., generálja újra a kulcs 1. Ez biztosítja, hogy kifejezetten nincs megadva az új kulcs hello toowhom többé nem lesz toohello tárfiók eléréséhez.

Ha a jelenleg használt kulcs 2, ugyanezt a folyamatot, de fordított hello kulcsnevek hello is használhatja.

Minden alkalmazás toouse hello új kulcs módosuló, valamint a közzététel néhány napon keresztül áttelepítheti. Miután végzett az összes, meg kell majd lépjen vissza, és hello régi kulcs újragenerálása, így már nem működik.

Másik lehetőség is tooput hello tárfiók kulcsa a egy [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) valamilyen titkos adatot, és az alkalmazások lekérése hello kulcs onnan. Majd hello kulcs újragenerálása és hello Azure Key Vault frissítése, hello alkalmazásokat nem kell újratelepíteni, mert azok felveszi hello új kulcsot az Azure Key Vault hello automatikusan toobe. Vegye figyelembe, hogy akkor is hello alkalmazás hello kulcs olvasásához minden alkalommal, esetleg szükség lenne rá, vagy gyorsítótár, a memória, és ha nem sikerül, használatakor lekérése hello kulcs újra hello Azure Key Vault.

Az Azure Key Vault használatával is egy másik védelmet biztosít a kulcsok számára. Ezt a módszert használja, ha soha nem fog hello tárolási kulcs szoftveresen kötött a konfigurációs fájlban, amely valaki fog hozzáférni toohello kulcsok külön engedélye nélkül az adott erőfeszítések eltávolítja.

Az Azure Key Vault használatának másik előnye az Azure Active Directoryval tooyour hívóbetűk is szabályozhatja. Ez azt jelenti, hogy hozzáférési toohello néhány olyan alkalmazásokat, amelyek az Azure Key Vault tooretrieve hello kulcsokat kell, és tudja, hogy más alkalmazások nem lesznek képesek tooaccess hello kulcsok anélkül, hogy azok kifejezetten engedéllyel kellene is meg lehet adni.

Megjegyzés: ajánlott toouse csak az egyik hello a kulcsok összes hello az alkalmazásokat azonos idő. Használatakor a kulcs 1 az egyes helyek és a kulcs 2 más, akkor nem lesz képes toorotate kell a kulcsok egy alkalmazás-hozzáférés elvesztése nélkül.

#### <a name="resources"></a>Erőforrások
* [Az Azure Storage-fiókokról](storage-create-storage-account.md#regenerate-storage-access-keys)

  A cikk áttekintést nyújt a tárfiókok, valamint ismerteti a megtekintése, másolása és tárelérési kulcsok újragenerálása.
* [Az Azure Storage erőforrás szolgáltató REST API-referencia](https://msdn.microsoft.com/library/mt163683.aspx)

  Ez a cikk hivatkozások toospecific cikket lekérése során hello tárfiókkulcsok és újragenerálása hello tárfiókkulcsok tartalmaz egy Azure-fiók hello REST API használatával. Megjegyzés: Ez az erőforrás-kezelő storage-fiókok.
* [A storage-fiókok műveletek](https://msdn.microsoft.com/library/ee460790.aspx)

  Hello Storage Service Manager REST API-referencia a cikkben hivatkozások toospecific cikkek beolvasása és hello REST API használatával hello tárfiókkulcsok újragenerálása. Megjegyzés: Ez a klasszikus tárfiókokkal hello.
* [Tegyük fel például goodbye tookey felügyeleti – az Azure AD hozzáférési tooAzure tárolási adatok kezelése](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

  Ez a cikk bemutatja, hogyan toouse Active Directory toocontrol hívóbetűk tooyour Azure Storage az Azure Key Vault. Azt is bemutatja, hogyan feladat toouse egy Azure Automation-tooregenerate hello kulcsok óránként.

## <a name="data-plane-security"></a>Adatbiztonság Vezérlősík
Adatbiztonság Vezérlősík használt toosecure hello adatobjektumainak tárolja az Azure Storage – hello BLOB, üzenetsorok, táblák és fájlok toohello módszerek hivatkozik. Is láttuk módszerek tooencrypt hello adatok és a biztonsági hello adatok átvitel során, de hogyan továbblépne hozzáférés engedélyezése toohello objektumok?

Alapvetően két módszer ellenőrző hozzáférés toohello adatok magukat az objektumokat. hello először ellenőrző hozzáférés toohello tárfiókkulcsok által, és a hello második megosztott hozzáférési aláírásokkal toogrant access toospecific adatok objektumok egy adott időtartamig.

Egy kivétel toonote, hogy nyilvános tooyour blobok hello hozzáférési szint hello tároló, amely hello blobok ennek megfelelően a beállításával engedélyezheti. Ha hozzáférést egy tároló tooBlob vagy a tároló, engedélyezi az adott tároló hello blobok nyilvános olvasási hozzáférését. Ez azt jelenti, hogy bárki, aki egy tooa blob a tárolóban mutató URL-címet is nyissa meg a böngésző segítségével egy közös hozzáférésű Jogosultságkód-nak hello tárfiókok kulcsait, vagy nélkül.

### <a name="storage-account-keys"></a>Tárfiókkulcsok
Tárfiókkulcsok olyan hozta létre, amely a, hello tárfiók neve, valamint használt tooaccess hello adatobjektumainak hello storage-fiókban tárolt Azure 512 bites karakterláncok.

Például akkor is olvassa el a blobokat tooqueues írási, táblák létrehozása, fájlok, és módosítását. Ezek a műveletek számos segítségével végezheti el hello Azure portálon, vagy sok Tártallózó alkalmazást egyikével. Is írhat kódot toouse hello REST API vagy valamelyik hello Storage Ügyfélkódtáraival tooperform ezeket a műveleteket.

A hello hello szakaszban leírtaknak megfelelően [felügyeleti Vezérlősík biztonsági](#management-plane-security), a klasszikus tárfiók adjon teljes hozzáférés toohello Azure-előfizetés által biztosított toohello tárolási kulcsok elérhető. Szerepköralapú hozzáférés-vezérlést (RBAC) hozzáférési toohello tároló kulcsainak listázása hello Azure Resource Manager modellt használó tárfiókot szabályozható.

### <a name="how-toodelegate-access-tooobjects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Hogyan érik el toodelegate tooobjects a megosztott hozzáférési aláírásokkal és hozzáférési házirendek tárolt fiók
Egy közös hozzáférésű Jogosultságkód lehet egy biztonsági jogkivonatot tartalmazó karakterlánc csatolt tooa URI, amely lehetővé teszi a toodelegate hozzáférés toostorage objektumokat, és adja meg a hello és hello dátum/idő tartomány hozzáférési korlátozásokat.

Hozzáférés tooblobs, tárolók, várólista-üzenetek, fájlok és táblák megadásához. Táblákkal ténylegesen adhat engedélyt tooaccess hello tábla entitástartományának hello partíció- és sorfejlécek kulcstartományokkal toowhich hello felhasználói toohave hozzáférést szeretne megadásával. Például ha földrajzi állapot partíciókulcsú tárolt adatokat, akkor adhat valaki hozzáférést toojust hello adatok kaliforniai a.

Egy másik példa, előfordulhat, hogy adjon egy webalkalmazás egy SAS-jogkivonatot, amely lehetővé teszi, hogy toowrite bejegyzések tooa várólista, és adjon a feldolgozói szerepkör alkalmazás hello egy SAS-token tooget üzenetek várólistára és dolgozza fel őket. Vagy nem ad egy ügyfél egy SAS-tokennel, akkor tooupload képek tooa tároló használata a Blob Storage tárolóban, és adjon a webes alkalmazás engedély tooread a képek. Mindkét esetben nincs kizárás kérdések – minden alkalmazáshoz is hozzáférést kell biztosítani csak hello, amelyre szükségük van, a rendezés tooperform feladatuknak. Ez az megosztott hozzáférési aláírásokkal hello használata révén nyílik lehetőség.

#### <a name="why-you-want-toouse-shared-access-signatures"></a>Miért érdemes toouse megosztott hozzáférési aláírásokkal
Miért érdemes toouse helyett csak a tárfiók kulcsára, amely így sokkal könnyebben kiadása egy SAS? A tárfiók kulcsára kiadása van például a megosztás a tárolási Királyság hello kulcsok. Teljes hozzáférést van. Valaki nem sikerült a kulcsok használatára, és töltse fel a teljes Zene könyvtár tooyour tárfiók. Ezek nem is vírus fertőzte verziók cserélje le a fájlokat, vagy az adatok ellopására. Korlátlan hozzáféréssel tooyour tárfiók átruházása olyan dolog, amire nem kell enyhén venni.

Megosztott hozzáférési aláírásokkal adhat egy ügyfél csak a szükséges idő csak korlátozott mennyiségű hello jogosultságok. Például, ha valaki van feltöltése a blob tooyour fiókok, azok írási hozzáférést biztosíthat éppen elegendő idő tooupload hello BLOB (attól függően hello blob mérete hello természetesen). És ha megváltoztatja döntését, visszavonhatja a hozzáférést.

Emellett megadhatja, hogy a SAS használatával kérelmek bizonyos IP-cím vagy IP-cím a tartomány külső tooAzure korlátozott tooa. Is megkövetelheti, hogy kérések (HTTPS vagy HTTP/HTTPS) egy adott protokoll használatával. Ez azt jelenti, ha csak a HTTPS-forgalom tooallow szeretné, csak a szükséges hello protokoll tooHTTPS állíthatja be, és HTTP-forgalom le lesz tiltva.

#### <a name="definition-of-a-shared-access-signature"></a>A közös hozzáférésű Jogosultságkód meghatározása
Egy közös hozzáférésű Jogosultságkód lekérdezési paraméterek toohello URL-t, a hello erőforrás lesz hozzáfűzve.

hello hozzáférésével kapcsolatos információkat engedélyezett, és mennyi ideig mely hello hozzáférés engedélyezett hello biztosít. Íme egy példa a; Ezt az URI tooa blob olvasási hozzáférést biztosít a öt perc. Vegye figyelembe, hogy a biztonsági Társítások lekérdezési paramétert kell URL-kódolású, például % 3A kettőspont (:) vagy a szóközt 20 %.

```
http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL toohello blob)
?sv=2015-04-05 (storage service version)
&st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
&se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
&sr=b (resource is a blob)
&sp=r (read access)
&sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
&spr=https (only allow HTTPS requests)
&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for hello authentication of hello SAS)
```

#### <a name="how-hello-shared-access-signature-is-authenticated-by-hello-azure-storage-service"></a>Hogyan közös hozzáférésű Jogosultságkód által hitelesített hello hello Azure Storage szolgáltatás
Amikor hello társzolgáltatás hello kérelmet kap, hello lekérdezési paraméterek fogadja el, és létrehoz egy aláírás használatával hello ugyanezt a módszert, mert a hívó program hello. Majd hello két aláírások hasonlítja össze. Ha elfogadja, majd hello tároló szolgáltatást is tekintse hello tárolási szolgáltatás verziója toomake meg arról, hogy érvényes ellenőrizze, hogy hello aktuális dátum és idő hello megadott időszakon belül, ellenőrizze a kért meg arról, hogy hello hozzáférés felel meg a toohello kérelmet, stb.

Például a fenti URL-cím, az Ha hello URL-cím lett mutat tooa fájl helyett a blob, a kérelem sikertelen lesz mert határoz meg, hogy a BLOB van közös hozzáférésű Jogosultságkód hello. Ha hello hívják meg a többi parancs tooupdate blob volt, mivel a hello közös hozzáférésű Jogosultságkód határozza meg, hogy engedélyezett-e a csak olvasási hozzáféréssel fognak működni.

#### <a name="types-of-shared-access-signatures"></a>Közös hozzáférésű Jogosultságkód típusai
* Egy szolgáltatási szint SAS lehet egy adott erőforráshoz használt tooaccess tárfiókokban. Néhány példa erre a tárolóban lévő blobok listájának lekérése, blob letöltése, egy tábla egy entitás frissítése, üzenetek tooa várólista hozzáadása vagy fájlmegosztás tooa fájl feltöltése.
* Egy fiók szintű SAS használt tooaccess bármi lehet, hogy a szolgáltatási szint SAS-kód nem használható. Emellett biztosíthat a beállítások tooresources, amelyek nem engedélyezettek a szolgáltatásiszint-SAS-kód, például a hello képességét toocreate tárolók, táblák, üzenetsorok és fájlmegosztások. Toomultiple szolgáltatást egyszerre is megadható. Például előfordulhat, hogy megkapja a valaki tooboth blobok és fájlokat a tárfiókban lévő.

#### <a name="creating-an-sas-uri"></a>Egy SAS URI-azonosító létrehozása
1. Létrehozhat egy ad hoc URI az igény szerinti határozni az összes olyan hello lekérdezési paraméterek minden alkalommal, amikor.

   Ez valóban rugalmas, de ha egy olyan logikai készlete, amelyek hasonló minden alkalommal, amikor paraméterek, a tárolt házirend használata segítenek meghatározni.
2. A tárolt hozzáférési házirend egy teljes tárolóhoz, a fájlmegosztásokhoz, a tábla vagy a várólista hozhat létre. Ezután ezzel hello alapjául a hello SAS URI-azonosítók hoz létre. Engedélyek tárolt hozzáférési házirendek alapján könnyen visszavonhatók. Akkor is minden egyes tároló, a várólista, a tábla vagy a fájlmegosztás definiált too5 házirendek össze.

   Például ha Ön volt toohave sokan olvasási hello blobot, amely egy adott tárolóhoz, létrehozhat egy tárolt hozzáférési házirend, amely szerint a "olvasási hozzáférést" és egyéb beállításokat, hogy minden alkalommal, amikor hello azonos. Majd hozhat létre egy SAS URI hello beállításainak hello tárolt házirend használatával és hello lejárati dátum/idő megadásával. hello ennek előnye, hogy nincs-e toospecify összes hello lekérdezési paramétert minden esetben.

#### <a name="revocation"></a>Visszavonási
Tegyük fel, hogy a biztonsági Társítások biztonsága sérült, vagy azt szeretné, hogy toochange azt a vállalati biztonsági vagy az előírásoknak való megfelelés követelmények miatt. Hogyan visszavonása hozzáférés tooa erőforrás, hogy a SAS használatával? Ez attól függ, hogy hogyan létrehozott hello SAS URI-t.

Ha ad hoc URI használata esetén lehetősége van három. SAS-tokenje rövid lejárati házirendek adja ki, és egyszerűen hello SAS tooexpire várja. Nevezze át, vagy (feltéve, hello token hatókörön belüli tooa egyetlen objektumhoz) hello erőforrás törlése. Hello tárfiókkulcsok módosíthatja. Az utolsó lehetőség nagy hatással lehet, attól függően, hogy hány szolgáltatásokat használ, hogy a tárfiók, és valószínűleg nincs meg a kívánt toodo néhány tervezés nélkül.

A tárolt házirend származó SAS használ, ha hozzáférés megszüntetheti a tárolt házirend hello visszavonása – ugyanúgy módosíthatja, már lejárt, vagy teljesen eltávolítja azt. Ez azonnal érvénybe lép, és minden SAS létre adott tárolt házirend érvényteleníti. Frissítése vagy eltávolítása hello hozzáférési házirendben tárolt hatással lehet a férnek hozzá, hogy adott tárolóhoz, a fájlmegosztást, a tábla vagy a várólista SAS keresztül, de ha hello készültek, hogy az ügyfelek egy új SAS kérnek, amikor hello régi érvénytelenné válik, Ez jól működnek.

Mert a tárolt házirend származó SAS használatával lehetővé teszi, hogy SAS azonnal, a rendszer hello ajánlott bevált gyakorlat tooalways hello képességét toorevoke használja tárolt hozzáférési házirendeket, ha lehetséges.

#### <a name="resources"></a>Erőforrások
További részletes információt a megosztott hozzáférési aláírásokkal és tárolt hozzáférési házirendeket, kész, de példák tekintse meg a következő cikkek toohello:

* Ezek a hello útmutatót.

  * [Szolgáltatásalapú SAS](https://msdn.microsoft.com/library/dn140256.aspx)

    Ez a cikk egy szolgáltatási szint SAS használatával blobokat, az üzenetsor-üzeneteket, a tábla tartományokkal és a fájlok példákat.
  * [A szolgáltatásalapú SAS létrehozása](https://msdn.microsoft.com/library/dn140255.aspx)
  * [SAS fiók létrehozása](https://msdn.microsoft.com/library/mt584140.aspx)
* Ezek a oktatóanyagok hello .NET ügyfél könyvtár toocreate megosztott hozzáférési aláírásokkal és a hozzáférési házirendek tárolja.

  * [Közös hozzáférésű Jogosultságkód (SAS) használatával](../storage-dotnet-shared-access-signature-part-1.md)
  * [Közös hozzáférésű Jogosultságkód, 2. rész: Létrehozása és SAS-kód használata hello Blob szolgáltatás](../blobs/storage-dotnet-shared-access-signature-part-2.md)

    A cikk magyarázatot hello SAS-modell, megosztott hozzáférési aláírásokkal, példákat tartalmaz, és javaslatok hello célszerű SAS használja. Azt is ismertetjük, hello visszavonása hello engedéllyel.
* A hozzáférés korlátozása IP-címet (IP ACL) alapján

  * [Mi az a végpont hozzáférés-vezérlési lista (ACL)?](../../virtual-network/virtual-networks-acl.md)
  * [A szolgáltatásalapú SAS létrehozása](https://msdn.microsoft.com/library/azure/dn140255.aspx)

    Ez az hello áttekintésével foglalkozó cikkben a szolgáltatásiszint-SAS; Ez magában foglalja az IP-ACLing példát.
  * [SAS fiók létrehozása](https://msdn.microsoft.com/library/azure/mt584140.aspx)

    Ez az a fiók szintű SAS; hello áttekintésével foglalkozó cikkben Ez magában foglalja az IP-ACLing példát.
* Authentication

  * [Hello Azure Storage szolgáltatásainak hitelesítése](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* Közös hozzáférésű Jogosultságkód első lépések útmutató

  * [Első lépések útmutató SAS](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

## <a name="encryption-in-transit"></a>Az átvitel során titkosítás
### <a name="transport-level-encryption--using-https"></a>Átviteli szintű titkosítást – HTTPS-kapcsolaton keresztül
Egy másik lépés megtétele tooensure hello adatok biztonsága érdekében az Azure Storage tooencrypt hello adatok hello ügyfél és az Azure Storage között. hello első ajánljuk, tooalways hello használata [HTTPS](https://en.wikipedia.org/wiki/HTTPS) protokoll, amely biztosítja a biztonságos kommunikáció érdekében keresztül hello nyilvános internethez.

toohave biztonságos kommunikációs csatornát, mindig használandó HTTPS tárolási objektumokat hello REST API-k hívása vagy eléréséhez. Emellett **megosztott hozzáférési aláírásokkal**, amely lehet használt toodelegate tooAzure tárolási objektum eléréséhez, egy beállítás toospecify, hogy csak HTTPS protokoll használható a megosztott hozzáférési aláírásokkal, ezzel biztosítható, hogy birtokában bárki használatakor hello tartalmazza az SAS-tokenje hivatkozások elküldésével hello megfelelő protokollt fogja használni.

A HTTPS protokoll használatát hello kényszerítheti a következő meghívásakor: hello REST API-k tooaccess objektumok tárfiókokban engedélyezésével [szükséges átviteli biztonságos](../storage-require-secure-transfer.md) hello tárfiók. Ha ez engedélyezve van a kapcsolatok HTTP-n keresztül program elutasítja.

### <a name="using-encryption-during-transit-with-azure-file-shares"></a>Az Azure fájlmegosztások titkosítással továbbítás során
Az Azure File storage REST API hello használata esetén támogatja a HTTPS PROTOKOLLT, de gyakrabban használt SMB-fájlmegosztás tooa VM hozzá van kapcsolva. SMB 2.1 nem támogatja a titkosítást, tehát kapcsolatok csak engedélyezett hello belül azonos Azure-régiót. Azonban az SMB 3.0 támogatja a titkosítást, és elérhető a Windows Server 2012 R2, Windows 8, Windows 8.1 és Windows 10, lehetővé téve a kereszt-régió hozzáférni, sőt akár hello asztalon hozzáférés.

Vegye figyelembe, hogy az Azure fájlmegosztások Unix használható, amíg hello Linux SMB-ügyfél még támogatja a titkosítást, így csak elérését az Azure-régiót belül. Linux titkosítás támogatása hello terv Linux fejlesztők SMB funkció felelős van. Ha titkosítás adnak hozzá, hogy ugyanazon képessége, egy Azure fájlmegosztás Linux eléréséhez a Windows hello.

Hello Azure fájlok szolgáltatás-titkosítás hello használatát kényszeríthetik engedélyezésével [szükséges átviteli biztonságos](../storage-require-secure-transfer.md) hello tárfiók. Ha a REST API-k használatával hello, HTTPs-kapcsolat szükséges. Az SMB-csak SMB-kapcsolatok, amely támogatja a titkosítást az sikeresen fog csatlakozni.

#### <a name="resources"></a>Erőforrások
* [Hogyan toouse Azure File storage Linux](../storage-how-to-use-files-linux.md)

  Ez a cikk bemutatja, hogyan toomount egy Azure-fájl megosztása egy Linux rendszer és a feltöltés/letöltési fájlokon.
* [Ismerkedés a Windowshoz készült Azure File Storage szolgáltatással](../storage-dotnet-how-to-use-files.md)

  Ez a cikk áttekintést nyújt az Azure fájlmegosztások és hogyan toomount, amelyekkel PowerShell és a .NET használatával.
* [Az Azure File Storage ismertetése](https://azure.microsoft.com/blog/inside-azure-file-storage/)

  Ez a cikk hello általános rendelkezésre állását Azure File storage időről és hello SMB 3.0-titkosítás vonatkozó technikai részleteket biztosít.

### <a name="using-client-side-encryption-toosecure-data-that-you-send-toostorage"></a>Ügyféloldali titkosítás toosecure adatokat, hogy küldjön toostorage használatával
Egy másik lehetőség, amelynek segítségével győződjön meg arról, hogy az adatok biztonságos egy ügyfélalkalmazást és a tároló között történő átvitel során az ügyféloldali titkosítás. hello adattitkosítás átvitel során az Azure Storage előtt. Hello adatok Azure Storage-ból beolvasásakor hello adatok visszafejtése hello ügyféloldalon fogadását követően. Annak ellenére, hogy hello adattitkosítás hello keresztülhaladnak a hálózaton keresztül is, azt javasoljuk, hogy is használja HTTPS, mert már van, amelyek segítenek hello adatok integritásának hello érintő hálózati hibák beépített adatok integritás-ellenőrzést.

Ügyféloldali titkosítás megegyezik is található, aktívan, az adatok titkosítására hello adatok a titkosított formában tárolja. Lesz döntésről bővebben a hello szakasz részletesen a [titkosítását](#encryption-at-rest).

## <a name="encryption-at-rest"></a>Aktívan titkosítása
Nincsenek három Azure szolgáltatást biztosító titkosítását. Az Azure Disk Encryption, az operációs rendszer használt tooencrypt hello és adatlemezek infrastruktúra-szolgáltatási virtuális gépeket. egyéb két hello – ügyféloldali titkosítás és SSE – amelyek mindkét használt tooencrypt adatokat az Azure Storage. Most ezen, tekintse meg a összehasonlítása és tekintse meg, ha azok már nem használható.

Használhatja az ügyféloldali titkosítás tooencrypt hello adatok az átvitel során (amely a tárolási titkosítás nélkül is tárol), azonban HTTPS toosimply használja inkább a hello átvitel során, és néhány hello adatok toobe automatikusan titkosított, amikor ez a lehetőség tárolja. Nincsenek két módon toodo ez--Azure Disk Encryption és SSE. Használt egyik toodirectly virtuális gépek által használt operációsrendszer- és adatlemezek hello adatok titkosításához, és más hello tooAzure Blob-tároló felhasznált tooencrypt adatokat.

### <a name="storage-service-encryption-sse"></a>Storage szolgáltatás titkosítási (SSE)
SSE lehetővé teszi, hogy hello tároló szolgáltatás automatikusan hello adatok titkosítása írásakor azt tooAzure tárolási toorequest. Olvasásakor hello adatok Azure Storage-ból, akkor lesz visszafejtve hello tároló szolgáltatás által visszaadott előtt. Ez lehetővé teszi az adatok anélkül, hogy toomodify code, vagy adja hozzá a kódot tooany alkalmazások toosecure.

Ez az a beállítást, amely a teljes tárfiókot toohello vonatkozik. Engedélyezi, és ez a funkció letiltása hello hello beállítás értékének módosításával. toodo, hello Azure-portálon PowerShell, hello Azure CLI használata, hello Storage erőforrás-szolgáltató REST API vagy hello .NET a Storage ügyféloldali kódtára. Alapértelmezés szerint az SSE ki van kapcsolva.

Jelenleg a Microsoft hello kulcsok hello titkosításhoz használt felügyeletét. Azt hello kulcsok létrehozásához eredetileg, és a belső Microsoft házirend által meghatározott hello biztonságos tárolására hello kulcsok, valamint a hello rendszeres Elforgatás felügyelete. Jövőbeli hello meg fog beolvasása hello képességét toomanage saját titkosítási kulccsal, és adja meg egy áttelepítési útvonal, a Microsoft által felügyelt kulcsok toocustomer által felügyelt kulcsok.

Ez a szolgáltatás hello erőforrás-kezelő telepítési modellel készült Standard és prémium szintű Storage-fiókok érhető el. SSE csak tooblock blobokat, lapblobokat, vonatkozik, és a hozzáfűző blobokhoz. hello más típusú adatok, beleértve a táblák, üzenetsorok és fájlok, nem lesznek titkosítva.

Csak adattitkosítás SSE engedélyezve van, és hello adatot ír tooBlob tároló. Engedélyezés vagy letiltás SSE nem befolyásolja a meglévő adatokat. Ez azt jelenti Ha engedélyezi ezt a titkosítást, nem lépjen vissza, és már létező; adatok titkosítása sem lesz, ha letiltja az SSE már létezik hello adatok visszafejtését.

Ha azt szeretné, toouse Ez a szolgáltatás egy klasszikus tárfiókot, hozzon létre egy új erőforrás-kezelő tárfiókot, és AzCopy toocopy hello adatok toohello új fiókot használja.

### <a name="client-side-encryption"></a>Ügyféloldali titkosítás
A Microsoft említett ügyféloldali titkosítás hello adatokat átvitel közben hello titkosításának ismertetésekor. Ez a funkció lehetővé teszi, hogy Ön tooprogrammatically titkosítani az adatokat egy ügyfélalkalmazást, mielőtt elküldené hello vezetékes toobe tooAzure tárolási írása között, és tooprogrammatically visszafejteni az adatokat az Azure Storage-ból beolvasása után.

Így a titkosítás az átvitel során, de emellett biztosítja az inaktív titkosítási hello szolgáltatást. Vegye figyelembe, hogy bár a hello adattitkosítás átvitel közben, továbbra is ajánlott hello beépített adatok integritás-ellenőrzést, amelyek segítenek hello adatok integritásának hello érintő hálózati hibák tootake előnyeit HTTPS használatával.

Amennyiben ezzel például ha egy webes alkalmazás, amely BLOB tárolja, és lekéri a blobok, de szeretné hello alkalmazás és adatok toobe megfelelő biztonsága. Ebben az esetben használja az ügyféloldali titkosítás. hello ügyfél és a hello Azure Blob szolgáltatás között hello forgalom titkosítva hello erőforrást tartalmaz, és senki sem hello adatok átvitel értelmezésére és a titkos blobok pótlására azt.

Ügyféloldali titkosítás hello Java és hello .NET storage ügyfélkódtáraival, amelyek viszont hello Azure kulcsot tároló API, így az Ön tooimplement igen egyszerű van beépítve. hello adatok titkosítása és visszafejtése hello folyamatán hello boríték módszerrel, és minden egyes tárolási objektum hello titkosításra által használt metaadatokat tárol. Például a blobok, tárolja azt hello blob metaadatai, a várólisták, hozzáadja az tooeach üzenetsorban lévő üzenetet.

Hello titkosítási magát Ön hozza létre és a saját titkosítási kulcsok kezeléséhez. Hello Azure Storage ügyféloldali kódtár által létrehozott kulcsok is használhatja, vagy beállíthatja, hogy az Azure Key Vault hello hello kulcsok létrehozásához. A titkosítási kulcsok tárolása a helyszíni kulcstároló, vagy tárolhatja őket az Azure Key Vault. Az Azure Key Vault lehetővé teszi toogrant hozzáférés toohello titkokat az Azure Key Vault toospecific felhasználók Azure Active Directory használatával. Ez azt jelenti, hogy nem csak birtokában bárki olvashatja hello Azure Key Vault, és lekéréséhez hello kulcsok ügyféloldali titkosítás használata.

#### <a name="resources"></a>Erőforrások
* [Titkosításához és visszafejtéséhez az Azure Key Vault használatával a Microsoft Azure Storage blobs](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)

  Ez a cikk bemutatja, hogyan toouse ügyféloldali titkosítás az Azure Key Vault, beleértve a hogyan toocreate hello KEK és a PowerShell használatával hello tárolóban tárolja.
* [A Microsoft Azure Storage ügyféloldali titkosítás és az Azure Key Vault](../storage-client-side-encryption.md)

  Ez a cikk ügyféloldali titkosítás magyarázatot ad, és példák a hello tárolási ügyfél tooencrypt és visszafejtése könyvtárerőforrások hello négy tárolási szolgáltatások használatával. Azt is Azure Key Vault szól.

### <a name="using-azure-disk-encryption-tooencrypt-disks-used-by-your-virtual-machines"></a>Használja az Azure Disk Encryption tooencrypt lemezt a virtuális gépek által használt
Az Azure Disk Encryption az új szolgáltatása. Ez a funkció lehetővé teszi tooencrypt hello az operációs rendszer és az infrastruktúra-szolgáltatási virtuális gép által használt adatok lemezek. A Windows hello meghajtók titkosítása szabványos BitLocker titkosítás technológia használatával. A Linux hello lemezek titkosítása hello DM-Crypt technológia használatával. Ez az integrálva van az Azure Key Vault tooallow meg toocontrol és hello lemez titkosítási kulcsok kezeléséhez.

hello megoldás támogatja a következő forgatókönyvek IaaS virtuális gépek esetén, ha engedélyezve van a Microsoft Azure-ban hello:

* Az Azure Key Vault integrációja
* Standard szint virtuális gépek: [A, D, DS, G, GS és stb adatsorozat IaaS virtuális gépeket](https://azure.microsoft.com/pricing/details/virtual-machines/)
* A Windows és Linux IaaS virtuális gépeket titkosítás engedélyezése
* Az operációs rendszer és az adatok titkosításának letiltása Windows IaaS virtuális meghajtók
* Linux IaaS virtuális gépeket az adatmeghajtók titkosításának letiltása
* Az infrastruktúra-szolgáltatási virtuális gépeken futó Windows-ügyfél operációs rendszert futtató titkosítás engedélyezése
* Csatlakoztatási elérési utakat tartalmazó kötetek titkosítás engedélyezése
* A Linux virtuális gépeken, a lemez (RAID) szétosztott mdadm használatával konfigurált titkosítás engedélyezése
* A Linux virtuális gépeken titkosítás engedélyezése adatlemezek LVM használatával
* A Windows virtuális gépeken, a tárolóhelyek használatával konfigurált titkosítás engedélyezése
* Minden Azure-nyilvános régió támogatottak.

hello megoldás nem támogatja a következő forgatókönyvek, funkcióit és technológia hello kiadásban hello:

* Az alapszintű csomag IaaS virtuális gépeket
* Linux IaaS virtuális gépeket egy operációs rendszer meghajtóján titkosításának letiltása
* Infrastruktúra-szolgáltatási virtuális gépeket hello klasszikus virtuális gép létrehozási módszerének használatával létrehozott
* Integráció a helyszíni kulcskezelő szolgáltatás
* Az Azure File storage (megosztott fájlrendszer), a hálózati fájlrendszer (NFS), a dinamikus köteteket és a Windows alapú szoftveres RAID-rendszerek használatára konfigurált virtuális gépek


> [!NOTE]
> A következő Linux terjesztésekről hello jelenleg támogatott Linux operációs rendszert futtató lemeztitkosítás: RHEL 7.2, CentOS 7.2n és Ubuntu 16.04.
>
>

Ez a szolgáltatás biztosítja, hogy a virtuális gépek lemezeit a összes adata titkosításra kerül-e az Azure Storage aktívan.

#### <a name="resources"></a>Erőforrások
* [Windows és Linux IaaS virtuális gépeket az Azure Disk Encryption](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)

### <a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Az Azure Disk Encryption, az SSE és az ügyféloldali titkosítás összehasonlítása
#### <a name="iaas-vms-and-their-vhd-files"></a>Infrastruktúra-szolgáltatási virtuális gépek és a VHD-fájlok
Infrastruktúra-szolgáltatási virtuális gépek által használt lemezek azt javasoljuk, Azure Disk Encryption. Ha bekapcsolja a SSE tooencrypt hello VHD-fájlokat, amelyek használt tooback azokat a lemezeket, az Azure Storage, de csak titkosítja a újonnan írt adatok. Ez azt jelenti, hogy hozzon létre egy virtuális Gépet, és engedélyez SSE hello tárfiók, amely a hello VHD-fájl tárolja, ha csak hello módosítása titkosítja, nem hello az eredeti VHD-fájlt.

Ha hello Azure Piactérről származó lemezkép virtuális gép létrehozása az Azure-ban egy [másolási sekély](https://en.wikipedia.org/wiki/Object_copying) hello kép tooyour az Azure Storage és a tárfiók nem titkosított akkor is, ha az SSE engedélyezve van. Hello VM hoz létre, és elindítja a hello lemezkép frissítése után SSE indul hello adatok titkosítására. Ezért ajánlott toouse Azure Disk Encryption virtuális gépeken hello Azure piactér képek alapján létrehozott, ha azt szeretné, teljesen titkosítottak.

Ha később egy előre titkosított virtuális Gépet az Azure a helyszíni, akkor tudja tooupload hello titkosítási kulcsok tooAzure Key Vault, és ezt a virtuális Gépet, hogy a helyszínen használt hello titkosítás használatának folytatásához. Az Azure Disk Encryption engedélyezett toohandle van ebben a forgatókönyvben.

Ha virtuális merevlemez nem titkosított a helyszíni, töltse fel az egyéni lemezképként hello gyűjteménye, és a virtuális gép kiépítéséhez. Ehhez hello Resource Manager-sablonok használatával, kérje meg az tooturn az Azure Disk Encryption amikor azt hello virtuális gép elindul.

Ha hozzá adatlemezt, és csatlakoztassa azt hello VM, bekapcsolhatja a Azure Disk Encryption a lemezen levő adatokat. Először titkosítja az adott adatok lemezek helyi, és hogy hello szolgáltatás alkalmazáskezelési réteg válasszon lehetőségek elleni tárolási Lusta írási, hello tárolási tartalom titkosítása.

#### <a name="client-side-encryption"></a>Ügyféloldali titkosítás
Ügyféloldali titkosítás a hello Ez a legbiztonságosabb módszer titkosítja az adatokat, mert átvitel előtt titkosítja azokat, és inaktív adatok hello titkosítja. Azonban azt igénylik, hogy a tároló, amely nem használó kód tooyour alkalmazások hozzáadása toodo. Ezekben az esetekben is a HTTPS protokollt használja a adatokat átvitel közben, és az SSE tooencrypt hello adatokat aktívan.

Az ügyféloldali titkosítással táblaentitásokat, az üzenetsor-üzeneteket és a blobok titkosíthatók. Az SSE blobok csak titkosíthatja. Ha a tábla- és várólista titkosított adatok toobe van szüksége, ügyféloldali titkosítás kell használnia.

Ügyféloldali titkosítás teljesen hello alkalmazás kezeli. Ez hello legbiztonságosabb módszert használja, de megkövetelik a toomake programozott módosítások tooyour alkalmazás, és kulcskezelés folyamatok helyen. Ha azt szeretné, hogy hello használna ez további biztonsági során átvitel során, és szeretné, hogy a tárolt adatok toobe titkosítva.

Ügyféloldali titkosítás hello ügyfél további terhelését, és rendelkezik tooaccount ehhez a méretezhetőség tervek, különösen akkor, ha titkosított és nagy mennyiségű adat átvitele.

#### <a name="storage-service-encryption-sse"></a>Storage szolgáltatás titkosítási (SSE)
Azure Storage SSE kezeli. SSE használatával nem nyújtanak a hello biztonsági hello adatok az átvitel során, de az adattitkosítás hello tooAzure tárolási változatlan formában. Nincs hatással a teljesítményre hello a szolgáltatás használata során.

Csak titkosítása blokkblobokat, hozzáfűző blobokat és lapblobokat SSE használatával. Ha tooencrypt tábla vagy várólista adatok van szüksége, érdemes ügyféloldali titkosítással történik.

Ha egy archív vagy a könyvtár a VHD-fájlokat az új virtuális gépek létrehozásának alapjaként használja, hozzon létre egy új tárfiókot, SSE engedélyezése és hello VHD-fájlok toothat fiók majd feltölteni. Ezeket a VHD-fájlok Azure Storage titkosítva legyen.

Ha az Azure Disk Encryption hello lemezek egy virtuális gép és a hello VHD-fájlokat tároló hello tárfiók engedélyezve SSE engedélyezve van, akkor fog működni részletes; kétszer titkosított újonnan írt adatok okoz.

## <a name="storage-analytics"></a>Storage Analytics
### <a name="using-storage-analytics-toomonitor-authorization-type"></a>Tárolási analitika toomonitor engedélyezési típus használatával
Minden tárfiók Azure Storage Analytics tooperform naplózás engedélyezése és a metrikák adatainak tárolásához. Egy remek eszköz toouse Ez akkor, ha szeretné, hogy toocheck hello teljesítménymutatók a tárfiók, vagy tootroubleshoot tárfiókot kell végrehajtani, mert a teljesítményproblémák lépnek.

Egy másik adat hello storage analytics naplók látható a hello hitelesítési módszer valaki tároló elérésekor használja. Például a Blob-tároló, megtekintheti azokat használni egy közös hozzáférésű Jogosultságkód vagy hello tárfiókok kulcsait, vagy ha elérhető hello blob lett nyilvános.

Ez valóban akkor hasznos, ha szorosan hozzáférés toostorage esetlegesen korán lehet. Például a Blob Storage tárolóban után az összes hello tárolók tooprivate is valósítja meg az SAS-szolgáltatás hello használata az alkalmazások teljes. Ezután ellenőrizheti a hello naplók rendszeresen toosee Ha blobok hello tárfiókok kulcsait, ami azt jelezheti a biztonság megsértése, érhető el, vagy ha hello blobok olyan nyilvános, de nem lehetnek.

#### <a name="what-do-hello-logs-look-like"></a>Mire hello naplók figyelni például?
Után engedélyeznie hello tárolási fiók metrikákat és naplózási hello Azure-portálon keresztül, analitikai adatok elindul tooaccumulate gyorsan. hello naplózása és az egyes szolgáltatásokhoz metrikák elkülönül; hello naplózási csak akkor ír, amikor a forgalom a tárfiókban lévő közben hello metrikák naplózza a rendszer, minden percben, óránként vagy naponta, attól függően, hogy hogyan konfigurálja.

hello naplók blokkblobokat a hello tárfiókban lévő $logs nevű tárolóban vannak tárolva. Ebben a tárolóban automatikusan jön létre, ha engedélyezve van a tárolási analitika. A tároló jön létre, nem törölhetők, de törölheti annak tartalmát.

Hello $logs tárolóban a mappa az egyes szolgáltatásokhoz, és majd nincsenek almappák a hello év/hónap/nap/óra. Az óra egyszerűen számozása hello naplókat. Ez az, hogy milyen hello könyvtárstruktúrát hasonlóan fog kinézni:

![Naplófájlok megtekintése](./media/storage-security-guide/image1.png)

A rendszer minden kérelem tooAzure tárolási naplózza. Íme egy naplófájlba, hello első néhány mezőket megjelenítő pillanatképet.

![Pillanatkép egy naplófájl](./media/storage-security-guide/image2.png)

Használható hello naplók tootrack hívások tooa tárfiók bármilyen tekintheti meg.

#### <a name="what-are-all-of-those-fields-for"></a>Mik azok a mezők összes?
Nincs tartalmazza hello hello sok mező a hello naplókat, valamint a használatuk felsorolt hello erőforrások az alábbi cikk. A mezők sorrendben hello listája itt található:

![Pillanatkép a mezők a naplófájlban](./media/storage-security-guide/image3.png)

Azt is érdekli GetBlob hello bejegyzéseket, és hogyan hitelesítik, ezért azt kell toolook bejegyzések művelet-típussal a "Get-Blob", és állapotának hello kérelem-(4<sup>th</sup> oszlop) és hello engedélyezési típusú (8<sup>th</sup> oszlop).

Például a hello első néhány sor a fenti hello listaelem hello kérelem-állapota "Sikeres", amely hello engedélyezési-type "hitelesített". Ez azt jelenti, hogy hello kérelem hello tárfiók kulcsa segítségével lett érvényesítve.

#### <a name="how-are-my-blobs-being-authenticated"></a>Hogyan vannak a blobok hitelesített?
Három olyan esetekben, amely azt is van.

1. hello blob nyilvános, valamint egy közös hozzáférésű Jogosultságkód nélkül egy URL-cím segítségével hozzáférés. Ebben az esetben hello kérelem-állapota "AnonymousSuccess" és hello engedélyezési-típusa "névtelen".

   1.0; 2015-11-17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**; 200; 124; 37; **Névtelen**; mystorage...
2. hello blob magánjellegű és egy közös hozzáférésű Jogosultságkód volt használva. Ebben az esetben hello kérelem-állapota "SASSuccess" és hello engedélyezési-type "sas".

   1.0; 2015-11-16T18:30:05.6556115Z; GetBlob; **SASSuccess**; 200; 416; 64; **SAS**; mystorage...
3. hello blob személyes, és hello biztonságitár-kulcs lett használt tooaccess azt. Ebben az esetben hello kérelem-állapota "**sikeres**"és hello engedélyezési-típus:"**hitelesített**".

   1.0; 2015-11-16T18:32:24.3174537Z; GetBlob; **Sikeres**; 206-os; 59; 22; **hitelesített**; mystorage...

Hello Microsoft Message Analyzert tooview használja, és ezek a naplók elemzése. Ez magában foglalja a keresési és szűrési lehetőségeket. Például érdemes a toosearch példányai GetBlob toosee Ha hello használata várt, azaz toomake meg arról, hogy valaki nem fér hozzá a tárfiók nem megfelelően.

#### <a name="resources"></a>Erőforrások
* [Storage Analytics](../storage-analytics.md)

  Ez a cikk a storage Analytics nyújt áttekintést, és hogyan tooenable őket.
* [Storage Analytics naplóformátumban](https://msdn.microsoft.com/library/azure/hh343259.aspx)

  Ez a cikk bemutatja, hello Storage Analytics naplóformátumban, és részletek hello elérhető mezők, beleértve a hitelesítés típusa, amely hello hello kéréshez használt hitelesítés típusa.
* [A figyelő a Tárfiókokat hello Azure-portálon](../storage-monitor-storage-account.md)

  Ez a cikk bemutatja, hogyan tooconfigure a mérőszámok figyelés és naplózás a tárfiók.
* [Azure Storage Metrics és a naplózás, az AzCopy és a Message Analyzer segítségével végpontok – hibaelhárítás](../storage-e2e-troubleshooting.md)

  Ez a cikk beszél hibaelhárítás hello Storage Analytics segítségével, és bemutatja, hogyan toouse hello Microsoft Message Analyzert.
* [Microsoft Message Analyzer üzemeltetési útmutató](https://technet.microsoft.com/library/jj649776.aspx)

  Ez a cikk hello Microsoft Message Analyzert hello hivatkozását, és magában foglalja a hivatkozások tooa oktatóanyag, a gyors üzembe helyezési és a szolgáltatások.

## <a name="cross-origin-resource-sharing-cors"></a>Eltérő eredetű erőforrások megosztása (CORS)
### <a name="cross-domain-access-of-resources"></a>Tartományok közötti elérést erőforrások
Ha egy tartományban futó webböngésző hajt végre egy HTTP-kérelem erőforrás más tartományokból, ez a lehetőség egy eltérő eredetű HTTP-kérelem. Például a contoso.com helyről HTML-lapot fabrikam.blob.core.windows.net üzemeltetett JPEG-fájlok kérelmet küld. Biztonsági okokból böngészők korlátozza a parancsprogramok, például a JavaScriptek belül kezdeményezett eltérő eredetű HTTP-kérelmekre. Ez azt jelenti, hogy ha néhány JavaScript-kódot egy weblapon a contoso.com tartományon kéri, hogy a fabrikam.blob.core.windows.net jpeg, hello böngésző nem engedélyezi a hello kérelem.

Ez funkciója az Azure Storage toodo rendelkezik? Is, ha statikus erőforrásokat, például a JSON- vagy XML-adatfájlok tárolja a Blob Storage a Fabrikam tárfiókot használ neve, hello tartomány hello eszközök lesz fabrikam.blob.core.windows.net és hello contoso.com webalkalmazás nem lesz képes tooaccess őket JavaScript használatával, mert hello tartományok eltérőek. Ez akkor is igaz, ha a kívánt Azure tárolási szolgáltatások – például a Table Storage – hello egyik toocall JSON-adatok toobe hello JavaScript-ügyfél által feldolgozott visszaadó.

#### <a name="possible-solutions"></a>Lehetséges megoldások
Egyirányú tooresolve Ez az egyéni tartománynév például a "storage.contoso.com" toofabrikam.blob.core.windows.net tooassign. hello probléma, hogy csak rendelhet hozzá, hogy az egyéni tartomány tooone tárfiók. Mi történik, ha hello eszközeit tárolják a több tárfiókot?

Egy másik módja tooresolve toohave hello webes alkalmazás act hello tárolási hívások proxyként azt. Ez azt jelenti, ha egy fájl tooBlob tárolási, hello webalkalmazás volna, vagy helyileg írják, és tooBlob tárolási másolja, vagy az ehhez a memóriába az összes és tooBlob tárolási írni. Alternatív megoldásként egy dedikált webes alkalmazás (például egy webes API), amely hello fájlok feltöltését, majd beírja őket tooBlob tárolási lehet írni. Mindkét módszer esetén, hogy adott funkcióval tooaccount Ha meghatározó hello méretezhetőséget.

#### <a name="how-can-cors-help"></a>Hogyan segíthetnek a CORS?
Az Azure Storage tooenable CORS – Cross eredetű erőforrások megosztása lehetővé teszi. Minden tárfiók hello erőforrásokhoz, hogy a tárfiók tartományokat is megadhat. Például a mi esetünkben a fent vázolt, azt is hello fabrikam.blob.core.windows.net tárfiók a CORS engedélyezése és tooallow hozzáférés toocontoso.com konfigurálja. Majd hello webes alkalmazás contoso.com közvetlenül a fabrikam.blob.core.windows.net hello erőforrások eléréséhez.

Egy dolog toonote, hogy a CORS lehetővé teszi a hozzáférést, de nem biztosít hitelesítést, amely szükséges az összes nem-nyilvános hozzáférés tároló-erőforrások. Ez azt jelenti, hogy csak blobokat, ha azok nyilvános vagy telepíthet egy közös hozzáférésű Jogosultságkód felkínálva hello megfelelő engedélyekkel. Táblák, üzenetsorok és fájlok nincs nyilvános hozzáférés, és a SAS-kód szükséges.

A CORS alapértelmezés szerint le van tiltva az összes szolgáltatás. A CORS hello REST API vagy hello tárolási ügyfél könyvtár toocall hello módszerek tooset hello szolgáltatás házirendek egyikét használatával engedélyezheti. Ha így tesz, meg kell adnia egy CORS szabály, amely az XML-Fájlban. Íme egy példa egy CORS szabályt, amely hello szolgáltatás tulajdonságainak beállítása művelet hello Blob szolgáltatás használ a tárfiók van beállítva. Ennek a műveletnek az Azure Storage hello a storage ügyféloldali kódtára vagy hello REST API-k használatával végezheti el.

```xml
<Cors>    
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
        <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
        <MaxAgeInSeconds>200</MaxAgeInSeconds>
    </CorsRule>
<Cors>
```

Ez minden egyes sorára jelenti:

* **AllowedOrigins** ez alapján, mely nem egyező tartományok kérelmezhet és fogadhat adatokat hello tároló szolgáltatást. Ez azt jelzi, hogy a contoso.com és fabrikam.com kérhetnek az adatok Blob-tároló egy adott tárfiók. Azt is beállíthatja a tooa helyettesítő karakter (\*) tooallow összes tartományok tooaccess kéri.
* **AllowedMethods** hello listájának módszert (HTTP-kérelem műveletek) használható, amikor hello kérést. Ebben a példában csak a PUT és a GET használható. Beállíthatja a tooa helyettesítő karakter (\*) összes módszerek toobe használt tooallow.
* **AllowedHeaders** hello kérelem forrástartományt hello fejlécek adhat meg, amikor hello kérést azt. Ebben a példában minden metaadat fejléc kezdve az x-ms-metaadatok, x-ms-metaadat-tároló és az x-ms-meta-abc engedélyezett. hello helyettesítő karakter (\*) azt jelzi, hogy bármely hello fejléc kezdődő megadott előtag engedélyezett.
* **ExposedHeaders** ez alapján mely válaszfejlécek elérhetővé tehető hello böngésző toohello kérelmet kibocsátó. Ebben a példában minden kezdve fejléc "x-ms - meta-" megjelenik.
* **MaxAgeInSeconds** hello maximális időt, hogy a böngésző gyorsítótárazza-e a hello ellenőrzési beállítások kérelem azt. (Hello elővizsgálati kérelmekre vonatkozó további információkért ellenőrizze a hello első cikkben.)

#### <a name="resources"></a>Erőforrások
További információ a CORS és hogyan tooenable, vegye ki ezeket az erőforrásokat.

* [Hello Azure Storage Services szolgáltatásról az Azure.com webhelyen Cross-Origin Resource Sharing (CORS) támogatása](../storage-cors-support.md)

  Ez a cikk a CORS és hogyan tooset hello szabályainak hello eltérőek a tárolási szolgáltatások áttekintése.
* [Hello Azure Storage szolgáltatásainak MSDN Cross-Origin Resource Sharing (CORS) támogatása](https://msdn.microsoft.com/library/azure/dn535601.aspx)

  Ez a CORS-támogatás hello Azure Storage szolgáltatások hello dokumentációját. Ez rendelkezik hivatkozások tooarticles alkalmazása tooeach tároló szolgáltatást, és példáját mutatja be, és egyes elemei hello CORS fájl ismerteti.
* [A Microsoft Azure Storage: A CORS bemutatása](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

  Ez az egy hivatkozás toohello kezdeti blog cikk a CORS bejelentése és megjelenítő hogyan toouse azt.

## <a name="frequently-asked-questions-about-azure-storage-security"></a>Azure Storage biztonsági kapcsolatos gyakori kérdések
1. **Hogyan ellenőrizhetem hello integritását I vagy abból az Azure Storage vagyok átvitele a hello HTTPS protokoll nem használható, ha hello blobok?**

   Ha bármilyen okból HTTP helyett HTTPS, és dolgozunk a blokkblobokhoz toouse van szüksége, használhatja az MD5 ellenőrzése toohelp ellenőrizni hello hello blobok átvitel során. Ez segít hálózati transport layer hibák elleni védelem, de nem feltétlenül közvetítő támadások.

   Ha a HTTPS-t, amely átvitelszintű biztonság, akkor használja, MD5 ellenőrzése redundáns és szükségtelen is használhatja.

   További információkért vegye ki hello [Azure Blob MD5 áttekintése](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).
2. **Mi a helyzet a FIPS-kompatibilitás az hello USA Kormánya?**

   az Amerikai Egyesült Államok Federal Information Processing Standard (FIPS) hello Egyesült Államok használatra jóváhagyott titkosítási algoritmusokat határozza meg Szövetségi kormányzati számítógépes rendszerek védelemre hello a bizalmas adatok. FIPS engedélyezése egy Windows server vagy az asztal mód be van állítva hello az operációs rendszer, hogy csak a FIPS használatával érvényesített kriptográfiai algoritmusok kell használni. Ha egy alkalmazás használja a nem megfelelő algoritmusok, hello alkalmazások megszakad. With.NET keretrendszer-verziók 4.5.2 vagy újabb rendszerre, hello alkalmazás automatikusan vált hello titkosítási algoritmusok toouse FIPS előírásainak megfelelő algoritmusok hello számítógép FIPS-módban van.

   Microsoft hagyja tooeach ügyfél toodecide fel e tooenable FIPS-módban. Úgy véljük, felhasználók, akik nincsenek tulajdonos toogovernment szabályzat tooenable FIPS-módban alapértelmezés szerint nincs kényszerítő ok.

   **Erőforrások**

* [Miért még nem javasoljuk "FIPS-módban" többé](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

  Blog cikkben FIPS áttekintést, és elmagyarázza, hogy miért ezek nem engedélyezi az FIPS-módban alapértelmezés szerint.
* [A FIPS 140 érvényesítése](https://technet.microsoft.com/library/cc750357.aspx)

  Ebben a cikkben megtudhatja hogyan Microsoft-termékek és a titkosítási modulok felel meg hello FIPS szabványnak az hello USA Szövetségi Kormánya.
* ["Rendszer-kriptográfia: használata FIPS szabványnak megfelelő algoritmusok használata titkosításhoz, kivonatoláshoz és aláíráshoz" biztonsági beállítások hatások a Windows XP és a Windows újabb verzióiban](https://support.microsoft.com/kb/811833)

  Ez a cikk beszél hello használata a régebbi Windows-számítógépek FIPS-módban.
