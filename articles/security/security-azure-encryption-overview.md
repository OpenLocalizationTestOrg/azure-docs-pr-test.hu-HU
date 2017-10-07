---
title: "aaaAzure titkosítási áttekintése |} Microsoft Docs"
description: "További tudnivalók az Azure-ban különböző titkosítási beállítások"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/18/2017
ms.author: barclayn
ms.openlocfilehash: ef9ab46de32b857e99e8fe628a61386b95cf197f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-overview"></a>Az Azure titkosítási áttekintése

Ez a cikk áttekintése titkosítási használatáról a Microsoft Azure-ban. Bemutatja, hello főbb területet a titkosítás, a felhőszolgáltató közötti átviteléhez, és a Key Vault kulcskezelés titkosítását, a titkosítási is beleértve. Minden szakasz hivatkozásokat részletes információkat tartalmaz.

## <a name="encryption-of-data-at-rest"></a>A tárolt adatok titkosítása

Inaktív adat fizikai adathordozó, bármilyen digitális formában állandó tárolóban lévő adatokat tartalmazza. Ez magában foglalja a fájlok mágneses vagy optikai adathordozón, az archivált adatok és az adatok biztonsági mentése. A Microsoft Azure különböző igényeket, ideértve a fájl, a lemez, a blob és a table storage adatok tárolási megoldások toomeet választékát kínálja. A Microsoft is biztosít a titkosítási tooprotect [Azure SQL Database](../sql-database/sql-database-technical-overview.md), [CosmosDB](../cosmos-db/introduction.md), és az Azure Data Lake.

Inaktív adatok titkosítása érhető el szolgáltatások között hello Azure szoftver,--szolgáltatás (SaaS) Platform,--szolgáltatás (PaaS), és a felhőszolgáltatás-modell infrastruktúra,--szolgáltatás (IaaS). Ez a dokumentum összefoglalja, és erőforrások toohelp biztosít Azure titkosítási beállításait használja.

Részletesebb leírását a hogyan inaktív adatok titkosítása az Azure-ban, lásd: hello dokumentum című [Azure Data Encryption nyugalmi](azure-security-encryption-atrest.md)

## <a name="azure-encryption-models"></a>Az Azure titkosítási modellek

Azure különböző titkosítási modellek, beleértve a szolgáltatás által kezelt kulcsokkal, felhasználó által felügyelt kulcsok használata az Azure Key Vault vagy kulcsokkal ügyfél által felügyelt ügyfél által felügyelt hardveren kiszolgálóoldali titkosítás támogatja. Ügyféloldali titkosítás lehetővé teszi, hogy a helyszíni toomanage és a tároló kulcsait, vagy egy másik biztonságos helyen.

### <a name="client-side-encryption"></a>Ügyféloldali titkosítás

Ügyféloldali titkosítás Azure-on kívüli történik. Ügyféloldali titkosítás tartalmazza:

- Az alkalmazások által létrehozott hello ügyfél adatközpontban fut, vagy a szolgáltatásalkalmazás titkosított adatok
- Azure által fogadott már titkosított adatokat.

Az ügyféloldali titkosítással hello felhőbeli szolgáltatás szolgáltatója nem rendelkezik hozzáféréssel toohello titkosítási kulcsokat, és nem tudja visszafejteni ezeket az adatokat. Teljes körű vezérlést biztosítanak hello kulcsok megmaradjanak.

### <a name="server-side-encryption"></a>Kiszolgálóoldali titkosítás

hello három kiszolgálóoldali titkosítás modellt kínál különböző kulcskezelés jellemzőkkel rendelkezik, amely igényeinek / választható ki.

- **Szolgáltatás-felügyelet alatt álló kulcsok** alacsony többletterhelést vezérlő és a felhasználók kényelme érdekében adjon meg

- **Ügyfél által felügyelt kulcsok** segítségével meghatározhatja hello kulcsok, többek között a hello képességét toobring a saját kulcs (használatának BYOK) vagy toogenerate újakat.

- **Kulcsok szolgáltatás által kezelt ccustomer-controlledhardware** lehetővé teszi a toomanage kulcsok a szellemi tulajdont képező tárházban Microsoft érdekeltségébe kívül esik. Ennek meghívása az állomás a saját Key (HYOK). Konfigurációs azonban bonyolult, és az Azure szolgáltatások nem támogatják ezt a modellt.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption

Windows és Linux virtuális gépek védhetők használatával [Azure Disk Encryption](azure-security-disk-encryption.md), hello használó [Windows BitLocker](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx) technológia és a Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) tooprotect operációsrendszer-lemez és adatlemezek a teljes kötettitkosítást.

