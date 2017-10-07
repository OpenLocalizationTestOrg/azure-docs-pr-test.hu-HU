---
title: aaaWhat a Traffic Manager |} Microsoft Docs
description: "Ez a cikk segít megérteni a Traffic Manager van, és hogy hello jobb forgalom útválasztási választás az alkalmazáshoz"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: kumud
ms.openlocfilehash: 8e63ed11cdcdc03ae9cd28f88f0d1f9dc2cd44ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-traffic-manager"></a>A Traffic Manager áttekintése

A Microsoft Azure Traffic Manager lehetővé teszi toocontrol hello terjesztési felhasználói forgalomnak a végpontok különböző adatközpontokban. Traffic Manager által támogatott szolgáltatás végpontok közé tartoznak az Azure virtuális gépeken, Web Apps, és a felhőalapú szolgáltatások. A Traffic Manager külső, nem Azure-végpontokkal együtt is használható.

A TRAFFIC Manager hello tartománynévrendszer (DNS) toodirect ügyfél kérelmek toohello leginkább megfelelő végpont a forgalom-útválasztási módszert és hello végpontok hello állapotának alapján használja. A TRAFFIC Manager biztosít számos különböző [forgalom-útválasztási módszert](traffic-manager-routing-methods.md) és [megfigyelési lehetőségek végpont](traffic-manager-monitoring.md) toosuit különböző alkalmazás igényeinek és automatikus feladatátvételt modellek. A TRAFFIC Manager rugalmas toofailure, többek között a hello sikertelen volt-e egy teljes Azure-régióban.

## <a name="traffic-manager-benefits"></a>A TRAFFIC Manager előnyei

A TRAFFIC Manager segítségével:

* **Rendelkezésre állás, a kritikus alkalmazások fejlesztése**

    A TRAFFIC Manager biztosítja a magas rendelkezésre állás, az alkalmazások figyelése a végpontokat, és automatikus feladatátvétel megadásával, amikor a végpont leáll.

* **A nagy teljesítményű alkalmazások válaszképességét javítása**

    Azure lehetővé teszi toorun felhőszolgáltatás vagy webhely található hello világ különböző adatközpontokban. A TRAFFIC Manager úgy irányítja a forgalmat toohello végpont a legkisebb hálózati késést hello hello ügyfél javítja az alkalmazások válaszkészségét.

* **Szolgáltatás-karbantartás állásidő nélkül**

    A leállás nélküli alkalmazások tervezett karbantartási műveleteket végezheti el. A TRAFFIC Manager irányítja a forgalmat tooalternative végpontok hello karbantartás ideje alatt megszakadnak.

* **A helyszíni és felhőalapú alkalmazások**

    A TRAFFIC Manager támogatja a külső, lehetővé téve a hibrid használt toobe-Azure végpontok a felhőalapú és helyszíni környezetekhez, például hello "kapacitásnövelés felhő," "áttelepítése felhő," és "feladatátvevő felhő" forgatókönyvek.

* **A forgalom nagy, összetett telepítések terjesztése**

    Használatával [Traffic Manager-profilok beágyazott](traffic-manager-nested-profiles.md), forgalom-útválasztási módszert lehet kombinált toocreate kifinomult és rugalmas szabályok toosupport hello nagyobb, összetettebb telepítések kell.

## <a name="how-traffic-manager-works"></a>A Traffic Manager működése

Az Azure Traffic Manager lehetővé teszi toocontrol hello terjesztési forgalom között az alkalmazási végpontokat. A végpont bármilyen internetre-szolgáltatott belül vagy kívül Azure.

A TRAFFIC Manager biztosít két fő előnnyel jár:

1. Megfelelően fel számos tooone forgalmat [forgalom-útválasztási módszer](traffic-manager-routing-methods.md)
2. [Folyamatosan figyelje a végpont állapotfigyelő](traffic-manager-monitoring.md) és automatikus feladatátvétel, ha a végpontok sikertelen

Amikor az ügyfél tooconnect tooa szolgáltatás, először azt kell tartoznia hello szolgáltatás tooan IP-cím hello DNS-nevét. hello majd ügyfélprogramja toothat IP-cím tooaccess hello szolgáltatás.

