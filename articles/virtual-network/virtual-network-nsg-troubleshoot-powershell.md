---
title: "Hálózati biztonsági csoport – aaaTroubleshoot PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot hálózati biztonsági csoportok hello Azure Resource Manager telepítési modell Azure PowerShell használatával."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 4c732bb7-5cb1-40af-9e6d-a2a307c2a9c4
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 95fd60fa72cf6d17fa990e3c3eb7d980878f7c15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Hálózati biztonsági csoportok az Azure PowerShell hibaelhárítása
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
> * [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

Ha konfigurálva a hálózati biztonsági csoportokkal (NSG-k) a virtuális gépen (VM), és virtuális gép kapcsolódási problémákat tapasztal, ez a cikk áttekintést diagnosztikai képességek az NSG-k toohelp további hibaelhárítást kell végeznie.

Az NSG-k lehetővé teszik a toocontrol hello típusú forgalom, hogy a virtuális gépek (VM) mindkét folyamata. NSG-ket lehet alkalmazott toosubnets egy Azure virtuális hálózatot (VNet), hálózati adapterek (NIC) vagy mindkettőt. hello hatékony szabályokat tooa hálózati adapter létező hello alkalmazott NSG-k tooa hálózati adapter és hello alhálózaton van csatlakoztatva hello szabály összesítést. Ezek az NSG-k között szabályok néha ütköznek egymással, és hatással lehet a virtuális gépek hálózati kapcsolattal.  

Az NSG-k, az összes hello hatékony biztonsági szabály tekintheti meg a virtuális gép hálózati adapterek alkalmazott. Ez a cikk bemutatja, hogyan tootroubleshoot VM ezeket szabályok használata a problémák hello Azure Resource Manager üzembe helyezési modellben. Ha még nem ismeri a virtuális hálózat és NSG fogalmakat, olvassa el a hello [virtuális hálózati](virtual-networks-overview.md) és [hálózati biztonsági csoportok](virtual-networks-nsg.md) áttekintése cikkeket.

## <a name="using-effective-security-rules-tootroubleshoot-vm-traffic-flow"></a>Hatékony biztonsági szabályok tootroubleshoot VM adatforgalmat használatával
hello-forgatókönyvekben, amelyek a következő gyakori kapcsolati probléma példája:

A virtuális gépek nevű *VM1* nevű alhálózat része *Alhalozat_1* nevű egy Vneten belül *WestUS-VNet1*. Sikertelen a virtuális gépet RDP a 3389-es TCP-porton keresztül egy kísérlet tooconnect toohello. Az NSG-k mindkét hello NIC szintjén alkalmazhatóak *VM1-NIC1* és alhálózati hello *Alhalozat_1*. Forgalom tooTCP 3389-es port használata engedélyezett hello hello hálózati interfészhez társított NSG *VM1-NIC1*, azonban a TCP Pingelje meg tooVM1 port 3389 sikertelen.

Ebben a példában a 3389-es portot használja, amíg hello lépéseket követve lehet használt toodetermine bejövő és kimenő kapcsolódási hibák bármely porton keresztül.

## <a name="detailed-troubleshooting-steps"></a>Részletes hibaelhárítási lépéseket
Hajtsa végre a következő lépések tootroubleshoot NSG-ket a virtuális gépek hello:

1. Indítsa el az Azure PowerShell-munkamenet és bejelentkezési tooAzure. Ha nem ismeri az Azure PowerShell használatával, olvassa el a hello [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk.
2. Adja meg a következő parancs tooreturn minden NSG-szabályok alkalmazása tooa nevű hálózati hello *VM1-NIC1* hello erőforráscsoportban *RG1*:
   
        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Ha egy hálózati adapter hello neve nem tudja, adja meg a következő parancs tooretrieve hello nevét egy erőforráscsoportban található hálózati adapterek összes hello: 
   > 
   > `Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`
   > 
   > 
   
    hello következő szövege minta hello hatékony szabályok kimenete hello vissza *VM1-NIC1* NIC:
   
        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
   
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]
   
    Vegye figyelembe a következő információ hello kimenet hello:
   
   * Két **hálózati biztonsági csoporthoz tartozik** szakaszok: tartozik hozzá egy alhálózat (*Alhalozat_1*) és egy hálózati Adapterhez társított (*VM1-NIC1*). Ebben a példában egy NSG alkalmazott tooeach volt.
   * **Társítás** hello erőforrás (alhálózati vagy hálózati adapter) jeleníti meg egy adott NSG társítva. Ha hello NSG-erőforrás áthelyezése vagy hozzárendelésének megszüntetése a parancs futtatása előtt azonnal, szükséges toowait néhány másodpercen belül hello módosítása tooreflect hello parancs kimenetében. 
   * hello vannak végrehajtásával kerüli meg a szabály neve *defaultSecurityRules*: amikor egy NSG jön létre, néhány alapértelmezett biztonsági szabályok jönnek létre benne. Alapértelmezett szabályok nem távolítható el, de magasabb prioritású szabályokkal felülbírálható lesz.
     Olvasási hello [NSG áttekintése](virtual-networks-nsg.md#default-rules) NSG kapcsolatos további információkért a cikk toolearn alapértelmezett biztonsági szabályokat.
   * **ExpandedAddressPrefix** hello címelőtagokat NSG alapértelmezett címkék bővíti. Címkék több címelőtagokat jelölik. Hello címkék bővítése akkor lehet hasznos, ha adott címelőtagokat és a virtuális gép hibaelhárítása. A VNETBEN társviszony-létesítést, hogy VIRTUAL_NETWORK címke például bővíti tooshow társviszonyban VNet-előtagok hello előző kimenet.
     
     > [!NOTE]
     > hello parancs csak látható hatékony szabályokat, ha az NSG tartozik alhálózat, egy hálózati Adaptert, vagy mindkettőt. Előfordulhat, hogy a virtuális gépek több hálózati adapterrel alkalmazott különböző NSG. Hibaelhárításához, hello parancs futtatása az egyes hálózati adapterhez.
     > 
     > 
3. NSG-szabályok, hogy a nagyobb számú szűrés tooease adja meg a következő további parancsok tootroubleshoot hello: 
   
        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView
   
    Az RDP-forgalmát (TCP-port 3389-es), a szűrő alkalmazása toohello rácsnézete, ahogy az alábbi képen hello:
   
    ![Szabályok listája](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
4. Ahogy látja, hello táblázatos nézetben, vannak-e mindkét engedélyezése és megtagadási szabályoknak RDP. hello kimenetét a 2. lépés bemutatja, hogy hello *DenyRDP* szabály hello alkalmazott NSG toohello alhálózat szerepel. A bejövő szabályok esetében alkalmazott NSG-k toohello alhálózati feldolgozása először. Ha van egyezés, hello alkalmazott NSG toohello hálózati illesztő nem lett feldolgozva. Ebben az esetben hello *DenyRDP* hello alhálózatból szabály blokkolja a virtuális gép RDP-toohello (**VM1**).
   
   > [!NOTE]
   > Előfordulhat, hogy a virtuális gépek több hálózati adapter csatolt tooit. Minden egyes lehet csatlakoztatott tooa alhálózaton. Mivel hello parancs hello előző lépésekben futtatása ellen egy hálózati Adaptert, fontos megadott tooensure hello hálózati adapter a hello csatlakozási hiba lépett fel. Ha nem biztos benne, is minden esetben futtathatók hello parancsok minden csatlakoztatott hálózati toohello virtuális gép.
   > 
   > 
5. a VM1, módosítás hello tooRDP *megtagadása RDP (3389-es)* szabály túl*RDP(3389) engedélyezése* a hello **Alhalozat_1-NSG** NSG. Győződjön meg arról, hogy 3389-es TCP-port meg nyitva egy RDP-kapcsolat toohello VM megnyitásával, vagy hello PsPing eszköz használata. Terhelésekről további információt a PsPing által olvasása hello [PsPing letöltési oldala](https://technet.microsoft.com/sysinternals/psping.aspx)
   
    Lehet, vagy távolítsa el a szabályok egy NSG-t a használatával a következő parancs hello hello kimenete hello információi alapján:
   
        Get-Help *-AzureRmNetworkSecurityRuleConfig

## <a name="considerations"></a>Megfontolandó szempontok
Vegye figyelembe a következő pontok csatlakozási problémák elhárításakor hello:

* Alapértelmezett NSG-szabályok blokkolja a bejövő hello hozzáférést internet és a virtuális hálózat engedély csak a bejövő forgalom. Szabályok kell adható hozzá közvetlen módon tooallow bejövő Internet, a szükséges hozzáférést.
* Ha nincsenek, amely a virtuális gép hálózati kapcsolat toofail NSG biztonsági szabályok, hello a probléma oka a következő lehet:
  * Hello virtuális gép operációs rendszerben futó tűzfal szoftver
  * A virtuális készülékek vagy a helyszíni forgalmi útvonalak. Internetes forgalom átirányított tooon helyszíni kényszerített bújtatás keresztül lehet. Az RDP/SSH-kapcsolat a hello Internet tooyour VM nem feltétlenül ezt a beállítást, attól függően, hogy miként hello a helyszíni hálózati hardver kezeli a forgalmat. Olvasási hello [hibaelhárítási útvonalak](virtual-network-routes-troubleshoot-powershell.md) cikk toolearn hogyan toodiagnose útvonal is lehet akadályozó problémákat hello és bezárja a forgalom hello virtuális gép. 
* Vnetek, alapértelmezés szerint rendelkezik társviszonyban, ha a hello VIRTUAL_NETWORK címke automatikusan tooinclude előtagok bontsa a társviszonyban Vnetek esetében. Ezeket az előtagokat megtekintheti a hello **ExpandedAddressPrefix** listájában, tootroubleshoot bármely társviszony-létesítés kapcsolódási problémák kapcsolódó tooVNet. 
* Hatékony biztonsági szabályok csak jelennek-e hello VM hálózati adapter és, vagy alhálózat társított egy NSG. 
* Ha nincs hálózati hello társított NSG-ket, vagy alhálózat és rendelkezik a nyilvános IP-cím hozzárendelése a virtuális gép tooyour, minden port nyissa meg a bejövő és kimenő hozzáférést lesz. Ha hello VM egy nyilvános IP-címmel rendelkezik, alkalmazza az NSG-k toohello hálózati adapter vagy az alhálózat erősen ajánlott.  

