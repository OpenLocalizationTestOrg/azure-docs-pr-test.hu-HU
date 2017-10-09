---
title: "Linux virtuális gép az Azure kiterjesztése aaaOMS |} Microsoft Docs"
description: "A Linux virtuális gépet egy virtuálisgép-bővítmény használatával hello OMS-ügynököt telepíteni."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: 0fc8003d1fae6c043eef18ae78d12f9304b70832
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="oms-virtual-machine-extension-for-linux"></a>Linux OMS virtuálisgép-bővítmény

## <a name="overview"></a>Áttekintés

Az Operations Management Suite (OMS) figyelési riasztási és riasztási szervizelési képességeket biztosít a felhő között és a helyszíni eszközök. hello OMS-ügynököt a virtuálisgép-bővítmény Linux közzétett és a Microsoft támogatja. hello bővítmény hello OMS-ügynököt telepít Azure virtuális gépeken, és regisztrálja a virtuális gépek be egy meglévő OMS-munkaterület. Ez a dokumentum részletek hello támogatott platformokat, a konfigurációk és a központi telepítési beállítások hello OMS virtuálisgép-bővítmény Linux.

## <a name="prerequisites"></a>Előfeltételek

### <a name="operating-system"></a>Operációs rendszer

hello OMS-ügynököt bővítmény futtatható a Linux terjesztéseket ellen.

| Disztribúció | Verzió |
|---|---|
| CentOS Linux | 5, 6 és 7 |
| Oracle Linux | 5, 6 és 7 |
| Red Hat Enterprise Linux Server | 5, 6, 7 |
| Debian GNU/Linux | 8, 6, 7 |
| Ubuntu | 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS |
| SUSE Linux Enterprise Server | 11 és 12 |

### <a name="internet-connectivity"></a>Internetkapcsolat

hello Linux kiterjesztése OMS-ügynököt kell lennie, hogy hello a cél virtuális gép csatlakoztatott toohello internet. 

## <a name="extension-schema"></a>A séma kiterjesztése

hello következő JSON látható hello OMS-ügynököt bővítmény hello sémáját. hello bővítmény hello munkaterület azonosítója és munkaterület hello cél OMS-munkaterület; a kulcs szükséges. Ezeket az értékeket az hello OMS-portálon található. Hello munkaterületkulcsot bizalmas adatokat kell kezelni, mert azt egy védett beállítás konfigurációban kell tárolni. Azure virtuális gépekre vonatkozó beállításával bővítmény védett adatok titkosítva, és csak visszafejteni hello cél virtuális gépen. Vegye figyelembe, hogy **workspaceId** és **workspaceKey** -és nagybetűk.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### <a name="property-values"></a>A tulajdonság értékek

| Név | Érték / – példa |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Közzétevő | Microsoft.EnterpriseCloud.Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.4 |
| workspaceId (például) | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (például) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ == |


## <a name="template-deployment"></a>Sablonalapú telepítés

Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető. Sablonok ideálisak, például a bevezetési tooOMS feladás egy vagy több központi telepítési beállításokra van szükség egy vagy több virtuális gépek telepítése során. Egy minta Resource Manager-sablon, amely tartalmazza az OMS-ügynök Virtuálisgép-bővítmény hello hello található [Azure Quick Start gyűjtemény](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

egy virtuálisgép-bővítmény hello JSON konfigurációja ágyazott hello virtuálisgép-erőforrást, vagy hello gyökér- vagy legfelső szintű erőforrás-kezelő JSON-sablon elhelyezve. hello JSON konfigurációs hello elhelyezését hello értéket hello erőforrás neve és típusa befolyásolja. További információkért lásd: [nevét és típusát gyermekerőforrásait beállítása](../../azure-resource-manager/resource-manager-template-child-resource.md). 

hello alábbi példa azt feltételezi, hogy hello OMS bővítmény hello virtuálisgép-erőforrás van beágyazva. Ha hello bővítmény erőforrás beágyazási, hello JSON hello kerül `"resources": []` objektum hello virtuális gép.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

Hello bővítmény JSON hello gyökerében hello sablon elhelyezésekor hello erőforrás neve tartalmaz egy hivatkozást toohello szülő virtuális gép, és hello típus hello beágyazott konfigurációs tükrözi.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a>Az Azure CLI-telepítés

hello Azure CLI használt toodeploy hello OMS-ügynök VM bővítmény tooan meglévő virtuális gép is lehet. Cserélje le a hello OMS kulcs és az OMS-azonosító az OMS-munkaterület együtt. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.4 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a>Hibaelhárítás és támogatás

### <a name="troubleshoot"></a>Hibaelhárítás

A bővítmény központi telepítések hello állapotával kapcsolatos információkat lehet adatokat beolvasni a hello Azure-portálon, és hello Azure parancssori felület használatával. egy adott virtuális Gépet, futtassa a következő parancs használatával hello kiterjesztéseinek toosee hello telepítési állapota hello Azure parancssori felület.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Bővítmény végrehajtási kimeneti van naplózott toohello következő fájl:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Hibakódok és azok jelentését

| Hibakód: | Jelentése | Lehetséges művelet |
| :---: | --- | --- |
| 10 | Virtuális gép már csatlakoztatott tooan OMS-munkaterület | tooconnect hello VM toohello munkaterület hello bővítmény sémában megadott stopOnMultipleConnections toofalse állítsa nyilvános beállításai, vagy távolítsa el ezt a tulajdonságot. Ez a virtuális gép lekérdezi számlázva után az egyes munkaterületeken van csatlakoztatva. |
| 11 | Érvénytelen a megadott konfigurációs toohello bővítmény | Kövesse az előző példák tooset a telepítéshez szükséges minden tulajdonság értékével hello. |
| 12 | hello dpkg Csomagkezelő zárolva van | Győződjön meg arról, hogy minden dpkg frissítési műveletek hello gépen végzett, és próbálkozzon újra. |
| 20 | Túl korán nevű engedélyezése | [Frissítés hello Azure Linux ügynök](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) toohello elérhető legújabb verzióra. |
| 51 | A bővítmény nem támogatott a hello virtuális gép operációs rendszer | |
| 55 | Nem lehet kapcsolódni a Microsoft Operations Management Suite szolgáltatás toohello | Ellenőrizze, hogy hello rendszernek van-e Internet-hozzáféréssel, vagy adtak meg, hogy érvényes HTTP-proxy. Ezenkívül ellenőrizze a helyességét hello hello munkaterület azonosítója. |

További hibaelhárítási információért hello található [OMS-ügynök-az-Linux hibaelhárítási útmutatója](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).

### <a name="support"></a>Támogatás

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure szakértői hello hello [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/). Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatást. Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).
