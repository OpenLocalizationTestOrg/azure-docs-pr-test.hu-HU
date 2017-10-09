---
title: "aaaDeploy SAS-jogkivonat és PowerShell Azure-sablonnal |} Microsoft Docs"
description: "Azure Resource Manager és az Azure PowerShell toodeploy erőforrások tooAzure védett sablonból SAS-jogkivonatot használja."
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
ms.openlocfilehash: b95e096591d6213f8ef79235c8cd85705c4b79ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-powershell"></a><span data-ttu-id="860f1-103">Az SAS-jogkivonat és Azure PowerShell titkos Resource Manager-sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="860f1-103">Deploy private Resource Manager template with SAS token and Azure PowerShell</span></span>

<span data-ttu-id="860f1-104">A sablon olyan tárfiókban helyezkedik el, ha korlátozni a hozzáférést toohello sablon, és adjon meg egy közös hozzáférésű jogosultságkód (SAS) token üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="860f1-104">When your template resides in a storage account, you can restrict access toohello template and provide a shared access signature (SAS) token during deployment.</span></span> <span data-ttu-id="860f1-105">Ez a témakör azt ismerteti, hogyan Resource Manager sablonok tooprovide egy SAS-jogkivonat üzembe helyezése során az Azure PowerShell toouse.</span><span class="sxs-lookup"><span data-stu-id="860f1-105">This topic explains how toouse Azure PowerShell with Resource Manager templates tooprovide a SAS token during deployment.</span></span> 

## <a name="add-private-template-toostorage-account"></a><span data-ttu-id="860f1-106">Saját sablon toostorage fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="860f1-106">Add private template toostorage account</span></span>

<span data-ttu-id="860f1-107">A sablonok tooa tárfiók hozzáadása, és toothem csatolása a SAS-jogkivonat a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="860f1-107">You can add your templates tooa storage account and link toothem during deployment with a SAS token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="860f1-108">Hello alábbi lépéseket követve hello sablon tartalmazó hello blob elérhető tooonly hello fiók tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="860f1-108">By following hello steps below, hello blob containing hello template is accessible tooonly hello account owner.</span></span> <span data-ttu-id="860f1-109">Amikor létrehoz egy SAS-jogkivonat hello blob, hello blob azonban, hogy az URI-azonosítójú elérhető tooanyone.</span><span class="sxs-lookup"><span data-stu-id="860f1-109">However, when you create a SAS token for hello blob, hello blob is accessible tooanyone with that URI.</span></span> <span data-ttu-id="860f1-110">Ha egy másik felhasználó elfogja hello URI, a felhasználó képes tooaccess hello sablont.</span><span class="sxs-lookup"><span data-stu-id="860f1-110">If another user intercepts hello URI, that user is able tooaccess hello template.</span></span> <span data-ttu-id="860f1-111">SAS-token használatával korlátozza a hozzáférést tooyour sablonok egy jó módszer, de kell nem tartalmazhat bizalmas adatok, például jelszavak közvetlenül hello sablonban.</span><span class="sxs-lookup"><span data-stu-id="860f1-111">Using a SAS token is a good way of limiting access tooyour templates, but you should not include sensitive data like passwords directly in hello template.</span></span>
> 
> 

<span data-ttu-id="860f1-112">hello alábbi példa egy személyes fiók tároló beállítja és feltölt egy sablont:</span><span class="sxs-lookup"><span data-stu-id="860f1-112">hello following example sets up a private storage account container and uploads a template:</span></span>
   
```powershell
# create a storage account for templates
New-AzureRmResourceGroup -Name ManageGroup -Location "South Central US"
New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name} -Type Standard_LRS -Location "West US"
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# create a container and upload template
New-AzureStorageContainer -Name templates -Permission Off
Set-AzureStorageBlobContent -Container templates -File c:\MyTemplates\storage.json
```

## <a name="provide-sas-token-during-deployment"></a><span data-ttu-id="860f1-113">Adja meg a SAS-jogkivonat központi telepítése során</span><span class="sxs-lookup"><span data-stu-id="860f1-113">Provide SAS token during deployment</span></span>
<span data-ttu-id="860f1-114">a storage-fiók titkos sablont toodeploy a SAS-token létrehozásához, és adja hozzá hello URI hello sablon.</span><span class="sxs-lookup"><span data-stu-id="860f1-114">toodeploy a private template in a storage account, generate a SAS token and include it in hello URI for hello template.</span></span> <span data-ttu-id="860f1-115">Hello lejárati idő tooallow elég toocomplete hello üzembe helyezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="860f1-115">Set hello expiry time tooallow enough time toocomplete hello deployment.</span></span>
   
```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# get hello URI with hello SAS token
$templateuri = New-AzureStorageBlobSASToken -Container templates -Blob storage.json -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri $templateuri
```

<span data-ttu-id="860f1-116">Például egy SAS-jogkivonat használatával a csatolt sablonokkal, [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="860f1-116">For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="860f1-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="860f1-117">Next steps</span></span>
* <span data-ttu-id="860f1-118">Egy bevezető toodeploying sablonok, lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="860f1-118">For an introduction toodeploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="860f1-119">Egy teljes parancsfájlt, amely telepít egy sablon, lásd: [sablonparancsfájlt erőforrás-kezelő telepítése](resource-manager-samples-powershell-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="860f1-119">For a complete sample script that deploys a template, see [Deploy Resource Manager template script](resource-manager-samples-powershell-deploy.md)</span></span>
* <span data-ttu-id="860f1-120">a sablon toodefine paraméterek lásd [sablonok készítése](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="860f1-120">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="860f1-121">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="860f1-121">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

