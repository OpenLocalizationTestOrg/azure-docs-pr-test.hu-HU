---
title: "a hálózati biztonsági funkciók aaaProtect személyes adatokat az Azure-ral |} Microsoft Docs"
description: "Az Azure hálózati biztonsági funkciókat használ, a személyes adatok védelme"
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
ms.openlocfilehash: be112a9408d327ccedf871656afe800fc7f775e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-with-network-security-features-azure-application-gateway-and-network-security-groups"></a>Hálózati biztonsági funkciók személyes adatok védelme: Azure Application Gateway és a hálózati biztonsági csoportok

Ez a cikk ismerteti, könnyebben Azure Application Gateway és a hálózati biztonsági csoportok tooprotect személyes adatok felhasználásával.

Egy többrétegű biztonsági stratégia tooprotect hello adatvédelmi személyes adatok egy fontos eleme a például az SQL-injektálás vagy többhelyes scripting közös biztonsági rések elleni védelmet. Induló az Azure virtual Networkhöz való tartása nem kívánt hálózati forgalom segíti a bizalmas adatokat, és a Microsoft Azure által biztosított eszközök toohelp védeni ellen, az adatok esetleges biztonsági sérülésekkel szembeni védelmére.

## <a name="scenario"></a>Forgatókönyv

A nagy körutazás vállalati telephelyének hello az Amerikai Egyesült Államokban, a műveletek toooffer útvonalak hello mediterrán, Adriai, és Balti tengerek, valamint hello Brit-szigetekre növekszik. Az adott erőfeszítéseket abból a célból, szerzett több kisebb luxushajókon Olaszország, német, Dánia és hello brit a sorok

hello vállalati hello toostore vállalati adatokat a felhőalapú és alkalmazások futtatásához a virtuális gépeken futó feldolgozni, és elérje ezeket az adatokat a Microsoft Azure használja. Ezen adatok tartalmazzák a személyes azonosításra alkalmas adatokat, például neveket, címeket, telefonszámokat, és a globális felhasználói bázis hitelkártya adatait. Minden helyen hagyományos emberi erőforrások adatokat, például a címet, telefonszámot, azonosító számokat és a vállalat alkalmazottai orvosi információt is tartalmaz. hello körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagok, amely tartalmazza az aktuális és korábbi ügyfelek tootrack kapcsolatokkal személyes adatokat.

Vállalati alkalmazottak hozzáférési hello hálózati hello vállalati távoli iroda és hello világ található utazás ügynökök rendelkezik toosome vállalati erőforrások eléréséhez, és használja az Azure virtuális gépek toointeract vele üzemeltetett webalapú alkalmazások.

## <a name="problem-statement"></a>Probléma leírása

hello vállalati kell védelmében hello az ügyfelek és alkalmazottak a személyes adatoknak a támadók számára a kártékony szoftverek biztonsági rések toorun rosszindulatú kód, amely a személyes adatok teheti ki a tárolt vagy hello vállalati felhőalapú alkalmazások által használt.

## <a name="company-goal"></a>Vállalati cél

hello vállalat cél tooensure, hogy jogosulatlan személyek nem fér hozzá vállalati Azure virtuális hálózatokhoz és hello alkalmazások és adatok, amelyek közös biztonsági réseket kihasználva nem találhatók. 

## <a name="solutions"></a>Megoldások

A Microsoft Azure biztonságot nyújt mechanizmusok toohelp megakadályozható, hogy a nem kívánt forgalom írja be az Azure virtuális hálózatot. Bejövő és kimenő forgalom irányítását hagyományosan tűzfalak végzi el. Az Azure hello Alkalmazásátjáró a hello webalkalmazási tűzfal és a hálózati biztonsági csoportok (NSG), egyszerű az elosztott tűzfalat el is használhatja. Ezek az eszközök lehetővé teszik a toodetect, és nem kívánt hálózati forgalom blokkolása.

### <a name="application-gatewayweb-application-firewall"></a>Átjáró vagy webes alkalmazás tűzfal

Hello [webalkalmazási tűzfal](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) hello (WAF) összetevőjét [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) webes alkalmazásokhoz, amelyek egyre célok rosszindulatú támadások, amelyek a közös ismert biztonsági rések elleni védelme biztonsági rések. Egy központi WAF webes támadások ellen, mind biztonságkezelés leegyszerűsíti az alkalmazások módosítása nélkül.

