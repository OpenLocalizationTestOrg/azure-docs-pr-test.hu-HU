---
title: "a Data Lake Store biztonsági aaaOverview |} Microsoft Docs"
description: "Megértse, hogyan Azure Data Lake Store egy biztonságosabb big Data típusú adatok tárolási"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 69e20bd046a9427202074baf59bff13f5b6a83ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-in-azure-data-lake-store"></a>Az Azure Data Lake Store biztonsági
Sok vállalat üzleti elemzések toohelp őket ellenőrizze döntések intelligens big data-elemzések vannak kihasználásával. Egy szervezet előfordulhat, hogy rendelkezik egy összetett és szabályozott környezetben, a különböző felhasználók növekvő számú. Nagyon fontos az olyan vállalati toomake meg arról, hogy kritikus fontosságú üzleti adatokat tárolja a rendszer biztonsága érdekében együtt hello megfelelő szintű hozzáférési engedélyt biztosítottak tooindividual felhasználók. Azure Data Lake Store úgy van kialakítva toohelp e biztonsági követelményeknek. Ebből a cikkből megtudhatja, Data Lake Store hello biztonsági képességeivel kapcsolatos többek között:

* Authentication
* Engedélyezés
* Hálózatelkülönítés
* Adatvédelem
* Naplózás

## <a name="authentication-and-identity-management"></a>Hitelesítés és identitás kezelése
A hitelesítés az hello folyamat, amellyel a felhasználó identitásának ellenőrzése, ha hello felhasználói együttműködik a Data Lake Store vagy a bármely szolgáltatás, amely a tooData Lake Store. Az Identitáskezelés és a hitelesítéshez, Data Lake Store az [Azure Active Directory](../active-directory/active-directory-whatis.md), egy átfogó identitás- és hozzáférés kezelési felhőalapú megoldás, amely egyszerűbbé teszi a felhasználók és csoportok hello felügyeleti.

Lehet, hogy minden Azure-előfizetéssel társítva van egy példányát az Azure Active Directory. Csak a felhasználók és a szolgáltatás-identitások az Azure Active Directory szolgáltatásban meghatározott hozzáférhet a Data Lake Store-fiók, hello Azure-portálon parancssori eszközök segítségével, vagy a szervezet hello Azure adatok használatával hoz létre ügyfélalkalmazások keresztül Lake Store SDK. Az Azure Active Directoryt használja a központi hozzáférés-vezérlési mechanizmus főbb előnyei a következők:

* Egyszerűsített identitás életciklusának kezelésére. egy felhasználó vagy egy szolgáltatást (egyszerű szolgáltatásidentitás) hello identitásának hozható létre gyorsan és egyszerűen törlésével vagy hello könyvtárban hello fiók letiltása gyorsan visszavonva.
* Többtényezős hitelesítés. [A multi-factor authentication](../multi-factor-authentication/multi-factor-authentication.md) egy további biztonsági réteget biztosít a felhasználói bejelentkezéseket és tranzakciókat.
* Bármely ügyfél szabványos protokollhoz, például az OAuth vagy OpenID keresztül hitelesítést.
* Összevonási vállalati címtárszolgáltatások és a felhőalapú identitás-szolgáltatóktól.

## <a name="authorization-and-access-control"></a>Engedélyezési és hozzáférés-vezérlés
Azure Active Directory hitelesíti a felhasználót, hogy hello felhasználó hozzáférhessen az Azure Data Lake Store, engedélyezési vezérlők férnek hozzá a Data Lake Store engedélyeit. Data Lake Store elválasztja az engedélyezési fiókhoz kapcsolódó és a kapcsolódó adatok tevékenységek, a következő módon hello:

* [Szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) felhasználóifiók-kezelés az Azure által biztosított (RBAC)
* POSIX hozzáférés-Vezérlési hello tároló adatok elérése

### <a name="rbac-for-account-management"></a>Az RBAC felhasználóifiók-kezelés
Négy alapvető szerepkörök alapértelmezett Data Lake Store vannak definiálva. hello szerepkörök lehetővé teszik a különböző műveleteket egy Data Lake Store-fiók hello Azure-portálon, a PowerShell-parancsmagok és a REST API-kon keresztül. hello tulajdonos és közreműködő szerepkört hello fiók számos felügyeleti feladatot hajthat végre. Hello olvasó szerepkört toousers ki csak végezhet az adatokkal rendelhet hozzá.

![Az RBAC-szerepkörök](./media/data-lake-store-security-overview/rbac-roles.png "RBAC-szerepkörök")