Titkosítási kulcsok és titkos kulcsokat a védelméről a [Azure Key Vault](../key-vault/key-vault-whatis.md) előfizetés. Készítsen biztonsági másolatot, és állítsa vissza a titkosított virtuális gépek hello KEK konfigurációval hello Azure Backup szolgáltatás használatával titkosított.

### <a name="azure-storage-service-encryption"></a>Az Azure Storage szolgáltatás titkosítási

Az Azure storage (Blob és fájl) inaktív adatok titkosíthatók a kiszolgálóoldali és az ügyféloldali forgatókönyvek esetében.

[Az Azure Storage szolgáltatás titkosítási](../storage/storage-service-encryption.md) (SSE) automatikusan az adatokat titkosíthatja előtt tárolja, és automatikusan visszafejti azokat amikor, így teljes mértékben transzparens felhasználók feldolgozni hello. Storage szolgáltatás titkosítási használ, 256 bites [AES titkosítási](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), amely egyik hello legerősebb blokk Rejtjelek érhető el, és kezelje a titkosítási, visszafejtési és kulcskezelés átlátható módon.

### <a name="client-side-encryption-of-azure-blobs"></a>Azure-blobokat ügyféloldali titkosítása

Az Azure-blobokat ügyféloldali titkosítás különböző módon hajtható végre.

Hello Azure Storage ügyféloldali kódtára a .NET NuGet csomag tooencrypt adatok belül használható az ügyfél alkalmazások előzetes toouploading azt tooAzure tároló.

