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
# <a name="add-an-application-certificate-tooa-service-fabric-cluster"></a><span data-ttu-id="bcf45-103">Az alkalmazás tanúsítvány tooa Service Fabric-fürt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bcf45-103">Add an application certificate tooa Service Fabric cluster</span></span>

<span data-ttu-id="bcf45-104">Ez a parancsfájlpélda hoz létre egy önaláírt tanúsítványt hello megadott az Azure key vault, és telepíti azt tooall hello Service Fabric-fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="bcf45-104">This sample script creates a self-signed certificate in hello specified Azure key vault and installs it tooall nodes of hello Service Fabric cluster.</span></span> <span data-ttu-id="bcf45-105">hello tanúsítvány is letölti tooa helyi mappát.</span><span class="sxs-lookup"><span data-stu-id="bcf45-105">hello certificate also downloads tooa local folder.</span></span> <span data-ttu-id="bcf45-106">hello letöltött tanúsítvány hello neve van hello ugyanaz, mint a key vaultban hello hello tanúsítvány hello neve.</span><span class="sxs-lookup"><span data-stu-id="bcf45-106">hello name of hello downloaded certificate is hello same as hello name of hello certificate in hello key vault.</span></span> <span data-ttu-id="bcf45-107">Hello paraméterek testreszabása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="bcf45-107">Customize hello parameters as needed.</span></span>

<span data-ttu-id="bcf45-108">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview) , majd futtassa `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="bcf45-108">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview) and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="bcf45-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="bcf45-109">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate tooa cluster")]

## <a name="script-explanation"></a><span data-ttu-id="bcf45-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="bcf45-110">Script explanation</span></span>

<span data-ttu-id="bcf45-111">A parancsfájl a következő parancsok hello: hello tábla minden egyes parancsa toocommand adott dokumentáció hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bcf45-111">This script uses hello following commands: Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="bcf45-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="bcf45-112">Command</span></span> | <span data-ttu-id="bcf45-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="bcf45-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bcf45-114">Adja hozzá AzureRmServiceFabricApplicationCertificate</span><span class="sxs-lookup"><span data-stu-id="bcf45-114">Add-AzureRmServiceFabricApplicationCertificate</span></span>](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | <span data-ttu-id="bcf45-115">Adjon hozzá egy új alkalmazás tanúsítvány toohello virtuálisgép-méretezési hello fürtöt alkotó.</span><span class="sxs-lookup"><span data-stu-id="bcf45-115">Add a new application certificate toohello virtual machine scale set that make up hello cluster.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="bcf45-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bcf45-116">Next steps</span></span>

<span data-ttu-id="bcf45-117">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bcf45-117">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="bcf45-118">Azure Service Fabric további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="bcf45-118">Additional Azure Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