Vegye figyelembe, hogy bár szerepkörök fiókkezelés vannak hozzárendelve, az egyes szerepkörök hozzáférési toodata hatással. Hozzáférés-vezérlési listák toocontrol hozzáférés toooperations, hogy a felhasználók által végrehajtható hello fájlrendszerben toouse van szüksége. a következő táblázat hello alapértelmezett szerepkörök jogosultságait és adatok hozzáférési jogosultságokat a(z) hello összegzését jeleníti meg.

| Szerepkörök | A rights Management | Adatok hozzáférési jogok | Magyarázat |
| --- | --- | --- | --- |
| Nem hozzárendelt szerepkör |None |Hozzáférés-vezérlési lista által szabályozott |hello felhasználó nem használhatja az Azure portál vagy az Azure PowerShell parancsmagok toobrowse Data Lake Store hello. hello felhasználó csak a parancssori eszközöket használhatja. |
| Tulajdonos |Összes |Összes |hello tulajdonos szerepköre felügyelő létrehozása. Ez a szerepkör mindent felügyelhetnek, és toodata teljes hozzáféréssel rendelkezik. |
| Olvasó |csak olvasható |Hozzáférés-vezérlési lista által szabályozott |hello olvasó szerepkört mindent megtekinthetnek felhasználóifiók-kezelés, például, hogy melyik felhasználó szerepét toowhich kapcsolatban. hello olvasó szerepkört nem módosíthatja. |
| Közreműködő |Hozzáadás és eltávolítás szerepkört kivéve |Hozzáférés-vezérlési lista által szabályozott |hello közreműködői szerepkör kezelheti adatait, például a központi telepítések és létrehozása és a riasztások kezelése az egyes funkcióit. hello közreműködői szerepkört hozzáadni és törölni a szerepkört. |
| Felhasználói hozzáférés rendszergazdája |Hozzáadása és eltávolítása a szerepkörök |Hozzáférés-vezérlési lista által szabályozott |hello felhasználói hozzáférés adminisztrátora szerepkör kezelheti a felhasználói hozzáférés tooaccounts. |

