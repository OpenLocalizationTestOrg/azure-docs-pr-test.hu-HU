---
title: "aaaProtect személyes adatokat átvitel közben a titkosítás az Azure-ban |} Microsoft Docs"
description: "Az Azure tooprotect személyes adatok titkosítása"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 218ad3f49326e8dec299a6d92b18116da65eae71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a>Az Azure titkosítási technológiák: személyes adatok védelmére átvitel titkosítással

Ez a cikk segít megérteni, és használja az Azure titkosítási technológiák toosecure adatokat átvitel közben. 

Személyes adatok védelmet nyújtó hello adatvédelmi hello hálózaton keresztül többrétegű védelemmel az olyan jellegű biztonsági stratégia nagyon fontos részét képezik. Az átvitel során titkosítás tervezett tooprevent egy támadó elfogja nem tud tooview használata hello adatok vagy az átvitelt.

## <a name="scenario"></a>Forgatókönyv

A nagy körutazás vállalati telephelyének hello az Amerikai Egyesült Államokban, a műveletek toooffer útvonalak hello mediterrán, Adriai, és Balti tengerek, valamint hello Brit-szigetekre növekszik. toosupport e erőfeszítéseket szerzett több kisebb körutazás sorok Olaszország, német, Dánia és hello brit 

hello vállalati hello felhő Microsoft Azure toostore vállalati adatait használja. Ez magában foglalja a személyes azonosításra alkalmas adatokat, például neveket, címeket, telefonszámokat, és a globális felhasználói bázis hitelkártya adatait. Minden helyen hagyományos emberi erőforrás adatokat, például a címet, telefonszámot, azonosító számokat és a vállalat alkalmazottai orvosi információt is tartalmaz. hello körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagok, amely tartalmazza az aktuális és korábbi ügyfelek tootrack kapcsolatokkal személyes adatokat.

Személyes ügyfelek adatok hello adatbázis hello vállalati távoli iroda és utazás ügynökök hello világ található. Felhasználói adatokat tartalmazó dokumentumok keresztül hello hálózati tooAzure tárolási továbbítása.

## <a name="problem-statement"></a>Probléma leírása

hello vállalati védelmét kell beállítani, hogy az ügyfelek hello adatvédelmi és közben az alkalmazottak a személyes adatok átvitel közben tooand az Azure-szolgáltatásokhoz.

## <a name="company-goal"></a>Vállalati cél

Vállalati cél tooensure hello lemez ki személyes adatok titkosítását. Jogosulatlan személyek intercept hello lemez személyes adatokat, ha megjelenítéséhez, nem olvasható formátumban kell lennie. Titkosítás alkalmazásakor kell lennie, vagy az teljesen átlátható, felhasználók és rendszergazdák könnyen.

## <a name="solutions"></a>Megoldások

Azure-szolgáltatásokhoz biztosít több eszközök és technológiák toohelp meg személyes adatok védelmére átvitel.

### <a name="azure-storage"></a>Azure Storage

Hello felhőben tárolt adatok hello ügyfélről, amely fizikailag bárhol hello world, Azure-adatközpont toohello kell keresnie. Felhasználók lekéri az adatokat, amikor adatcsere ebben az esetben a hello ellenkező irányban. Adatok átvitel közben keresztüli hello nyilvános Internet mindig az támadók hozzáférés veszélyben van. Fontos tooprotect hello adatvédelmi személyes adatok, mert átvitel során helyek közötti átviteli szintű titkosítást toosecure használatával is.

hello HTTPS protokollon keresztül hello Internet biztosítja a biztonságos, titkosított kommunikációs csatornát. HTTPS kell használt tooaccess objektumok és Azure Storage REST API-k hívásakor. Hello HTTPS protokoll használatát kényszeríti használatakor [megosztott hozzáférési aláírásokkal](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) toodelegate access tooAzure tároló objektumok. SAS két típusa van: szolgáltatás SAS és a fiók SAS.

#### <a name="how-do-i-construct-a-service-sas"></a>Hogyan hozható létre a szolgáltatás SAS?

Szolgáltatás SAS delegáltak hozzáférés tooa erőforrás csak egy hello tárolási szolgáltatásokhoz (blob, a várólista, a tábla vagy a fájl szolgáltatás). a szolgáltatás SAS tooconstruct hello a következő:

1. Adja meg az aláírt Verziómezőjének hello

2. Adja meg a hello aláírt erőforrás (Blob, és csak a szolgáltatás fájl)

3. Adja meg a lekérdezési paraméterek tooOverride válaszfejlécek (Blob szolgáltatás és a szolgáltatás csak fájl)

