---
title: "az Azure az Azure Traffic Manager aaaHigh rendelkezésre állása közötti földrajzi AD FS üzembe helyezése |} Microsoft Docs"
description: "Ez a dokumentum megtudhatja, hogyan toodeploy AD FS az Azure-ban magas elérhetőségét."
keywords: "Az ad fs és az Azure traffic manager Azure Traffic Managerben, földrajzi, több adatközpont, földrajzi adatközpontok, több földrajzi adatközpontok, AD FS az azure AD FS üzembe helyezése, az azure AD FS, az azure AD FS, az azure Active Directory összevonási szolgáltatások telepítése, központi telepítése az AD FS, ad fs, az azure AD FS üzembe helyezése az AD FS központi telepítése az azure AD FS azure, a az AD FS azure, a bevezetés tooAD FS, Azure, az Azure iaas, az AD FS, az AD FS üzembe helyezése helyezze át az AD FS tooazure"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: c5838d749cdc5c8aabbe62b255d568525da747ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Az AD FS nagy rendelkezésre állású, több földrajzi régióra kiterjedő üzembe helyezése az Azure Traffic Managerrel
[Az AD FS üzembe helyezése az Azure-ban](active-directory-aadconnect-azure-adfs.md) biztosít részletes iránymutatás toohow, akkor telepítheti egy egyszerű AD FS infrastruktúrát a szervezet az Azure-ban. Ez a cikk ismerteti hello lépések egy kereszt-földrajzi toocreate telepítési be az Azure Active Directory összevonási szolgáltatások [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Az Azure Traffic Manager segítségével hozzon létre egy földrajzilag terjedésének magas rendelkezésre állás és teljesítmény szempontjából az AD FS infrastruktúra a szervezet számára azáltal, hogy számos különböző elérhető toosuit kell hello infrastruktúrától útválasztási metódusait használatát.

A magas rendelkezésre állású, földrajzi határokon átívelő AD FS-infrastruktúra a következőket teszi lehetővé:

* **A hibaérzékeny pontok kialakulását eltávolítási:** feladatátvételi szolgáltatásokat az Azure Traffic Manager érhet el egy magas rendelkezésre állású AD FS infrastruktúra akkor is, ha egy hello adatközpontokban hello földgömb részét leáll
* **Növeli a teljesítményt:** használható hello javasolt központi telepítést, ez a cikk tooprovide egy nagy teljesítményű AD FS infrastruktúra, amelynek segítségével a felhasználók gyorsabban hitelesítéséhez. 

## <a name="design-principles"></a>Tervezési alapelvek
![Általános tervezési szempontok](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

hello alapvető tervezési alapelvek lesz ugyanazon a tervezési alapelvek hello cikkben AD FS üzembe helyezése az Azure-ban. hello a fenti ábrán egy egyszerű bővítmény hello alapszintű központi telepítést tooanother földrajzi régió. Az alábbiakban néhány pontok tooconsider a központi telepítés toonew földrajzi régió bővítésekor

* **Virtuális hálózat:** hozzon létre egy új virtuális hálózat hello földrajzi régióban toodeploy további AD FS infrastruktúra szeretné. A fenti diagram hello látni Geo1 VNET és Geo2 VNET, hello két virtuális hálózat minden egyes földrajzi régióban.
* **Tartományvezérlők és AD FS-kiszolgáló új földrajzi virtuális:** toodeploy tartományvezérlők hello új földrajzi régióban javasolt, hogy az AD FS hello kiszolgálók hello új régióban nem rendelkeznek toocontact tartományvezérlő egy másik sokkal kötelező hálózati toocomplete hitelesítést, és ezáltal javítása hello teljesítmény.
* **Tárfiókok:** A tárfiókok egy adott régióhoz tartoznak. Fog egy új földrajzi régióban gépek telepítése, mert toocreate új tárfiókok toobe használt hello régióban fog rendelkezni.  
* **Hálózati biztonsági csoportok:** A tárfiókokhoz hasonlóan az egy adott régióban létrehozott hálózati biztonsági csoportok sem használhatók egy másik földrajzi régióban. Ezért szüksége lesz toocreate új hálózati biztonsági csoportok hasonló toothose hello első földrajzi régióban INT és DMZ alhálózati hello új földrajzi régióban.
* **Nyilvános IP-címet a DNS-címkére:** Azure Traffic Manager hivatkozhat tooendpoints csak DNS-címkére keresztül. Ezért szükség toocreate DNS-címkére hello külső Terheléselosztók nyilvános IP-címekhez.
* **Az Azure Traffic Manager:** Microsoft Azure Traffic Manager lehetővé teszi a felhasználói forgalom tooyour szolgáltatás végpontok különböző adatközpontokban hello world körül fut toocontrol hello terjesztési. Az Azure Traffic Manager DNS szint hello működik. DNS válaszok toodirect végfelhasználói forgalom tooglobally elosztott végpontokat használ. Az ügyfelek ezután csatlakoznak toothose végpontok közvetlenül. Különböző beállításokkal útválasztási a teljesítmény, Weighted és prioritás Ehelyett egyszerűen választható a szervezet igényeinek leginkább megfelelő hello lehetőséget. 
* **V-net tooV-net kapcsolat két régiók közötti:** nem kell hello virtuális hálózatok közötti toohave kapcsolatot saját magát. Mivel minden egyes virtuális hálózati hozzáférési toodomain tartományvezérlők rendelkezik, és maga is van az AD FS és a WAP-kiszolgáló, azokat is megvalósítható, ha bármely hello különböző régiókban lévő virtuális hálózatok közötti kapcsolatot. 

## <a name="steps-toointegrate-azure-traffic-manager"></a>Azure Traffic Manager lépéseket toointegrate
### <a name="deploy-ad-fs-in-hello-new-geographical-region"></a>AD FS hello új földrajzi régióban üzembe helyezése
Hajtsa végre a hello lépéseket és irányelveinek [AD FS üzembe helyezése az Azure-ban](active-directory-aadconnect-azure-adfs.md) toodeploy hello azonos topológia hello új földrajzi régióban.

### <a name="dns-labels-for-public-ip-addresses-of-hello-internet-facing-public-load-balancers"></a>DNS-címkére hello Internet Facing (nyilvános) Terheléselosztók nyilvános IP-címeit
Fent említett Azure Traffic Manager hello csak hivatkozhat tooDNS címkék végpontként, és ezért nem fontos toocreate DNS-címkére hello külső Terheléselosztók nyilvános IP-címekhez. Képernyőkép alatt bemutatja, hogyan konfigurálhatja a DNS-címke hello nyilvános IP-cím. 

![DNS-címke](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Az Azure Traffic Manager telepítése
Kövesse az alábbi toocreate egy traffic manager-profil hello lépéseket. További információkért olvassa el a is túl[kezelése az Azure Traffic Manager-profil](../traffic-manager/traffic-manager-manage-profiles.md).

1. **Traffic Manager-profil létrehozása:** Lássa el a Traffic Manager-profilt egy egyedi névvel. Ez a név hello-profil hello DNS-név része, és úgy működik, mint egy előtagot hello Traffic Manager tartománynév-címke. hello neve / előtag kerül too.trafficmanager.net toocreate DNS-címke a traffic Manager. hello az alábbi képernyőfelvétel hello a traffic manager DNS-előtagot, mysts és az eredményül kapott DNS-címke nem mysts.trafficmanager.net beállítása. 
   
    ![Traffic Manager-profil létrehozása](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Forgalomirányítási módszer:** A Traffic Manager három útválasztási beállítást tartalmaz:
   
   * Prioritás 
   * Teljesítmény
   * Súlyozott
     
     **Teljesítmény** hello ajánlott beállítás tooachieve válaszidejű AD FS infrastruktúra. Azonban ehelyett bármilyen másik útválasztási módszert is megadhat, ha az jobban megfelel a környezeti igényeknek. hello AD FS funkciók nincs hatással a hello útválasztási jelölőnégyzetet. További információkért lásd: [A Traffic Manager forgalomirányítási módszerei](../traffic-manager/traffic-manager-routing-methods.md). Ezen hello minta képernyőképe, fent látható hello **teljesítmény** kijelölt módszert.
3. **Végpontok konfigurálása:** hello traffic manager oldalon, kattintson a végpontok, és válassza a Hozzáadás. Ekkor megnyílik egy Hozzáadás végpont lap hasonló toohello alábbi képernyőképen látható
   
   ![Végpontok konfigurálása](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Hello különböző bemenetek kövesse az alábbi hello iránymutatás:
   
   **Típus:** válassza ki az Azure-végpont, mert azt fogja mutató tooan Azure nyilvános IP-cím.
   
   **Name:** tooassociate hello végpont kívánt nevét. Ez nem hello DNS-neve, amely nincs hatással a DNS-rekordokat.
   
   **Erőforrás céltípust:** válasszon nyilvános IP-cím hello érték toothis tulajdonság. 
   
   **Célerőforrás:** ekkor kap egy beállítás toochoose a hello másik DNS-címkére rendelkezik érhető el az előfizetéshez tartozó. Válassza ki a DNS-címke konfigurálásakor megfelelő toohello végpont hello.
   
   Minden egyes földrajzi régió hello Azure Traffic Manager tooroute forgalmat kívánt végpont hozzáadása.
   További információk és részletes lépései tooadd / végpontok konfigurálása a traffic Managerben, tekintse meg a túl[végpontok hozzáadása, letiltása, engedélyezése vagy törlése](../traffic-manager/traffic-manager-endpoints.md)
4. **Mintavétel:** hello traffic manager oldalon kattintson a konfiguráció. Hello konfigurálása lapon a toochange hello figyelő beállítások tooprobe: 80-as HTTP-port és a relatív elérési út /adfs/probe szüksége
   
    ![Mintavétel konfigurálása](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Győződjön meg arról, hogy hello hello végpontok állapota ONLINE hello konfigurálásának befejezését követően**. Végpontjai "csökkentett teljesítményű" állapota esetén az Azure Traffic Manager fog tenni a legjobb kísérlet tooroute hello forgalom hello diagnosztikai nem megfelelő, és minden végpontok érhetők el.
   > 
   > 
5. **DNS-rekordok módosítása az Azure Traffic Manager:** az összevonási szolgáltatás egy CNAME toohello Azure Traffic Manager DNS-nevet kell lennie. Hozzon létre egy CNAME REKORDOT a hello nyilvános DNS-rekordokat, hogy ténylegesen személy, aki tooreach hello összevonási szolgáltatás megpróbál eléri a hello Azure Traffic Manager.
   
    Ha például toopoint hello összevonási szolgáltatás fs.fabidentity.com toohello Traffic Manager kellene tooupdate a DNS-erőforrás rekord toobe hello a következő:
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-hello-routing-and-ad-fs-sign-in"></a>Hello az Útválasztás és az AD FS-bejelentkezés tesztelése
### <a name="routing-test"></a>Útválasztási teszt
Egy nagyon egyszerű teszt hello irányításához tootry ping hello összevonási szolgáltatás DNS-neve minden egyes földrajzi régióban gépről lenne. Hello útválasztási módszer van kiválasztva, attól függően, hogy ténylegesen pingeli hello végpont hello ping megjelenítési fog szerepelni. Például hello teljesítmény választva, majd hello végpont legközelebbi toohello útválasztási hello ügyfél régió lejár. Alább van két különböző régióban ügyfél gépekről két pingelésre hello pillanatképe, egy EastAsia régióban pedig az USA nyugati régiója. 

![Útválasztási teszt](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>AD FS-bejelentkezési teszt
hello IdpInitiatedSignon.aspx lap segítségével hello legegyszerűbb módja tootest AD FS el. A sorrend toobe képes toodo, hogy-e szükséges tooenable hello IdpInitiatedSignOn hello AD FS-tulajdonságok. Kövesse az alábbi tooverify hello lépéseket az AD FS beállítására

1. Futtatás hello parancsmagot a powershellel, tooset hello AD FS-kiszolgáló alatt azt tooenabled. 
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
2. Bármilyen külső gépről keresse fel a következő címet: https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. AD FS hello oldal megjelenik alá, például:
   
    ![ADFS-teszt – hitelesítő kérdés](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    a sikeres bejelentkezés után pedig az alább látható sikert jelző üzenetet kapja:
   
    ![ADFS-teszt – sikeres hitelesítés](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>Kapcsolódó hivatkozások
* [Alapszintű AD FS-telepítés az Azure-ban](active-directory-aadconnect-azure-adfs.md)
* [Microsoft Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)
* [A Traffic Manager forgalomirányítási módszerei](../traffic-manager/traffic-manager-routing-methods.md)

## <a name="next-steps"></a>Következő lépések
* [Az Azure Traffic Manager-profilok kezelése](../traffic-manager/traffic-manager-manage-profiles.md)
* [Végpontok felvétele, letiltása, engedélyezése és törlése](../traffic-manager/traffic-manager-endpoints.md) 

