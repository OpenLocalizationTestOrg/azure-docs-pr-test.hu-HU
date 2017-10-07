---
title: "az Azure-ban címfeloldási DNS aaaOverview |} Microsoft Docs"
description: "Megtudhatja, hogyan címfeloldási DNS működik, és hogyan használható az Azure-ban"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 687663fb83469ab8e696bb714649d0856915bad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-reverse-dns-and-support-in-azure"></a>A címfeloldási DNS- és támogatás az Azure-ban – áttekintés

Ez a cikk áttekintést hogyan címfeloldási DNS működik, és hello címfeloldási DNS-forgatókönyvek az Azure-ban támogatott.

## <a name="what-is-reverse-dns"></a>Mi az a címfeloldási DNS?

Hagyományos DNS-rekordokat a DNS nevét (például "www.contoso.com") tooan IP-címről (például 64.4.6.100) leképezéseket engedélyezése.  Névkeresési DNS lehetővé teszi, hogy a hello fordítását egy IP-cím (64.4.6.100) hátsó tooa neve ("www.contoso.com").

Névkeresési DNS-rekordok számos különféle helyzetekben is használja. Például címfeloldási DNS-rekordok széles körben használt e-mail levélszemét elleni hello feladó e-mail üzenet ellenőrzésével.  hello fogadó levelezési kiszolgáló lekéri hello hello kiszolgáló IP-címet a címfeloldási DNS-rekord, és ellenőrzi, hogy van a hello engedélyezett toosend e-mail származó tartomány. 

## <a name="how-reverse-dns-works"></a>Hogyan címfeloldási DNS működik

Speciális DNS-zónák, úgynevezett "ARPA" zónák tárolt címfeloldási DNS-rekordokat.  Ezekben a zónákban olyan külön DNS hierarchiáját képezik, például "contoso.com" tartományt üzemeltet hello normál hierarchia párhuzamosan.

Például hello DNS-rekord "www.contoso.com" hello "www" hello zónában "contoso.com" nevű "A" DNS-rekord segítségével van megvalósítva.  Ez A rekord toohello megfelelő IP-cím, ebben az esetben 64.4.6.100 mutat.  hello névkeresési segítségével van megvalósítva külön-külön, a "PTR" rekord nevű "100" hello zónában "6.4.64.in-addr.arpa" (vegye figyelembe, hogy IP-címek felcseréli ARPA-zónák.)  A PTR típusú rekord megfelelően lett konfigurálva mutat az toohello neve "www.contoso.com".

Amikor egy szervezet hozzá van rendelve egy IP-Címblokk, azokat is szerez hello jobb toomanage hello megfelelő ARPA zónát. az ARPA-zónák megfelelő toohello IP-cím Azure által használt blokkok tárolt és kezelt Microsoft hello. Az Internetszolgáltató meg hello ARPA zóna a saját IP-címek is tartalmazhat, vagy előfordulhat, hogy engedélyezi tooyou állomás hello ARPA zónát a DNS-szolgáltatás az Ön által választott, például az Azure DNS-ben.

> [!NOTE]
> DNS-címkeresés és a DNS-névlekérdezés különálló, párhuzamos DNS hierarchiákban valósíthatók meg. a "www.contoso.com" Hello névlekérdezés **nem** üzemeltetett "contoso.com" hello zónában, ahelyett, hogy helyezkedik el a megfelelő IP-Címblokk hello hello ARPA zónában. Különálló zónákra az IPv4 és IPv6-címterületet használnak.

### <a name="ipv4"></a>IPv4-alapú

egy IPv4-névkeresési zóna neve hello hello a következő formátumban kell lennie: `<IPv4 network prefix in reverse order>.in-addr.arpa`.

Például a névkeresési zóna toohost rekordok létrehozásakor állomások IP-címek, amelyek az hello 192.0.2.0/24 előtag, hello Zónanév hozhatók létre azoknak a hálózati-előtagja hello hello cím (192.0.2) felcserélni hello rendelés (2.0.192) és hello hozzáadása utótag `.in-addr.arpa`.

|Alhálózati osztály|Hálózati előtag  |Fordított hálózati előtag  |Standard utótag  |Névkeresési zóna neve |
|-------|----------------|------------|-----------------|---------------------------|
|A osztály|203.0.0.0/8     | 203        | .in-addr.arpa   | `203.in-addr.arpa`        |
|B osztály|198.51.0.0/16   | 51.198     | .in-addr.arpa   | `51.198.in-addr.arpa`     |
|C osztály|192.0.2.0/24    | 2.0.192    | .in-addr.arpa   | `2.0.192.in-addr.arpa`    |

### <a name="classless-ipv4-delegation"></a>Classless IPv4-delegálás

Bizonyos esetekben hello IP-címtartomány tooan szervezet lefoglalt értéke kisebb a C osztályú (/ 24) tartományon. Ebben az esetben hello IP-címtartomány nem tartozik a zóna keretén belül hello `.in-addr.arpa` hierarchia zónát, és ezért nem delegálható gyermek zónának.

Ehelyett egy másik módszer használata egyéni névkeresési (PTR) tootransfer irányítását dedikált tooa DNS-zóna rögzíti. A mechanizmus delegálja a gyermekzónát minden IP-címtartomány, majd minden egyes IP-címe a hello maps tartomány külön-külön toothat gyermekzónát CNAME rekordok használatával.

