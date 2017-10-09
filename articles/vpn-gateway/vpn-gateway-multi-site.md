---
title: "Csatlakozás a virtuális hálózati toomultiple helyek VPN-átjáró és a PowerShell használatával: klasszikus |} Microsoft Docs"
description: "Ez a cikk végigvezeti csatlakozás több helyszíni helyi helyek tooa virtuális hálózat VPN-átjáró hello klasszikus telepítési modell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-service-management
ms.assetid: b043df6e-f1e8-4a4d-8467-c06079e2c093
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: yushwang
ms.openlocfilehash: 5404b1c55ed3453b4dbc94dfd93e47c0812025f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-site-to-site-connection-tooa-vnet-with-an-existing-vpn-gateway-connection-classic"></a>Adja hozzá a pont-pont kapcsolat tooa virtuális hálózatot egy meglévő VPN-kapcsolattal átjáró (klasszikus)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klasszikus)](vpn-gateway-multi-site.md)
>
>

Ez a cikk bemutatja, hogyan PowerShell tooadd webhelyek (közötti S2S) kapcsolatok tooa VPN-átjáró, amely rendelkezik egy létező kapcsolat használatával. Ez a kapcsolat típus gyakran hivatkozott tooas egy "több" Helykonfiguráció. hello a cikkben ismertetett alkalmazása hello klasszikus üzembe helyezési modellel (más néven szolgáltatásfelügyelet) használatával létrehozott toovirtual hálózatok. Ezeket a lépéseket ne alkalmazza a tooExpressRoute/pont-pont vizsgálatát a kísérő kapcsolat konfigurációkat.

### <a name="deployment-models-and-methods"></a>Üzemi modellek és módszerek

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Ebben a táblázatban, új cikket frissítjük, és további eszközök ehhez a konfigurációhoz elérhetővé válnak. Egy cikk nem érhető el, hogy hivatkozás közvetlenül tooit ebből a táblázatból.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a>Csatlakozás

Több helyszíni helyek tooa egyetlen virtuális hálózat is elérheti. Ez egy különösen vonzó hibrid felhőalapú megoldás felépítéséhez. Többhelyes kapcsolat tooyour Azure virtuális hálózati átjáró létrehozása hasonló toocreating van más webhelyek kapcsolatokat. Valójában használhatja egy meglévő Azure VPN gateway mindaddig, amíg hello átjáró dinamikus (útválasztó-alapú).

Ha már rendelkezik egy statikus átjáró csatlakoztatott tooyour virtuális hálózatot, hello átjáró típusa toodynamic rendelés tooaccommodate több helyen toorebuild hello virtuális hálózati anélkül módosíthatja. Hello útválasztási típus módosítása, előtt győződjön meg arról, hogy a helyszíni VPN-átjáró támogatja-e az útvonalalapú VPN konfigurációival.

![többhelyes diagram](./media/vpn-gateway-multi-site/multisite.png "többhelyes")

## <a name="points-tooconsider"></a>Pontok tooconsider

**Nem fog tudni toouse hello portál toomake módosítások toothis virtuális hálózat.** Toomake módosítások toohello hálózati konfigurációs fájl helyett hello portál használata szükséges. Ha módosítja a hello portálon, a többhelyes hivatkozás beállításai a virtuális hálózat lesz felülírja.

Úgy kell érzi, hogy kényelmes hello hálózati konfigurációs fájl használatával hello idő hello többhelyes eljárás regisztrációja befejeződött. Azonban ha több személy dolgozik a hálózati konfigurációt, szüksége lesz a toomake meg arról, hogy mindenki ismer ezt a korlátozást is. Ez nem jelenti azt, hogy hello portal egyáltalán nem használható. Minden más, kivéve, hogy konfigurációs módosításokat toothis adott virtuális hálózati használhatja.

## <a name="before-you-begin"></a>Előkészületek

A konfigurálás elkezdése előtt győződjön meg arról, hogy rendelkezik-e hello következő:

* Az egyes hardver VPN helyszíni hely. Ellenőrizze [kapcsolatos VPN-eszközök a virtuális hálózati kapcsolatra](vpn-gateway-about-vpn-devices.md) tooverify Ha hello eszköz, amelyet az toouse valamit, amely kompatibilis toobe ismert.
* Külsőleg irányuló nyilvános IPv4-alapú IP-címet minden egyes VPN-eszközön. a NAT mögött hello IP-cím nem található Ez a követelmény.
* Tooinstall hello legújabb verziójára hello Azure PowerShell-parancsmagok lesz szüksége. Győződjön meg arról, továbbá toohello erőforrás-kezelő verziójában hello Service Management (SM) verzióját telepíti. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) további információt.
* Személynek értelemben haladó szinten konfigurálása a VPN-hardver álljanak. Összekapcsolta toohave erős megismernie az tooconfigure a VPN-eszköz vagy valakinek, aki a használata.
* hello IP-címtartományok, amelyet toouse a virtuális hálózat (Ha még nem már létre).
* hello IP-címtartományok az egyes hello helyi hálózati helyek, amely meg fog csatlakozni. Arról, hogy hello IP-címtartományát egyes hello helyi hálózati helyek, amelyet az tooconnect toodo nem átfedés toomake lesz szüksége. Ellenkező esetben hello portálon vagy REST API hello el fogják utasítani feltöltendő hello konfigurációs.<br>Például ha két helyi hálózati helyek, hogy mindkettő rendelkezik hello IP-cím tartomány 10.2.3.0/24, és rendelkezik a cél címe 10.2.3.3 csomaghoz, Azure nem tudja toosend hello csomag toobecause hello címtartományok vannak kívánt hely átfedő. tooprevent útválasztási problémák, Azure nem engedélyezi tooupload átfedő tartományokat rendelkező konfigurációs fájlt.

