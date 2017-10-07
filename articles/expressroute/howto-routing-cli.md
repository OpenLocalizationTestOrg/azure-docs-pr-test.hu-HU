---
title: "Hogyan tooconfigure útválasztást Azure ExpressRoute-kapcsolatcsoportot: CLI |} Microsoft Docs"
description: "Ez a cikk segít létrehozásának és kiosztásának hello saját, a nyilvános és a Microsoft társviszony-létesítést az ExpressRoute-kapcsolatcsoportot. Ez a cikk is bemutatja, hogyan toocheck hello állapotát, a frissítést, vagy a-kör társviszony törlése."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: anzaman,cherylmc
ms.openlocfilehash: 33130af050045527cdb316e77821c6d101b6a82a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-routing-for-an-expressroute-circuit-using-cli"></a>Létrehozásához és módosításához útválasztás az ExpressRoute-kapcsolatcsoportot parancssori felület használatával

Ez a cikk segít hozhat létre és kezelhet egy ExpressRoute-kapcsolatcsoportot hello Resource Manager üzembe helyezési modellel parancssori felület használatával útválasztási konfigurációja. Is hello állapota, update vagy delete ellenőrizze és kiosztásának megszüntetése ExpressRoute-kör társviszony. Ha egy másik módszer toowork toouse a kapcsolatcsoport rendelkező, jelölje be a cikk a következő lista hello:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - privát társviszony-létesítés](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - nyilvános társviszony-létesítés](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Videó – a Microsoft társviszony-létesítés](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasszikus)](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a>Konfigurációs előfeltételek

* Megkezdése előtt telepítse a legújabb verziójának hello hello parancssori felület parancsait (2.0-s vagy újabb). Hello parancssori felület parancsait telepítésével kapcsolatos információkért lásd: [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).
* Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamat](expressroute-workflows.md) lapok: konfigurálás elkezdése előtt.
* Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége. Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](howto-circuit-cli.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör. hello ExpressRoute-kapcsolatcsoportot meg toobe képes toorun hello parancsok ebben a cikkben kiépített és engedélyezett állapotban kell lennie.

Ezek az utasítások csak a 2. rétegbeli kapcsolatot szolgáltatásokat nyújtó szolgáltatók létre toocircuits vonatkoznak. A szolgáltató által kezelt használata réteg (általában egy IPVPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.

Beállíthatja, hogy egy, kettő vagy ExpressRoute-kör összes három esetében (Azure saját, az Azure nyilvános, és a Microsoft). A társviszony-létesítéseket tetszőleges sorrendben konfigurálhatja. Azonban kell győződjön meg arról, hogy elvégezte-e minden társviszony-létesítési egyszerre csak egy hello konfigurációját.

## <a name="azure-private-peering"></a>Azure privát társviszony-létesítés

Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése hello Azure privát társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot.

### <a name="toocreate-azure-private-peering"></a>az Azure magánhálózati társviszony-létesítés toocreate

1. Hello Azure parancssori felület legújabb verziójának telepítéséhez. Hello hello Azure parancssori felület (CLI) legújabb verzióját kell használnia. * felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.

  ```azurecli
  az login
  ```

  Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetés

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Hozzon létre egy ExpressRoute-kapcsolatcsoportot. Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](howto-circuit-cli.md) , illetve hozta-e hello kapcsolat szolgáltatóját.

  Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet. Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat. Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is hello lépések a konfiguráció.
3. Ellenőrizze a hello ExpressRoute körön toomake arról, hogy van üzembe helyezve és is engedélyezve van. A következő példa hello használata:

  ```azurecli
  az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
  ```

  a rendszer a következő példa hasonló toohello hello választ:

  ```azurecli
  "allowClassicOperations": false,
  "authorizations": [],
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
  "gatewayManagerEtag": null,
  "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
  "location": "westus",
  "name": "MyCircuit",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
  "serviceProviderNotes": null,
  "serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
  },
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. Az Azure magánhálózati társviszony-létesítés hello kör megadása Győződjön meg arról, hogy rendelkezik-e hello hello lépések folytatása előtt a következő elemek:

  * / 30-as alhálózat hello elsődleges kapcsolathoz. hello alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.
  * / 30-as alhálózat hello másodlagos kapcsolathoz. hello alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.
  * Egy érvényes VLAN-azonosító tooestablish a társviszony. Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.
  * Egy AS-szám a társviszony-létesítéshez. 2 és 4 bájtos AS-számokat is használhat. Ehhez a társviszony-létesítéshez használhat privát AS-számokat is. Ne használja a 65515 számot.
  * **Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.

  A következő példa tooconfigure magánhálózati társviszony-létesítés a kör Azure hello használata:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering
  ```

  Ha úgy dönt, toouse az MD5 kivonatoló, használja a következő példa hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.
  > 
  > 

### <a name="tooview-azure-private-peering-details"></a>tooview Azure magánhálózati társviszony-létesítés részletei

Konfigurációs részletek kaphat a következő példa hello használata:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

hello hasonló toohello a következő példa a kimenetre:

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePrivatePeering",
  "ipv6PeeringConfig": null,
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePrivatePeering",
  "peerAsn": 7671,
  "peeringType": "AzurePrivatePeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="tooupdate-azure-private-peering-configuration"></a>tooupdate Azure magánhálózati társviszony-létesítési konfiguráció

Frissítheti, használja a következő példa hello hello konfiguráció bármely részeként. Ebben a példában a 100 too500 hello VLAN-Azonosítót a kapcsolatcsoport hello frissül.

```azurecli
az network express-route peering update --vlan-id 500 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

### <a name="toodelete-azure-private-peering"></a>az Azure magánhálózati társviszony-létesítés toodelete

A társviszony-létesítési konfiguráció a következő példa hello futtatásával távolíthatja el:

> [!WARNING]
> Bizonyosodjon meg, hogy az összes virtuális hálózatot az ExpressRoute-kapcsolatcsoportot hello megszüntetni ebben a példában futtatása előtt. 
> 
> 

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

## <a name="azure-public-peering"></a>Azure nyilvános társviszony-létesítés

Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és hello Azure nyilvános társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot törli.

### <a name="toocreate-azure-public-peering"></a>az Azure nyilvános társviszony toocreate

1. Hello Azure parancssori felület legújabb verziójának telepítéséhez. Hello hello Azure parancssori felület (CLI) legújabb verzióját kell használnia. * felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.

  ```azurecli
  az login
  ```

  Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetést.

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Hozzon létre egy ExpressRoute-kapcsolatcsoportot.  Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](howto-circuit-cli.md) , illetve hozta-e hello kapcsolat szolgáltatóját.

  Ha a kapcsolat szolgáltatójánál felügyelt 3. rétegbeli szolgáltatásokat nyújt, kérheti, hogy a kapcsolat szolgáltatójánál lehetővé teszi, hogy az Azure magánhálózati társviszony-létesítés meg. Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat. Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is hello lépések a konfiguráció.
3. Ellenőrizze a hello ExpressRoute körön tooensure van üzembe helyezve, és is engedélyezve van. A következő példa hello használata:

  ```azurecli
  az network express-route list
  ```

  a rendszer a következő példa hasonló toohello hello választ:

  ```azurecli
  "allowClassicOperations": false,
  "authorizations": [],
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
  "gatewayManagerEtag": null,
  "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
  "location": "westus",
  "name": "MyCircuit",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
  "serviceProviderNotes": null,
  "serviceProviderProperties": {
    "bandwidthInMbps": 200,
    "peeringLocation": "Silicon Valley",
    "serviceProviderName": "Equinix"
  },
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. Az Azure nyilvános társviszony-létesítés hello kör megadása Győződjön meg arról, hogy rendelkezik-e hello előtt végrehajtásának folytatásához a következő információkat.

  * / 30-as alhálózat hello elsődleges kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.
  * / 30-as alhálózat hello másodlagos kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.
  * Egy érvényes VLAN-azonosító tooestablish a társviszony. Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.
  * Egy AS-szám a társviszony-létesítéshez. 2 és 4 bájtos AS-számokat is használhat.
  * **Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.

  Futtassa a következő példa tooconfigure Azure nyilvános társviszony-létesítés a kör hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering
  ```

  Ha úgy dönt, toouse az MD5 kivonatoló, használja a következő példa hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.

### <a name="tooview-azure-public-peering-details"></a>tooview Azure nyilvános társviszony-létesítés részletei

Kaphat a konfigurációs adatait a következő példa hello használata:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

hello hasonló toohello a következő példa a kimenetre:

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePublicPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePublicPeering",
  "peerAsn": 7671,
  "peeringType": "AzurePublicPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="tooupdate-azure-public-peering-configuration"></a>az Azure nyilvános társviszony-létesítési konfiguráció tooupdate

Frissítheti, használja a következő példa hello hello konfiguráció bármely részeként. Ebben a példában a 200 too600 hello VLAN-Azonosítót a kapcsolatcsoport hello frissül.

```azurecli
az network express-route peering update --vlan-id 600 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

### <a name="toodelete-azure-public-peering"></a>az Azure nyilvános társviszony toodelete

A társviszony-létesítési konfiguráció a következő példa hello futtatásával távolíthatja el:

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

## <a name="microsoft-peering"></a>Microsoft társviszony-létesítés

Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése hello Microsoft társviszony-létesítési konfiguráció ExpressRoute-kör.

> [!IMPORTANT]
> Előzetes tooAugust 1 Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok volt beállítva, akkor 2017 fog rendelkezni az összes szolgáltatás előtagok keresztül hello Microsoft társviszony-létesítést, meghirdetett, akkor is, ha nincsenek megadva útvonal szűrők. A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat meghirdetett, amíg egy útvonal-szűrő nem csatlakoztatja toohello körön. További információkért lásd: [konfigurálása a Microsoft társviszony-létesítéshez útvonal szűrő](how-to-routefilter-powershell.md).
> 
> 

### <a name="toocreate-microsoft-peering"></a>toocreate Microsoft társviszony-létesítés

1. Hello Azure parancssori felület legújabb verziójának telepítéséhez. Használjon hello legújabb verziójára hello Azure parancssori felület (CLI). * felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.

  ```azurecli
  az login
  ```

  Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetést.

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Hozzon létre egy ExpressRoute-kapcsolatcsoportot. Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](howto-circuit-cli.md) , illetve hozta-e hello kapcsolat szolgáltatóját.

  Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet. Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat. Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is hello lépések a konfiguráció.

