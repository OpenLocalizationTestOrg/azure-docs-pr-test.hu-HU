---
title: "az IoT-központ Azure CLI-vel (az.py) aaaCreate |} Microsoft Docs"
description: "Hogyan egy Azure IoT hub használatával toocreate hello platformfüggetlen Azure CLI 2.0 (az.py)."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 9c9639235c2ac343e6ceb9578291dafaea26ea24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-cli-20"></a><span data-ttu-id="bd54e-103">Létrehoz egy IoT-központot hello Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="bd54e-103">Create an IoT hub using hello Azure CLI 2.0</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="bd54e-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="bd54e-104">Introduction</span></span>

<span data-ttu-id="bd54e-105">Használhatja az Azure CLI 2.0 (az.py) toocreate és Azure IoT-központok programozott kezelését.</span><span class="sxs-lookup"><span data-stu-id="bd54e-105">You can use Azure CLI 2.0 (az.py) toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="bd54e-106">Ez a cikk bemutatja, hogyan toouse hello Azure CLI 2.0 (az.py) toocreate egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="bd54e-106">This article shows you how toouse hello Azure CLI 2.0 (az.py) toocreate an IoT hub.</span></span>

<span data-ttu-id="bd54e-107">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="bd54e-107">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="bd54e-108">[Az Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md) – hello CLI hello klasszikus és resource management üzembe helyezési modellek.</span><span class="sxs-lookup"><span data-stu-id="bd54e-108">[Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md) – hello CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="bd54e-109">Az Azure CLI 2.0 (az.py) – hello következő generációs CLI hello erőforrás felügyeleti telepítési modell ebben a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="bd54e-109">Azure CLI 2.0 (az.py) - hello next generation CLI for hello resource management deployment model as described in this article.</span></span>

<span data-ttu-id="bd54e-110">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="bd54e-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="bd54e-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="bd54e-111">An active Azure account.</span></span> <span data-ttu-id="bd54e-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="bd54e-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="bd54e-113">[Az Azure CLI 2.0][lnk-CLI-install].</span><span class="sxs-lookup"><span data-stu-id="bd54e-113">[Azure CLI 2.0][lnk-CLI-install].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="bd54e-114">Jelentkezzen be, és állítsa be az Azure-fiókjával</span><span class="sxs-lookup"><span data-stu-id="bd54e-114">Sign in and set your Azure account</span></span>

<span data-ttu-id="bd54e-115">Jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="bd54e-115">Sign in tooyour Azure account and select your subscription.</span></span>

1. <span data-ttu-id="bd54e-116">Hello parancssorban futtassa a hello [bejelentkezési parancs][lnk-login-command]:</span><span class="sxs-lookup"><span data-stu-id="bd54e-116">At hello command prompt, run hello [login command][lnk-login-command]:</span></span>
    
    ```azurecli
    az login
    ```

    <span data-ttu-id="bd54e-117">Hajtsa végre a hello utasításokat tooauthenticate hello kód használatával, és jelentkezzen be Azure-fiók egy webböngészőn keresztül tooyour.</span><span class="sxs-lookup"><span data-stu-id="bd54e-117">Follow hello instructions tooauthenticate using hello code and sign in tooyour Azure account through a web browser.</span></span>

2. <span data-ttu-id="bd54e-118">Ha több Azure-előfizetéssel rendelkezik, birtokában tooall hozzáférhet tooAzure bejelentkezés hello a hitelesítő adatok társított Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="bd54e-118">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure accounts associated with your credentials.</span></span> <span data-ttu-id="bd54e-119">Hello következő [parancs toolist hello Azure-fiókra] [ lnk-az-account-command] meg toouse érhető el:</span><span class="sxs-lookup"><span data-stu-id="bd54e-119">Use hello following [command toolist hello Azure accounts][lnk-az-account-command] available for you toouse:</span></span>
    
    ```azurecli
    az account list 
    ```

    <span data-ttu-id="bd54e-120">A következő parancs tooselect előfizetést, amelyet toouse toorun hello parancsok toocreate az IoT hub hello használata.</span><span class="sxs-lookup"><span data-stu-id="bd54e-120">Use hello following command tooselect subscription that you want toouse toorun hello commands toocreate your IoT hub.</span></span> <span data-ttu-id="bd54e-121">Hello előfizetés név vagy azonosító hello kimenetből hello előző parancs használható:</span><span class="sxs-lookup"><span data-stu-id="bd54e-121">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a><span data-ttu-id="bd54e-122">IoT Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd54e-122">Create an IoT Hub</span></span>

