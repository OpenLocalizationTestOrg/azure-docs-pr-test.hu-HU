---
title: "aaaDNS zónák és áttekintés – Azure DNS rögzíti |} Microsoft Docs"
description: "DNS-zónák és a Microsoft Azure DNS-rekordok támogatása áttekintése."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
editor: 
ms.assetid: be4580d7-aa1b-4b6b-89a3-0991c0cda897
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: jonatul
ms.openlocfilehash: f214c3e2e810a80a000281820acd35f0aaf5a7e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-dns-zones-and-records"></a>A DNS-zónák és áttekintése

Ez a lap ismerteti hello kapcsolatos főbb fogalmakat tartományok, a DNS-zónák, és a DNS-rekordok és a rekordhalmazok és a hogyan támogatja őket az Azure DNS-ben.

## <a name="domain-names"></a>Tartománynevek

hello tartománynévrendszer tartományok hierarchiájából áll. hello hierarchia első eleme hello "gyökértartomány", amelynek neve egyszerűen "**.**".  Ez alatt találhatók a legfelső szintű tartományok, mint a „com”, a „net”, az „org”, az „uk” vagy a „jp”.  Ezek alatt találhatók a másodlagos szintű tartományok, mint az „org.uk” vagy a „co.jp”. hello tartományok hello DNS hierarchia globálisan fel vannak osztva, DNS-névkiszolgálók hello világ üzemelteti.

A tartományregisztráló egy szervezet, amely lehetővé teszi egy, a tartomány nevét, például "contoso.com" toopurchase.  Egy tartomány neve ad meg a megfelelő toocontrol hello DNS-hierarchiában a névvel, például hogy lehetővé teszi a toodirect hello neve "www.contoso.com" tooyour vállalati webhely hello megvásárlásáról. hello regisztráló a gazdagép saját névkiszolgálók az Ön nevében hello tartományában, vagy toospecify alternatív névkiszolgálók engedélyezi.

Az Azure DNS globálisan elosztott, magas rendelkezésre állású neve kiszolgálói infrastruktúrát, amely akkor is biztosít a tartomány toohost használata. Az Azure DNS-tartományt üzemeltet, a DNS-rekordokat hello is kezelheti ugyanazon hitelesítő adatokat, API-k, eszközök, számlázási és támogatási, mint az egyéb Azure-szolgáltatások.

Az Azure DNS jelenleg nem támogatja tartománynevek megvásárlását. Ha azt szeretné, hogy toopurchase egy tartomány nevét, egy külső regisztrációs toouse kell. hello regisztráló általában egy kis éves díj költségek. hello tartományok majd az Azure DNS-lehet üzemeltetni, a DNS-rekordok kezelése. Lásd: [delegálása a tartományi tooAzure DNS](dns-domain-delegation.md) részleteiről.

## <a name="dns-zones"></a>DNS-zónák

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="dns-records"></a>DNS-rekordok

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

### <a name="time-to-live"></a>Élő idő

hello során toolive, és a TTL-t, határozza meg, hogy mennyi ideig gyorsítótárazzák az egyes rekordokat az ügyfelek újbóli lekérdezés előtt. A fenti példa hello a hello élettartam 3600 másodperc, vagyis 1 óra.

Az Azure DNS szolgáltatásban hello TTL megadott hello rekordhalmaz, nem az egyes rekordokhoz, úgy, hogy a rekord összes rekordján ugyanaz az érték érvényesül hello beállítása.  Megadhatja, hogy bármely élettartam értéke 1 és 2 147 483 647 másodperc között.

### <a name="wildcard-records"></a>Helyettesítő rekordok

