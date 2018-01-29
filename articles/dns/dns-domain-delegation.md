---
title: "Az Azure DNS-delegálás áttekintése | Microsoft Docs"
description: "Ismerje meg, hogyan módosíthatja a tartományok delegálását és használhatja tartományszolgáltatóként az Azure DNS-névkiszolgálóit."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: kumud
ms.openlocfilehash: 4ba615a1fc607392fa1a2a910b4674cfbcf51da1
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/19/2017
---
# <a name="delegation-of-dns-zones-with-azure-dns"></a>DNS-zónák delegálása az Azure DNS-sel

Az Azure DNS használatával DNS-zónákat üzemeltethet, és kezelheti a tartomány DNS-rekordjait az Azure felületén. Egy tartomány DNS-lekérdezései csak akkor érik el az Azure DNS-t, ha a tartomány delegálva van az Azure DNS-be a szülőtartományból. Ne feledje: nem az Azure DNS a tartományregisztráló. Ez a cikk ismerteti a tartománydelegálás működését és a tartományok Azure DNS-be való delegálását.

## <a name="how-dns-delegation-works"></a>A DNS-delegálás működése

### <a name="domains-and-zones"></a>Tartományok és zónák

A tartománynévrendszer tartományok hierarchiájából áll. A hierarchia első eleme a „gyökértartomány”, amelynek neve egyszerűen „**.**”.  Ez alatt találhatók a legfelső szintű tartományok, mint a „com”, a „net”, az „org”, az „uk” vagy a „jp”.  A legfelső szintű tartományok alatt találhatók a másodlagos szintű tartományok, mint az „org.uk” vagy a „co.jp”.  És így tovább. A DNS-hierarchia tartományait különálló DNS-zónák üzemeltetik. A zónák globálisan fel vannak osztva, és a világ különböző pontjain található DNS-névkiszolgálók üzemeltetik őket.

**DNS-zóna** – A tartományok egyedi nevek a tartománynévrendszerben, például „contoso.com”. Az egyes tartományokhoz tartozó DNS-rekordok üzemeltetése DNS-zónákban történik. A „contoso.com” tartomány például számos DNS-rekordot tartalmazhat: „mail.contoso.com” (levelezési kiszolgálóhoz) és „www.contoso.com” (webhelyhez).

**Tartományregisztráló** – A tartományregisztráló egy olyan cég, amely internetes tartományneveket biztosít. Ezek a cégek ellenőrzik, hogy a használni kívánt internetes tartomány elérhető-e, és ők engedélyezik azok megvásárlását. A tartománynév regisztrálása után Ön annak a jogos tulajdonosa. Ha már van internetes tartománya, az aktuális tartományregisztrálóval delegálhat az Azure DNS-be.

