---
title: "az internetre irányuló terheléselosztót aaaDeploy IPv6 - Azure-sablon alapján |} Microsoft Docs"
description: "Hogyan toodeploy IPv6 Azure terheléselosztó és az elosztott terhelésű virtuális gépek támogatása."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
keywords: "IPv6-alapú, azure load balancer, kettős verem, nyilvános IP-cím, natív ipv6, mobil, iot"
ms.assetid: 2998e943-13fc-4ea9-a68c-875e53a08db3
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2016
ms.author: kumud
ms.openlocfilehash: 68b9ba874a50161243577b64c4a6d9c76b39156c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Megoldás üzembe helyezéséhez egy internetre irányuló terheléselosztót IPv6-sablon használatával

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Sablon](load-balancer-ipv6-internet-template.md)

Az Azure Load Balancer 4. szintű (TCP, UDP) terheléselosztónak minősül. hello terheléselosztó bejövő forgalmat a felhőszolgáltatások kifogástalan szolgáltatáspéldány vagy a load balancer csoportban lévő virtuális gépek között elosztásával magas rendelkezésre állást biztosít. Az Azure Load Balancer a szolgáltatásokat több portra vagy több IP-címre, illetve portokra és IP-címekre egyaránt továbbíthatja.

## <a name="example-deployment-scenario"></a>Központi telepítési példa

hello következő diagram azt ábrázolja, hello terheléselosztási megoldás üzembe helyezéséhez sablonnal hello példa a cikkben.