Útmutatásért lásd: [felhasználók vagy biztonsági csoport hozzárendelése tooData Lake Store-fiók](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Hozzáférés-vezérlési listák segítségével a fájlrendszerek műveleteihez
Data Lake Store hierarchikus fájlrendszer például a Hadoop elosztott fájlrendszerrel (HDFS), és támogatja [POSIX hozzáférés-vezérlési listák](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Azt szabályozza, hogy olvasási (r), (w) írási és végrehajtási (x) engedélyek tooresources hello tulajdonosi szerepkört, hello tulajdonosok csoport és más felhasználókhoz és csoportokhoz. Hello Data Lake Store nyilvános előzetes (hello aktuális kiadás), a hozzáférés-vezérlési listák engedélyezhető hello legfelső szintű mappa, almappák és a fájlokat. További információkat a hozzáférés-vezérlési listák Data Lake Store-környezetben való működéséről a következő témakörben talál: [Hozzáférés-vezérlés a Data Lake Store-ban](data-lake-store-access-control.md).

Azt javasoljuk, hogy Ön hozzáférés-vezérlési listák többfelhasználós segítségével meghatározhatók [biztonsági csoportok](../active-directory/active-directory-accessmanagement-manage-groups.md). Felhasználók tooa biztonsági csoport hozzáadása, és hozzárendelheti a fájl vagy mappa toothat biztonsági csoport hello ACL-listát. Ez akkor hasznos, ha azt szeretné, hogy egyéni hozzáférési tooprovide, mert Ön korlátozott tooadding egy legfeljebb kilenc egyéni hozzáférési. Hogyan toobetter biztonságos Azure Active Directory biztonsági csoportok használatával a Data Lake Store-ban tárolt adatok kapcsolatos további információkért lásd: [rendeljen felhasználók vagy biztonsági csoport az Azure Data Lake Store-fájlrendszer hozzáférés-vezérlési listák toohello](data-lake-store-secure-data.md#filepermissions).

![Normál és egyéni hozzáférési listában](./media/data-lake-store-security-overview/adl.acl.2.png "normál és egyéni hozzáférési listában")

## <a name="network-isolation"></a>Hálózatelkülönítés
Használjon Data Lake Store toohelp vezérlő hozzáférés tooyour adattárolási hello szintjén. Állítson be tűzfalak, és az IP-címtartományok definiálása a megbízható ügyfeleket. Az IP-címtartományt csak olyan ügyfelek hello definiált tartományon belüli IP-cím tooData Lake Store is elérheti.

![Tűzfalbeállítások és IP-hozzáférési](./media/data-lake-store-security-overview/firewall-ip-access.png "tűzfalbeállítások és IP-cím")

## <a name="data-protection"></a>Adatvédelem
Azure Data Lake Store teljes életciklusa során az adatok védelmére. Az adatok az átvitel során a Data Lake Store hello szabványos Transport Layer Security (TLS) protokoll toosecure adatok az hello hálózaton keresztül.

![A Data Lake Store titkosítási](./media/data-lake-store-security-overview/adls-encryption.png "titkosítási Data Lake Store-ban")

Data Lake Store is biztosít a hello fiókban tárolt adatok titkosítását. Kiválasztott toohave képes a titkosított adatok, vagy választhat titkosítás nélkül. Csatlakozott a titkosításhoz, ha a Data Lake Store-ban tárolt adatok állandó adathordozón titkosított előzetes toostoring. Ebben az esetben a Data Lake Store automatikusan titkosítja az adatokat a korábbi toopersisting, és visszafejti adatok előzetes tooretrieval, így teljes mértékben transzparens toohello ügyfél hello adatok elérése. Nincs szükség a hello ügyféloldali tooencrypt/visszafejtési adatok kód változás.

Kulcskezelés a Data Lake Store két üzemmódot biztosít a fő titkosítási kulcsok (MEKs), amelyek szükségesek a Data Lake Store hello tárolt adatokat visszafejtése kezeléséhez. Vagy engedélyezheti a Data Lake Store kezelheti a hello MEKs, vagy válassza az Azure Key Vault fiókkal hello MEKs tooretain tulajdonjogát. Kulcskezelés hello módját közben az Data Lake Store-fiók létrehozása közben adja meg. További információt a tooprovide titkosítással kapcsolatos konfigurációs, lásd: [Ismerkedés az Azure Data Lake Store használatának hello Azure Portal](data-lake-store-get-started-portal.md).

## <a name="auditing-and-diagnostic-logs"></a>Naplózási és diagnosztikai naplókat
Naplózási és diagnosztikai naplók, attól függően, hogy Ön által keresett naplókat a további tevékenységek kezelésével kapcsolatos vagy adatok kapcsolatos tevékenységeket is használhatja.

* Felügyelettel kapcsolatos tevékenységek Azure Resource Manager API-k, és a hello Azure-portálon keresztül naplók illesztett.
* Tevékenységek kapcsolódó adatok WebHDFS REST API-k, és a hello Azure-portálon keresztül diagnosztikai naplók illesztett.

### <a name="auditing-logs"></a>Naplófájlok
toocomply szabályozásoknak, egy szervezet szükség lehet megfelelő elkérése Ha toodig a meghatározott incidenseket. Data Lake Store rendelkezik beépített figyelés és naplózás, és minden fiók felügyeleti tevékenységeket naplózza.

A fióknak felügyeleti napló ellenőrzését, megtekintése és hello oszlopok kiválasztása, hogy szeretné-e toolog. Naplózási naplók tooAzure tárolási is exportálhatja.

![Naplók](./media/data-lake-store-security-overview/audit-logs.png "Naplók")

### <a name="diagnostic-logs"></a>Diagnosztikai naplók
Adat-hozzáférési napló ellenőrzését hello (a diagnosztikai beállítások) Azure-portálon beállítani, és hozzon létre egy Azure Blob storage-fiók hello naplók tárolására.

![Diagnosztikai naplók](./media/data-lake-store-security-overview/diagnostic-logs.png "diagnosztikai naplók")

Diagnosztikai beállítások konfigurálása után megtekintheti a hello hello naplók **diagnosztikai naplók** fülre.

Diagnosztikai naplók az Azure Data Lake Store munkavégzés további információkért lásd: [hozzáférni a diagnosztikai naplókat a Data Lake Store](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Összefoglalás
A vállalati ügyfelek egy analytics felhőalapú adatplatform, amely biztonságosabb és egyszerűbb toouse igény. Azure Data Lake Store úgy van kialakítva toohelp cím ezeket a követelményeket, az Identitáskezelés és a hitelesítési Azure Active Directory-integráció, ACL-alapú engedélyezési, a hálózati elkülönítés, adattitkosítás átvitel közben, és inaktív (hamarosan hello keresztül jövőbeli), és a naplózás.

Ha azt szeretné, hogy a Data Lake Store toosee új funkcióiról, küldjön visszajelzést a hello [Data Lake Store UserVoice fórum](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Lásd még:
* [Az Azure Data Lake Store áttekintése](data-lake-store-overview.md)
* [Bevezetés a Data Lake Store használatába](data-lake-store-get-started-portal.md)
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)