További információt a tartománynevek tulajdonosairól, illetve a tartományok vásárlásáról az [Internet domain management in Azure AD](https://msdn.microsoft.com/library/azure/hh969248.aspx) (Internetes tartományok kezelése az Azure AD-ben) című cikkben talál.

### <a name="resolution-and-delegation"></a>Feloldás és delegálás

Kétféle DNS-kiszolgáló létezik:

* A *mérvadó* DNS-kiszolgáló üzemelteti a DNS-zónákat. Csak az ezekben a zónákban található rekordokra irányuló DNS-lekérdezéseket válaszolja meg.
* A *rekurzív* DNS-kiszolgáló nem üzemeltet DNS-zónákat. Minden DNS-lekérdezést megválaszol a mérvadó DNS-kiszolgálók adatait összegyűjtve.

Az Azure DNS mérvadó DNS szolgáltatást nyújt.  Rekurzív DNS szolgáltatást nem biztosít. Az Azure felhőszolgáltatásai és virtuális gépei automatikusan egy rekurzív DNS szolgáltatás használatára vannak konfigurálva, amelyet az Azure-infrastruktúra külön biztosít. Ha további információt szeretne kapni ezen DNS-beállítások módosításáról, olvassa el az [Azure-beli névfeloldást](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) ismertető cikket.

A számítógépes vagy mobileszköz-kompatibilis DNS-ügyfelek általában egy rekurzív DNS-kiszolgálóval végeztetik el az ügyfélalkalmazások számára szükséges DNS-lekérdezéseket.

Amikor egy rekurzív DNS-kiszolgáló egy DNS-rekordra (például „www.contoso.com”) vonatkozó lekérdezést kap, először meg kell keresnie a „contoso.com” tartomány zónáját üzemeltető névkiszolgálót. A névkiszolgáló megkereséséhez a gyökér-névkiszolgálótól kiindulva megkeresi a „com” zónát üzemeltető névkiszolgálókat. Ezután lekérdezi a „com” névkiszolgálóktól a „contoso.com” zónát üzemeltető névkiszolgálókat.  Végül lekérdezi ezektől a névkiszolgálóktól a „www.contoso.com” címet.

Ez az eljárás a DNS-név feloldása. Szigorú értelemben véve a DNS-feloldás más lépéseket is tartalmaz, például a CNAME-rekordok követését, de ez nem fontos a DNS-delegálás megértéséhez.

Hogyan „mutat rá” egy szülőzóna a gyermekzóna névkiszolgálóira? Ezt egy speciális DNS-rekorddal, az úgynevezett névkiszolgálói rekorddal hajtja végre. Például a gyökérzóna tartalmazza a „com” névkiszolgálói rekordjait, és megjeleníti a „com” zóna névkiszolgálóit. A „com” zóna pedig tartalmazza a „contoso.com” névkiszolgálói rekordjait, amelyek a „contoso.com” zóna névkiszolgálóit mutatják. Egy szülőzónán belüli gyermekzóna névkiszolgálói rekordjainak beállítását nevezzük tartománydelegálásnak.

Az alábbi képen egy példa DNS-lekérdezés látható. A contoso.net és a partners.contoso.net Azure DNS-zónák.

![DNS-névkiszolgáló](./media/dns-domain-delegation/image1.png)

1. Az ügyfél lekéri a `www.partners.contoso.net` címet a helyi DNS-kiszolgálóról.
1. A helyi DNS-kiszolgálón nem található meg a rekord, így lekéri azt a gyökér-névkiszolgálótól.
1. A gyökér-névkiszolgálón sem található meg a rekord, azonban ismeri a `.net`-névkiszolgáló címét, és megadja azt a DNS-kiszolgálónak.
1. A DNS-kiszolgáló elküldi a kérést a `.net`-névkiszolgálónak. Ezen sem található meg a rekord, azonban ismeri a contoso.net névkiszolgáló címét. Ebben az esetben ez egy DNS-zóna, amely az Azure DNS-en fut.
1. A `contoso.net` zónában nem található meg a rekord, azonban ismeri a `partners.contoso.net` névkiszolgálója címét, és ezt adja vissza válaszként. Ebben az esetben ez egy DNS-zóna, amely az Azure DNS-en fut.
1. A DNS-kiszolgáló lekéri a `partners.contoso.net` IP-címét a `partners.contoso.net` zónából. Ezen megtalálható az „A” rekord, és az IP-címet adja vissza válaszként.
1. A DNS-kiszolgáló megadja az IP-címet az ügyfélnek.
1. Az ügyfél csatlakozik a `www.partners.contoso.net` webhelyhez.

A delegálások a névkiszolgálói rekordok két példányával rendelkeznek: egy a gyermekzónára mutató szülőzónában, egy pedig magában a gyermekzónában található. A „contoso.net” zóna a „net” névkiszolgálói rekordjai mellett a „contoso.net” névkiszolgálói rekordjait is tartalmazza. Ezek a rekordok az úgynevezett mérvadó névkiszolgálói rekordok, és a gyermekzóna tetején találhatók.

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan [delegálhat tartományokat az Azure DNS-be](dns-delegate-domain-azure-dns.md).