3. Ellenőrizze a hello ExpressRoute körön toomake arról, hogy van üzembe helyezve és is engedélyezve van. A következő példa hello használata:

  ```azurecli
  az network express-route list
  ```

  a rendszer a következő példa hasonló toohello hello választ:

  ```azurecli
  "allowClassicOperations": false,
  "authorizations": [],
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
  "gatewayManagerEtag": null,
  "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
  "location": "westus",
  "name": "MyCircuit",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
  "serviceProviderNotes": null,
  "serviceProviderProperties": {
    "bandwidthInMbps": 200,
    "peeringLocation": "Silicon Valley",
    "serviceProviderName": "Equinix"
  },
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. Konfigurálja a Microsoft hello-kör társviszony-létesítés. Győződjön meg arról, hogy rendelkezik-e folytatni a következő információk előtt hello.

  * / 30-as alhálózat hello elsődleges kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.
  * / 30-as alhálózat hello másodlagos kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.
  * Egy érvényes VLAN-azonosító tooestablish a társviszony. Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.
  * Egy AS-szám a társviszony-létesítéshez. 2 és 4 bájtos AS-számokat is használhat.
  * Meghirdetett előtagok: meg kell adnia a lista az összes előtagok hello BGP munkameneten keresztül tervezi tooadvertise. A rendszer kizárólag a nyilvános IP-cím-előtagokat fogadja el. Ha azt tervezi, toosend előtagok készlete, elküldheti a vesszővel tagolt listáját. Ezeket az előtagokat kell lenniük egy RIR-ben regisztrált tooyou vagy IRR-ben.
  * **Választható -** ügyfél ASN: Ha hirdetési-előtagok nem regisztrált toohello társviszony-létesítés SZÁMOT, megadhatja hello számú toowhich regisztrálva vannak.
  * Útválasztási beállításjegyzék-név: Hello RIR megadhat / BMR mely hello ellen, számát, és előtagok regisztrálva van.
  * **Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.

   Futtassa a következő példa tooconfigure Microsoft társviszony-létesítés a kör hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 123.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 123.0.0.4/30 --vlan-id 300 --peering-type MicrosoftPeering --advertised-public-prefixes 123.1.0.0/24
  ```

