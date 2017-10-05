---
title: "Az Azure PowerShell-parancsfájl minta - alkalmazás tanúsítvány hozzáadása a fürthöz |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minták – az alkalmazás tanúsítványának felvétele a Service Fabric-fürt."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 9ccd6bb0458bc03e52103fa70cad26bd6bf98bd5
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Az alkalmazás tanúsítványának felvétele a Service Fabric-fürt

Ez a parancsfájlpélda létrehoz egy önaláírt tanúsítványt a megadott Azure-tárolóban, és a Service Fabric-fürt összes csomópontjára telepíti azt. A tanúsítvány is letölti egy helyi mappába. A letöltött tanúsítvány neve megegyezik a key vault a tanúsítvány neve. A paraméterek testreszabása, igény szerint.

Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview) , majd futtassa `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral. 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[fő](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "egy alkalmazás-tanúsítvány hozzáadása a fürthöz")]

## <a name="script-explanation"></a>Parancsfájl ismertetése

Ezt a parancsfájlt az alábbi parancsokat használja: minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [Adja hozzá AzureRmServiceFabricApplicationCertificate](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | Új alkalmazás tanúsítvány hozzáadása a virtuálisgép-méretezési csoport a fürtöt alkotó.  |

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

Azure Service Fabric további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../service-fabric-powershell-samples.md).
