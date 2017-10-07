---
title: "Azure Storage használó aaaSecuring PaaS alkalmazások |} Microsoft Docs"
description: " Tudnivalók Azure Storage biztonsági gyakorlati tanácsok a PaaS webes és mobilalkalmazásokhoz biztonságossá tételéhez. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: TomShinder
ms.openlocfilehash: 3fed75cb121e7f32eb8b948ee12ca35fb25eca7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-paas-web-and-mobile-applications-using-azure-storage"></a>PaaS webes és mobilalkalmazások Azure Storage használatával biztonságossá tétele
Ez a cikk arról lesz szó Azure Storage ajánlott biztonsági eljárások az PaaS webes és mobilalkalmazások védelme gyűjteménye. Az alábbi gyakorlati tanácsok az Azure-ral tapasztalatunk származik, és ügyfelek hello lép, például saját maga.

Hello [Azure Storage biztonsági útmutató](../storage/common/storage-security-guide.md) részletes információt az Azure Storage és a biztonsági nagyszerű forrás.  Ez a cikk megoldást magas szinten néhány hello fogalmak hello biztonsági útmutató és hivatkozások toohello biztonsági útmutatója tartalmazza, valamint a más forrásokból, további információ található.

## <a name="azure-storage"></a>Azure Storage
Azure teszi lehetséges toodeploy és -felhasználási tárolási módon könnyen elérhető helyi. Az Azure storage méretezhetőségi és viszonylag kevés munkát rendelkezésre állási magas szintű érheti el. Nem csak az Azure storage hello foundation van a Windows és Linux Azure Virtual Machines, azt is képes támogatni nagy elosztott alkalmazások.

Az Azure storage hello a következő négy szolgáltatást biztosítja: Blob-tároló, a Table storage, Queue storage és a File storage. több, lásd: toolearn [Azure Storage bemutatása tooMicrosoft](../storage/storage-introduction.md).

## <a name="best-practices"></a>Ajánlott eljárások
Ez a cikk a következő gyakorlati tanácsok hello foglalkozik:

- Hozzáférés-védelem:
   - Közös hozzáférésű jogosultságkódok (SAS)
   - Felügyelt lemezes
   - Szerepköralapú hozzáférés-vezérlés (RBAC)

- Tárolás titkosítása:
   - Ügyféloldali titkosítása az értékes adatok
   - Az Azure Disk Encryption virtuális gépek (VM)
   - Storage Service Encryption

## <a name="access-protection"></a>Hozzáférés-védelem
### <a name="use-shared-access-signature-instead-of-a-storage-account-key"></a>Közös hozzáférésű Jogosultságkód használata helyett a tárfiók kulcsa