Azure WAF címek különböző, beleértve az SQL-injektálás, helyközi scripting, HTTP protokollal kapcsolatos szabálysértés és rendellenességeket, botok, webbejárók, képolvasók, közös alkalmazás hibás konfigurációja, HTTP szolgáltatásmegtagadásos támadás kategóriák, és más közös támadásokkal, például a parancs injektálási, HTTP kérelem való, a HTTP-válasz felosztásával, és a távoli fájl befoglalási támadásokat. 

Hozzon létre egy alkalmazás WAF, vagy adja hozzá a WAF tooan meglévő Alkalmazásátjáró. Mindkét esetben az Azure Application Gateway saját alhálózatba van szükség.

#### <a name="how-do-i-create-an-application-gateway-with-waf"></a>Hogyan hozható létre olyan átjárót a WAF? 

WAF engedélyezve van, egy új alkalmazás átjárójának toocreate hello a következő:

1. Azure-portálon toohello és hello napló **Kedvencek** hello portal panelén kattintson **új**

2. A hello **új** panelen kattintson a **hálózati**.

3. Kattintson a **Alkalmazásátjáró**.

4. Keresse meg az Azure-portálon toohello **kattintson az új \> hálózati \> Alkalmazásátjáró.**

   ![alkalmazásátjárót létrehozása](media/protect-netsec/app-gateway-01.png)

5. A hello **alapjai** panelen megjelenő, adja meg a mezőket a következő hello hello értékét: név, a réteg (Standard vagy WAF), Termékváltozat-méretét (kicsi, közepes vagy nagy) példány száma (a magas rendelkezésre állás 2), előfizetés, erőforráscsoportot, és Hely.

6. A hello **beállítások** panel alatt megjelenő **virtuális hálózati**, kattintson a **virtuális hálózatot választ**. Ez a lépés nyílik meg hello válasszon virtuális hálózat panel.

7. Kattintson a **hozzon létre új** tooopen hello **virtuális hálózat létrehozása** panelen.

8. Adja meg a következő értékek hello: név, címtartomány, az alhálózat neve, az alhálózati címtartományt. Kattintson az **OK** gombra.

9. A hello **beállítások** részen **előtérbeli IP-konfiguráció**, válassza ki a hello IP-cím típusa.

10. Kattintson a **egy nyilvános IP-cím kiválasztása** majd **hozzon létre új.**

11. Fogadja el alapértékként hello, és kattintson a **OK gombra.**

12. Hello a **beállítások** részen **figyelő konfigurációs**, válassza ki a HTTP vagy HTTPS alatt toouse **protokoll**. toouse HTTPS, egy tanúsítványra szükség.

13. Hello WAF adott beállításainak a konfigurálásához: **állapot tűzfal** (**engedélyezve**) és **tűzfal módban** (**megelőzési**). Ha úgy dönt, **észlelési** hello módot forgalmat csak naplózza.

14. Felülvizsgálati hello **összegzés** lapot, és kattintson **OK**. Most hello Alkalmazásátjáró sorba és létre.

Hello Alkalmazásátjáró létrehozása után léphet tooit hello portálon, és folytassa hello Alkalmazásátjáró konfigurálását.

![az Alkalmazásátjáró létrehozása](media/protect-netsec/adatum-app-gateway.png)

#### <a name="how-do-i-add-waf-tooan-existing-application"></a>Hogyan hozzá WAF tooan meglévő alkalmazást?

egy meglévő alkalmazás átjáró toosupport WAF tooupdate megelőzési módban hello a következő:

1. Az Azure-portálon hello **Kedvencek** ablaktáblán kattintson a **összes erőforrás**.

2. Kattintson a hello meglévő Alkalmazásátjáró hello **összes erőforrás** panelen. 
>[!NOTE]
Megjegyzés: Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja hello neve hello szűrő név szerint... mezőbe tooeasily hozzáférés hello DNS-zónát.
3. Kattintson a **webalkalmazási tűzfal** és hello Alkalmazásátjáró beállításainak frissítése: **tooWAF réteg frissítése** (be van jelölve), **állapot tűzfal** (engedélyezve),  **Tűzfal módban** (megelőzési). Szükség tooconfigure hello szabálykészletben, és a letiltott szabályok konfigurálása.