![Terheléselosztói forgatókönyv](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

Ebben a forgatókönyvben a következő Azure-erőforrások hello hozza létre:

* az egyes virtuális gépekhez rendelt IPv4 és IPv6-címmel rendelkező virtuális hálózati illesztő
* egy internetre irányuló terheléselosztót egy IPv4-és IPv6 nyilvános IP-cím
* két terheléselosztási szabályok toomap hello nyilvános virtuális IP-címek toohello titkos végpontok betöltése
* rendelkezésre állási csoport, amely tartalmazza a hello két virtuális gépek
* két virtuális gépek (VM)

## <a name="deploying-hello-template-using-hello-azure-portal"></a>Hello sablon használatával hello Azure-portál telepítése

Ez a cikk hivatkozik a sablont, amely közzé van téve a hello [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) gyűjteménye. Hello sablon letöltése hello gyűjteményből, vagy indítsa el a hello telepítési hello gyűjteményből közvetlenül az Azure-ban. Ez a cikk feltételezi, hogy a letöltött hello sablon tooyour helyi számítógépen.

1. Nyissa meg a hello Azure-portálon és be kell jelentkezniük az engedélyek toocreate virtuális gépek és a hálózati erőforrásokat belül Azure-előfizetéssel rendelkező fiókkal. Is, kivéve, ha meglévő erőforrásokat használ, hello fiókot kell engedély toocreate az erőforráscsoportot és a storage-fiók.
2. Kattintson a "+ új" az hello menü majd hello keresőmezőbe a "sablon" típus. Válassza ki a "Sablon-üzembehelyezés" hello a keresési eredmények.

    ![LB-ipv6-portál – 2. lépés](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. A hello mindent panelen kattintson a "Sablon-üzembehelyezés."

    ![LB-ipv6-portál – 3. lépés](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Kattintson a "Create".

    ![LB-ipv6-portal-step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Kattintson a "Sablon szerkesztése." Törölje a meglévő tartalom hello és másolja és illessze be a hello sablon fájl teljes tartalmát hello (tooinclude hello kezdő és befejező {}), majd kattintson a "Mentés".

    > [!NOTE]
    > Használata a Microsoft Internet Explorer, illessze be, amikor megjelenik egy párbeszédpanel, tooallow hozzáférés toohello Windows vágólapra kéri. Kattintson a "Hozzá lehessen férni."

    ![LB-ipv6-portal-step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Kattintson a "Paraméterek szerkesztése." A hello (paraméterek) panelen adja meg az hello értékeket hello útmutatásának hello sablon paraméterek szakaszban, majd kattintson a "Mentés" tooclose hello (paraméterek) panelen. Hello egyéni telepítési panelen jelölje ki az előfizetését, egy meglévő erőforráscsoportot, vagy hozzon létre egyet. Ha egy erőforráscsoportot hoz létre, majd válassza ki a hello erőforrásnak a helyét. Ezután kattintson **jogi feltételeket**, majd kattintson a **beszerzési** hello jogi feltételek. Azure kezdődik hello erőforrásokat üzembe helyezi. Néhány perc toodeploy szükséges összes hello erőforrás.

    ![LB-ipv6-portal-step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Ezek a paraméterek kapcsolatos további információkért lásd: hello [sablon paraméterek és változók](#template-parameters-and-variables) szakasz a cikk későbbi részében.

7. hello sablon által létrehozott toosee hello erőforrások kattintson a Tallózás gombra, amíg tekintse meg a "Erőforráscsoportok", majd kattintson rá a hello listáját görgetve.

    ![LB-ipv6-portal-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Hello erőforrás-csoportok paneljén kattintson a 6. lépésben megadott hello erőforráscsoport hello nevét. Megtekintheti az összes hello telepítve vannak-e. Összes hiba is, ha az üzenetnek kell megjelennie "Succeeded" a "Legutóbbi telepítés." Ha nem, akkor gondoskodjon arról, hogy hello használata fióknak engedélyek toocreate hello szükséges erőforrásokat.

    ![LB-ipv6-portal-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [!NOTE]
    > Ha közvetlenül a 6. lépés befejezése után az erőforráscsoportokat tallózással, "Utolsó telepítés" hello erőforrások bevezetése közben "A" hello állapotát megjeleníti.

9. Kattintson az erőforrások listájához hello "myIPv6PublicIP". Láthatja, hogy rendelkezik-e az IP-cím IPv6-címet, és hogy a DNS-névvel hello hello dnsNameforIPv6LbIP paraméter 6. lépésben megadott érték. Ehhez az erőforráshoz hello nyilvános IPv6 cím és nevet, amely elérhető tooInternet-ügyfelek.

    ![LB-ipv6-portal-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Kapcsolat ellenőrzése

Amikor hello sablon sikeresen telepítve van, ellenőrizheti a kapcsolat hello a következő feladatok végrehajtásával:

1. Azure-portálon toohello bejelentkezhet, és csatlakozzon a hello hello sablon-üzembehelyezés által létrehozott virtuális gépek tooeach. Ha telepítette a Windows Server virtuális gép, futtassa az ipconfig/minden a parancssorból. Láthatja, hogy hello virtuális gépek rendelkezik-e az IPv4 és IPv6-címeket is. Ha telepített Linux virtuális gépek, meg kell tooconfigure hello Linux operációs rendszert futtató tooreceive dinamikus IPv6-címek használata a Linux-disztribúció hello olvasható utasításokat.
2. Egy IPv6-alapú internetre csatlakozó ügyfélszámítógépről kezdeményezzen kapcsolatot toohello nyilvános IPv6-címnek hello terheléselosztó. amely a terheléselosztó hello tooconfirm van terheléselosztás hello két futó virtuális gépek közötti, egy webkiszolgáló, például a Microsoft Internet Information Services (IIS) telepítheti minden egyes hello virtuális gépen. alapértelmezett weblap hello minden kiszolgálón tartalmazhatnak hello szöveg "Server0" vagy "Kiszolgáló1" toouniquely azonosítani. Ezt követően nyisson meg egy böngészőprogramot, egy IPv6-alapú internetre csatlakoztatott ügyfélen, és keresse meg a hello terhelés terheléselosztó tooconfirm végpont IPv6 kapcsolatot tooeach VM hello dnsNameforIPv6LbIP paraméterében megadott toohello állomásnév. Ha csak weblap jelenik meg hello csak egy kiszolgálóról, szükség lehet tooclear a gyorsítótárban. Nyissa meg a több privát böngészés ideje. Meg kell jelennie egy minden kiszolgáló válaszát.
3. Egy IPv4-alapú internetre csatlakozó ügyfélszámítógépről kezdeményezzen kapcsolatot toohello nyilvános IPv4-címnek hello terheléselosztó. amely a terheléselosztó hello tooconfirm-e terheléselosztási hello két virtuális gépeket, IIS-t használja, mint 2. lépésben részletes tesztelése sikerült.
4. A virtuális gépek kezdeményezni egy kimenő kapcsolat tooan IPv6 vagy Internet IPv4-csatlakozóval csatlakoztatott eszközre. Mindkét esetben hello forrás IP-címe szerinti hello céleszköz az hello nyilvános IPv4- vagy IPv6 cím hello terheléselosztó.

> [!NOTE]
> Az IPv4 és IPv6 ICMP le van tiltva, a hello Azure-hálózatot. Ennek eredményeképpen ICMP eszközök, például a ping mindig sikertelen lesz. tootest kapcsolatot, a TCP alternatív TCPing például használja, vagy PowerShell Test-NetConnection parancsmagról hello. Ne feledje, hogy hello IP-címek hello ábrán is látható értékeket, amelyeket láthat példát. Hello IPv6-címek hozzárendelésének dinamikusan, mert a hello címek kap eltérőek, és régiónként eltérőek lehetnek. Azt is közös hello nyilvános IPv6-cím hello load balancer toostart különböző előtaggal kezdődik, mint hello saját IPv6-címek hello háttér-készletben.

## <a name="template-parameters-and-variables"></a>Sablon paraméterek és változók

Az Azure Resource Manager-sablon több változók és testre szabhatja tooyour igények paraméterek tartalmazza. Változók nem szeretné, hogy egy felhasználó toochange rögzített értékek érvényesek. Paraméterek, amelyet a felhasználó tooprovide hello sablon telepítésekor értékek szerepelnek. Ebben a cikkben ismertetett hello forgatókönyv hello példa sablon van konfigurálva. Ez a környezet tooneeds testre.

Ebben a cikkben használt sablon tartalmazza a következő hello hello példa változók és a Paraméterek:

| A paraméter / változó | Megjegyzések |
| --- | --- |
| adminUsername |Adja meg a hello hello rendszergazdai fiókot használja toosign toohello virtuális gépekben. |
| adminPassword |Adja meg a hello rendszergazdai fiók jelszavát hello toosign használt toohello virtuális gépekben. |
| dnsNameforIPv4LbIP |Adja meg a kívánt tooassign hello terheléselosztó hello nyilvános neveként hello DNS-állomásnevet. Ez a neve feloldható toohello terheléselosztójának nyilvános IPv4-cím. hello nevét kell a kis és hello regex egyezik: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP |Adja meg a kívánt tooassign hello terheléselosztó hello nyilvános neveként hello DNS-állomásnevet. Ez a neve feloldható toohello terheléselosztójának nyilvános IPv6-cím. hello nevét kell a kis és hello regex egyezik: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. Ez lehet hello azonos IPv4-cím hello nevet. Amikor egy ügyfél küld a név Azure DNS-lekérdezést fog visszaszolgáltassák hello A és AAAA típusú rekordot is hello neve meg van osztva. |
| vmNamePrefix |Adja meg a hello virtuális gép nevének előtagja. hello sablon hozzáfűz egy számot (0, 1, stb.) Ha hello toohello neve virtuális gépek jönnek létre. |
| nicNamePrefix |Adja meg a hálózati illesztő előtagja hello. hello sablon hozzáfűz egy számot (0, 1, stb.) toohello neve hello hálózati illesztők létrehozásakor. |
| storageAccountName |Meglévő tárfiók hello nevét, illetve megadhat egy hello sablon által létrehozott új egy toobe hello nevét. |
| availabilitySetName |Adja meg a hello rendelkezésre állási nevét állítja be toobe hello virtuális gépek együtt |
| addressPrefix |hello címelőtag használt toodefine hello címtartományán hello virtuális hálózat |
| subnetName |hello VNet létrehozott hello alhálózatának hello neve |
| subnetPrefix |hello címelőtag használt toodefine hello hello alhálózat címtartománya |
| vnetName |Adjon meg nevet hello hello hello virtuális gépek által használt virtuális hálózat. |
| ipv4PrivateIPAddressType |hello elosztási módszert használt hello privát IP-cím (statikus vagy dinamikus) |
| ipv6PrivateIPAddressType |hello privát IP-cím (dinamikus) használatos hello kiosztási módszerrel. IPv6 csak dinamikus foglalási támogatja. |
| numberOfInstances |betöltési hello száma kiegyensúlyozott hello sablon által telepített példányok |
| ipv4PublicIPAddressName |Adja meg a kívánt toouse toocommunicate hello nyilvános IPv4-címmel rendelkező terheléselosztó hello hello DNS-nevét. |
| ipv4PublicIPAddressType |hello foglalási módjának hello nyilvános IP-cím (statikus vagy dinamikus) |
| Ipv6PublicIPAddressName |Adja meg a kívánt toouse toocommunicate hello nyilvános IPv6-címmel rendelkező terheléselosztó hello hello DNS-nevét. |
| ipv6PublicIPAddressType |hello elosztási módszert hello nyilvános IP-cím (dinamikus) használt. IPv6 csak dinamikus foglalási támogatja. |
| lbName |Adja meg a terheléselosztó hello hello nevét. Ez a név hello portálon jelenik meg, vagy a parancssori felületen vagy a PowerShell-paranccsal tooit hivatkozás során használt. |

hello sablonban hello fennmaradó változók értékeit származtatott létrehozásakor a Azure hello erőforrásokhoz vannak hozzárendelve. Ne változtassa meg ezeket a változókat.
