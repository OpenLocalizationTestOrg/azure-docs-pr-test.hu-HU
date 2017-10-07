---
title: "aaaAzure DNS – gyakori kérdések |} Microsoft Docs"
description: "Az Azure DNS kapcsolatos gyakori kérdések"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: jonatul
ms.openlocfilehash: 55006e9d8b311f1e94678eb9d35ce3448b936488
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-faq"></a>Az Azure DNS – gyakori kérdések

## <a name="about-azure-dns"></a>Az Azure DNS szolgáltatással kapcsolatos

### <a name="what-is-azure-dns"></a>Mi az Azure DNS?

Tartománynévrendszer hello, vagy a DNS, felelős fordítása (vagy feloldása) egy webhelyre vagy szolgáltatásba név tooits IP-cím. Az Azure DNS egy olyan üzemeltetési szolgáltatás DNS-tartományok, biztosítani a névfeloldást a Microsoft Azure-infrastruktúra használatával. Által az Azure-ban a tartományt üzemeltet, kezelheti a DNS-rekordok hello ugyanazon hitelesítő adatokkal, API-k, eszközök és számlázási, a más Azure-szolgáltatásokkal.

Az Azure DNS-DNS-tartományok Azure DNS névkiszolgálóit globális hálózata üzemelnek. Nem egyedi, hogy minden DNS-lekérdezés melléket hello legközelebbi elérhető DNS-kiszolgáló hálózati is használjuk. Gyors teljesítmény és a magas rendelkezésre állást biztosít a tartományhoz.

hello Azure DNS szolgáltatást az Azure Resource Manager alapul. Ilyen előnyöket az erőforrás-kezelő szolgáltatásait, például a szerepköralapú hozzáférés-vezérlés, a vizsgálati naplók és a erőforrás zárolását. A tartományok és a rekordok hello Azure-portálon az Azure PowerShell-parancsmagok segítségével kezelhetők, és platformfüggetlen Azure CLI hello. Automatikus DNS-kezelési igénylő alkalmazások integrálhatók a hello keresztül hello szolgáltatás REST API-t és az SDK-k.

### <a name="how-much-does-azure-dns-cost"></a>Milyen mértékű nem költségeket az Azure DNS?

hello Azure DNS számlázási modellt az Azure DNS-ben és a DNS-lekérdezések kapnak hello száma tárolt DNS-zóna hello számán alapul. Kedvezményeket biztosított-használata alapján.

