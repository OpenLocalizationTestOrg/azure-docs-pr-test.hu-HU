---
title: "Az SAS-jogkivonat és PowerShell Azure-sablon üzembe helyezése |} Microsoft Docs"
description: "Azure Resource Manager és az Azure PowerShell használatával telepítse az erőforrások az Azure által védett SAS-jogkivonat sablonból."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 1e3cea027b599e2b1af1ced0fdf14e2cc8a0db82
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-powershell"></a><span data-ttu-id="3bbb9-103">Az SAS-jogkivonat és Azure PowerShell titkos Resource Manager-sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="3bbb9-103">Deploy private Resource Manager template with SAS token and Azure PowerShell</span></span>

<span data-ttu-id="3bbb9-104">Ha a sablon olyan tárfiókban található, korlátozza a hozzáférést a sablont, és adjon meg egy közös hozzáférésű jogosultságkód (SAS) token üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="3bbb9-104">When your template resides in a storage account, you can restrict access to the template and provide a shared access signature (SAS) token during deployment.</span></span> <span data-ttu-id="3bbb9-105">Ez a témakör ismerteti az Azure PowerShell használata a Resource Manager-sablonok központi telepítése során egy SAS-jogkivonat biztosításához.</span><span class="sxs-lookup"><span data-stu-id="3bbb9-105">This topic explains how to use Azure PowerShell with Resource Manager templates to provide a SAS token during deployment.</span></span> 

## <a name="add-private-template-to-storage-account"></a><span data-ttu-id="3bbb9-106">A tárfiók saját sablon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3bbb9-106">Add private template to storage account</span></span>

<span data-ttu-id="3bbb9-107">A sablonok hozzáadása egy tárfiókot, és hivatkozzon őket egy SAS-jogkivonat a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="3bbb9-107">You can add your templates to a storage account and link to them during deployment with a SAS token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3bbb9-108">Az alábbi lépéseket követve sablont tartalmazó blob elérhető, csak a fiók tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="3bbb9-108">By following the steps below, the blob containing the template is accessible to only the account owner.</span></span> <span data-ttu-id="3bbb9-109">Amikor létrehoz egy SAS-jogkivonat a blob, a blob azonban, hogy az URI-azonosítójú bárki hozzáférhet.</span><span class="sxs-lookup"><span data-stu-id="3bbb9-109">However, when you create a SAS token for the blob, the blob is accessible to anyone with that URI.</span></span> <span data-ttu-id="3bbb9-110">Ha egy másik felhasználó elfogja az URI, a felhasználó hozzáférjen a sablon lesz.</span><span class="sxs-lookup"><span data-stu-id="3bbb9-110">If another user intercepts the URI, that user is able to access the template.</span></span> <span data-ttu-id="3bbb9-111">A SAS-jogkivonat használatával egy jó módszer a sablonok való hozzáférés korlátozása, de kell nem tartalmazhat bizalmas adatok, például jelszavak közvetlenül a sablonban.</span><span class="sxs-lookup"><span data-stu-id="3bbb9-111">Using a SAS token is a good way of limiting access to your templates, but you should not include sensitive data like passwords directly in the template.</span></span>
> 
> 

<span data-ttu-id="3bbb9-112">Az alábbi példa állít be egy személyes fiók tárolót, és feltölti a sablont:</span><span class="sxs-lookup"><span data-stu-id="3bbb9-112">The following example sets up a private storage account container and uploads a template:</span></span>
   
```powershell
# create a storage account for templates
New-AzureRmResourceGroup -Name ManageGroup -Location "South Central US"
New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name} -Type Standard_LRS -Location "West US"
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# create a container and upload template
New-AzureStorageContainer -Name templates -Permission Off
Set-AzureStorageBlobContent -Container templates -File c:\MyTemplates\storage.json
```

## <a name="provide-sas-token-during-deployment"></a><span data-ttu-id="3bbb9-113">Adja meg a SAS-jogkivonat központi telepítése során</span><span class="sxs-lookup"><span data-stu-id="3bbb9-113">Provide SAS token during deployment</span></span>
<span data-ttu-id="3bbb9-114">A tárfiókban lévő titkos sablon telepítése, a SAS-token létrehozásához, és adja hozzá a sablon az URI azonosító.</span><span class="sxs-lookup"><span data-stu-id="3bbb9-114">To deploy a private template in a storage account, generate a SAS token and include it in the URI for the template.</span></span> <span data-ttu-id="3bbb9-115">Állítsa be a lejárati időpont hagyjon elegendő időt a telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="3bbb9-115">Set the expiry time to allow enough time to complete the deployment.</span></span>
   
```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# get the URI with the SAS token
$templateuri = New-AzureStorageBlobSASToken -Container templates -Blob storage.json -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri $templateuri
```

<span data-ttu-id="3bbb9-116">Például egy SAS-jogkivonat használatával a csatolt sablonokkal, [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3bbb9-116">For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="3bbb9-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3bbb9-117">Next steps</span></span>
* <span data-ttu-id="3bbb9-118">Megismerkedhet a sablonok központi telepítése, lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="3bbb9-118">For an introduction to deploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="3bbb9-119">Egy teljes parancsfájlt, amely telepít egy sablon, lásd: [sablonparancsfájlt erőforrás-kezelő telepítése](resource-manager-samples-powershell-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="3bbb9-119">For a complete sample script that deploys a template, see [Deploy Resource Manager template script](resource-manager-samples-powershell-deploy.md)</span></span>
* <span data-ttu-id="3bbb9-120">Sablon paraméterek megadásához tekintse meg a [sablonok készítése](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="3bbb9-120">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="3bbb9-121">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="3bbb9-121">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

