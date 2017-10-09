---
title: "PowerShell parancsfájl minta - aaaAzure alkalmazás cert tooa fürt hozzáadása |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minták – az alkalmazás tanúsítvány tooa Service Fabric-fürt hozzáadása."
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
ms.openlocfilehash: aba62598e2e4775012f89b5070bef5e61aec64f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-an-application-certificate-tooa-service-fabric-cluster"></a>Az alkalmazás tanúsítvány tooa Service Fabric-fürt hozzáadása

Ez a parancsfájlpélda hoz létre egy önaláírt tanúsítványt hello megadott az Azure key vault, és telepíti azt tooall hello Service Fabric-fürt csomópontja. hello tanúsítvány is letölti tooa helyi mappát. hello letöltött tanúsítvány hello neve van hello ugyanaz, mint a key vaultban hello hello tanúsítvány hello neve. Hello paraméterek testreszabása, igény szerint.

Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview) , majd futtassa `Login-AzureRmAccount` toocreate Azure kapcsolatot. 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate tooa cluster")]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok hello: hello tábla minden egyes parancsa toocommand adott dokumentáció hivatkozásokat tartalmaz.

| Parancs | Megjegyzések |
|---|---|
| [Adja hozzá AzureRmServiceFabricApplicationCertificate](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | Adjon hozzá egy új alkalmazás tanúsítvány toohello virtuálisgép-méretezési hello fürtöt alkotó.  |

## <a name="next-steps"></a>Következő lépések

Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

Azure Service Fabric további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../service-fabric-powershell-samples.md).
