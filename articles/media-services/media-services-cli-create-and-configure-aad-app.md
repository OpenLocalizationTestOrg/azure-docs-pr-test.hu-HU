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
# <a name="use-cli-20-toocreate-an-aad-app-and-configure-it-tooaccess-azure-media-services-api"></a>CLI 2.0 toocreate egy AAD-alkalmazást használja, és konfigurálja az Azure Media Services API tooaccess

Ez a témakör bemutatja, hogyan toouse CLI 2.0 toocreate egy Azure Active Directory (Azure AD) alkalmazás és szolgáltatás egyszerű tooaccess Azure Media Services erőforrásokat. 

## <a name="prerequisites"></a>Előfeltételek

- Egy Azure-fiók. További részletek: [Ingyenes Azure-próbafiók](https://azure.microsoft.com/pricing/free-trial/). 
- Egy Media Services-fiók. További információkért lásd: [hello Azure-portál használatával Azure Media Services-fiók létrehozása](media-services-portal-create-account.md).

## <a name="use-hello-azure-cloud-shell"></a>Hello Azure Cloud rendszerhéj használata

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Indítsa el a felhő rendszerhéj hello hello portál hello felső navigációs ablaktáblán.

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

További információkért lásd: [áttekintés az Azure felhőalapú rendszerhéj](../cloud-shell/overview.md).

## <a name="create-an-azure-ad-app-and-configure-access-toohello-media-account-with-cli-20"></a>Az Azure AD-alkalmazás létrehozása és CLI 2.0 hozzáférési toohello media fiók konfigurálása
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create -- assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

Példa:

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

Ebben a példában hello **hatókör** van hello teljes erőforrás elérési útja hello Media services-fiók. Azonban hello **hatókör** bármely szinten lehet.

Például lehet hello következő szintek egyikét:
 
* Hello **előfizetés** szintjét.
* Hello **erőforráscsoport** szintjét.
* Hello **erőforrás** szint (például egy adathordozó fiókot).

További információkért lásd: [hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)

Lásd még: [Manage Role-Based hozzáférés-vezérlés hello Azure parancssori felület](../active-directory/role-based-access-control-manage-access-azure-cli.md). 

## <a name="next-steps"></a>Következő lépések

Ismerkedés a [fájlok feltöltése a tooyour fiók](media-services-portal-upload-files.md).