**hello legfontosabb pont toounderstand, hogy a Traffic Manager hello DNS szinten működik-e.**  A TRAFFIC Manager által használt DNS toodirect ügyfelek toospecific szolgáltatás végpontjait hello forgalom-útválasztási módszer hello szabályok alapján. Az ügyfelek kapcsolódnak kijelölt toohello végpont **közvetlenül**. A TRAFFIC Manager nincs olyan proxy vagy az átjáró. A TRAFFIC Manager hello ügyfél és a hello szolgáltatás közötti információátadáshoz hello forgalom nem jelenik meg.

### <a name="traffic-manager-example"></a>A TRAFFIC Manager – példa

Contoso vállalat egy új partner portál szert. a portál URL-címe hello https://partners.contoso.com/login.aspx. az Azure régiói három hello alkalmazás üzemel. tooimprove rendelkezésre állási és globális teljesítmény maximalizálása, azok a Traffic Manager toodistribute ügyfél forgalom toohello legközelebbi elérhető végpontot használni.

tooachieve Ez a konfiguráció befejezéséhez hello a következő lépéseket:

1. A szolgáltatás három példányát telepítse. hello DNS-nevek két központi telepítését a következők: "contoso-us.cloudapp .net", "contoso-eu.cloudapp .net" és "contoso-asia.cloudapp .net".
2. Hozzon létre Traffic Manager-profil, "contoso.trafficmanager.net" nevű, és konfigurálja úgy legyenek hello három toouse hello "Teljesítmény" forgalom-útválasztási módszert.
* Konfigurálja a személyes tartomány nevét, a "partners.contoso.com néven", a toopoint too'contoso.trafficmanager.net ", egy DNS CNAME rekord használatával.

![A TRAFFIC Manager DNS-konfiguráció][1]

> [!NOTE]
> Amikor kreatív tartomány használata az Azure Traffic Manager, a személyes tartomány neve tooyour Traffic Manager szolgáltatásbeli tartománynevére egy CNAME toopoint kell használnia. DNS-szabványokból nem teszik lehetővé toocreate egy olyan CNAME REKORDOT a "legfelső pontján" hello (vagy a főtanúsítvány) tartomány. Így nem hozható létre egy CNAME REKORDOT a "contoso.com" (más néven "csupasz" tartományhoz). Csak a "contoso.com", például "www.contoso.com" tartomány egy olyan CNAME REKORDOT hozhat létre. a korlátozás toowork, azt javasoljuk, egy egyszerű HTTP-átirányítási toodirect kérések "contoso.com" tooan alternatív nevének például "www.contoso.com".

### <a name="how-clients-connect-using-traffic-manager"></a>Hogy az ügyfelek hogyan kapcsolódnak a Traffic Managerrel

A Folytatás hello előző példa, ha egy ügyfél lekérdezi hello lap https://partners.contoso.com/login.aspx hello ügyfél hajtja végre a következő lépéseket tooresolve hello DNS-név hello és kapcsolatot létesíteni:

![A Traffic Manager használ a kapcsolat felépítése][2]

1. hello ügyfél küld a DNS lekérdezés konfigurált tooits rekurzív DNS szolgáltatás tooresolve hello neve "partners.contoso.com"néven. A rekurzív DNS-szolgáltatás, más néven a "helyi DNS" szolgáltatás nem fut közvetlenül DNS-tartományok. Ehelyett hello ügyfél off-loads hello munkahelyi hello való kapcsolódás a különböző mérvadó DNS-szolgáltatások hello szükséges Internet tooresolve között egy DNS-nevet.
2. tooresolve hello DNS-név, hello rekurzív DNS-szolgáltatás hello névkiszolgálók megkeresi a "contoso.com" tartomány hello. Ezután csatlakozik a ezeket neve kiszolgálók toorequest hello "partners.contoso.com" DNS-rekordot. hello contoso.com DNS-kiszolgálók hello CNAME rekordot, amely toocontoso.trafficmanager.net mutat vissza.
3. A következő hello rekurzív DNS-szolgáltatás hello névkiszolgálók hello Azure Traffic Manager szolgáltatás által biztosított hello "trafficmanager.net" tartomány talál. Majd küld hello "contoso.trafficmanager.net" DNS-rekord toothose a DNS-kiszolgálók.
4. a Traffic Manager névkiszolgálók hello hello kérés fogadásához. A végpont alapján választ:

    - minden egyes végpont konfigurált hello állapotának (letiltott végpontok nem adott vissza)
    - minden egyes végpont hello Traffic Manager állapota alapján hello aktuális állapotát ellenőrzi. További információkért lásd: [Traffic Manager-végpont figyelés](traffic-manager-monitoring.md).
    - a kiválasztott forgalom-útválasztási módszer hello. További információkért lásd: [Traffic Manager útválasztási metódusok](traffic-manager-routing-methods.md).