### <a name="tooget-microsoft-peering-details"></a>tooget Microsoft társviszony-létesítési részletei

Konfigurációs részletek kaphat a következő példa hello használata:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzureMicrosoftPeering
```

hello hasonló toohello a következő példa a kimenetre:

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzureMicrosoftPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": {
    "advertisedPublicPrefixes": [
        ""
      ],
     "advertisedPublicPrefixesState": "",
     "customerASN": ,
     "routingRegistryName": ""
  }
  "name": "AzureMicrosoftPeering",
  "peerAsn": ,
  "peeringType": "AzureMicrosoftPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="tooupdate-microsoft-peering-configuration"></a>a Microsoft társviszony-létesítési konfiguráció tooupdate

Frissítheti, hello konfiguráció bármely részeként. hello meghirdetett hello áramkör előtagok programból 123.1.0.0/24 too124.1.0.0/24 hello a következő példa a frissített:

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroup --peering-type MicrosoftPeering --advertised-public-prefixes 124.1.0.0/24
```

### <a name="toodelete-microsoft-peering"></a>toodelete Microsoft társviszony-létesítés

A társviszony-létesítési konfiguráció a következő példa hello futtatásával távolíthatja el:

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name MicrosoftPeering
```

## <a name="next-steps"></a>Következő lépések

Következő lépés, [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](howto-linkvnet-cli.md).

* Az ExpressRoute-munkafolyamatokkal kapcsolatos további információkért lásd: [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-munkafolyamatok).
* A kapcsolatcsoportok társviszony-létesítéseivel kapcsolatos további információkért lásd: [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-kapcsolatcsoportok és útválasztási tartományok).
* További információkért a virtuális hálózatok használatáról lásd: [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtuális hálózatok áttekintése).