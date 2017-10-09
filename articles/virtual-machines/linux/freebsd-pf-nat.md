---
title: "aaaUse freebsd rendszerű csomagszűrők toocreate a tűzfal az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy NAT tűzfal használata freebsd rendszerű számítógépén PF az Azure-ban."
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: 3d3a5dde2ca03ba6fc384581c786f5eb746e6d92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-freebsds-packet-filter-toocreate-a-secure-firewall-in-azure"></a>Hogyan toouse freebsd rendszerű csomagszűrők toocreate biztonságos tűzfal az Azure-ban
Ez a cikk bemutatja hogyan toodeploy freebsd rendszerű a csomagoló szűrő Azure Resource Manager sablon segítségével használatával általános web server helyzetre NAT-tűzfalon.

## <a name="what-is-pf"></a>Mi az a PF?
PF (csomagszűrők, is írt pf) BSD licencelt csomag szűrő, egy központi szoftvertermék a firewalling. PF azóta kialakulásának gyorsan, és most keresztül elérhető tűzfalak több előnye is van. Hálózati címfordítás (NAT) PF nap egyet, majd csomagütemező óta, és aktív várólista felügyeleti lettek integrálva a PF, hello ALTQ integrálása, és így PF tartozó konfigurálással konfigurálható. Szolgáltatások például pfsync és a carp-ot a feladatátvételi és a redundanciát, a authpf munkamenet hitelesítési és ftp-proxy tooease firewalling hello nehéz FTP protokollt is meghosszabbította PF. PF rövid, hatékony és gazdag tűzfal. 

## <a name="get-started"></a>Bevezetés
Ha szeretné a biztonságos tűzfal beállítása a webkiszolgálók hello felhőben, majd lássunk neki. Az Azure Resource Manager sablon tooset fel a hálózati topológia használt hello parancsfájlok is alkalmazhat.
hello Azure Resource Manager sablon állítson be egy freebsd rendszerű virtuális gépet, amely hajt végre a NAT /redirection PF és két freebsd rendszerű virtuális gépek használata a hello Nginx webkiszolgálón telepítve és konfigurálva. Továbbá a hello két webkiszolgálók NAT tooperforming kilépési forgalom, hello NAT/átirányítás virtuális gép elfogja a HTTP-kérelmekre, és átirányítja őket toohello ciklikus multiplexeléssel két webkiszolgálók. hello VNet hello titkos nem átirányítható IP cím terület 10.0.0.2/24 használ, és megváltoztathatja hello sablon hello paramétereit. hello Azure Resource Manager-sablon is meghatároz egy útvonaltábla hello teljes virtuális hálózatot, amely külön útvonalak gyűjteménye használt toooverride Azure alapértelmezett útvonalak hello cél IP-címe alapján. 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a>Az Azure CLI segítségével végzi a telepítést
Hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login). Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal. hello alábbi példa létrehoz egy erőforráscsoport neve `myResourceGroup` a hello `West US` helyét.

```azurecli
az group create --name myResourceGroup --location westus
```

Ezután telepítse a hello sablon [pf-freebsd rendszerű-telepítés](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) rendelkező [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create). Töltse le [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) a hello ugyanazt az útvonalat, és határozza meg, mint a saját erőforrás értékek `adminPassword`, `networkPrefix`, és `domainNamePrefix`. 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

Öt perc múlva elérhetővé válik a üdvözlő hatásával `"provisioningState": "Succeeded"`. Akkor is ssh toohello előtérbeli virtuális gép (NAT) vagy nem érhető el a böngészőt hello nyilvános IP-cím vagy FQDN-jét hello előtérbeli virtuális gép (NAT) Nginx-webkiszolgálón. hello alábbi példa felsorolja teljes tartománynév és a nyilvános IP-cím toohello elülső rétegbeli virtuális gép (NAT) hello rendelt `myResourceGroup` erőforráscsoportot. 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a>Következő lépések
Szeretné tooset be a saját NAT az Azure-ban? Nyissa meg a forrást, szabad, de hatékony? PF akkor érdemes választani. Hello sablonnal [pf-freebsd rendszerű-telepítés](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), csak a szükséges öt perc tooset fel a NAT-tűzfalon ciklikus multiplexelés használatával freebsd rendszerű tartozó terheléselosztási PF a web server forgatókönyve az Azure-ban. 

Ha azt szeretné, hogy az Azure-ban freebsd rendszerű toolearn hello elérhető, tekintse meg a túl[bemutatása tooFreeBSD Azure](freebsd-intro-on-azure.md).

Ha azt szeretné, hogy további információk a PF tooknow, tekintse meg a túl[freebsd rendszerű kézikönyv](https://www.freebsd.org/doc/handbook/firewalls-pf.html) vagy [PF-útmutató](https://www.freebsd.org/doc/handbook/firewalls-pf.html).
