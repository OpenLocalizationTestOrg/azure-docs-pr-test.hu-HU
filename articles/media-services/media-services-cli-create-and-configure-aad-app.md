---
title: "az Azure AD alkalmazás aaaUse CLI 2.0 toocreate és állítsa be az Azure Media Services API tooaccess |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toouse CLI 2.0 toocreate egy Azure AD alkalmazás és az Azure Media Services API tooaccess konfigurálja."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: c865e2701722374b5dd17b0e20fa848c07065006
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-20-toocreate-an-aad-app-and-configure-it-tooaccess-azure-media-services-api"></a><span data-ttu-id="13619-103">CLI 2.0 toocreate egy AAD-alkalmazást használja, és konfigurálja az Azure Media Services API tooaccess</span><span class="sxs-lookup"><span data-stu-id="13619-103">Use CLI 2.0 toocreate an AAD app and configure it tooaccess Azure Media Services API</span></span>

<span data-ttu-id="13619-104">Ez a témakör bemutatja, hogyan toouse CLI 2.0 toocreate egy Azure Active Directory (Azure AD) alkalmazás és szolgáltatás egyszerű tooaccess Azure Media Services erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="13619-104">This topic shows you how toouse CLI 2.0 toocreate an Azure Active Directory (Azure AD) application and service principal tooaccess Azure Media Services resources.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="13619-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="13619-105">Prerequisites</span></span>

- <span data-ttu-id="13619-106">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="13619-106">An Azure account.</span></span> <span data-ttu-id="13619-107">További részletek: [Ingyenes Azure-próbafiók](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="13619-107">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="13619-108">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="13619-108">A Media Services account.</span></span> <span data-ttu-id="13619-109">További információkért lásd: [hello Azure-portál használatával Azure Media Services-fiók létrehozása](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="13619-109">For more information, see [Create an Azure Media Services account using hello Azure portal](media-services-portal-create-account.md).</span></span>

## <a name="use-hello-azure-cloud-shell"></a><span data-ttu-id="13619-110">Hello Azure Cloud rendszerhéj használata</span><span class="sxs-lookup"><span data-stu-id="13619-110">Use hello Azure Cloud Shell</span></span>

1. <span data-ttu-id="13619-111">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="13619-111">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="13619-112">Indítsa el a felhő rendszerhéj hello hello portál hello felső navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="13619-112">Launch hello Cloud Shell from hello upper navigation pane of hello portal.</span></span>

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

<span data-ttu-id="13619-114">További információkért lásd: [áttekintés az Azure felhőalapú rendszerhéj](../cloud-shell/overview.md).</span><span class="sxs-lookup"><span data-stu-id="13619-114">For more information, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md).</span></span>

## <a name="create-an-azure-ad-app-and-configure-access-toohello-media-account-with-cli-20"></a><span data-ttu-id="13619-115">Az Azure AD-alkalmazás létrehozása és CLI 2.0 hozzáférési toohello media fiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="13619-115">Create an Azure AD app and configure access toohello media account with CLI 2.0</span></span>
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create -- assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

<span data-ttu-id="13619-116">Példa:</span><span class="sxs-lookup"><span data-stu-id="13619-116">For example:</span></span>

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

<span data-ttu-id="13619-117">Ebben a példában hello **hatókör** van hello teljes erőforrás elérési útja hello Media services-fiók.</span><span class="sxs-lookup"><span data-stu-id="13619-117">In this example, hello **scope** is hello full resource path for hello media services account.</span></span> <span data-ttu-id="13619-118">Azonban hello **hatókör** bármely szinten lehet.</span><span class="sxs-lookup"><span data-stu-id="13619-118">However, hello **scope** can be at any level.</span></span>

<span data-ttu-id="13619-119">Például lehet hello következő szintek egyikét:</span><span class="sxs-lookup"><span data-stu-id="13619-119">For example, it could be one of hello following levels:</span></span>
 
* <span data-ttu-id="13619-120">Hello **előfizetés** szintjét.</span><span class="sxs-lookup"><span data-stu-id="13619-120">hello **subscription** level.</span></span>
* <span data-ttu-id="13619-121">Hello **erőforráscsoport** szintjét.</span><span class="sxs-lookup"><span data-stu-id="13619-121">hello **resource group** level.</span></span>
* <span data-ttu-id="13619-122">Hello **erőforrás** szint (például egy adathordozó fiókot).</span><span class="sxs-lookup"><span data-stu-id="13619-122">hello **resource** level (for example, a Media account).</span></span>

<span data-ttu-id="13619-123">További információkért lásd: [hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="13619-123">For more information, see [Create an Azure service principal with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)</span></span>

<span data-ttu-id="13619-124">Lásd még: [Manage Role-Based hozzáférés-vezérlés hello Azure parancssori felület](../active-directory/role-based-access-control-manage-access-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="13619-124">Also see [Manage Role-Based Access Control with hello Azure command-line interface](../active-directory/role-based-access-control-manage-access-azure-cli.md).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="13619-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13619-125">Next steps</span></span>

<span data-ttu-id="13619-126">Ismerkedés a [fájlok feltöltése a tooyour fiók](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="13619-126">Get started with [uploading files tooyour account](media-services-portal-upload-files.md).</span></span>
