---
title: "aaaDeploy SAS-jogkivonat és az Azure CLI Azure-sablonnal |} Microsoft Docs"
description: "Azure Resource Manager és az Azure parancssori felület toodeploy erőforrások tooAzure védett sablonból SAS-jogkivonatot használja."
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
ms.openlocfilehash: 59c64616d6e1f5e456d88a72854d0ed99e1bdc0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-cli"></a><span data-ttu-id="d7bb8-103">Az SAS-jogkivonat és az Azure parancssori felület titkos Resource Manager-sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="d7bb8-103">Deploy private Resource Manager template with SAS token and Azure CLI</span></span>

<span data-ttu-id="d7bb8-104">A sablon olyan tárfiókban helyezkedik el, ha korlátozni a hozzáférést toohello sablon, és adjon meg egy közös hozzáférésű jogosultságkód (SAS) token üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="d7bb8-104">When your template resides in a storage account, you can restrict access toohello template and provide a shared access signature (SAS) token during deployment.</span></span> <span data-ttu-id="d7bb8-105">Ez a témakör azt ismerteti, hogyan Resource Manager sablonok tooprovide egy SAS-jogkivonat üzembe helyezése során az Azure PowerShell toouse.</span><span class="sxs-lookup"><span data-stu-id="d7bb8-105">This topic explains how toouse Azure PowerShell with Resource Manager templates tooprovide a SAS token during deployment.</span></span> 

## <a name="add-private-template-toostorage-account"></a><span data-ttu-id="d7bb8-106">Saját sablon toostorage fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d7bb8-106">Add private template toostorage account</span></span>

<span data-ttu-id="d7bb8-107">A sablonok tooa tárfiók hozzáadása, és toothem csatolása a SAS-jogkivonat a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="d7bb8-107">You can add your templates tooa storage account and link toothem during deployment with a SAS token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d7bb8-108">Hello alábbi lépéseket követve hello sablon tartalmazó hello blob elérhető tooonly hello fiók tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="d7bb8-108">By following hello steps below, hello blob containing hello template is accessible tooonly hello account owner.</span></span> <span data-ttu-id="d7bb8-109">Amikor létrehoz egy SAS-jogkivonat hello blob, hello blob azonban, hogy az URI-azonosítójú elérhető tooanyone.</span><span class="sxs-lookup"><span data-stu-id="d7bb8-109">However, when you create a SAS token for hello blob, hello blob is accessible tooanyone with that URI.</span></span> <span data-ttu-id="d7bb8-110">Ha egy másik felhasználó elfogja hello URI, a felhasználó képes tooaccess hello sablont.</span><span class="sxs-lookup"><span data-stu-id="d7bb8-110">If another user intercepts hello URI, that user is able tooaccess hello template.</span></span> <span data-ttu-id="d7bb8-111">SAS-token használatával korlátozza a hozzáférést tooyour sablonok egy jó módszer, de kell nem tartalmazhat bizalmas adatok, például jelszavak közvetlenül hello sablonban.</span><span class="sxs-lookup"><span data-stu-id="d7bb8-111">Using a SAS token is a good way of limiting access tooyour templates, but you should not include sensitive data like passwords directly in hello template.</span></span>
> 
> 

<span data-ttu-id="d7bb8-112">hello alábbi példa egy személyes fiók tároló beállítja és feltölt egy sablont:</span><span class="sxs-lookup"><span data-stu-id="d7bb8-112">hello following example sets up a private storage account container and uploads a template:</span></span>
   
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

### <a name="provide-sas-token-during-deployment"></a><span data-ttu-id="d7bb8-113">Adja meg a SAS-jogkivonat központi telepítése során</span><span class="sxs-lookup"><span data-stu-id="d7bb8-113">Provide SAS token during deployment</span></span>
<span data-ttu-id="d7bb8-114">a storage-fiók titkos sablont toodeploy a SAS-token létrehozásához, és adja hozzá hello URI hello sablon.</span><span class="sxs-lookup"><span data-stu-id="d7bb8-114">toodeploy a private template in a storage account, generate a SAS token and include it in hello URI for hello template.</span></span> <span data-ttu-id="d7bb8-115">Hello lejárati idő tooallow elég toocomplete hello üzembe helyezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="d7bb8-115">Set hello expiry time tooallow enough time toocomplete hello deployment.</span></span>
   
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

<span data-ttu-id="d7bb8-116">Például egy SAS-jogkivonat használatával a csatolt sablonokkal, [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d7bb8-116">For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7bb8-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7bb8-117">Next steps</span></span>
* <span data-ttu-id="d7bb8-118">Egy bevezető toodeploying sablonok, lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="d7bb8-118">For an introduction toodeploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy-cli.md).</span></span>
* <span data-ttu-id="d7bb8-119">Egy teljes parancsfájlt, amely telepít egy sablon, lásd: [sablonparancsfájlt erőforrás-kezelő telepítése](resource-manager-samples-cli-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="d7bb8-119">For a complete sample script that deploys a template, see [Deploy Resource Manager template script](resource-manager-samples-cli-deploy.md)</span></span>
* <span data-ttu-id="d7bb8-120">a sablon toodefine paraméterek lásd [sablonok készítése](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="d7bb8-120">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="d7bb8-121">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="d7bb8-121">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