További információkért lásd: hello [árképzést ismertető oldalra Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-hello-sla-for-azure-dns"></a>Mi az az Azure DNS hello SLA-t?

Garantáljuk, hogy érvényes DNS-kérelmek fog válasznak legalább egy Azure DNS-név kiszolgálóról legalább hello idő 99,99 %.

További információkért lásd: hello [Azure DNS szolgáltatásiszint-szerződés lapon](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-hello-same-as-a-dns-domain"></a>Újdonságok a DNS-zónát? Az DNS tartományként hello azonos? 

Egy tartomány hello tartománynévrendszerben, például "contoso.com" egy egyedi nevet.

A DNS-zónák használt toohost hello DNS-rekordok az adott tartományban. Hello "contoso.com" tartomány például számos DNS-rekordokat, például "mail.contoso.com" (levelezési kiszolgálóhoz) és "www.contoso.com" (webhelyhez) is tartalmazhat. Ezek a támadó által hello DNS-zóna "contoso.com".

A tartománynév *csak egy nevet*, mivel a DNS-zónát egy adatforrás, melyhez a tartománynév DNS-rekordjainak hello tartalmazó. Az Azure DNS lehetővé teszi a DNS-zóna toohost, és egy tartományhoz az Azure-ban hello DNS-rekordok kezelése. Is biztosít a DNS-névkiszolgálók tooanswer DNS-lekérdezéseket a hello Internet.

### <a name="do-i-need-toopurchase-a-dns-domain-name-toouse-azure-dns"></a>Szükséges DNS tartomány neve toouse Azure DNS toopurchase? 

Nem feltétlenül.

Nem kell egy DNS-zóna Azure DNS-tartomány toohost toopurchase. Létrehozhat egy DNS-zóna bármikor hello tartományneve a tulajdonos nélkül. A zóna a DNS-lekérdezések csak oldható fel, ha azok a rendszer átirányítja toohello Azure DNS névkiszolgálóit hozzárendelt toohello zóna.

Ha a kívánt toolink a DNS-zóna hello globális DNS-hierarchia – ezzel a DNS-lekérdezi az bárhol a hello world toofind a DNS-zónát, és a DNS-rekordokat a válaszolni kell toopurchase hello tartomány nevét.

## <a name="azure-dns-features"></a>Az Azure DNS-szolgáltatások

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Támogatja az Azure DNS DNS-alapú forgalom-útválasztási vagy végpont feladatátvevő?

DNS-alapú forgalom útválasztási és -végpont feladatátvételi Azure Traffic Manager által biztosított. Ez az Azure DNS együtt használható külön Azure szolgáltatás. További információkért lásd: hello [Traffic Manager – áttekintés](../traffic-manager/traffic-manager-overview.md).

Az Azure DNS csak támogatja a "static" DNS-tartomány, ahol minden egyes megadott DNS-rekord a DNS-lekérdezés mindig kapja hello üzemeltető azonos DNS-választ.

### <a name="does-azure-dns-support-domain-name-registration"></a>Támogatja az Azure DNS tartománynév-regisztrációs?

Nem. Az Azure DNS jelenleg nem támogatja tartománynevek megvásárlását. Ha azt szeretné, hogy toopurchase tartományok, egy külső regisztrációs toouse kell. hello regisztráló általában egy kis éves díj költségek. hello tartományok majd az Azure DNS-lehet üzemeltetni, a DNS-rekordok kezelése. Lásd: [delegálása a tartományi tooAzure DNS](dns-domain-delegation.md) részleteiről.

Ez a tulajdonság azt a várakozó fájlok számát a nyomon követett. A visszajelzési webhelyet használhatja túl[regisztrálja ezt a szolgáltatást a támogatása](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-private-domains"></a>Támogatja az Azure DNS "private" tartományok?

Nem. Az Azure DNS jelenleg csak az Internet felé néző tartományokat támogatja.

Ez a tulajdonság azt a várakozó fájlok számát a nyomon követett. A visszajelzési webhelyet használhatja túl[regisztrálja ezt a szolgáltatást a támogatása](https://feedback.azure.com/forums/217313-networking/suggestions/10737696-enable-split-dns-for-providing-both-public-and-int).

Információ a belső DNS-beállítások az Azure-ban: [névfeloldás virtuális gépek és a Szerepkörpéldányok](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="does-azure-dns-support-dnssec"></a>Azure DNS-ben támogatja a DNSSEC?

Nem. Az Azure DNS jelenleg nem támogatja a DNSSEC.

Ez a tulajdonság azt a várakozó fájlok számát a nyomon követett. A visszajelzési webhelyet használhatja túl[regisztrálja ezt a szolgáltatást a támogatása](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Támogatja az Azure DNS zónaletöltés (AXFR/IXFR)?

Nem. Az Azure DNS jelenleg nem támogatja a zónaátvitelek. DNS-zónák lehet [importálja az Azure DNS hello Azure parancssori felület használatával](dns-import-export.md). DNS-rekordok majd hello szolgáltatással kezelendő [Azure DNS-kezelési portál](dns-operations-recordsets-portal.md), a [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell-parancsmagok](dns-operations-recordsets.md), vagy [ Parancssori eszköz](dns-operations-recordsets-cli.md).

Ez a tulajdonság azt a várakozó fájlok számát a nyomon követett. A visszajelzési webhelyet használhatja túl[regisztrálja ezt a szolgáltatást a támogatása](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Támogatja az Azure DNS URL-cím átirányítások?

Nem. URL-cím átirányítási szolgáltatások nincsenek ténylegesen egy DNS-szolgáltatás – hello DNS szint, hanem hello HTTP szinten működnek. Néhány DNS szolgáltatók toobundle az általános ajánlat részeként átirányítási URL-cím szolgáltatásként. Ez nem jelenleg támogatott Azure DNS által.

Ez a funkció a várakozó fájlok számát a kulcsban követhető nyomon. A visszajelzési webhelyet használhatja túl[regisztrálja ezt a szolgáltatást a támogatása](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

## <a name="using-azure-dns"></a>Az Azure DNS-sel

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Telepíthetek üzemeltetni használata az Azure DNS- és egy másik DNS-szolgáltatónál tartományhoz?

Igen. Az Azure DNS támogatja a párhuzamos üzemeltetési tartományok más DNS-szolgáltatásokkal.

toodo Igen, szükséges toomodify hello Névkiszolgálói rekordok hello tartomány (mely vezérlő, mely szolgáltatók kapjanak DNS lekérdezések hello tartomány) toopoint toohello névkiszolgálók mindkét szolgáltatót. Ezeket a Névkiszolgálói rekordokat kell 3 helyen módosított toobe: az Azure DNS szolgáltatásban a hello más szolgáltatót, és hello szülőzónában (hello regisztrációs keresztül általában konfigurálva). A DNS-delegálás további információkért lásd: [DNS-tartomány delegálás](dns-domain-delegation.md).

Továbbá, hogy mindkét DNS-szolgáltató közötti szinkronban-e hello tartomány DNS-rekordjait hello tooensure van szüksége. Az Azure DNS jelenleg nem támogatja a DNS zónaletöltés. Toosynchronize DNS-rekordok vagy hello segítségével kell [Azure DNS-kezelési portál](dns-operations-recordsets-portal.md), a [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell-parancsmagok](dns-operations-recordsets.md) , vagy [CLI eszköz](dns-operations-recordsets-cli.md).

### <a name="do-i-have-toodelegate-my-domain-tooall-4-azure-dns-name-servers"></a>Van toodelegate a tartomány tooall 4 Azure DNS névkiszolgálóit?

Igen. Az Azure DNS névkiszolgálóit 4 tooeach DNS-zóna, a tartalék elkülönítési és nagyobb rugalmasság rendeli hozzá. a tooqualify hello Azure DNS SLA-t, a tartomány tooall 4 névkiszolgálók toodelegate szüksége.

### <a name="what-are-hello-usage-limits-for-azure-dns"></a>Az Azure DNS hello használati korlátai

a következő alapértelmezett korlátokat hello Azure DNS használata esetén alkalmazza:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Áthelyezhető egy Azure DNS-zóna, erőforráscsoport-sablonok vagy előfizetések között?

Igen. DNS-zónák akkor helyezheti át, erőforráscsoport-sablonok vagy előfizetések között.

Nincs hatással a DNS-lekérdezéseket a DNS-zónák áthelyezésekor. hello toohello zónához hozzárendelt névkiszolgálók azonos, és a DNS-lekérdezések feldolgozásának teljes szokásos módon hello maradnak.

További információt és útmutatást toomove DNS-zónák, lásd: [erőforrások tooa új erőforráscsoportba vagy előfizetésbe áthelyezése](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-tootake-effect"></a>Mennyi időt vesz igénybe DNS módosítások tootake hatás?

Új DNS-zónák és DNS-rekordok általában megjelennek a hello Azure DNS névkiszolgálóit gyorsan, néhány másodpercen belül.

Módosítások tooexisting DNS-rekordok még több időt vesz igénybe, de kell még mindig tükrözi az Azure DNS névkiszolgálóit hello 60 másodpercen belül. Ebben az esetben DNS-gyorsítótárazás kívül Azure DNS-ben (amelyet a DNS-ügyfelek és a DNS rekurzív feloldókat) is hatással lehet hello idő egy DNS-módosítás toobe hatékony. A gyorsítótárazás időtartama hello idő-Élettartam (TTL) tulajdonsága minden rekordhalmaz használatával lehet irányítani.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>Hogyan védheti meg a DNS-zónák véletlen törlése ellen?

Az Azure DNS által felügyelt Azure Resource Manager használatával, és számos előnyt biztosít az hello hozzáférési Alkalmazásvezérlési szolgáltatásokat, hogy az Azure Resource Manager biztosít. Szerepköralapú hozzáférés-vezérlés lehet használt toocontrol, mely felhasználó rendelkezik olvasási vagy írási hozzáférést tooDNS zónák és a rekordhalmazok jelentésével. Erőforrás zárolások alkalmazott tooprevent véletlen módosításának és törlésének DNS-zónák és a rekordhalmazok lehet.

További információkért lásd: [védelme DNS-zónák és rekordok](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Hogyan állíthatom be SPF-rekordokat az Azure DNS?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Hogyan állíthatom be egy nemzetközi tartományneveket (IDN) az Azure DNS?

Nemzetközi tartományneveket (IDN) működnek-e kódolását minden DNS neve "[punycode](https://en.wikipedia.org/wiki/Punycode)". DNS-lekérdezéseket tesz a punycode titkosítású nevek használatával.

Konfigurálhatja az Azure DNS-nemzetközi tartományneveket (IDN) első formátumba való átalakítása hello zóna neve, vagy a rekordhalmaz nevének toopunycode. Az Azure DNS jelenleg nem támogatja a punycode és a beépített átalakítását.

## <a name="next-steps"></a>Következő lépések

[További tudnivalók az Azure DNS-ben](dns-overview.md)
<br>
[További információ a DNS-zónák és rekordok](dns-zones-records.md)
<br>
[Ismerkedés az Azure DNS-ben](dns-getstarted-portal.md)

