---
title: "a virtuális hálózattal - Azure HDInsight aaaExtend |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Virtual Network tooconnect HDInsight tooother felhőalapú erőforrásokat, vagy az adatközpontban lévő erőforrások"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 37b9b600-d7f8-4cb1-a04a-0b3a827c6dcc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: larryfr
ms.openlocfilehash: ba80be4d9f280c6c62fa8acc996ef5f921acdbbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a>Azure virtuális hálózat használatával Azure HDInsight kiterjesztése

Megtudhatja, hogyan toouse HDInsight a egy [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Azure virtuális hálózat használatával lehetővé teszi, hogy a következő forgatókönyvek hello:

* Csatlakozás tooHDInsight közvetlenül a helyi hálózatról.

* Egy Azure virtuális hálózatra csatlakozó HDInsight toodata tárolja.

* Közvetlen elérése során Hadoop-szolgáltatásokat nem érhető el több mint nyilvánosan hello internet. Például Kafka API-k vagy hello HBase Java API.

> [!WARNING]
> a dokumentumban szereplő információk hello kell a TCP/IP-hálózat érteni. Ha nem ismeri a TCP/IP-hálózat, meg kell felkereshetők a személy, aki módosításainak tooproduction hálózatok elvégzése előtt.

## <a name="planning"></a>Tervezés

Az alábbiakban hello válaszolnia kell a virtuális hálózatban HDInsight tooinstall tervezésekor hello kérdésekre kaphat választ:

* HDInsight tooinstall létrehozni meglévő virtuális hálózatban kell? Új hálózat létrehozása vagy?

    Ha egy meglévő virtuális hálózatot használ, szükség lehet toomodify hello hálózati konfiguráció HDInsight telepítése előtt. További információkért lásd: hello [HDInsight tooan meglévő virtuális hálózat hozzáadása](#existingvnet) szakasz.

* Tooconnect hello virtuális hálózati tartalmazó HDInsight tooanother virtuális hálózat vagy a helyszíni hálózat van szüksége?

    tooeasily munkahelyi erőforrások hálózatok között, előfordulhat, hogy egy egyéni DNS toocreate kell és DNS-továbbító konfigurálása. További információkért lásd: hello [kapcsolódó több hálózatok](#multinet) szakasz.

* Szeretné, hogy toorestrict/átirányítás bejövő vagy kimenő forgalom tooHDInsight?

    A HDInsight az IP-címek hello Azure-adatközpontban kommunikálni kell üzemmódban. Van még számos portot, az ügyfél-kommunikációhoz tűzfalon keresztül kell engedélyezni. További információkért lásd: hello [hálózati forgalom vezérlése](#networktraffic) szakasz.

## <a id="existingvnet"></a>HDInsight tooan meglévő virtuális hálózat hozzáadása

Ez a szakasz toodiscover hello lépéseket használata útmutató tooadd egy új HDInsight tooan meglévő Azure-beli virtuális hálózathoz.

> [!NOTE]
> Nem adhat hozzá egy meglévő HDInsight-fürt virtuális hálózatba.

1. Használja a klasszikus és Resource Manager üzembe helyezési modellben hello virtuális hálózat?

    HDInsight 3.4 és nagyobb erőforrás-kezelő virtuális hálózat szükséges. A HDInsight korábbi verzióiban a klasszikus virtuális hálózatot szükséges.

    Ha a meglévő hálózat a klasszikus virtuális hálózatot, majd kell létrehozni egy erőforrás-kezelő virtuális hálózat és két hello csatlakozzon. [Csatlakozás a hagyományos Vneteket toonew Vnetek](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

    Miután csatlakozott, HDInsight hello erőforrás-kezelő hálózatán telepített kezelheti hello hagyományos hálózati erőforrásokhoz.

2. Használja a kényszerített bújtatás? A kényszerített bújtatás az alhálózat-beállítással, amely kényszeríti a kimenő Internet forgalom tooa eszköz a vizsgálat és naplózás. A HDInsight nem támogatja a kényszerített bújtatást. Távolítsa el a kényszerített bújtatás HDInsight egy alhálózatba telepítése előtt, vagy hozzon létre egy új alhálózatot a HDInsight.

3. Használhatók a hálózati biztonsági csoportok, felhasználó által definiált útvonalak és virtuális hálózati berendezések toorestrict forgalom virtuális gépbe vagy onnan hello virtuális hálózatot?

    Felügyelt szolgáltatásként HDInsight korlátlan hozzáférést tooseveral IP-címek hello Azure-adatközpontban van szükség. tooallow kommunikációt a következő IP-címek, a meglévő hálózati biztonsági csoport vagy felhasználó által definiált útvonalak frissítése.

    HDInsight tároló több szolgáltatásokat, amelyek különféle portokat. Nem forgalom toothese portok blokkolása. Virtuális készülék tűzfalon keresztüli portok tooallow listáját lásd: hello [biztonsági](#security) szakasz.

    toofind a meglévő biztonsági konfiguráció, a következő Azure PowerShell vagy Azure CLI parancsok használata hello:

    * Network security groups (Hálózati biztonsági csoportok)

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
        get-azurermnetworksecuritygroup -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
        az network nsg list --resource-group $RESOURCEGROUP
        ```

        További információkért lásd: hello [hibaelhárítása a hálózati biztonsági csoportok](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) dokumentum.

        > [!IMPORTANT]
        > Hálózati biztonsági csoportszabályok szabály prioritási sorrendben alkalmazza. hello első szabály hello forgalom bizonyos mintázatnak megfelelő érvényes, és nincs más erre a forgalomra alkalmazza. A leghatékonyabb tooleast megengedő szabályok. További információkért lásd: hello [hálózati forgalmat hálózati biztonsági csoportokkal](../virtual-network/virtual-networks-nsg.md) dokumentum.

    * Felhasználó által megadott útvonalak

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
        get-azurermroutetable -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
        az network route-table list --resource-group $RESOURCEGROUP
        ```

        További információkért lásd: hello [útvonalak hibaelhárítása](../virtual-network/virtual-network-routes-troubleshoot-portal.md) dokumentum.

4. HDInsight-fürtök létrehozása és hello Azure virtuális hálózat konfigurálása során. Kövesse a következő dokumentumok toounderstand hello fürtlétrehozás hello hello lépéseket:

    * [Hozzon létre a HDInsight a hello Azure-portálon](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [HDInsight létrehozása az Azure PowerShell-lel](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [A HDInsight használata az Azure CLI 1.0 létrehozása](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [HDInsight használata az Azure Resource Manager-sablon létrehozása](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

  > [!IMPORTANT]
  > HDInsight hozzáadása tooa virtuális hálózat egy opcionális konfigurációs lépésre. Lehet, hogy tooselect hello virtuális hálózati hello fürt konfigurálásakor.

## <a id="multinet"></a>Több hálózatok csatlakoztatása

hello legnagyobb kihívás a több hálózati konfiguráció névfeloldás hello hálózatok között.

Azure névfeloldást biztosít az Azure virtuális hálózatban telepített szolgáltatások. A beépített névfeloldás lehetővé teszi, hogy a HDInsight tooconnect toohello erőforrások egy teljesen minősített tartománynevét (FQDN) használatával a következő:

* Minden erőforrás elérhető internet hello. Ha például a Microsoft.com webhelyre mutat, google.com.

* Bármely erőforrása, amely a rendszer a hello azonos Azure virtuális hálózatban, hello segítségével __belső DNS-név__ hello erőforrás. Például hello alapértelmezett névhozzárendelés használatakor hello a következők példa belső DNS nevek hozzárendelt tooHDInsight feldolgozó csomópontok:

    * wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
    * wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    Mindkét ezeket a csomópontokat közvetlenül kommunikálhatnak egymással, és a Hdinsightban, más csomópontok belső DNS-nevek használatával.

hello alapértelmezett névhozzárendelés does __nem__ illesztett toohello virtuális hálózatokon lévő erőforrások hello nevének HDInsight tooresolve engedélyezése. Például közös toojoin a helyszíni hálózati toohello virtuális hálózat. Csak hello alapértelmezett a névfeloldás HDInsight nem tud hozzáférni a hello a helyszíni hálózati erőforrások neve. Ellenkező hello igaz is, a helyszíni hálózati erőforrások neve nem tud hozzáférni hello virtuális hálózatán lévő erőforrásokat.

> [!WARNING]
> Hello egyéni DNS-kiszolgáló létrehozása és hello virtuális hálózati toouse konfigurálnia kell, mielőtt létrehozná hello HDInsight-fürthöz.

a névfeloldás tooenable hello virtuális hálózat és a csatlakoztatott hálózatokon lévő erőforrások között, végezze el a következő műveletek hello:

1. Hozzon létre egy egyéni DNS-kiszolgáló hello Azure Virtual Network tooinstall HDInsight tervezett.

2. Hello virtuális hálózati toouse hello egyéni DNS-kiszolgáló konfigurálása.

3. Megkeresi a hello Azure hozzárendelése a virtuális hálózat DNS-utótagot. Ez az érték túl hasonló`0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`. Az hello DNS-utótag megkereséséről további információkért lásd: hello [példa: egyéni DNS](#example-dns) szakasz.

4. Konfigurálja a továbbítási hello DNS-kiszolgálók között. hello konfigurációs távoli hálózati hello típusától függ.

    * Ha hello távoli hálózati egy a helyszíni hálózat, konfigurálja a DNS az alábbiak szerint:
        
        * __Egyéni DNS__ (a virtuális hálózati hello):

            * Továbbítási kérelmek hello DNS-utótag az hello virtuális hálózati toohello Azure rekurzív feloldó (168.63.129.16). Azure virtuális hálózat hello erőforrásokra vonatkozó kéréseket kezeli

            * Továbbítsa az összes többi kérelmek toohello a helyi DNS-kiszolgáló. hello helyszíni DNS kezeli az összes többi feloldási kérések, még akkor is, kérelmek az internetes erőforrásokhoz, például a Microsoft.com webhelyre mutat.

        * __A helyi DNS__: hello virtuális hálózat DNS-utótag toohello egyéni DNS kiszolgáló kérelmeket továbbítja. hello egyéni DNS-kiszolgáló majd toohello Azure rekurzív feloldó továbbítja.

        A konfigurációs útvonalak kérelmek teljes tartománynevek DNS-utótagja hello hello virtuális hálózati toohello egyéni DNS-kiszolgáló tartalmazó. (Még a nyilvános internet címek) minden más kérelemhez hello a helyi DNS-kiszolgáló kezeli.

    * Ha hello távoli hálózat egy másik Azure virtuális hálózatnak, konfigurálja a DNS az alábbiak szerint:

        * __Egyéni DNS__ (az egyes virtuális hálózati):

            * Hello DNS-utótag hello virtuális hálózatok kérelmeket a rendszer továbbítja toohello egyéni DNS-kiszolgálók. az egyes virtuális hálózati DNS hello felelős megoldása érdekében a hálózati erőforrásokat.

            * Minden más kérelmek toohello Azure rekurzív feloldó továbbítja. hello rekurzív feloldó felelős helyi feloldására és az internetes erőforrásokban.

        hello DNS-kiszolgáló minden hálózat továbbítja a kérelmek toohello más, a DNS-utótagot. Küldött egyéb kérések hello Azure rekurzív feloldó használatával oldja fel.

    Minden egyes konfigurációs példát lásd: hello [példa: egyéni DNS](#example-dns) szakasz.

További információkért lásd: hello [névfeloldás virtuális gépek és a Szerepkörpéldányok](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) dokumentum.

## <a name="directly-connect-toohadoop-services"></a>Közvetlen csatlakozás tooHadoop szolgáltatások

A legtöbb dokumentáció a HDInsight feltételezi, hogy rendelkezik-e hozzáférési toohello fürt keresztül hello internet. Például, hogy csatlakozhasson https://CLUSTERNAME.azurehdinsight.net toohello fürthöz. A cím hello nyilvános átjárón, amely esetén nem érhető el, használja az NSG-k vagy udr-EK toorestrict a hozzáférést a hello internet használja.

tooconnect tooAmbari és más weblapok hello virtuális hálózaton keresztül, használja a következő lépéseket hello:

1. toodiscover hello belső teljes tartománynév (FQDN) hello HDInsight fürtcsomópont nevét, a hello a következő módszerek valamelyikével:

    ```powershell
    $resourceGroupName = "hello resource group that contains hello virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    Hello csomópontlista visszaadott hello FQDN található hello átjárócsomópontokkal és használja a hello teljes tartománynevek tooconnect tooAmbari és egyéb webszolgáltatások. Tegyük fel például, `http://<headnode-fqdn>:8080` tooaccess Ambari.

    > [!IMPORTANT]
    > Egyes hello átjárócsomópontokkal üzemeltetett szolgáltatások aktívak csak az egyik csomópont egyszerre. Próbálja meg egy központi csomóponton szolgáltatások elérésére és a 404-es hibaüzenetet ad vissza, ha Váltás más átjárócsomópont toohello.

2. toodetermine hello csomópont és a portot, amelyet a szolgáltatás érhető el, lásd: hello [a HDInsight Hadoop-szolgáltatás által használt portok](./hdinsight-hadoop-port-settings-for-services.md) dokumentum.

## <a id="networktraffic"></a>Hálózati forgalom vezérlése

Egy Azure virtuális hálózatot a hálózati forgalmat a következő módszerek hello vezérelhető:

* **Hálózati biztonsági csoportok** (NSG) lehetővé teszik toofilter bejövő és kimenő forgalom toohello hálózati. További információkért lásd: hello [hálózati forgalmat hálózati biztonsági csoportokkal](../virtual-network/virtual-networks-nsg.md) dokumentum.

    > [!WARNING]
    > A HDInsight nem támogatja a kimenő forgalom korlátozása.

* **Felhasználó által definiált útvonalak** (UDR) határozza meg, hogyan a forgalom között hello hálózatán lévő erőforrásokat. További információkért lásd: hello [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md) dokumentum.

* **Virtuális készülékekre** replikálja az eszközök, például a tűzfalak és az útválasztók hello funkcióit. További információkért lásd: hello [hálózati berendezések](https://azure.microsoft.com/solutions/network-appliances) dokumentum.

Felügyelt szolgáltatásként HDInsight korlátlan hozzáférést tooAzure állapotát és a felügyeleti szolgáltatások hello Azure felhőben igényel. Az NSG-k és udr-EK használata esetén győződjön meg róla, hogy a HDInsight ezen szolgáltatások továbbra is kommunikál a HDInsight.

HDInsight szolgáltatások számos portot teszi elérhetővé. Ha egy virtuális készülékre tűzfalat használ, engedélyeznie kell a hello forgalmat mely portokon folyik a szolgáltatások. További információkért lásd: hello [szükséges portok] szakasz.

### <a id="hdinsight-ip"></a>A hálózati biztonsági csoportok és a felhasználó által definiált útvonalak HDInsight

Ha a kíván használni **hálózati biztonsági csoportok** vagy **felhasználó által definiált útvonalak** toocontrol hálózati forgalom, hajtsa végre a hello műveletek HDInsight telepítése előtt a következő:

1. Azonosítsa a hello toouse tervezi a HDInsight az Azure-régió.

2. Azonosítsa a HDInsight által igényelt hello IP-címek. További információkért lásd: hello [HDInsight által megkövetelt IP-címek](#hdinsight-ip) szakasz.

3. Létrehozhat vagy módosíthat hello hálózati biztonsági csoport vagy felhasználó által definiált útvonalak tooinstall HDInsight megtervezni hello alhálózat be.

    * __Hálózati biztonsági csoportok__: engedélyezése __bejövő__ porton forgalom __443-as__ hello IP-címről.
    * __Felhasználó által definiált útvonalak__: hozzon létre egy útvonal tooeach IP-címet, és állítsa be a hello __a következő ugrás típusa__ too__Internet__.

A hálózati biztonsági csoport vagy felhasználó által definiált útvonalak további információkért lásd: a következő dokumentáció hello:

* [Hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md)

* [Felhasználó által definiált útvonalak](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling"></a>Alagúthasználat kényszerítése

Kényszerített bújtatás a beállítás a felhasználói útválasztási ahol alhálózatból származó összes forgalmat, de kényszerített tooa adott hálózati helyre, például a helyszíni hálózat. HDInsight does __nem__ támogatási kényszerített bújtatást.

## <a id="hdinsight-ip"></a>Szükséges IP-címek

> [!IMPORTANT]
> hello Azure állapotát, és szolgáltatások a hdinsight eszközzel képes toocommunicate kell lennie. Ha hálózati biztonsági csoport vagy felhasználó által definiált útvonalak, hello érkező forgalom engedélyezése ezen szolgáltatások tooreach HDInsight IP-címet.
>
> Ha hálózati biztonsági csoport vagy felhasználó által definiált útvonalak toocontrol forgalom nem használ, figyelmen kívül hagyhatja ebben a szakaszban.

Ha a hálózati biztonsági csoport vagy felhasználó által definiált útvonalak, engedélyeznie kell a hello Azure állapotát és a felügyeleti szolgáltatások tooreach HDInsight-forgalmat. Használja a következő lépéseket toofind hello IP-címek, engedélyezni kell az hello:

1. Mindig engedélyeznie kell a következő IP-címek hello érkező forgalmat:

    | IP-cím | Engedélyezett port | Irány |
    | ---- | ----- | ----- |
    | 168.61.49.99 | 443 | Bejövő |
    | 23.99.5.239 | 443 | Bejövő |
    | 168.61.48.131 | 443 | Bejövő |
    | 138.91.141.162 | 443 | Bejövő |

2. Ha a HDInsight-fürt hello a következő területek közül, majd engedélyeznie kell a felsorolt hello régió hello IP-címekről érkező forgalmat:

    > [!IMPORTANT]
    > Ha hello használata az Azure-régió nem szerepel a listában, majd csak hello négy IP-címek használatára 1. lépésben.

    | Ország | Régió | Engedélyezett IP-címek | Engedélyezett port | Irány |
    | ---- | ---- | ---- | ---- | ----- |
    | Ázsia | Kelet-Ázsia | 23.102.235.122</br>52.175.38.134 | 443 | Bejövő |
    | &nbsp; | Délkelet-Ázsia | 13.76.245.160</br>13.76.136.249 | 443 | Bejövő |
    | Ausztrália | Kelet-Ausztrália | 104.210.84.115</br>13.75.152.195 | 443 | Bejövő |
    | &nbsp; | Délkelet-Ausztrália | 13.77.2.56</br>13.77.2.94 | 443 | Bejövő |
    | Brazília | Dél-Brazília | 191.235.84.104</br>191.235.87.113 | 443 | Bejövő |
    | Kanada | Kelet-Kanada | 52.229.127.96</br>52.229.123.172 | 443 | Bejövő |
    | &nbsp; | Közép-Kanada | 52.228.37.66</br>52.228.45.222 | 443 | Bejövő |
    | Kína | Észak-Kína | 42.159.96.170</br>139.217.2.219 | 443 | Bejövő |
    | &nbsp; | Kelet-Kína | 42.159.198.178</br>42.159.234.157 | 443 | Bejövő |
    | Európa | Észak-Európa | 52.164.210.96</br>13.74.153.132 | 443 | Bejövő |
    | &nbsp; | Nyugat-Európa| 52.166.243.90</br>52.174.36.244 | 443 | Bejövő |
    | Németország | Közép-Németország | 51.4.146.68</br>51.4.146.80 | 443 | Bejövő |
    | &nbsp; | Északkelet-Németország | 51.5.150.132</br>51.5.144.101 | 443 | Bejövő |
    | India | Közép-India | 52.172.153.209</br>52.172.152.49 | 443 | Bejövő |
    | Japán | Kelet-Japán | 13.78.125.90</br>13.78.89.60 | 443 | Bejövő |
    | &nbsp; | Nyugat-Japán | 40.74.125.69</br>138.91.29.150 | 443 | Bejövő |
    | Korea | Korea középső régiója | 52.231.39.142</br>52.231.36.209 | 433 | Bejövő |
    | &nbsp; | Korea déli régiója | 52.231.203.16</br>52.231.205.214 | 443 | Bejövő
    | Egyesült Királyság | Az Egyesült Királyság nyugati régiója | 51.141.13.110</br>51.141.7.20 | 443 | Bejövő |
    | &nbsp; | Az Egyesült Királyság déli régiója | 51.140.47.39</br>51.140.52.16 | 443 | Bejövő |
    | Egyesült Államok | USA középső régiója | 13.67.223.215</br>40.86.83.253 | 443 | Bejövő |
    | &nbsp; | USA északi középső régiója | 157.56.8.38</br>157.55.213.99 | 443 | Bejövő |
    | &nbsp; | USA nyugati középső régiója | 52.161.23.15</br>52.161.10.167 | 443 | Bejövő |
    | &nbsp; | USA nyugati régiója, 2. | 52.175.211.210</br>52.175.222.222 | 443 | Bejövő |

    Információk hello IP-címek toouse az Azure Government, lásd: hello [Azure Government Eszközintelligencia + analitika](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) dokumentum.

3. Ha egy egyéni DNS-kiszolgáló használ a virtuális hálózat, is engedélyeznie kell a hozzáférést a __168.63.129.16__. Ez a cím az Azure rekurzív feloldó. További információkért lásd: hello [virtuális gépek és a szerepkör névfeloldását példányok](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) dokumentum.

További információkért lásd: hello [hálózati forgalom vezérlése](#networktraffic) szakasz.

## <a id="hdinsight-ports"></a>Szükséges portok

Ha a hálózat használatával kíván **virtuális készülék tűzfal** toosecure hello virtuális hálózat, engedélyeznie kell a kimenő forgalmat a következő portok hello:

* 53
* 443
* 1433
* 11000-11999
* 14000-14999

A portok adott szolgáltatások listájáért lásd: hello [a HDInsight Hadoop-szolgáltatás által használt portok](hdinsight-hadoop-port-settings-for-services.md) dokumentum.

A virtuális készülékek vonatkozó tűzfalszabályok további információkért lásd: hello [virtuális berendezésre telepítik](../virtual-network/virtual-network-scenario-udr-gw-nva.md) dokumentum.

## <a id="hdinsight-nsg"></a>Példa: hálózati biztonsági csoportok a hdinsight eszközzel

Ebben a szakaszban hello példák bemutatják, hogyan toocreate hálózati biztonsági csoport szabályokat, amelyek engedélyezik a HDInsight toocommunicate hello az Azure szolgáltatások. Példák hello használatához állítsa be úgy a hello IP címek toomatch hello néhányat a meglévők közül hello használata Azure-régiót. Ez az információ az hello található [HDInsight a hálózati biztonsági csoportok és a felhasználó által definiált útvonalak](#hdinsight-ip) szakasz.

### <a name="azure-resource-management-template"></a>Az Azure erőforrás-kezelés sablon

hello következő erőforrás-kezelés sablonnal hoz létre egy virtuális hálózatot, amely korlátozza a bejövő forgalmat, de lehetővé teszi, hogy a HDInsight által igényelt hello IP-címekről érkező forgalom. Ez a sablon is létrehoz egy HDInsight-fürt hello virtuális hálózatban.

* [A biztonságos Azure virtuális hálózat és egy HDInsight Hadoop-fürt központi telepítése](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]
> A példa toomatch hello Azure-régió, használja a használt hello IP-címek módosítása Ez az információ az hello található [HDInsight a hálózati biztonsági csoportok és a felhasználó által definiált útvonalak](#hdinsight-ip) szakasz.

### <a name="azure-powershell"></a>Azure PowerShell

A következő PowerShell-parancsfájl toocreate bejövő forgalmát, és lehetővé teszi, hogy a forgalom hello IP-címek hello Észak-Európa régió virtuális hálózat hello használata.

> [!IMPORTANT]
> A példa toomatch hello Azure-régió, használja a használt hello IP-címek módosítása Ez az információ az hello található [HDInsight a hálózati biztonsági csoportok és a felhasználó által definiált útvonalak](#hdinsight-ip) szakasz.

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with hello resource group hello virtual network is in"
$subnetName = "Replace with hello name of hello subnet that you plan toouse for HDInsight"
# Get hello Virtual Network object
$vnet = Get-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get hello region hello Virtual network is in.
$location = $vnet.Location
# Get hello subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for hello HDInsight health and management services.
$nsg = New-AzureRmNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "blockeverything" `
        -Description "Block everything else" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "*" `
        -SourceAddressPrefix "Internet" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Deny `
        -Priority 500 `
        -Direction Inbound
# Set hello changes toohello security group
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply hello NSG toohello subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

> [!IMPORTANT]
> Ez a példa bemutatja, hogyan tooadd szabályok tooallow bejövő adatforgalmat a szükséges hello IP-címek. A szabály toorestrict nem tartalmaz más forrásokból hozzáférés bejövő.
>
> hello a következő példa bemutatja, hogy tooenable SSH hogyan férhetnek hozzá az internethez hello:
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a>Azure CLI

A következő lépéseket toocreate bejövő forgalmát, de lehetővé teszi, hogy a HDInsight által igényelt hello IP-címekről érkező forgalom virtuális hálózat hello használata.

1. A következő parancs toocreate új hálózati biztonsági csoport nevű használata hello `hdisecure`. Cserélje le **RESOURCEGROUPNAME** hello erőforráscsoporttal, amely tartalmazza a hello Azure-beli virtuális hálózathoz. Cserélje le **hely** hello helyen lévő (régió) hello csoport jött létre.

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    Hello csoport létrehozása után megjelenik a hello új csoport adatai.

2. A következő tooadd szabályok toohello új hálózati biztonsági csoportot, amely hello Azure HDInsight állapotát és a felügyeleti szolgáltatás a 443-as portot a bejövő kommunikáció hello használata. Cserélje le **RESOURCEGROUPNAME** hello Azure Virtual Network tartalmazó erőforráscsoport hello hello névvel.

    > [!IMPORTANT]
    > A példa toomatch hello Azure-régió, használja a használt hello IP-címek módosítása Ez az információ az hello található [HDInsight a hálózati biztonsági csoportok és a felhasználó által definiált útvonalak](#hdinsight-ip) szakasz.

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n block --protocol "*" --source-port-range "*" --destination-port-range "*" --source-address-prefix "Internet" --destination-address-prefix "VirtualNetwork" --access "Deny" --priority 500 --direction "Inbound"
    ```

3. tooretrieve hello a hálózati biztonsági csoport egyedi azonosítója, a következő parancs hello használata:

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    Ez a parancs visszaadja a szöveg a következő érték hasonló toohello:

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    Használjon a hello parancs azonosítója dupla idézőjelbe, ha a várt hello eredmények nem jelenik.

4. A következő parancs tooapply hello hálózati biztonsági csoport tooa alhálózati hello használata. Cserélje le a hello __GUID__ és __RESOURCEGROUPNAME__ hello előző lépésben által visszaadott értékek hello néhányat a meglévők közül. Cserélje le __VNETNAME__ és __SUBNETNAME__ hello virtuálishálózat-névnek és alhálózat neve, amelyet az toocreate.

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    Ez a parancs után a HDInsight a virtuális hálózati hello is telepítheti.

> [!IMPORTANT]
> Ezeket a lépéseket csak nyissa meg a toohello HDInsight állapotát és a felügyeleti szolgáltatás a hello Azure felhőben. Bármely más hozzáférési toohello HDInsight-fürt a virtuális hálózaton kívül hello le van tiltva. tooenable hozzáférés a külső hello virtuális hálózatról, hozzá kell adnia a további hálózati biztonsági csoportszabályok.
>
> hello a következő példa bemutatja, hogy tooenable SSH hogyan férhetnek hozzá az internethez hello:
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <a id="example-dns"></a>Példa: DNS-konfiguráció

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a>Névfeloldás egy virtuális és a csatlakoztatott helyszíni hálózat között

Ebben a példában a következő feltételek hello teszi:

* Rendelkezik egy Azure virtuális hálózatot, amely csatlakoztatott tooan a helyszíni hálózat VPN-átjáró használatával.

* hello egyéni DNS-kiszolgáló hello virtuális hálózat futtató Linux vagy Unix hello operációs rendszer.

* [Kötési](https://www.isc.org/downloads/bind/) hello egyéni DNS-kiszolgálón telepítve van.

Hello egyéni DNS-kiszolgálón a virtuális hálózati hello:

1. Azure PowerShell vagy az Azure CLI toofind hello DNS-utótagjának használata hello virtuális hálózat:

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Hello egyéni DNS-kiszolgálón hello virtuális hálózat, használja a szöveg hello hello tartalmát, a következő hello `/etc/bind/named.conf.local` fájlt:

    ```
    // Forward requests for hello virtual network suffix tooAzure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    Cserélje le a hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` hello DNS-utótagját, a virtuális hálózat értéket.

    Ez a konfiguráció hello DNS-utótag hello virtuális hálózati toohello Azure rekurzív feloldó az összes DNS-kérelmek irányítja.

2. Hello egyéni DNS-kiszolgálón hello virtuális hálózat, használja a szöveg hello hello tartalmát, a következő hello `/etc/bind/named.conf.options` fájlt:

    ```
    // Clients tooaccept requests from
    // TODO: Add hello IP range of hello joined network toothis list
    acl goodclients {
        10.0.0.0/16; # IP address range of hello virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent toohello following
            forwarders {
                192.168.0.1; # Replace with hello IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform tooRFC1035
            listen-on { any; };
    };
    ```
    
    * Cserélje le a hello `10.0.0.0/16` hello IP-címtartomány a virtuális hálózat értéket. Ez a bejegyzés lehetővé teszi, hogy a név feloldása kérelmek címek ebbe a tartományba.

    * Hello IP-címtartomány a hello a helyszíni hálózati toohello hozzáadása `acl goodclients { ... }` szakasz.  bejegyzés lehetővé teszi, hogy a források névfeloldási hello a helyi hálózaton.
    
    * Cserélje le a hello érték `192.168.0.1` a helyi DNS-kiszolgáló hello IP-címmel. Ez a bejegyzés minden más DNS kérelmek toohello a helyi DNS-kiszolgáló irányítja.

3. toouse hello konfigurációs, indítsa újra a kötés. Például: `sudo service bind9 restart`.

4. Feltételes továbbítók toohello a helyi DNS-kiszolgáló hozzáadása. Konfigurálhatja a hello feltételes továbbítók toosend kérések hello DNS-utótag lépés 1 toohello egyéni DNS-kiszolgálóról.

    > [!NOTE]
    > Hello dokumentációjából tájékozódhat arról, hogy miként a DNS-szoftver tooadd feltételes továbbítót.

A lépések elvégzése után tooresources vagy a hálózat teljes tartománynevek (FQDN) használatával is elérheti. Most már telepítheti HDInsight hello virtuális hálózathoz.

### <a name="name-resolution-between-two-connected-virtual-networks"></a>Két csatlakoztatott virtuális hálózatok közötti névfeloldás

Ebben a példában a következő feltételek hello teszi:

* VPN-átjáró használatával, vagy a társviszony-létesítés csatlakoztatott két Azure virtuális hálózat rendelkezik.

* mindkét hálózat hello egyéni DNS-kiszolgáló fut. Linux vagy Unix hello operációs rendszert.

* [Kötési](https://www.isc.org/downloads/bind/) hello egyéni DNS-kiszolgálókon telepítve van.

1. Azure PowerShell vagy az Azure CLI toofind hello DNS-utótagját, mindkét virtuális hálózat használata:

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Szöveg hello hello tartalmát, a következő használatát hello `/etc/bind/named.config.local` fájl hello egyéni DNS-kiszolgálón. Adja meg ezt a módosítást az egyéni DNS-kiszolgáló hello mindkét virtuális hálózat.

    ```
    // Forward requests for hello virtual network suffix tooAzure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # hello IP address of hello DNS server in hello other virtual network
    };
    ```

    Cserélje le a hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` hello DNS-utótagja hello értékének __más__ virtuális hálózat. Ez a bejegyzés irányítja a DNS-utótagja hello hello távoli hálózati toohello vonatkozó egyéni DNS-ét erre a hálózatra.

3. Hello egyéni DNS-kiszolgálókon mindkét virtuális hálózatban, használja a szöveg hello hello tartalmát, a következő hello `/etc/bind/named.conf.options` fájlt:

    ```
    // Clients tooaccept requests from
    acl goodclients {
        10.1.0.0/16; # hello IP address range of one virtual network
        10.0.0.0/16; # hello IP address range of hello other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver         
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform tooRFC1035
            listen-on { any; };
    };
    ```
    
    * Cserélje le a hello `10.0.0.0/16` és `10.1.0.0/16` értékek hello IP-címtartományok a virtuális hálózatok. Ez a bejegyzés lehetővé teszi, hogy minden hálózati erőforrások toomake kérelmek hello DNS-kiszolgálók.

    Minden kérést, amelyek nincsenek a hello DNS-utótagok hello virtuális hálózatok (Fontos) hello Azure rekurzív feloldó kezeli.

4. toouse hello konfigurációs, indítsa újra a kötés. Például `sudo service bind9 restart` mindkét DNS-kiszolgálókon.

A lépések elvégzése után tooresources hello virtuális hálózatban, teljes tartománynevek (FQDN) használatával is elérheti. Most már telepítheti HDInsight hello virtuális hálózathoz.

## <a name="next-steps"></a>Következő lépések

* Például egy végpontok közötti HDInsight tooconnect tooan a helyszíni hálózat konfigurálása, lásd: [csatlakozás HDInsight tooan a helyszíni hálózat](./connect-on-premises-network.md).

* Egy Azure virtuális hálózatot további információkért lásd: hello [Azure Virtual Network áttekintése](../virtual-network/virtual-networks-overview.md).

* Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).

* A felhasználó által definiált útvonalak további információkért lásd: [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md).