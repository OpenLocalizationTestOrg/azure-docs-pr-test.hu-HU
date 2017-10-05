---
title: "Az Azure Key Vault használatával az Azure Automation kezelése |} Microsoft Docs"
description: "További tudnivalók hogyan az Azure Automation szolgáltatás használható-e az Azure Key Vault kezeléséhez."
services: Key-Vault, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 4e780762-19b6-4ca6-b894-ebb44c538f35
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte;csand
ms.openlocfilehash: dee39662472fe54776b591977f2b1ecb39d15b00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-key-vault-using-azure-automation"></a><span data-ttu-id="c635b-103">Az Azure Key Vault használatával az Azure Automation kezelése</span><span class="sxs-lookup"><span data-stu-id="c635b-103">Managing Azure Key Vault using Azure Automation</span></span>
<span data-ttu-id="c635b-104">Az útmutatóból megismerheti az Azure Automation szolgáltatás, és hogyan használható egyszerűbbé teheti a kulcsok és titkos kulcsok Azure Key Vault a kezelését.</span><span class="sxs-lookup"><span data-stu-id="c635b-104">This guide will introduce you to the Azure Automation service and how it can be used to simplify management of your keys and secrets in Azure Key Vault.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="c635b-105">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="c635b-105">What is Azure Automation?</span></span>
<span data-ttu-id="c635b-106">[Azure Automation szolgáltatásbeli](../automation/automation-intro.md) van egy Azure-felhő kezelésüket folyamatok automatizálásához és a kívánt állapot konfigurációs szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c635b-106">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation and desired state configuration.</span></span> <span data-ttu-id="c635b-107">Használja az Azure Automation, manuális, ismétlődik, hosszan futó és hibalehetőséget feladatok automatizálhatók megbízhatóságát, hatékonyságát és a szervezet hamarabb növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="c635b-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="c635b-108">Azure Automation szolgáltatásbeli biztosít egy magas rendelkezésre állású, nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely méretezi az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="c635b-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales to meet your needs.</span></span> <span data-ttu-id="c635b-109">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="c635b-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="c635b-110">Csökkentheti a működési munkaterhelés és szabadítson fel informatikai és DevOps alkalmazottak munka, amely üzleti értéket hozzáadja a felhőbeli felügyeleti feladatok automatikusan Azure Automation által futtatandó mozgatásával összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="c635b-110">Reduce operational overhead and free up IT and DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a><span data-ttu-id="c635b-111">Hogyan segíthet az Azure Automation kezelése az Azure Key Vault?</span><span class="sxs-lookup"><span data-stu-id="c635b-111">How can Azure Automation help manage Azure Key Vault?</span></span>
<span data-ttu-id="c635b-112">Key Vault használatával kezelhető az Azure Automationben a [AzureRM Key Vault parancsmagjainak](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) és [Azure klasszikus Key Vault parancsmagjainak](https://msdn.microsoft.com/library/azure/dn868052.aspx).</span><span class="sxs-lookup"><span data-stu-id="c635b-112">Key Vault can be managed in Azure Automation by using the [AzureRM Key Vault cmdlets](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) and [Azure Classic Key Vault cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx).</span></span> <span data-ttu-id="c635b-113">A klasszikus Key Vault kezeléséhez Azure modul automatikusan az Azure Automationben érhető el, és importálhatja a [AzureRM-KeyVault modul](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) az Azure Automation, hogy a Key Vault felügyeleti feladatok a szolgáltatáson belül végezheti el.</span><span class="sxs-lookup"><span data-stu-id="c635b-113">The Azure module for managing classic Key Vault is available automatically in Azure Automation, and you can import the [AzureRM-KeyVault module](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) into Azure Automation, so that you can perform many of your Key Vault management tasks within the service.</span></span> <span data-ttu-id="c635b-114">Ezek a parancsmagok az Azure Automationben más Azure-szolgáltatások, Azure-szolgáltatások és a 3. fél rendszerek között összetett feladatok automatizálása a parancsmagjaival is párosítható.</span><span class="sxs-lookup"><span data-stu-id="c635b-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="c635b-115">Az Azure Key Vault parancsmagjainak hajthatja végre ezeket a feladatokat, többek között:</span><span class="sxs-lookup"><span data-stu-id="c635b-115">With the Azure Key Vault cmdlets you can perform these tasks among others:</span></span> 

* <span data-ttu-id="c635b-116">Hozza létre és konfigurálja a kulcstároló</span><span class="sxs-lookup"><span data-stu-id="c635b-116">Create and configure a key vault</span></span>
* <span data-ttu-id="c635b-117">Hozzon létre vagy kulcs importálása</span><span class="sxs-lookup"><span data-stu-id="c635b-117">Create or import a key</span></span>
* <span data-ttu-id="c635b-118">Létrehozni vagy frissíteni a titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="c635b-118">Create or update a secret</span></span>
* <span data-ttu-id="c635b-119">Attribútumok kulcs frissítése</span><span class="sxs-lookup"><span data-stu-id="c635b-119">Update attributes of a key</span></span>
* <span data-ttu-id="c635b-120">A kulcsok vagy titkos kulcsok beszerzése</span><span class="sxs-lookup"><span data-stu-id="c635b-120">Get a key or secret</span></span>
* <span data-ttu-id="c635b-121">Kulcs vagy titkos kulcs törlése</span><span class="sxs-lookup"><span data-stu-id="c635b-121">Delete a key or secret</span></span>

<span data-ttu-id="c635b-122">Íme néhány példa a Key Vault kezelése a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="c635b-122">Here are some examples of using PowerShell to manage Key Vault:</span></span>  

* [<span data-ttu-id="c635b-123">Az Azure Key Vault - lépésről lépésre</span><span class="sxs-lookup"><span data-stu-id="c635b-123">Azure Key Vault - Step by Step</span></span>](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [<span data-ttu-id="c635b-124">Beállítása és konfigurálása az Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c635b-124">Setting Up and Configuring an Azure Key Vault</span></span>](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a><span data-ttu-id="c635b-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c635b-125">Next steps</span></span>
<span data-ttu-id="c635b-126">Most, hogy megismerte az Azure Automation, és hogyan használható az Azure Key Vault kezeléséhez alapjait, az alábbi hivatkozásokból tudhat meg többet az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="c635b-126">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure Key Vault, follow these links to learn more about Azure Automation.</span></span>

* <span data-ttu-id="c635b-127">Tekintse meg az Azure Automation szolgáltatásbeli [használatába bevezető oktatóanyagot](../automation/automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="c635b-127">See the Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md).</span></span>
* <span data-ttu-id="c635b-128">Tekintse meg a [Azure Key Vault PowerShell-parancsfájlok](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).</span><span class="sxs-lookup"><span data-stu-id="c635b-128">See the [Azure Key Vault PowerShell scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).</span></span>