5. a kiválasztott hello végpont egy másik DNS CNAME rekord adja vissza a rendszer. Ebben az esetben ossza meg velünk tegyük fel, hogy a contoso-us.cloudapp.net adja vissza.
6. A következő hello rekurzív DNS-szolgáltatás hello névkiszolgálók hello "cloudapp.net" tartomány talál. Az adott név kiszolgálók toorequest hello "contoso-us.cloudapp .net" kapcsolatba lép a DNS-rekordot. Hello USA-alapú szolgáltatás végpontjának hello IP-címét tartalmazó DNS "A" rekordot ad vissza.
7. hello rekurzív DNS-szolgáltatás hello eredményeit összesíti, és egyetlen DNS-válasz toohello ügyfél adja vissza.
8. hello ügyfél hello DNS eredményeket kap, és csatlakozik a megadott IP-címhez toohello. hello ügyfél közvetlenül kapcsolódik a toohello alkalmazás szolgáltatási végpont, nem Traffic Manager használatával. Mivel ez egy HTTPS-végpontot, hello ügyfél hello szükséges SSL/TLS-kézfogás hajt végre, és majd lehetővé teszi egy HTTP GET kérést hello ' / login.aspx "lapon.

hello rekurzív DNS-szolgáltatás gyorsítótárazza a hello DNS-válaszok kap. hello DNS-feloldási hello ügyféleszközön is gyorsítótáraz hello eredménye. Gyorsítótárazás lehetővé teszi, hogy a DNS-lekérdezések későbbi toobe megválaszolása gyorsabban hello gyorsítótárból adatokkal, nem pedig más névkiszolgálók lekérdezése. hello "idő TTL" (TTL) tulajdonsága minden DNS-rekord hello gyorsítótár hello időtartamát határozza meg. Rövidebb értékek gyorsabb gyorsítótár lejárati és így további üzenetváltások toohello Traffic Manager névkiszolgálók eredményez. Hosszabb értékek azt jelenti, hogy hosszabb toodirect forgalom sikertelen végpont többet is igénybe vehet. A TRAFFIC Manager lehetővé teszi tooconfigure hello élettartam a Traffic Manager DNS-válaszok toobe engedélyezett minimumérték megegyezik 0 másodperc használt és magas, mint 2 147 483 647 másodperc (hello megfelelnek a tartomány maximális [RFC-1035 szabványnak megfelelően](https://www.ietf.org/rfc/rfc1035.txt)), amely lehetővé teszi, toochoose hello érték amely az alkalmazás hello igényeinek legjobban egyensúlyba kerüljön.

## <a name="pricing"></a>Díjszabás

Díjszabási információkért lásd: [Traffic Manager árazás](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="faq"></a>GYIK

Lásd: gyakran ismételt kérdések a Traffic Manager [Traffic Manager – gyakori kérdések](traffic-manager-FAQs.md)

## <a name="next-steps"></a>Következő lépések

A Traffic Manager további [végpont figyelési és automatikus feladatátvételt](traffic-manager-monitoring.md).

A Traffic Manager további [forgalom-útválasztási módszerei](traffic-manager-routing-methods.md).

Megismerhet néhány hello más kulcs [hálózati lehetőségeket](../networking/networking-overview.md) Azure.

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