Infrastruktúra-szolgáltatási megoldásban, általában a Windows Server vagy Linux rendszerű virtuális gépek futó fájlokat védi a közzététel és hozzáférés-vezérlő mechanizmusok használatával ügyfelén fenyegetések. Használja a Windows [hozzáférés-vezérlési listák (ACL)](../virtual-network/virtual-networks-acl.md) és valószínűleg használna Linux [chmod](https://en.wikipedia.org/wiki/Chmod). Alapvetően Ez az pontosan mit kellene tennie, ha a kiszolgálón a saját adatközpont napjainkban volt védelmét.

A PaaS nem egyezik. Hello leggyakoribb módon toostore fájlokat a Microsoft Azure-ban egyik toouse [Azure Blob Storage tárolóban](../storage/storage-dotnet-how-to-use-blobs.md). A Blob storage és egyéb fájlok tárolására hello fájl i/o, de a fájl i/o hello védelmi módszert.

Hozzáférés-vezérlés fontos. szabályozhatja a hozzáférést tooAzure tárolási toohelp, hello rendszer hoz létre két 512 bites tárfiókkulcsok (SAKs) amikor Ön [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md). kulcs redundanciájának szintjét hello lehetővé teszi, tooavoid szolgáltatás megszakítás rutin kulcs Elforgatás során.

Tárelérési kulcsok magas prioritású titkok és használatuk csak akkor érhető el toothose tároló hozzáférés-vezérlés felelős. Hello illetéktelenek szerezze be a hozzáférési toothese kulcsok, ha azok fog rendelkezik teljes körű vezérlést biztosítanak a tárolási és nem sikerült cserélje le, törlése vagy hozzáadása fájlok toostorage. Ez magában foglalja a kártevő szoftverek és más potenciálisan csökkenthetik munkahelye vagy az ügyfelek tartalmat.

Szüksége van egy módon tooprovide hozzáférés tooobjects tárolóban. részletesebb tooprovide elérhető, kihasználhatják a [közös hozzáférésű Jogosultságkód](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (SAS). hello SAS lehetővé teszi az Ön tooshare konkrét objektumokat, megőrzés egy előre meghatározott időtartam alatt, és meghatározott engedélyekkel. Egy közös hozzáférésű Jogosultságkód toodefine lehetővé teszi:

- hello keresztül mely hello SAS érvényességi időtartama, beleértve hello kezdési ideje és hello lejárati idő.
- hello engedélyekre hello SAS. Például a blob SAS előfordulhat, hogy adjon egy felhasználó olvasási és írási engedélyek toothat blob, de nem törli az engedélyeket.
- Azure Storage fogad el egy nem kötelező IP-címet vagy az IP-címek hello SAS. Például előfordulhat, hogy adja meg az IP-címek egy tartozó tooyour szervezet. Ez lehetővé teszi a biztonsági Társítások biztonsági egy másik mérték.
- hello protokoll, amelyben Azure Storage hello SAS fogad el. Ez nem kötelező paraméter toorestrict hozzáférés tooclients HTTPS-kapcsolaton keresztül is használhatja.

SAS tooshare tartalom hello kívánt tooshare lehetővé teszi a Tárfiók kulcsait átruházása nélkül. Mindig az alkalmazásban használt SAS van egy biztonságos módon tooshare a tárolási erőforrások a tárfiók kulcsait veszélyeztetése nélkül.

több, lásd: toolearn [megosztott hozzáférési aláírásokkal használatával](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (SAS). További információk az esetleges kockázatokat és javaslatok toomitigate toolearn kockázatok, lásd: [ajánlott eljárásokat, amikor a SAS használatával](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="use-managed-disks-for-vms"></a>Használja a felügyelt lemezeit a virtuális gépek

Ha úgy dönt, [Azure felügyelt lemezek](../storage/storage-managed-disks-overview.md), Azure kezeli hello storage-fiókok, amelyek a virtuális gép lemezeit használja. Toodo szüksége a lemez (prémium és Standard) és hello lemezméretet; hello típusának kiválasztása A gyakorlatban elvégzi az Azure storage hello rest. Előfordulhat, hogy egyéb szükség tooyou toomultiple tárfiókok méretezhetőségének korlátai kapcsolatos tooworry nincs.

több, lásd: toolearn [gyakran ismételt kérdések készül felügyelt és nem felügyelt a premium lemezek](../storage/storage-faq-for-disks.md).

### <a name="use-role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés használata

Korábban tárgyalt közös hozzáférésű Jogosultságkód (SAS) toogrant korlátozott hozzáférés tooobjects használatát a tárolási fiók tooother ügyfelek anélkül, hogy a fiók tárfiók kulcsára. Egyes esetekben a tárfiók egy adott művelethez társított hello kockázatok járó SAS hello előnyeit. Esetenként egyszerűbb toomanage hozzáférési más módon.

Egy másik módja toomanage hozzáférést toouse [átruházásához hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) (RBAC). Az RBAC, hello van szükségük, pontos engedélyeket ad az alkalmazottak a fókusz a hello kell tooknow és a minimális jogosultság biztonsági alapelvek alapján. Túl sok engedélyek egy fiók tooattackers is elérhetővé teheti. Túl kevés engedélyek, az azt jelenti, hogy az alkalmazottak nem munkavégzéséhez hatékony. Az RBAC segít a részletes hozzáféréskezelést az Azure felajánlásával oldja meg a problémát. Ez elengedhetetlen a szervezeteknek, amelyek szeretné, hogy az adatelérési tooenforce biztonsági házirendeket.

Kihasználhatja a beépített RBAC szerepkörök az Azure tooassign jogosultságokkal toousers. Fontolja meg a felhő üzemeltetői toomanage storage-fiókok és a klasszikus tárolási fiók közreműködői szerepkör toomanage klasszikus tárfiókokba igénylő tárolási fiók közreműködői használatát. A felhő üzemeltetői igénylő virtuális gépek toomanage, de nem hello virtuális hálózati vagy tárolási fiók toowhich vannak csatlakoztatva fontolja meg, azokat toohello virtuális gép közreműködő szerepkört.

A szervezeteknek, amelyek kényszeríti ki a hozzáférés-vezérlés képességeinek például RBAC által előfordulhat, hogy kell jogosultságot ad mint azok a felhasználók számára szükséges további engedélyekkel. Ennek eredményeképpen előfordulhat, toodata biztonsági sérülés azáltal, hogy egyes felhasználók hozzáférési toodata hello első helyen nem rendelkeznek.

toolearn RBAC kapcsolatos információkért tekintse meg:

- [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)
- [Az Azure szerepköralapú hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md)
- [Az Azure Storage biztonsági útmutató](../storage/common/storage-security-guide.md) hogyan toosecure a tárolási fiók az RBAC a részletek

## <a name="storage-encryption"></a>Tárolás titkosítása
### <a name="use-client-side-encryption-for-high-value-data"></a>Ügyféloldali titkosítás használata értékes adatok

Ügyféloldali titkosítás lehetővé teszi, hogy Ön tooprogrammatically adatokat átvitel közben tooAzure tárolási feltöltés előtt titkosítja, és programozott módon fejteni az adatokat, amikor fogadása a tárolási.  Ez az átvitel során adatok titkosítását biztosítja, de emellett biztosítja az inaktív adatok titkosítását.  Ügyféloldali titkosítás hello legbiztonságosabb módszer az adatok titkosítása, azonban használatba toomake programozott módosítások tooyour alkalmazás, kulcskezelés folyamatok vezetnek be.

Ügyféloldali titkosítás is lehetővé teszi a titkosítási kulcsok toohave egyedüli irányítása.  Ön hozza létre, és a saját titkosítási kulcsok kezeléséhez.  Ügyféloldali titkosítás használ egy boríték technika, ahol hello Azure storage ügyféloldali kódtár állít elő, a tartalom titkosítási kulcsot (CEK), ezt követően (titkosított) hello kulcs titkosítási kulcs (KEK) használatával. hello KEK kulcsazonosítójával azonosíthatók és aszimmetrikus kulcspár, vagy egy szimmetrikus kulcsot és is kezelhetők helyileg vagy tárolt [Azure Key Vault](../key-vault/key-vault-whatis.md).

Ügyféloldali titkosítás hello Java és hello .NET storage ügyfélkódtáraival van beépítve.  Lásd: [ügyféloldali titkosítás és a Microsoft Azure tárolás az Azure Key Vault](../storage/storage-client-side-encryption.md) lévő adatok ügyfélalkalmazásokon belüli titkosítását és létrehozása és kezelése a saját titkosítási kulccsal.

### <a name="azure-disk-encryption-for-vms"></a>Az Azure Disk Encryption virtuális gépekhez
Az Azure Disk Encryption egy olyan képességet, amely segít a Windows és Linux IaaS virtuális gépek lemezeit titkosításához. Az Azure Disk Encryption hello iparági szabványos BitLocker-szolgáltatás a Windows és Linux operációs rendszer hello tooprovide kötettitkosítást hello DM-Crypt funkciója és hello adatlemezek kihasználja. hello megoldás integrálva van az Azure Key Vault toohelp vezérlik, és hello lemez-titkosítási kulcsokat és titkos kódokat a kulcstároló-előfizetésben. hello megoldás biztosítja azt is, hogy a virtuálisgép-lemezeket hello lévő összes adat titkosítva legyenek az Azure tárolás közben.

Lásd: [Windows és Linux IaaS virtuális gépeket az Azure Disk Encryption](azure-security-disk-encryption.md).

### <a name="storage-service-encryption"></a>Storage Service Encryption
Ha [Storage szolgáltatás titkosítási](../storage/storage-service-encryption.md) a File storage használata engedélyezett, hello adatok titkosítása automatikusan az AES-256 titkosítás a. A Microsoft kezeli az összes hello titkosítási, visszafejtési és kulcskezelést. Ez a szolgáltatás LRS, GRS és redundancia típus esetében érhető el.

## <a name="next-steps"></a>Következő lépések
Ez a cikk vezette be Azure Storage ajánlott biztonsági eljárások az PaaS webes és mobilalkalmazások védelme tooa gyűjteménye. További információ a PaaS üzemelő példányok védelme toolearn lásd:

- [PaaS-környezetek védelme](security-paas-deployments.md)
- [A PaaS webes és mobilalkalmazásokhoz, az Azure App Services biztonságossá tétele](security-paas-applications-using-app-services.md)
- [Az Azure-ban PaaS-adatbázisok védelme](security-paas-applications-using-sql.md)