## <a name="1-create-a-site-to-site-vpn"></a>1. Helyek közötti VPN létrehozása
Ha már rendelkezik egy dinamikus útválasztási átjáró, pont-pont VPN nagy! A Folytatás túl[hello virtuális hálózati konfigurációs beállítások exportálása](#export). Ha nem, a hello, a következő:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Ha a pont-pont virtuális hálózat már rendelkezik, de statikus (házirend-alapú) útválasztó átjáró rendelkezik:
1. Módosítsa az átjáró típusa toodynamic útválasztást. Többhelyes VPN dinamikus (más néven útválasztó-alapú) útválasztó átjáró szükséges. toochange az átjáró típus, akkor lesz toofirst delete hello meglévő átjáró kell, majd hozzon létre egy újat. Útmutatásért lásd: [hogyan toochange hello VPN-útválasztási típus az átjáró](vpn-gateway-configure-vpn-gateway-mp.md).  
2. Az új átjáró konfigurálása, és a VPN-alagút létrehozása. Útmutatásért lásd: [VPN-átjáró konfigurálása a klasszikus Azure portál hello](vpn-gateway-configure-vpn-gateway-mp.md). Először módosítsa az átjáró típusa toodynamic útválasztást.

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Ha még nem rendelkezik pont-pont virtuális hálózat:
1. A pont-pont virtuális hálózat létrehozása ezeket az utasításokat: [virtuális hálózat létrehozása a klasszikus Azure portál hello pont-pont VPN-kapcsolattal](vpn-gateway-site-to-site-create.md).  
2. A jelen útmutatást dinamikus útválasztó átjáró konfigurálása: [VPN-átjáró konfigurálása](vpn-gateway-configure-vpn-gateway-mp.md). Lehet, hogy tooselect **dinamikus útválasztási** az átjáró típusának.

## <a name="export"></a>2. Hello hálózati konfigurációs fájl exportálása
Az Azure-hálózat konfigurációs fájl exportálása hello a következő parancs futtatásával. Hello helyét hello fájl tooexport tooa máshová szükség esetén módosíthatja.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-hello-network-configuration-file"></a>3. Nyissa meg hello hálózati konfigurációs fájl
Nyissa meg a letöltött hello hálózati konfigurációs fájllal hello utolsó lépésében megadja. Bármilyen XML-szerkesztő, amely tetszés használja. hello fájl hasonló toohello következő kell kinéznie:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. Több mutató hivatkozások hozzáadása
Amikor hozzá, vagy távolítsa el a webhely referencia jellegű információt, lesz konfigurációmódosításokat toohello ConnectionsToLocalNetwork/LocalNetworkSiteRef. Azure toocreate hozzáadása egy új helyi hivatkozás elindítja egy új csatornán. Hello az alábbi példában a hello hálózati konfiguráció egyetlen helyen kapcsolat van. Mentse a hello fájlt, a változtatások befejezése után.

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

(a többhelyes konfiguráció létrehozása) tooadd további webhelyekre mutató hivatkozások egyszerűen adja hozzá a további "LocalNetworkSiteRef" sorok, a hello az alábbi példában látható módon:

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-hello-network-configuration-file"></a>5. Importálás hello hálózati konfigurációs fájl
Importálja a hello hálózati konfigurációs fájlt. Amikor importálja ezt a fájlt hello módosításokkal, hello új alagutak lesz hozzáadva. hello alagutak hello dinamikus átjáró korábban létrehozott fogja használni. Használhatja a hello klasszikus portál vagy PowerShell tooimport hello fájlt.

## <a name="6-download-keys"></a>6. Töltse le a kulcsok
Az új alagutak hozzáadása után használja a hello PowerShell parancsmag "Get-AzureVNetGatewayKey" tooget hello IPsec/IKE előmegosztott kulccsal minden alagúthoz.

Példa:

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

Tetszés szerint használhatja hello *első virtuális hálózati átjáró megosztott kulcsos* REST API tooget hello előmegosztott kulccsal.

## <a name="7-verify-your-connections"></a>7. Kapcsolatok ellenőrzése
Ellenőrizze a hello többhelyes alagút állapotát. Hello kulcsok minden egyes alagúthoz a letöltés után érdemes tooverify kapcsolatok. Használja a "Get-AzureVnetConnection" tooget bújtatja virtuális hálózati listáját, az alábbi hello példában látható módon. VNet1 hello hello VNet neve.

```powershell
Get-AzureVnetConnection -VNetName VNET1
```

A példa visszatérési:

```
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : hello connectivity state for hello local network site 'Site1' changed from Not Connected tooConnected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : hello connectivity state for hello local network site 'Site2' changed from Not Connected tooConnected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
```

## <a name="next-steps"></a>Következő lépések

További információ a VPN-átjárók, toolearn lásd [VPN-átjárók](vpn-gateway-about-vpngateways.md).
