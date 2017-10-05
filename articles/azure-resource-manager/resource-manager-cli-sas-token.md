---
title: "Az SAS-jogkivonat és az Azure CLI Azure-sablon üzembe helyezése |} Microsoft Docs"
description: "Azure Resource Manager és az Azure parancssori felület használatával telepítse az erőforrások az Azure által védett SAS-jogkivonat sablonból."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 22387aadd8f53a65efb76a29a9403c46a2c25954
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-cli"></a><span data-ttu-id="0cc3e-103">Az SAS-jogkivonat és az Azure parancssori felület titkos Resource Manager-sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="0cc3e-103">Deploy private Resource Manager template with SAS token and Azure CLI</span></span>

<span data-ttu-id="0cc3e-104">Ha a sablon olyan tárfiókban található, korlátozza a hozzáférést a sablont, és adjon meg egy közös hozzáférésű jogosultságkód (SAS) token üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="0cc3e-104">When your template resides in a storage account, you can restrict access to the template and provide a shared access signature (SAS) token during deployment.</span></span> <span data-ttu-id="0cc3e-105">Ez a témakör ismerteti az Azure PowerShell használata a Resource Manager-sablonok központi telepítése során egy SAS-jogkivonat biztosításához.</span><span class="sxs-lookup"><span data-stu-id="0cc3e-105">This topic explains how to use Azure PowerShell with Resource Manager templates to provide a SAS token during deployment.</span></span> 

## <a name="add-private-template-to-storage-account"></a><span data-ttu-id="0cc3e-106">A tárfiók saját sablon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0cc3e-106">Add private template to storage account</span></span>

<span data-ttu-id="0cc3e-107">A sablonok hozzáadása egy tárfiókot, és hivatkozzon őket egy SAS-jogkivonat a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="0cc3e-107">You can add your templates to a storage account and link to them during deployment with a SAS token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0cc3e-108">Az alábbi lépéseket követve sablont tartalmazó blob elérhető, csak a fiók tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="0cc3e-108">By following the steps below, the blob containing the template is accessible to only the account owner.</span></span> <span data-ttu-id="0cc3e-109">Amikor létrehoz egy SAS-jogkivonat a blob, a blob azonban, hogy az URI-azonosítójú bárki hozzáférhet.</span><span class="sxs-lookup"><span data-stu-id="0cc3e-109">However, when you create a SAS token for the blob, the blob is accessible to anyone with that URI.</span></span> <span data-ttu-id="0cc3e-110">Ha egy másik felhasználó elfogja az URI, a felhasználó hozzáférjen a sablon lesz.</span><span class="sxs-lookup"><span data-stu-id="0cc3e-110">If another user intercepts the URI, that user is able to access the template.</span></span> <span data-ttu-id="0cc3e-111">A SAS-jogkivonat használatával egy jó módszer a sablonok való hozzáférés korlátozása, de kell nem tartalmazhat bizalmas adatok, például jelszavak közvetlenül a sablonban.</span><span class="sxs-lookup"><span data-stu-id="0cc3e-111">Using a SAS token is a good way of limiting access to your templates, but you should not include sensitive data like passwords directly in the template.</span></span>
> 
> 

<span data-ttu-id="0cc3e-112">Az alábbi példa állít be egy személyes fiók tárolót, és feltölti a sablont:</span><span class="sxs-lookup"><span data-stu-id="0cc3e-112">The following example sets up a private storage account container and uploads a template:</span></span>
   
```azurecli
az group create --name "ManageGroup" --location "South Central US"
az storage account create \
    --resource-group ManageGroup \
    --location "South Central US" \
    --sku Standard_LRS \
    --kind Storage \
    --name {your-unique-name}
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
az storage container create \
    --name templates \
    --public-access Off \
    --connection-string $connection
az storage blob upload \
    --container-name templates \
    --file vmlinux.json \
    --name vmlinux.json \
    --connection-string $connection
```

### <a name="provide-sas-token-during-deployment"></a><span data-ttu-id="0cc3e-113">Adja meg a SAS-jogkivonat központi telepítése során</span><span class="sxs-lookup"><span data-stu-id="0cc3e-113">Provide SAS token during deployment</span></span>
<span data-ttu-id="0cc3e-114">A tárfiókban lévő titkos sablon telepítése, a SAS-token létrehozásához, és adja hozzá a sablon az URI azonosító.</span><span class="sxs-lookup"><span data-stu-id="0cc3e-114">To deploy a private template in a storage account, generate a SAS token and include it in the URI for the template.</span></span> <span data-ttu-id="0cc3e-115">Állítsa be a lejárati időpont hagyjon elegendő időt a telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="0cc3e-115">Set the expiry time to allow enough time to complete the deployment.</span></span>
   
```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
token=$(az storage blob generate-sas \
    --container-name templates \
    --name vmlinux.json \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name vmlinux.json \
    --output tsv \
    --connection-string $connection)
az group deployment create --resource-group ExampleGroup --template-uri $url?$token
```

<span data-ttu-id="0cc3e-116">Például egy SAS-jogkivonat használatával a csatolt sablonokkal, [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="0cc3e-116">For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0cc3e-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0cc3e-117">Next steps</span></span>
* <span data-ttu-id="0cc3e-118">Megismerkedhet a sablonok központi telepítése, lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="0cc3e-118">For an introduction to deploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy-cli.md).</span></span>
* <span data-ttu-id="0cc3e-119">Egy teljes parancsfájlt, amely telepít egy sablon, lásd: [sablonparancsfájlt erőforrás-kezelő telepítése](resource-manager-samples-cli-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="0cc3e-119">For a complete sample script that deploys a template, see [Deploy Resource Manager template script](resource-manager-samples-cli-deploy.md)</span></span>
* <span data-ttu-id="0cc3e-120">Sablon paraméterek megadásához tekintse meg a [sablonok készítése](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="0cc3e-120">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="0cc3e-121">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="0cc3e-121">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