Tegyük fel például, egy szervezet hello IP-címtartomány 192.0.2.128/26 megkapja az Internetszolgáltató által. 64 IP-címek, a 192.0.2.128 jelképez too192.0.2.191. Névkeresési DNS ezt a tartományt a következőképpen történik:
- hello szervezet 128-26.2.0.192.in-addr.arpa nevű névkeresési zónát hoz létre. "128-26' jelöli hello hálózati hozzárendelt szegmens toohello szervezeten belül hello előtag hello C osztályú (/ 24) tartományon.
- hello Internetszolgáltató hello C osztályú szülőzónában Névkiszolgálói rekordok tooset hello hello fent zóna DNS-delegálás másolatot hoz létre. Is létre CNAME rekordok hello szülő (C osztályú) névkeresési zónát, leképezési hello IP tartomány toohello új zónában hello szervezet által létrehozott összes IP-címe:

```
$ORIGIN 2.0.192.in-addr.arpa
; Delegate child zone
128-26    NS       <name server 1 for 128-26.2.0.192.in-addr.arpa>
128-26    NS       <name server 2 for 128-26.2.0.192.in-addr.arpa>
; CNAME records for each IP address
129       CNAME    129.128-26.2.0.192.in-addr.arpa
130       CNAME    130.128-26.2.0.192.in-addr.arpa
131       CNAME    131.128-26.2.0.192.in-addr.arpa
; etc
```
- hello szervezet hello egyedi PTR rekordok belül a gyermekzóna kezeli.

```
$ORIGIN 128-26.2.0.192.in-addr.arpa
; PTR records for each UIP address. Names match CNAME targets in parent zone
129      PTR    www.contoso.com
130      PTR    mail.contoso.com
131      PTR    partners.contoso.com
; etc
```
A névkeresési hello IP cím "192.0.2.129" lekérdezések a PTR típusú rekord "129.2.0.192.in-addr.arpa" nevű. Ez a lekérdezés hello szülő zóna toohello PTR típusú rekord hello gyermekzónát a CNAME hello segítségével oldja fel.

### <a name="ipv6"></a>IPv6

egy IPv6-névkeresési zóna neve hello hello a következő formában kell:`<IPv6 network prefix in reverse order>.ip6.arpa`

Például. A névkeresési zóna toohost rekordok létrehozásakor állomások IP-címek, amelyek a hello 2001:db8:1000:abdc:: / 64 előtag hello Zónanév hozhatók létre hello hálózati előtag hello cím elkülönítése (2001:db8:abdc::). Ezután bontsa ki a hello IPv6 hálózati előtag tooremove [tömörítés nulla](https://technet.microsoft.com/library/cc781672(v=ws.10).aspx), ha a használt tooshorten hello IPv6-címelőtagot volt (2001:0db8:abdc:0000::). Fordított sorrendben hello, időszak, elválasztó karakter közötti hello előtag hexadecimális számoknak hello használva toobuild hello fordított irányú hálózati előtag (`0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2`), és adja hozzá a hello utótag `.ip6.arpa`.


|Hálózati előtag  |Bővített és a fordított hálózati előtag |Standard utótag |Névkeresési zóna neve  |
|---------|---------|---------|---------|
|2001:db8:abdc:: / 64    | 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2        | . ip6.arpa        | `0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa`       |
|2001:db8:1000:9102:: / 64    | 2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2        | . ip6.arpa        | `2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2.ip6.arpa`        |


## <a name="azure-support-for-reverse-dns"></a>A címfeloldási DNS Azure-támogatás

Azure tooreverse DNS vonatkozó két külön forgatókönyveket támogat:

**Az üzemeltetési hello névkeresési zóna megfelelő tooyour IP-Címblokk.**
Az Azure DNS túl használható[a névkeresési zónák tárolására és kezelésére hello PTR-rekordok minden DNS-névkeresési](dns-reverse-dns-hosting.md), az IPv4 és IPv6.  hello hello (ARPA) névkeresési zóna hello delegálás beállítása létrehozásának folyamatán, és PTR konfigurálása rekordok van hello ugyanaz, mint a normál DNS-zónák.  hello csak a különbségek hello delegálás kell konfigurálni a DNS-regisztráló webhelyén, hanem az Internetszolgáltató, és csak a PTR típusú rekord típusa hello kell használni.

**Hello névkeresési DNS-rekordot hello IP-cím tooyour Azure-szolgáltatások konfigurálásához.** Azure lehetővé teszi a túl[hello névkeresési konfigurálásához hello IP-címek lefoglalt tooyour Azure szolgáltatás](dns-reverse-dns-for-azure-services.md).  A névkeresési PTR típusú rekord hello megfelelő ARPA zónát az Azure-ban van konfigurálva.  Ezek az ARPA-zónák, megfelelő tooall IP-címtartományok hello Azure, használja a Microsoft által üzemeltetett

## <a name="next-steps"></a>Következő lépések

A címfeloldási DNS-további információkért lásd: [Wikipedia névkeresési DNS](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Ismerje meg, hogyan túl[állomás hello névkeresési zónát az Internetszolgáltató által hozzárendelt IP-címtartományt, az Azure DNS-](dns-reverse-dns-for-azure-services.md).
<br>
Ismerje meg, hogyan túl[címfeloldási DNS-rekordjait az Azure-szolgáltatások kezelése](dns-reverse-dns-for-azure-services.md).