4. Adja meg a hello (Table szolgáltatás csak) tábla neve

5. Adja meg a hello hozzáférési házirend

6. Adja meg az aláírás érvényességi időszakának hello

8. Engedélyek megadása

9. Adja meg az IP-cím vagy IP-címtartomány

10. Adja meg a HTTP protokoll hello

11. Adja meg a hozzáférés tartomány

12. Adja meg a hello aláírt azonosítója

13. Adja meg a hello aláírás

Részletes utasítások, lásd: [hozhat létre, egy szolgáltatás SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).

#### <a name="how-do-i-construct-an-account-sas"></a>Hogyan hozható létre egy fiókot SAS?

Egy fiók SAS egy vagy több hello tárolószolgáltatások hozzáférés tooresources delegálja. Delegálhatja hozzáférés tooread, írási és törlési műveletek a blobtárolók, táblák, üzenetsorok és fájlmegosztások a szolgáltatásalapú SAS nem engedélyezett. Egy fiók SAS építése egy szolgáltatás SAS hasonló toothat. Részletes útmutatásért lásd: [hozhat létre egy fiókot SAS.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a>Hogyan kényszerítése meg HTTPS, amikor a program meghívja a REST API-k?

a storage-fiókok REST API-k tooaccess objektumok meghívásakor HTTPS tooenforce hello használata, engedélyezheti a biztonságos átviteléhez szükséges hello tárfiók. 

1. Hello Azure-portálon, válassza ki **Storage-fiók létrehozása**, vagy válassza a meglévő tárfiókot, **beállítások** , majd **konfigurációs**.

2. A **biztonságos átviteléhez szükséges**, jelölje be **engedélyezve**.

![A storage-fiók létrehozása](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

Részletesebb leírását, beleértve a tooenable biztonságos átviteléhez szükséges programozott módon, lásd: hogyan [szükséges biztonságos átviteli](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a>Hogyan titkosítják az adatokat az Azure File Storage?

az átvitt adatokat tooencrypt [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), használhatja az SMB Windows 8, 8.1 és 10 és Windows Server 2012 R2 és Windows Server 2016 3.x. Hello Azure fájlok szolgáltatást használja, a titkosítás nélküli kapcsolat sikertelen lesz, ha engedélyezve van a "Biztonságos szükséges átviteli". Ez magában foglalja a forgatókönyveket SMB 2.1, SMB 3.0-titkosítás nélkül, és néhány változatban is elkészíti a hello Linux SMB-ügyfél használatával.

#### <a name="azure-client-side-encryption"></a>Az Azure ügyféloldali titkosítás

Másik lehetőségként a személyes adatok védelme alatt álló ügyfélalkalmazást és az Azure Storage közötti átvitel közben [ügyféloldali titkosítás](https://docs.microsoft.com/azure/storage/storage-client-side-encryption). hello adattitkosítás előtt Azure Storage történő átvitel során, és amikor hello adatok Azure Storage-ból, hello adatok visszafejtése hello ügyféloldalon fogadását követően.

### <a name="azure-site-to-site-vpn"></a>Az Azure--webhelyek közötti VPN

Egy hatékony módot tooprotect személyes adatok átvitele történik a vállalati hálózaton vagy a felhasználó és a hello Azure-beli virtuális hálózat között toouse egy [pont-pont](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) vagy [pont-pont](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) virtuális magánhálózati (VPN). VPN-kapcsolat egy biztonságos, titkosított csatornán keresztül hello Internet hoz létre.

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a>Hogyan hozható létre a pont-pont VPN-kapcsolatot?

A telephelyek közötti VPN hello vállalati hálózat tooAzure több felhasználó csatlakozik. hello Azure-portálon a pont-pont kapcsolat toocreate hello a következő:

1. Hozzon létre egy virtuális hálózatot.

2. Adjon meg egy DNS-kiszolgáló.

3. Hozzon létre hello átjáró-alhálózatot.

4. Hello VPN-átjáró létrehozásához. 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. Hello helyi hálózati átjáró létrehozása.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. Konfigurálja a VPN-eszköz.

7. Hello VPN-kapcsolat létrehozása.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. Hello VPN-kapcsolat ellenőrzése.

Részletes utasítások a hogyan toocreate a pont-pont kapcsolat hello Azure portál, lásd: [hello Azure Portal kapcsolat létrehozása a pont-pont.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a>Hogyan hozható létre egy pont – hely VPN-kapcsolatot?

A pont-pont VPN biztonságos kapcsolatot hoz létre egy egyéni ügyfél számítógép tooa virtuális hálózathoz. Ez akkor hasznos, ha azt szeretné, tooconnect tooAzure távoli helyről, például otthoni vagy a szállodák vagy konferencia központból. toocreate egy pont – hely kapcsolatot hello Azure-portálon

1. Hozzon létre egy virtuális hálózatot.

2. Adjon hozzá egy átjáró-alhálózatot.

3. Adjon meg egy DNS-kiszolgáló. (nem kötelező)

4. A virtuális hálózati átjáró létrehozása.

5. Tanúsítványok előállításához.

6. Adja hozzá a hello ügyfélcímkészlete.

7. Hello legfelső szintű tanúsítvány nyilvános Tanúsítványadatok feltöltése.

8. Készítése és hello VPN-ügyfélcsomag konfigurációs telepítése.

9. Telepíti az exportált ügyféltanúsítványt.

10. Csatlakozás tooAzure.

11. Ellenőrizze a kapcsolatot.

Részletes utasításokat, lásd: [konfigurálása egy pont – hely kapcsolat tooa VNet-alapú hitelesítést használó: Azure portálon.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)

### <a name="ssltls"></a>SSL/TLS

A Microsoft azt javasolja, hogy mindig használjon SSL/TLS protokollok tooexchange adatok különböző helyek között. A szervezetek, amelyek toouse [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) toomove nagy adatkészletek egy dedikált nagy sebességű WAN-kapcsolaton keresztül is titkosíthatók hello hello SSL/TLS- vagy egyéb protokollok használatával további alkalmazásszintű adatok.

### <a name="encryption-by-default"></a>Alapértelmezés szerint titkosítás

A Microsoft titkosítási tooprotect adatokat használja az ügyfelek és az Azure felhőszolgáltatások közötti átvitel során. Interakció Azure Storage hello Azure portálon keresztül, ha minden tranzakciója történik, HTTPS használatával.

[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS), hogy a Microsoft azon adatközpontjainak megpróbál toonegotiate tooMicrosoft felhőszolgáltatásokhoz csatlakozó ügyfél rendszerekkel hello protokoll. A TLS nyújt erős hitelesítés, üzenetvédelem, és integritását (lehetővé teszi, hogy a üzenet illetéktelen hozzáférés és hamisítására észlelése), együttműködés, az algoritmusok rugalmassága, könnyű telepítése és használata.

[Továbbítási titkosítása tökéletes](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) is, hogy az egyes ügyfelek ügyfél és a Microsoft olyan felhőszolgáltatásai közötti kapcsolat egyedi kulcsok használata alkalmazzák. Kapcsolatok tooMicrosoft felhőszolgáltatások kihasználhatják alapú RSA 2048 bites titkosítást kulcshossza. hello TLS, RSA 2048 bit, kulcshosszok kombinációja, így a PFS jelentős mértékben megnehezíti mások toointercept és a hozzáférési adatok, amely a Microsoft más felhőszolgáltatásaival és az ügyfelek közötti átvitel közben.

Adatok az átvitel során mindig titkosítja az [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview). Ezenkívül tooencrypting előzetes toostoring toopersistent adathordozók, hello adatok is minden esetben biztosítva átvitel közben HTTPS használatával. HTTPS hello egyetlen protokoll, amely Data Lake Store REST felületeihez hello támogatott.

## <a name="summary"></a>Összefoglalás

hello vállalati megvalósításához azáltal, HTTPS-kapcsolatok tooAzure tárolási, megosztott hozzáférési aláírásokkal használatával biztonságos átviteléhez szükséges engedélyezését hello tárfiókok személyes adatok és hello adatvédelmi az ilyen adatok védelme a célja. Személyes adatok védelme az SMB 3.0-kapcsolatot is használ, és ügyféloldali titkosítás végrehajtásával is. Pont-pont VPN-kapcsolatok a vállalati hálózat toohello Azure-beli virtuális hálózat hello és a pont-pont VPN-kapcsolatok az egyes felhasználó létrehoz egy biztonságos csatornán keresztül, amelyek személyes adatokat is biztonságosan változatlan marad. A Microsoft alapértelmezett titkosítási eljárásokat további megvédi a hello adatvédelmi személyes adatok.

## <a name="next-steps"></a>Következő lépések

- [Az Azure Data biztonsági és a titkosítás gyakorlati tanácsok](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [A VPN Gateway tervezése és kialakítása](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [VPN Gateway – gyakori kérdések](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [Vásároljon és SSL-tanúsítvány konfigurálása az Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-purchase-ssl-web-site)