További részletes információt egy új alkalmazás átjárójának WAF toocreate és hogyan tooadd WAF tooan meglévő Alkalmazásátjáró, lásd: [hozzon létre egy alkalmazás webalkalmazási tűzfal hello portál használatával.](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)

### <a name="network-security-groups"></a>Network Security Groups (Hálózati biztonsági csoportok)

A [hálózati biztonsági csoport](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) felsorolja azokat a szabályokat, amelyek engedélyezik vagy megtagadják a hálózati forgalom tooresources túl csatlakoztatott[Azure virtuális hálózatok](https://docs.microsoft.com/azure/virtual-network/) (VNet). NSG-ket lehet társított toosubnets vagy az egyes virtuális gépeken. Amikor egy NSG társított tooa alhálózati, hello szabályokat alkalmazni tooall erőforrások csatlakoztatott toohello alhálózat. Forgalom tovább korlátozhatja is társításával egy NSG tooa VM vagy a hálózati adapterhez.

Az NSG-k négy tulajdonságokat tartalmazzák: név, régió, erőforráscsoport és szabályok.
>[!Note]
Bár az NSG szerepel egy erőforráscsoport, azok bármely erőforráscsoport társított tooresources mindaddig, amíg hello erőforrás hello része egy Azure-régióban van hello NSG.

NSG-szabályok kilenc tulajdonságokat tartalmazzák: név, a protokoll (TCP, UDP vagy \*, mely tartalmazza az ICMP, valamint az UDP és a TCP), forrás-porttartomány, Célporttartomány, forráscímelőtag, célcím-előtag, irányát (bejövő vagy kimenő), a prioritás (100 és 4096 közötti szám) és a hozzáférés típusa (engedélyezése vagy megtagadása). Minden NSG tartalmaz egy alapértelmezett szabálykészletet, hogy törölték, vagy létrehozhat hello szabályok felülbírálják.

#### <a name="how-do-i-implement-nsgs"></a>Hogyan valósítja meg az NSG-ket?

Az NSG-k végrehajtására is szükség van, és számos tervezési szempontot figyelembe kell tootake. Ezek közé tartozik a vonatkozó korlátokat a hello / előfizetést és NSG; szabálynál NSG-k száma VNet és alhálózat kialakítása, különleges szabályokat, az ICMP-forgalmat, elszigetelési meghatározási alhálózatok, terheléselosztó és több.

Planning and implementing NSG-ket, és egy minta környezet-forgatókönyv és a További útmutatóért lásd: [hálózati forgalmat hálózati biztonsági csoportokkal.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)

#### <a name="how-do-i-create-rules-in-an-nsg"></a>Hogyan hozható létre szabályokat egy NSG?

toocreate bejövő szabályok egy meglévő NSG, a következő hello:

1. Kattintson a **Tallózás**, majd **hálózati biztonsági csoportok**.

2. Az NSG-k hello listájában kattintson **NSG-előtérbeli**, majd **bejövő biztonsági szabályok.**

3. A bejövő biztonsági szabályok hello listáján kattintson **hozzáadása.**

4. Írja be a következő mezők hello hello értékét: nevét, a prioritás, a forrás, a protokoll, a forrás között, célt cél porttartomány és művelet.

Új szabály hello hello NSG néhány másodpercen belül megjelenik.

![hálózati biztonsági szabályok](media/protect-netsec/inbound-security.png)

Az NSG-k toocreate alhálózatok, a szabályok létrehozása, és a társít egy NSG előtér- és alhálózattal, további útmutatásért lásd: [hello Azure-portál használatával a hálózati biztonsági csoportok létrehozása.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)

## <a name="next-steps"></a>Következő lépések

[Az Azure hálózati biztonság](https://azure.microsoft.com/blog/azure-network-security/)

[Azure-hálózat ajánlott biztonsági eljárások](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices)

[A hálózati biztonsági csoportok adatainak beolvasása](https://docs.microsoft.com/rest/api/network/virtualnetwork/get-information-about-a-network-security-group)

[Webalkalmazási tűzfal (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