Az Azure DNS [helyettesítő rekordok](https://en.wikipedia.org/wiki/Wildcard_DNS_record) használatát is támogatja. Helyettesítő rekordok vannak lekérdezés által visszaadott válaszban tooany egyező nevű (kivéve, ha egy nem helyettesítő rekordhalmaz származó közelebbi találat). Az Azure DNS támogatja a helyettesítő rekordhalmazok minden rekordtípus, kivéve az NS és SOA.

toocreate helyettesítő rekord megadásához használja hello rekordhalmazának neve "\*". Másik megoldásként használhatja nevet "\*", a bal szélső címkéjével, például"\*.foo".

### <a name="cname-records"></a>A CNAME-rekordok

CNAME-rekordhalmazok nem használható együtt más rekordhalmazok hello az azonos név. Például nem hozható létre egy CNAME típusú hello relatív neve "www" rekordhalmaz és egy A jegyezze fel a hello relatív neve "www" hello, azonos idő.

Mivel hello zóna felső pontja (név = "@") mindig tartalmazza a hello NS és SOA rekordot hello zóna létrehozásakor létrehozott csoportok esetében nem hozható létre egy CNAME rekordot a zóna csúcsán hello beállítása.

Ezek a megkötések hello DNS-szabványokból erednek, és nem az Azure DNS korlátozásai.

### <a name="ns-records"></a>A Névkiszolgálói rekordokat

hello NS rekord beállított hello zóna felső pontja (név "@") minden DNS-zóna automatikusan létrejön, és hello zóna törlésekor automatikusan törlődik (ezt nem lehet törölni külön-külön).

A rekordhalmaz hello Azure DNS-név kiszolgálók hozzárendelt toohello zóna hello nevét tartalmazza. Hozzáadhat további neve kiszolgálók toothis NS rekordhalmaz, toosupport közös futtató tartományok egynél több DNS-szolgáltatónál. Hello TTL-t és a rekordhalmaz metaadatait is módosíthatja. Azonban nem lehet eltávolítani vagy módosítani hello előre megadott Azure DNS névkiszolgálóit. 

Vegye figyelembe, hogy ez érvényes csak toohello NS rekord beállított hello zóna felső pontja. Más NS rekordhalmazok, amelyek a zónához (a használt toodelegate gyermekzónákhoz) hozható létre, módosított és törölt korlátozás nélkül.

### <a name="soa-records"></a>SOA-rekord

SOA típusú rekordhalmaz automatikusan jön létre az egyes zónák hello csúcsán (név = "@"), és hello zóna törlésekor automatikusan törlődik.  SOA rekordokat nem hozható létre vagy törölt külön-külön.

Hello SOA típusú rekordjának hello "fogadó" tulajdonsággal, amely az előre konfigurált toorefer toohello elsődleges neve kiszolgáló Azure DNS által nyújtott kivételével minden tulajdonságait módosíthatja.

### <a name="spf-records"></a>SPF-rekordokat

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="srv-records"></a>SRV-rekordok

[SRV-rekordok](https://en.wikipedia.org/wiki/SRV_record) szolgáltatások toospecify kiszolgáló különböző helyein használják. Ha egy SRV rekordot az Azure DNS:

* Hello *szolgáltatás* és *protokoll* kell hello rekordhalmaz nevének részeként megadott, előtagként az aláhúzás karaktereket tartalmazhatnak.  Például "\_sip.\_ TCP.Name ".  Nincs szükség toospecify hello zóna felső pontja, a bejegyzés nincs "@" hello rekord nevében, egyszerűen hello szolgáltatást és protokollt használ, például "\_sip.\_ TCP ".
* Hello *prioritás*, *súly*, *port*, és *cél* hello rekordhalmaz egyes rekordjainak paraméterek vannak megadva.

### <a name="txt-records"></a>TXT-rekord

TXT rekord használt toomap tartomány nevek tooarbitrary szöveges karakterláncot. Több alkalmazás, különösen kapcsolódó tooemail konfigurációját, például hello használva [küldő házirend keretrendszer (SPF)](https://en.wikipedia.org/wiki/Sender_Policy_Framework) és [DomainKeys azonosított Mail (DKIM)](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail).

hello DNS-szabványokból lehetővé teszik egy egyetlen TXT rekord toocontain több karakterláncok, amelyek mindegyike be too254 karakter hosszúságú lehet. Ha több karakterláncot használnak, ezeket ügyfelek összefűzendő és egyetlen karakterláncként kezelni.

Hívásakor hello Azure DNS REST API-t, kell toospecify minden TXT karakterlánc külön-külön.  Hello Azure portál használata esetén PowerShell vagy a CLI-felületekkel adjon meg egy rekord, amely automatikusan oszlik 254 karakter szegmensek szükség esetén egy karakterlánc.

a DNS-rekord több karakterláncok nem keverendő össze a hello hello egy TXT rekord készlet több TXT rekord.  TXT rekord is tartalmazhatnak több rekord *, amelyek mindegyike* több karakterláncokat tartalmazhat.  Az Azure DNS egy teljes karakterlánchosszát too1024 karakterek másolatot minden TXT rekord (történő bejegyzésekkel együtt) támogatja.

## <a name="tags-and-metadata"></a>Címkék és metaadatok

### <a name="tags"></a>Címkék

Címkék név-érték párok listáját, és Azure Resource Manager toolabel erőforrások használják.  Az Azure Resource Manager használ az Azure számlázásának címkék tooenable szűrt nézeteinek, és azt is lehetővé teszi egy házirendet, mely címkéket a szükségesek tooset. Címkékkel kapcsolatos további információkért lásd: [Using címkéket tooorganize az Azure-erőforrások](../azure-resource-manager/resource-group-using-tags.md).

Az Azure DNS támogatja az Azure Resource Manager címkék használatával a DNS-zóna erőforrás.  Nem támogatja címkék a DNS-rekordhalmazok, bár a "metadata" alternatív támogatja a DNS-rekord beállítja, ahogy az alábbi leírásban.

### <a name="metadata"></a>Metaadatok

Egy alternatív toorecord állíthat be címkéket, a Azure DNS rekordhalmazok használatával "metadata" ellátása megjegyzésekkel támogatja.  Hasonló tootags metaadatok lehetővé teszi, hogy Ön tooassociate név-érték párokat az minden rekordhalmaz.  Ez akkor lehet hasznos, például a rekordokban toorecord hello céljának beállítása.  Címkék, eltérően metaadatok nem lehet használt tooprovide az Azure számlázásának szűrt nézete, és az Azure Resource Manager-házirend nem adható meg.

## <a name="etags"></a>ETag-EK

Tegyük fel, hogy a két személynek vagy két folyamatok próbálja toomodify egy DNS-rekordja, hello azonos idő. Melyik wins? És hello győztes nem tudja, hogy azok már felül más által létrehozott módosításokat?

Az Azure DNS ETag-EK toohandle egyidejű változtatások toohello használja ugyanazt az erőforrást biztonságosan. ETag-EK nem azonosak a [Azure Resource Manager "Címke"](#tags). Minden DNS-erőforrásrekordok (zóna vagy rekordhalmaz) rendelkezik egy ETag-gel társítva. Erőforrás lekérése, amikor a rendszer is lekéri az Etag. Amikor frissíti egy erőforrást, dönthet úgy toopass vissza hello Etag, így az Azure DNS ellenőrizheti, hogy hello Etag hello kiszolgáló megfelel a. Minden egyes frissítés tooa erőforrás hello helyreállítás alatt Etag eredményez, mivel az egy Etag nem egyezik azt jelzi, egyidejű módosítás történt. ETag-EK is használható, amikor hoz létre egy új erőforrás tooensure hello erőforrás már nem létezik.

Alapértelmezés szerint az Azure DNS PowerShell használja ETag-EK tooblock egyidejű változtatások toozones és a rekordhalmazok jelentésével. nem kötelező hello *-felülírási* kapcsoló használt toosuppress Etag-ellenőrzéseket, ebben az esetben minden egyidejű végrehajtott módosításokat a rendszer felülírja.

Hello szintjén hello Azure DNS REST API-t ETag-EK megadott HTTP-fejlécek használatával.  A következő táblázat hello viselkedésük kap:

| Fejléc | Viselkedés |
| --- | --- |
| None |A PUT mindig sikeres (nincs Etag ellenőrzése) |
| IF-match<etag> |PUT csak akkor sikeres, ha erőforrás létezik, és az Etag megfelel |
| IF-match * |A PUT csak akkor sikeres, ha erőforrás létezik-e |
| IF-none-match * |PUT csak akkor sikeres, ha az erőforrás nem létezik. |


## <a name="limits"></a>Korlátok

a következő alapértelmezett korlátokat hello Azure DNS használata esetén alkalmazza:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

## <a name="next-steps"></a>Következő lépések

* az Azure DNS használatával toostart megtudhatja, hogyan túl[hozzon létre egy DNS-zóna](dns-getstarted-create-dnszone-portal.md) és [DNS-rekordok létrehozása](dns-getstarted-create-recordset-portal.md).
* egy meglévő DNS-zóna toomigrate megtudhatja, hogyan túl[importálni és exportálni egy DNS-zónafájlját](dns-import-export.md).
