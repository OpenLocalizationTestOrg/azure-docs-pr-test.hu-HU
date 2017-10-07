---
title: "az Azure Key Vault használatával az Azure Automation aaaManage |} Microsoft Docs"
description: "Azure Automation szolgáltatás hello hogyan lehet használt toomanage Azure Key Vault megismerése."
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
ms.openlocfilehash: 7f46ecc1206a96e8aeb1d086285461cb5b205472
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-key-vault-using-azure-automation"></a><span data-ttu-id="6fb69-103">Az Azure Key Vault használatával az Azure Automation kezelése</span><span class="sxs-lookup"><span data-stu-id="6fb69-103">Managing Azure Key Vault using Azure Automation</span></span>
<span data-ttu-id="6fb69-104">Az útmutatóból megismerheti toohello Azure Automation szolgáltatás, és hogyan lehet a kulcsok és titkos kulcsok Azure Key Vault a használt toosimplify kezelését.</span><span class="sxs-lookup"><span data-stu-id="6fb69-104">This guide will introduce you toohello Azure Automation service and how it can be used toosimplify management of your keys and secrets in Azure Key Vault.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="6fb69-105">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="6fb69-105">What is Azure Automation?</span></span>
<span data-ttu-id="6fb69-106">[Azure Automation szolgáltatásbeli](../automation/automation-intro.md) van egy Azure-felhő kezelésüket folyamatok automatizálásához és a kívánt állapot konfigurációs szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6fb69-106">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation and desired state configuration.</span></span> <span data-ttu-id="6fb69-107">Azure Automation használ, manuális, ismétlődő, hosszan futó és hibalehetőséget feladatok lehet automatizált tooincrease megbízhatóságát, hatékonyságát és idő toovalue a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="6fb69-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="6fb69-108">Azure Automation szolgáltatásbeli biztosít a magas rendelkezésre állású, nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely toomeet méretezi igényeinek.</span><span class="sxs-lookup"><span data-stu-id="6fb69-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales toomeet your needs.</span></span> <span data-ttu-id="6fb69-109">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="6fb69-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="6fb69-110">Csökkentheti a működési munkaterhelés és szabadítson fel informatikai, és automatikusan Azure Automation futtathatja DevOps személyzet toofocus munka, amely azáltal, hogy a felhő felügyeleti feladatok toobe üzleti értéket ad meg.</span><span class="sxs-lookup"><span data-stu-id="6fb69-110">Reduce operational overhead and free up IT and DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a><span data-ttu-id="6fb69-111">Hogyan segíthet az Azure Automation kezelése az Azure Key Vault?</span><span class="sxs-lookup"><span data-stu-id="6fb69-111">How can Azure Automation help manage Azure Key Vault?</span></span>
<span data-ttu-id="6fb69-112">Key Vault hello segítségével is kezelhető az Azure Automationben [AzureRM Key Vault parancsmagjainak](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) és [Azure klasszikus Key Vault parancsmagjainak](https://msdn.microsoft.com/library/azure/dn868052.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fb69-112">Key Vault can be managed in Azure Automation by using hello [AzureRM Key Vault cmdlets](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) and [Azure Classic Key Vault cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx).</span></span> <span data-ttu-id="6fb69-113">hello Azure modul, a klasszikus Key Vault kezeléséhez érhető el automatikusan az Azure Automationben, és importálhatja hello [AzureRM-KeyVault modul](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) az Azure Automation, hogy a Key Vault felügyeleti számos végezheti el feladatok hello szolgáltatáson belül.</span><span class="sxs-lookup"><span data-stu-id="6fb69-113">hello Azure module for managing classic Key Vault is available automatically in Azure Automation, and you can import hello [AzureRM-KeyVault module](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) into Azure Automation, so that you can perform many of your Key Vault management tasks within hello service.</span></span> <span data-ttu-id="6fb69-114">Ezeket a parancsmagokat az Azure Automationben más Azure-szolgáltatások, az összetett feladatok tooautomate hello-parancsmagjaival is párosítható az Azure-szolgáltatások és a 3. fél rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="6fb69-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="6fb69-115">Hello Azure Key Vault parancsmagjainak ezeket a feladatokat, többek között végezheti el:</span><span class="sxs-lookup"><span data-stu-id="6fb69-115">With hello Azure Key Vault cmdlets you can perform these tasks among others:</span></span> 

* <span data-ttu-id="6fb69-116">Hozza létre és konfigurálja a kulcstároló</span><span class="sxs-lookup"><span data-stu-id="6fb69-116">Create and configure a key vault</span></span>
* <span data-ttu-id="6fb69-117">Hozzon létre vagy kulcs importálása</span><span class="sxs-lookup"><span data-stu-id="6fb69-117">Create or import a key</span></span>
* <span data-ttu-id="6fb69-118">Létrehozni vagy frissíteni a titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="6fb69-118">Create or update a secret</span></span>
* <span data-ttu-id="6fb69-119">Attribútumok kulcs frissítése</span><span class="sxs-lookup"><span data-stu-id="6fb69-119">Update attributes of a key</span></span>
* <span data-ttu-id="6fb69-120">A kulcsok vagy titkos kulcsok beszerzése</span><span class="sxs-lookup"><span data-stu-id="6fb69-120">Get a key or secret</span></span>
* <span data-ttu-id="6fb69-121">Kulcs vagy titkos kulcs törlése</span><span class="sxs-lookup"><span data-stu-id="6fb69-121">Delete a key or secret</span></span>

<span data-ttu-id="6fb69-122">Íme néhány példa a PowerShell toomanage Key Vault használatával:</span><span class="sxs-lookup"><span data-stu-id="6fb69-122">Here are some examples of using PowerShell toomanage Key Vault:</span></span>  

* [<span data-ttu-id="6fb69-123">Az Azure Key Vault - lépésről lépésre</span><span class="sxs-lookup"><span data-stu-id="6fb69-123">Azure Key Vault - Step by Step</span></span>](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [<span data-ttu-id="6fb69-124">Beállítása és konfigurálása az Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="6fb69-124">Setting Up and Configuring an Azure Key Vault</span></span>](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a><span data-ttu-id="6fb69-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6fb69-125">Next steps</span></span>
<span data-ttu-id="6fb69-126">Most, hogy megismerte az Azure Automation, és hogyan lehet az Azure Key Vault használt toomanage hello alapjait, kövesse az alábbi hivatkozások toolearn Azure automatizálásával kapcsolatos további.</span><span class="sxs-lookup"><span data-stu-id="6fb69-126">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure Key Vault, follow these links toolearn more about Azure Automation.</span></span>

* <span data-ttu-id="6fb69-127">Lásd: hello Azure Automation [első lépéseket ismertető oktatóanyagban](../automation/automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="6fb69-127">See hello Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md).</span></span>
* <span data-ttu-id="6fb69-128">Lásd: hello [Azure Key Vault PowerShell-parancsfájlok](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).</span><span class="sxs-lookup"><span data-stu-id="6fb69-128">See hello [Azure Key Vault PowerShell scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).</span></span>