toolearn további információk és a letöltési hello Azure Storage ügyféloldali kódtára a .NET NuGet-csomagot, lásd: hello dokumentum című [Windows Azure Storage 8.3.0](https://www.nuget.org/packages/WindowsAzure.Storage)

Ügyféloldali titkosítás az Azure Key Vault használatakor az adatok titkosítása a egy egyszeri szimmetrikus tartalom titkosítási kulcs (CEK) hello Azure Storage ügyfél SDK által generált. hello CEK titkosítása egy kulcs titkosítási kulcscserekulcs (KEK), amely lehet, vagy a szimmetrikus kulcs, vagy az aszimmetrikus kulcspár. Az adatbázis felügyeletét helyileg, vagy tárolja az Azure Key Vault. hello titkosított adatok van majd feltöltött tooAzure tároló szolgáltatást.

További információk az Azure Key Vault ügyféloldali titkosítás toolearn és első lépések útmutató-tooinstructions, című dokumentumban találhatók hello [oktatóanyag: titkosításához és visszafejtéséhez az Azure Key Vault használatával a Microsoft Azure Storage blobs](../storage/storage-encrypt-decrypt-blobs-key-vault.md)

Java tooperform ügyféloldali titkosítás előtt adatfeltöltés adatok tárolási tooAzure és toodecrypt hello letöltésekor azt toohello ügyfél végül hello Azure Storage ügyféloldali kódtár is használhatja. A kódtár emellett támogatja való integráció [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) a tárfiókkulcs-kezelés.

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Az Azure SQL database inaktív adatok titkosítása

[Az Azure SQL Database](../sql-database/sql-database-technical-overview.md) van egy általános célú relációs adatbázis-szolgáltatás, például a relációs adatok, a JSON térbeli, és az XML-szerkezetek támogató Microsoft Azure-ban. Az Azure SQL keresztül hello átlátszó Data Encryption (TDE) funkció kiszolgálóoldali titkosítás és ügyféloldali titkosítás keresztül hello mindig titkosítja funkció támogatja.

#### <a name="transparent-data-encryption"></a>Transzparens adattitkosítás

[TDE átlátható adattitkosítási](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) használt tooencrypt van [SQL Server](https://www.microsoft.com/sql-server/sql-server-2016), [Azure SQL Database](../sql-database/sql-database-technical-overview.md), és [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) valós idejű adatok fájlok adatbázis-titkosítási kulcs (adattitkosítási kulcs), ami a hello adatbázis rendszerindító rekord a rendelkezésre állás érdekében helyreállítás során használ.

TDE védi adatainak és naplókönyvtárainak fájlok AES és a 3DES titkosítási algoritmus használatával. Titkosítási hello adatbázisfájl történik hello lap szinten; egy titkosított adatbázis hello lapok toodisk írás előtt titkosítja, és lesznek visszafejtve, amikor azok még a memóriába. TDE engedélyezve van alapértelmezés szerint az újonnan létrehozott Azure SQL-adatbázisok.

#### <a name="always-encrypted"></a>Always Encrypted

Hello [mindig titkosítja](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) Azure SQL-szolgáltatás lehetővé teszi, hogy Ön tooencrypt belül ügyfél alkalmazások előzetes toostoring az Azure SQL Database, és lehetővé teszi a helyi adatbázis felügyeleti toothird tooenable delegálása Felek és azok, akik saját és tekinthetők meg és azok, akik a kezeléshez, de nem rendelkezhet hozzáférés tooit hello adatok elkülönítése karbantartása.

#### <a name="cellcolumn-level-encryption"></a>/ Cellaoszlopának a blokkszintű titkosítás

Az Azure SQL Database tooapply szimmetrikus titkosítási tooa adatoszlop Transact-SQL használatával lehetővé teszi. Ez a lehetőség [a blokkszintű titkosítás vagy oszlopot a blokkszintű titkosítás cella](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data) (törlése), mert használata tooencrypt adott oszlopok vagy a megadott cella adatait különböző titkosítási kulcsokkal. Ez lehetővé teszi mint TDE, amely titkosítja az adatokat lapjain részletesebb meghajtótitkosítási képességet.

Törlése használható tooencrypt adatokat, és szimmetrikus vagy aszimmetrikus kulcsok, egy tanúsítvány nyilvános kulcsának hello, vagy egy jelszót, 3DES használatával beépített funkciókkal rendelkezik.

### <a name="cosmos-db-database-encryption"></a>A cosmos DB az adatbázis-titkosítás

[Az Azure Cosmos DB](../cosmos-db/database-encryption-at-rest.md) Microsoft globálisan elosztott, több modellre adatbázis. A Cosmos DB nem felejtő (SSD-meghajtó) tárolt felhasználói adatokat titkosított alapértelmezett; Nincsenek nem vezérlők tooturn azt be és ki. Titkosítását számos biztonsági technológia, beleértve a biztonságos kulcs tárolása rendszerek, a titkosított hálózatokat és a kriptográfiai API-k segítségével történik. Titkosítási kulcsok a Microsoft által felügyelt, és a Microsoft belső iránymutatásokat / legyenek-e elforgatva.

### <a name="at-rest-encryption-in-azure-data-lake"></a>Azure Data Lake nyugalmi titkosítás

[Azure Data Lake](../data-lake-store/data-lake-store-encryption.md) egy vállalati szintű tárház minden típusú adatok gyűjtését egy helyen előzetes tooany definícióját követelmények vagy séma van. Azure Data Lake Store támogatja az "a alapértelmezés szerint" átlátható titkosítási az adatok aktívan, amely be van állítva a fiókjához hello létrehozása során. Alapértelmezés szerint Data Lake Store hello kulcsok kezeli az Ön, de a hello beállítás toomanage azokat saját maga.

Három típusú kulcsok használt adatok titkosítása és visszafejtése: hello fő titkosítási kulcsát (MEK), az adatok titkosítási kulcs-(adattitkosítási kulcs) és a blokk titkosítási kulcs (BEK). hello MEK használt tooencrypt hello adattitkosítási kulcs, ami állandó adathordozón, és hello BEK hello adattitkosítási kulcs és hello adatblokk van származtatva. A saját kulcsok kezelése, forgathatók hello MEK.

## <a name="encryption-of-data-in-transit"></a>Az átvitel adatok titkosítása

Azure az adatok személyes tartása egy helyen tooanother az átvitel során számos mechanizmust kínálja.

### <a name="tlsssl-encryption-in-azure"></a>A TLS/SSL-titkosítást az Azure-ban

A Microsoft hello használja [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) protokoll tooprotect adatok hello felhőszolgáltatások és az ügyfelek közötti útközben. A Microsoft azon adatközpontjainak egyezteti az ügyfélrendszerek tooAzure szolgáltatásokhoz csatlakozó TLS kapcsolatot. A TLS nyújt erős hitelesítés, üzenetvédelem, és integritását (teszi lehetővé az észlelést az üzenet illetéktelen hozzáférés és hamisítására), együttműködés, az algoritmusok rugalmassága, könnyű telepítése és használata.

[Továbbítási titkosítása tökéletes](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) az ügyfelek ügyfél és a Microsoft olyan felhőszolgáltatásai közötti kapcsolatok védi egyedi kulcsok vannak. Kapcsolatok kulcshossz 2048 bites RSA-alapú titkosítást is használhatja. Ez a kombináció megnehezíti mások az átvitel közbeni toointercept és a hozzáférési adatok.

### <a name="azure-storage-transactions"></a>Az Azure Storage-tranzakció

Hello Azure-portálon keresztül az Azure Storage kapcsolatba, ha az összes tranzakció HTTPS-KAPCSOLATON keresztül kerül sor. Az Azure Storage hello Storage REST API-n keresztül HTTPS toointeract használhatja. HTTPS hello használata is előírható, hello REST API-k tooaccess objektumok a tárfiókokban hello tárfiók szükséges biztonságos átvitele engedélyezésével hívásakor.

Közös hozzáférésű Jogosultságkód ([SAS](../storage/storage-dotnet-shared-access-signature-part-1.md)), amely lehet használt toodelegate tooAzure tárolási objektum eléréséhez, egy beállítás toospecify, hogy csak HTTPS protokoll használható a megosztott hozzáférési aláírásokkal használatakor hello tartalmazza. Ez biztosítja, hogy birtokában bárki küldi ki az SAS-tokenje hivatkozások hello megfelelő protokollt használja.

[Az SMB 3.0](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption) használt tooaccess Azure fájlmegosztásokat támogatja a titkosítást, és a Windows Server 2012 R2, Windows 8, Windows 8.1 és Windows 10-es kereszt-régió hozzáférés érhető el, és hello asztalon is elérheti.

Ügyféloldali titkosítás hello adatok titkosítására előtt küldött tooAzure tárolására, így hello hálózaton keresztül titkosítva van.

### <a name="smb-encryption-over-azure-virtual-networks"></a>Az Azure virtuális hálózatokon SMB-titkosítás 

[Az SMB 3.0](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) a Windows Server 2012 rendszert futtató Azure virtuális gépek és az újabb ad meg hello képességét toomake adatok átvitel való megfelelést az adatokat átvitel közben Azure virtuális hálózaton keresztül történő biztonságos tooprotect illetéktelen módosítás és a lehallgatás elleni támadásokat. Rendszergazdák engedélyezhetik SMB-titkosítás hello teljes kiszolgálóhoz, vagy csak adott megosztásokon.

Alapértelmezés szerint ha a megosztás vagy a kiszolgálón, be van kapcsolva a SMB-titkosítás csak az SMB 3-ügyfelek engedélyezettek tooaccess Titkosított hello megosztások.

## <a name="in-transit-encryption-in-azure-virtual-machines"></a>Az átvitel közbeni titkosítás, az Azure virtuális gépeken

Az átvitel során, valamint a Windows rendszert futtató Azure virtuális gépek közötti adattitkosítás a többféleképpen, attól függően, hogy hello jellege hello kapcsolat.

### <a name="rdp-sessions"></a>RDP-munkamenetekhez

Csatlakozhat, és jelentkezzen be tooan hello használó Azure virtuális gépek [a távoli asztal protokoll](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) (RDP) Windows rendszerű ügyfélszámítógépek, vagy egy Mac egy RDP-ügyfél telepítve van. TLS adatokat átvitel közben az RDP-munkamenetekhez hello hálózaton keresztül is védi.

Használhatja a távoli asztal tooconnect tooa Linux virtuális gép az Azure-ban.

### <a name="secure-access-toolinux-vms-with-ssh"></a>Biztonságos hozzáférés SSH tooLinux virtuális gépek

Használhat [Secure Shell](../virtual-machines/linux/ssh-from-windows.md) (SSH) tooconnect tooLinux virtuális gépek Azure-beli távoli kezelésére szolgál. Az SSH egy olyan titkosított kapcsolati protokoll, amely lehetővé teszi, hogy a biztonságos bejelentkezés titkosítatlan kapcsolaton keresztül. Hello alapértelmezett csatlakozási protokoll Azure szolgáltatásban üzemeltetett Linux virtuális gépek esetén is. SSH-kulcsok használata a hitelesítéshez, kiküszöbölheti a jelszavak toolog a hello szükségességét. SSH, egy nyilvános/titkos kulcspár (aszimmetrikus titkosítási) használ.

## <a name="azure-vpn-encryption"></a>Az Azure VPN-titkosítás

TooAzure létrehoz egy biztonságos csatornán tooprotect hello adatvédelmi hello hálózaton keresztül küldött hello adatokat virtuális magánhálózaton keresztül is elérheti.

### <a name="azure-vpn-gateway"></a>Azure VPN Gateway

[Az Azure VPN gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) használt toosend titkosított forgalom a virtuális hálózat és a nyilvános kapcsolaton keresztül a helyszíni hely között, vagy toosend forgalom virtuális hálózatok között.

Telephelyek közötti VPN használ [IPsec](https://en.wikipedia.org/wiki/IPsec) átviteli titkosításhoz. Az Azure VPN gatewayek alapértelmezett javaslatokat készletének használata. Akkor is Azure VPN-átjárók toouse egyéni IPsec/IKE-házirendet konfigurálhat titkosítási algoritmusokat és kulcs szintjeiről helyett hello Azure alapértelmezett házirend beállítása.

### <a name="point-to-site-vpn"></a>Pont – hely típusú VPN

Pont-pont VPN teszi lehetővé az egyes ügyfélszámítógépek tooan Azure virtuális hálózat eléréséhez. [hello Secure Socket Tunneling Protocol](https://technet.microsoft.com/library/2007.06.cableguy.aspx) (SSTP) használt toocreate hello VPN-alagút, és továbbítható tűzfalak (hello alagút jelenik meg egy HTTPS-kapcsolat). A pont – hely kapcsolatot használhatja a saját belső PKI legfelső szintű hitelesítésszolgáltató.

Pont-pont virtuális Magánhálózati kapcsolat tooa virtuális hálózat hello Azure-portálon tanúsítványhitelesítés vagy a PowerShell használatával konfigurálható.

toolearn pont-pont VPN kapcsolatok tooAzure Vnetek, kapcsolatos további információkért lásd: [egy pont – hely kapcsolat tooa virtuális hálózat konfigurálása hitelesítő hitelesítést használó: Azure-portálon](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) és

[Egy pont – hely kapcsolat tooa virtuális hálózat konfigurálása tanúsítvány hitelesítése: PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpn"></a>Telephelyek közötti VPN 

Pont-pont VPN gateway-kapcsolattal használt tooconnect a helyszíni hálózati tooan Azure-beli virtuális hálózat (IKEv1 vagy IKEv2) IPsec/IKE VPN-alagúton keresztül. Ilyen típusú kapcsolat egy VPN található helyszíni Eszközkezelési, amely rendelkezik egy külsőleg irányuló nyilvános IP-cím tooit igényel.

A pont-pont VPN kapcsolatot tooa virtuális hálózat hello Azure-portálon, a PowerShell vagy a hello Azure parancssori felület (CLI) használatával konfigurálható.

Olvassa el az alábbi további információk:

[Pont-pont kapcsolat létrehozása hello Azure-portálon](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[Pont-pont kapcsolat létrehozása](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[Virtuális hálózat létrehozása a parancssori felület használatával pont-pont VPN-kapcsolattal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-azure-data-lake"></a>Azure Data Lake az átvitel közbeni titkosítás

Az átvitt adatok (azaz a mozgásban lévő adatok) titkosítása is mindig a Data Lake Store-ban történik. Ezenkívül tooencrypting előzetes toostoring toopersistent adathordozók, hello adatok is minden esetben biztosítva átvitel közben HTTPS használatával. HTTPS hello egyetlen protokoll, amely Data Lake Store REST felületeihez hello támogatott.

További információ az Azure Data Lake, átvitel adatok titkosítása toolearn című című hello dokumentum [az Azure Data Lake Store adatok titkosítását.](../data-lake-store/data-lake-store-encryption.md)

## <a name="key-management-with-azure-key-vault"></a>Az Azure Key Vault kulcskezelés

Nem megfelelő védelme és kezelése hello kulcsok titkosítási használhatatlan lett megjelenítve. Az Azure Key Vault egy Microsoft által ajánlott megoldás kezelő és felhőalapú szolgáltatás által használt tooencryption hívóbetűk vezérlő. Engedélyek tooaccess kulcsok rendelhetők hozzá tooservices vagy toousers keresztül Azure Active Directory-fiókokat.

Az Azure Key Vault mentesíti szervezetek hello kell tooconfigure, javítás, valamint a hardveres biztonsági modulokkal (HSM) és a kulcskezelést szoftver karbantartása. Az Azure Key Vault Microsoft soha nem látja a kulcsokat, és alkalmazások nem rendelkeznek közvetlen hozzáférést toothem; irányítást tarthat fenn. Importálja vagy kulcsok létrehozása hardveres biztonsági modulok is.

## <a name="next-steps"></a>Következő lépések

- [Az Azure biztonsági szolgáltatásainak áttekintése](security-get-started-overview.md)
- [Az Azure biztonsági áttekintése](security-network-overview.md)
- [Azure-adatbázis biztonsági áttekintése](azure-database-security-overview.md)
- [Az Azure virtuális gépek biztonsági áttekintése](security-virtual-machines-overview.md)
- [Adattitkosítás inaktív állapotban](azure-security-encryption-atrest.md)
- [Az adatbiztonsággal és a titkosítással kapcsolatos ajánlott eljárások](azure-security-data-encryption-best-practices.md)