<span data-ttu-id="bd54e-123">Hello Azure CLI toocreate erőforráscsoport használja, és adja hozzá az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="bd54e-123">Use hello Azure CLI toocreate a resource group and then add an IoT hub.</span></span>

1. <span data-ttu-id="bd54e-124">Amikor létrehoz egy IoT-központot, erőforráscsoportban kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="bd54e-124">When you create an IoT hub, you must create it in a resource group.</span></span> <span data-ttu-id="bd54e-125">Használjon egy meglévő erőforráscsoportot, vagy futtassa a következő hello [parancs toocreate erőforráscsoport][lnk-az-resource-command]:</span><span class="sxs-lookup"><span data-stu-id="bd54e-125">Either use an existing resource group, or run hello following [command toocreate a resource group][lnk-az-resource-command]:</span></span>
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > <span data-ttu-id="bd54e-126">hello előző példa hello erőforráscsoport hello USA nyugati régiója helyet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bd54e-126">hello previous example creates hello resource group in hello West US location.</span></span> <span data-ttu-id="bd54e-127">Hello parancs futtatásával megtekintheti a rendelkezésre álló helyek listáját `az account list-locations -o table`.</span><span class="sxs-lookup"><span data-stu-id="bd54e-127">You can view a list of available locations by running hello command `az account list-locations -o table`.</span></span>
    >
    >

2. <span data-ttu-id="bd54e-128">Futtassa a következő hello [parancs toocreate az IoT-központ] [ lnk-az-iot-command] az erőforráscsoportban, globálisan egyedi névvel az IoT hub:</span><span class="sxs-lookup"><span data-stu-id="bd54e-128">Run hello following [command toocreate an IoT hub][lnk-az-iot-command] in your resource group, using a globally unique name for your IoT hub:</span></span>
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> <span data-ttu-id="bd54e-129">előző parancs hello az IoT-központ IP-címek, amelynek számlázása hello S1 hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bd54e-129">hello previous command creates an IoT hub in hello S1 pricing tier for which you are billed.</span></span> <span data-ttu-id="bd54e-130">További információkért lásd: [Azure IoT Hub árképzési][lnk-iot-pricing].</span><span class="sxs-lookup"><span data-stu-id="bd54e-130">For more information, see [Azure IoT Hub pricing][lnk-iot-pricing].</span></span>
>
>

## <a name="remove-an-iot-hub"></a><span data-ttu-id="bd54e-131">Távolítsa el az IoT-központ</span><span class="sxs-lookup"><span data-stu-id="bd54e-131">Remove an IoT Hub</span></span>

<span data-ttu-id="bd54e-132">Használhatja az Azure parancssori felület hello túl[egyedi erőforrás törlése][lnk-az-resource-command], például egy IoT-központ, vagy törölje az erőforráscsoportot és az ahhoz tartozó összes erőforrást, beleértve az IoT-központok.</span><span class="sxs-lookup"><span data-stu-id="bd54e-132">You can use hello Azure CLI too[delete an individual resource][lnk-az-resource-command], such as an IoT hub, or delete a resource group and all its resources, including any IoT hubs.</span></span>

<span data-ttu-id="bd54e-133">az IoT-központ toodelete hello a következő parancsot futtassa:</span><span class="sxs-lookup"><span data-stu-id="bd54e-133">toodelete an IoT hub, run hello following command:</span></span>

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

<span data-ttu-id="bd54e-134">toodelete az erőforráscsoportot és a hozzá tartozó összes erőforrásnak, a következő futtatási hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="bd54e-134">toodelete a resource group and all its resources, run hello following command:</span></span>

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a><span data-ttu-id="bd54e-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bd54e-135">Next steps</span></span>
<span data-ttu-id="bd54e-136">toolearn IoT-központot, fejlesztésével kapcsolatos további hello a következő cikkekben talál:</span><span class="sxs-lookup"><span data-stu-id="bd54e-136">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="bd54e-137">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="bd54e-137">[IoT Hub developer guide][lnk-devguide]</span></span>

<span data-ttu-id="bd54e-138">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="bd54e-138">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="bd54e-139">[Az Azure portál toomanage IoT-központ hello használata][lnk-portal]</span><span class="sxs-lookup"><span data-stu-id="bd54e-139">[Using hello Azure portal toomanage IoT Hub][lnk-portal]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 
