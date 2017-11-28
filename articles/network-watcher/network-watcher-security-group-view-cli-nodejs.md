---
title: "aaaAnalyze hálózati biztonság Azure hálózati figyelő biztonsági csoport láthassák - Azure CLI 1.0 |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse Azure CLI 1.0 tooanalyze a virtuális gépek biztonsági a biztonsági csoport megtekintése."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: a986ff4f-7e0c-4994-95e1-4ac824986500
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 96383a734b94d215d5b0f3d47339e46940d700b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-10"></a><span data-ttu-id="fabb7-103">A virtuális gép biztonsági a biztonsági csoport nézet segítségével az Azure CLI 1.0 elemzése</span><span class="sxs-lookup"><span data-stu-id="fabb7-103">Analyze your Virtual Machine security with Security Group View using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="fabb7-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fabb7-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="fabb7-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="fabb7-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="fabb7-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="fabb7-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="fabb7-107">REST API</span><span class="sxs-lookup"><span data-stu-id="fabb7-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="fabb7-108">Biztonsági csoport megtekintése konfigurált és hatékony hálózati biztonsági szabályok, amelyek a virtuális gép alkalmazott tooa adja vissza.</span><span class="sxs-lookup"><span data-stu-id="fabb7-108">Security group view returns configured and effective network security rules that are applied tooa virtual machine.</span></span> <span data-ttu-id="fabb7-109">Ez a funkció hasznos tooaudit és diagnosztizálhatja a hálózati biztonsági csoportok és a virtuális gépek tooensure forgalma konfigurált szabályok folyamatban van megfelelően engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="fabb7-109">This capability is useful tooaudit and diagnose Network Security Groups and rules that are configured on a VM tooensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="fabb7-110">Ebben a cikkben megmutatjuk, hogyan tooretrieve hello konfigurálva és hatékony biztonsági szabályok tooa virtuális gépet az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="fabb7-110">In this article, we show you how tooretrieve hello configured and effective security rules tooa virtual machine using Azure CLI</span></span>

<span data-ttu-id="fabb7-111">Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="fabb7-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="fabb7-112">Hálózati figyelőt jelenleg használ Azure CLI 1.0 CLI támogatására.</span><span class="sxs-lookup"><span data-stu-id="fabb7-112">Network Watcher currently uses Azure CLI 1.0 for CLI support.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="fabb7-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="fabb7-113">Before you begin</span></span>

<span data-ttu-id="fabb7-114">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="fabb7-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="fabb7-115">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="fabb7-115">Scenario</span></span>

<span data-ttu-id="fabb7-116">a cikkben szereplő hello forgatókönyv lekéri a konfigurált hello és a hatékony biztonsági szabályok egy adott virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="fabb7-116">hello scenario covered in this article retrieves hello configured and effective security rules for a given virtual machine.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="fabb7-117">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="fabb7-117">Get a VM</span></span>

<span data-ttu-id="fabb7-118">A virtuális gép szükség toorun hello `vm list` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fabb7-118">A virtual machine is required toorun hello `vm list` cmdlet.</span></span> <span data-ttu-id="fabb7-119">hello következő parancs kilistázza hello virtuális machinese erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="fabb7-119">hello following command lists hello virtual machinese in a resource group:</span></span>

```azurecli
azure vm list -g resourceGroupName
```

<span data-ttu-id="fabb7-120">Miután eldöntötte, hello virtuális gép, használhatja a hello `vm show` parancsmag tooget az erőforrás-azonosítót:</span><span class="sxs-lookup"><span data-stu-id="fabb7-120">Once you know hello virtual machine, you can use hello `vm show` cmdlet tooget its resource Id:</span></span>

```azurecli
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="fabb7-121">Biztonsági csoport nézet beolvasása</span><span class="sxs-lookup"><span data-stu-id="fabb7-121">Retrieve security group view</span></span>

<span data-ttu-id="fabb7-122">hello tovább tooretrieve hello biztonsági csoport megtekintése eredménye.</span><span class="sxs-lookup"><span data-stu-id="fabb7-122">hello next step is tooretrieve hello security group view result.</span></span> <span data-ttu-id="fabb7-123">Hozzáadását hello "--json" jelző formázza hello eredmények json-ban.</span><span class="sxs-lookup"><span data-stu-id="fabb7-123">Adding hello "--json" flag will format hello results in json.</span></span>

```azurecli
azure network watcher security-group-view -g resourceGroupName -n networkWatcherName -t targetResourceId --json
```

## <a name="viewing-hello-results"></a><span data-ttu-id="fabb7-124">Hello eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="fabb7-124">Viewing hello results</span></span>

<span data-ttu-id="fabb7-125">hello következő példa egy rövidített választ hello eredményt adott vissza.</span><span class="sxs-lookup"><span data-stu-id="fabb7-125">hello following example is a shortened response of hello results returned.</span></span> <span data-ttu-id="fabb7-126">hello eredményeket jelenít meg minden hello hatékony és alkalmazott biztonsági szabályokat csoportok bontásban hello virtuális gépen **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, és  **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="fabb7-126">hello results show all hello effective and applied security rules on hello virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testnic",
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testvm",
          "securityRules": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/test-nsg/securityRules/default-allow-rdp",
              "protocol": "TCP",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "*",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 1000,
              "direction": "Inbound",
              "provisioningState": "Succeeded",
              "name": "default-allow-rdp",
              "etag": "W/\"00000000-0000-0000-0000-000000000000\""
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/networkSecurityGroups//defaultSecurityRules/",
            "description": "Allow inbound traffic from all VMs in VNET",
            "protocol": "*",
            "sourcePortRange": "*",
            "destinationPortRange": "*",
            "sourceAddressPrefix": "VirtualNetwork",
            "destinationAddressPrefix": "VirtualNetwork",
            "access": "Allow",
            "priority": 65000,
            "direction": "Inbound",
            "provisioningState": "Succeeded",
            "name": "AllowVnetInBound"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="fabb7-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fabb7-127">Next steps</span></span>

<span data-ttu-id="fabb7-128">Látogasson el [naplózás hálózati biztonsági csoportok (NSG) rendelkező hálózati figyelőt](network-watcher-nsg-auditing-powershell.md) toolearn hogyan hálózati biztonsági csoportok tooautomate érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="fabb7-128">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md) toolearn how tooautomate validation of Network Security Groups.</span></span>

<span data-ttu-id="fabb7-129">További tudnivalók hello biztonsági szabályokat alkalmazott tooyour hálózati erőforrások ellátogatva [biztonsági csoport megtekintése – áttekintés](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="fabb7-129">Learn more about hello security rules that are applied tooyour network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
