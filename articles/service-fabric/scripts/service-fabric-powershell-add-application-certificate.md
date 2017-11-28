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
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a><span data-ttu-id="d5be6-103">Az alkalmazás tanúsítványának felvétele a Service Fabric-fürt</span><span class="sxs-lookup"><span data-stu-id="d5be6-103">Add an application certificate to a Service Fabric cluster</span></span>

<span data-ttu-id="d5be6-104">Ez a parancsfájlpélda létrehoz egy önaláírt tanúsítványt a megadott Azure-tárolóban, és a Service Fabric-fürt összes csomópontjára telepíti azt.</span><span class="sxs-lookup"><span data-stu-id="d5be6-104">This sample script creates a self-signed certificate in the specified Azure key vault and installs it to all nodes of the Service Fabric cluster.</span></span> <span data-ttu-id="d5be6-105">A tanúsítvány is letölti egy helyi mappába.</span><span class="sxs-lookup"><span data-stu-id="d5be6-105">The certificate also downloads to a local folder.</span></span> <span data-ttu-id="d5be6-106">A letöltött tanúsítvány neve megegyezik a key vault a tanúsítvány neve.</span><span class="sxs-lookup"><span data-stu-id="d5be6-106">The name of the downloaded certificate is the same as the name of the certificate in the key vault.</span></span> <span data-ttu-id="d5be6-107">A paraméterek testreszabása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="d5be6-107">Customize the parameters as needed.</span></span>

<span data-ttu-id="d5be6-108">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview) , majd futtassa `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="d5be6-108">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview) and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="d5be6-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="d5be6-109">Sample script</span></span>

<span data-ttu-id="d5be6-110">[!code-powershell[fő](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "egy alkalmazás-tanúsítvány hozzáadása a fürthöz")]</span><span class="sxs-lookup"><span data-stu-id="d5be6-110">[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate to a cluster")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="d5be6-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="d5be6-111">Script explanation</span></span>

<span data-ttu-id="d5be6-112">Ezt a parancsfájlt az alábbi parancsokat használja: minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="d5be6-112">This script uses the following commands: Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d5be6-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="d5be6-113">Command</span></span> | <span data-ttu-id="d5be6-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d5be6-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d5be6-115">Adja hozzá AzureRmServiceFabricApplicationCertificate</span><span class="sxs-lookup"><span data-stu-id="d5be6-115">Add-AzureRmServiceFabricApplicationCertificate</span></span>](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | <span data-ttu-id="d5be6-116">Új alkalmazás tanúsítvány hozzáadása a virtuálisgép-méretezési csoport a fürtöt alkotó.</span><span class="sxs-lookup"><span data-stu-id="d5be6-116">Add a new application certificate to the virtual machine scale set that make up the cluster.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="d5be6-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d5be6-117">Next steps</span></span>

<span data-ttu-id="d5be6-118">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d5be6-118">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="d5be6-119">Azure Service Fabric további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d5be6-119">Additional Azure Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
