---
title: "egy Azure aaaConsume felügyelt alkalmazás |} Microsoft Docs"
description: "Ismerteti, hogyan ügyfél hoz létre az Azure által felügyelt alkalmazás közzétett fájlokból."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: b8510086eb05304c0e351a391b7e0cf34a467568
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-internal-managed-application"></a><span data-ttu-id="3dd5b-103">Belső felügyelt alkalmazás felhasználása</span><span class="sxs-lookup"><span data-stu-id="3dd5b-103">Consume an internal managed application</span></span>

<span data-ttu-id="3dd5b-104">Azure felhasználhat [kezelt alkalmazások](managed-application-overview.md) szolgálnak, hogy a szervezet tagjaira.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-104">You can consume Azure [managed applications](managed-application-overview.md) that are intended for members of your organization.</span></span> <span data-ttu-id="3dd5b-105">Kiválaszthatja például a kezelt alkalmazások az informatikai részleg, amely a vállalati szabványoknak való megfelelés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-105">For example, you can select managed applications from your IT department that ensure compliance with organizational standards.</span></span> <span data-ttu-id="3dd5b-106">A kezelt alkalmazások hello szolgáltatáskatalógus, nem hello Azure piactéren keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-106">These managed applications are available through hello Service Catalog, not hello Azure Marketplace.</span></span>

<span data-ttu-id="3dd5b-107">Ez a cikk folytatása előtt az előfizetéshez tartozó hello szolgáltatáskatalógus elérhető kezelt alkalmazás kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-107">Before proceeding with this article, you must have a managed application available in hello service catalog for your subscription.</span></span> <span data-ttu-id="3dd5b-108">Ha valaki van a szervezet rendelkezik nem hozott létre egy felügyelt alkalmazást, lásd: [– belső használat a kezelt alkalmazás közzététele](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="3dd5b-108">If someone in your organization has not already created a managed application, see [Publish a managed application for internal consumption](managed-application-publishing.md).</span></span>

<span data-ttu-id="3dd5b-109">Jelenleg az Azure parancssori felület vagy hello Azure portál tooconsume egy felügyelt alkalmazást is használhatja.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-109">Currently, you can use either Azure CLI or hello Azure portal tooconsume a managed application.</span></span>

## <a name="create-hello-managed-application-by-using-hello-portal"></a><span data-ttu-id="3dd5b-110">Hello portál használatával felügyelt hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3dd5b-110">Create hello managed application by using hello portal</span></span>

<span data-ttu-id="3dd5b-111">kezelt alkalmazás hello portálon keresztül toodeploy kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3dd5b-111">toodeploy a managed application through hello portal, follow these steps:</span></span>

1. <span data-ttu-id="3dd5b-112">Nyissa meg az Azure portál toohello.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-112">Go toohello Azure portal.</span></span> <span data-ttu-id="3dd5b-113">Keresse meg **szolgáltatáskatalógus felügyelt alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-113">Search for **Service Catalog Managed Application**.</span></span>

   ![Szolgáltatáskatalógus felügyelt alkalmazás](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. <span data-ttu-id="3dd5b-115">Jelölje be hello elérhető megoldások listájából hello toocreate kívánt alkalmazás kezeli.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-115">Select hello managed application you want toocreate from hello list of available solutions.</span></span> <span data-ttu-id="3dd5b-116">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-116">Select **Create**.</span></span>

   ![Kezelt alkalmazás kiválasztása](./media/managed-application-consumption/select-offer.png)

1. <span data-ttu-id="3dd5b-118">Adja meg, amely szükséges tooprovision hello erőforrás hello paraméterek értékét.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-118">Provide values for hello parameters that are required tooprovision hello resources.</span></span> <span data-ttu-id="3dd5b-119">Válassza ki **nyugati középső Régiójában** helyéhez.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-119">Select **West Central US** for location.</span></span> <span data-ttu-id="3dd5b-120">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-120">Select **OK**.</span></span>

   ![A felügyelt alkalmazási paraméterek](./media/managed-application-consumption/input-parameters.png)

1. <span data-ttu-id="3dd5b-122">hello sablon hello értékektől ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-122">hello template validates hello values you provided.</span></span> <span data-ttu-id="3dd5b-123">Ha az érvényesítés sikeres, válassza ki a **OK** toostart hello központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-123">If validation succeeds, select **OK** toostart hello deployment.</span></span>

   ![A felügyelt alkalmazási érvényesítése](./media/managed-application-consumption/validation.png)

<span data-ttu-id="3dd5b-125">Hello központi telepítés befejezése után hello megfelelő erőforrásokkal hello sablonban meghatározott hello felügyelt erőforráscsoportban megadott törlődnek.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-125">After hello deployment finishes, hello appropriate resources defined in hello template are provisioned in hello managed resource group you provided.</span></span>

## <a name="create-hello-managed-application-by-using-azure-cli"></a><span data-ttu-id="3dd5b-126">Hello felügyelt alkalmazás létrehozása az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="3dd5b-126">Create hello managed application by using Azure CLI</span></span>

<span data-ttu-id="3dd5b-127">Nincsenek két módon toocreate kezelt alkalmazás Azure parancssori felület használatával:</span><span class="sxs-lookup"><span data-stu-id="3dd5b-127">There are two ways toocreate a managed application by using Azure CLI:</span></span>

* <span data-ttu-id="3dd5b-128">Hello paranccsal kezelt alkalmazások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-128">Use hello command for creating managed applications.</span></span>
* <span data-ttu-id="3dd5b-129">Hello rendszeres sablon telepítési paranccsal.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-129">Use hello regular template deployment command.</span></span>

### <a name="use-hello-template-deployment-command"></a><span data-ttu-id="3dd5b-130">Hello sablon telepítési parancs</span><span class="sxs-lookup"><span data-stu-id="3dd5b-130">Use hello template deployment command</span></span>

<span data-ttu-id="3dd5b-131">Hello applianceMainTemplate.json fájl, amely létre szállító hello telepítése.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-131">Deploy hello applianceMainTemplate.json file that hello vendor created.</span></span>

<span data-ttu-id="3dd5b-132">Ezután hozzon létre két erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-132">Then create two resource groups.</span></span> <span data-ttu-id="3dd5b-133">hello első erőforráscsoport egy hello kezelt alkalmazás erőforrás létrehozási helyének: Microsoft.Solutions/appliances.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-133">hello first resource group is where hello managed application resource is created: Microsoft.Solutions/appliances.</span></span> <span data-ttu-id="3dd5b-134">hello második erőforráscsoport összes hello erőforrásai mainTemplate.json definiálva.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-134">hello second resource group contains all hello resources defined in mainTemplate.json.</span></span> <span data-ttu-id="3dd5b-135">Ez az erőforráscsoport hello ISV kezeli.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-135">This resource group is managed by hello ISV.</span></span>

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="3dd5b-136">Használjon `westcentralus` hello erőforráscsoport hello helyként.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-136">Use `westcentralus` as hello location of hello resource group.</span></span>
>

<span data-ttu-id="3dd5b-137">a mainResourceGroup, a következő parancs használata hello applianceMainTemplate.json toodeploy:</span><span class="sxs-lookup"><span data-stu-id="3dd5b-137">toodeploy applianceMainTemplate.json in mainResourceGroup, use hello following command:</span></span>

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

<span data-ttu-id="3dd5b-138">Hello megelőző sablon fut, után megkérdezi hello hello sablonban meghatározott hello paraméterek értékeit.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-138">After hello preceding template runs, it prompts you for hello values of hello parameters that are defined in hello template.</span></span> <span data-ttu-id="3dd5b-139">Ezenkívül toohello paramétereket szükséges tooprovision erőforrások sablonban, két fő paraméterértékek van szüksége:</span><span class="sxs-lookup"><span data-stu-id="3dd5b-139">In addition toohello parameters that are needed tooprovision resources in a template, you need two key parameter values:</span></span>

- <span data-ttu-id="3dd5b-140">**managedResourceGroupId**: hello hello erőforrás csoportot tartalmazó hello erőforrások applianceMainTemplate.json definiált azonosítója.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-140">**managedResourceGroupId**: hello ID of hello resource group containing hello resources defined in applianceMainTemplate.json.</span></span> <span data-ttu-id="3dd5b-141">hello azonosító: hello űrlap `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-141">hello ID is of hello form `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`.</span></span> <span data-ttu-id="3dd5b-142">A fenti példa hello, hello azonosítója tartozó `managedResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-142">In hello preceding example, it's hello ID of `managedResourceGroup`.</span></span>
- <span data-ttu-id="3dd5b-143">**applianceDefinitionId**: hello hello azonosítója felügyelt alkalmazás erőforrás-definícióban.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-143">**applianceDefinitionId**: hello ID of hello managed application definition resource.</span></span> <span data-ttu-id="3dd5b-144">Ez az érték hello ISV biztosítja.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-144">This value is provided by hello ISV.</span></span>

> [!NOTE]
> <span data-ttu-id="3dd5b-145">hello publisher hozzáférés toohello erőforráscsoport hello felügyelt alkalmazás definícióját tartalmazó kell biztosítania.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-145">hello publisher must grant access toohello resource group that contains hello managed application definition.</span></span> <span data-ttu-id="3dd5b-146">hello definition erőforrás hello publisher előfizetés jön létre.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-146">hello definition resource is created in hello publisher subscription.</span></span> <span data-ttu-id="3dd5b-147">Ezért egy felhasználó, a felhasználói csoport vagy az alkalmazás ügyfél-bérlőben hello szükséges olvasási hozzáférés toothis erőforrás.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-147">Therefore, a user, user group, or application in hello customer tenant needs read access toothis resource.</span></span>

<span data-ttu-id="3dd5b-148">Hello központi telepítés sikeres befejezése után megjelenik a felügyelt hello alkalmazás mainResourceGroup jön létre.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-148">After hello deployment finishes successfully, you see hello managed application is created in mainResourceGroup.</span></span> <span data-ttu-id="3dd5b-149">hello storageAccount erőforrás managedResourceGroup jön létre.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-149">hello storageAccount resource is created in managedResourceGroup.</span></span>

### <a name="use-hello-create-command"></a><span data-ttu-id="3dd5b-150">A create parancs használata hello</span><span class="sxs-lookup"><span data-stu-id="3dd5b-150">Use hello create command</span></span>

<span data-ttu-id="3dd5b-151">Használhatja a hello `az managedapp create` parancs toocreate hello a kezelt alkalmazás felügyelt alkalmazás definícióját.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-151">You can use hello `az managedapp create` command toocreate a managed application from hello managed application definition.</span></span>

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* <span data-ttu-id="3dd5b-152">**készülék Meghatározásazonosítóra**: hello erőforrás-azonosítója, hello által felügyelt hello az előző lépésben létrehozott alkalmazás-definíciót.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-152">**appliance-definition-Id**: hello resource ID of hello managed application definition created in hello preceding step.</span></span> <span data-ttu-id="3dd5b-153">tooobtain ezt az Azonosítót, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="3dd5b-153">tooobtain this ID, run hello following command:</span></span>

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  <span data-ttu-id="3dd5b-154">Ez a parancs visszaadja a felügyelt hello definíciót.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-154">This command returns hello managed application definition.</span></span> <span data-ttu-id="3dd5b-155">Hello azonosító tulajdonság értékének hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-155">You need hello value of hello ID property.</span></span>

* <span data-ttu-id="3dd5b-156">**felügyelt rg-azonosító**: hello erőforrás csoportot tartalmazó hello erőforrások applianceMainTemplate.json definiált hello nevét.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-156">**managed-rg-id**: hello name of hello resource group containing hello resources defined in applianceMainTemplate.json.</span></span> <span data-ttu-id="3dd5b-157">Ez az erőforráscsoport hello felügyelt erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-157">This resource group is hello managed resource group.</span></span> <span data-ttu-id="3dd5b-158">Hello publisher kezeli.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-158">It's managed by hello publisher.</span></span> <span data-ttu-id="3dd5b-159">Ha még nem létezik, az Ön létrejön.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-159">If it doesn't exist, it's created for you.</span></span>
* <span data-ttu-id="3dd5b-160">**Erőforráscsoport**: ahol hello felügyelt alkalmazás erőforrás hello erőforráscsoportban jön létre.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-160">**resource-group**: hello resource group where hello managed application resource is created.</span></span> <span data-ttu-id="3dd5b-161">Ez az erőforráscsoport él hello Microsoft.Solutions/appliance erőforrás.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-161">hello Microsoft.Solutions/appliance resource lives in this resource group.</span></span>
* <span data-ttu-id="3dd5b-162">**paraméterek**: hello applianceMainTemplate.json definiált hello erőforrások szükséges paraméterek.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-162">**parameters**: hello parameters that are needed for hello resources defined in applianceMainTemplate.json.</span></span>

## <a name="known-issues"></a><span data-ttu-id="3dd5b-163">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="3dd5b-163">Known issues</span></span>

<span data-ttu-id="3dd5b-164">Az előzetes kiadás tartalmazza a következő problémák hello:</span><span class="sxs-lookup"><span data-stu-id="3dd5b-164">This preview release includes hello following issues:</span></span>

* <span data-ttu-id="3dd5b-165">500 belső kiszolgálóhiba hello felügyelt alkalmazás hello létrehozása során jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-165">A 500 internal server error appears during hello creation of hello managed application.</span></span> <span data-ttu-id="3dd5b-166">Ha ezt a problémát tapasztal, valószínűleg átmeneti toobe is.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-166">If you run into this issue, it's likely toobe intermittent.</span></span> <span data-ttu-id="3dd5b-167">Ismételje meg a hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-167">Retry hello operation.</span></span>
* <span data-ttu-id="3dd5b-168">Új erőforráscsoport hello felügyelt erőforráscsoportban van szükség.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-168">A new resource group is needed for hello managed resource group.</span></span> <span data-ttu-id="3dd5b-169">Ha egy meglévő erőforráscsoportot használ, hello telepítése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-169">If you use an existing resource group, hello deployment fails.</span></span>
* <span data-ttu-id="3dd5b-170">hello hello Microsoft.Solutions/appliances erőforrás tartalmazó erőforráscsoportot kell létrehozni hello **westcentralus** helyét.</span><span class="sxs-lookup"><span data-stu-id="3dd5b-170">hello resource group that contains hello Microsoft.Solutions/appliances resource must be created in hello **westcentralus** location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3dd5b-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3dd5b-171">Next steps</span></span>

* <span data-ttu-id="3dd5b-172">Egy bevezető toomanaged alkalmazások, lásd: [felügyelt használatát áttekintő cikkben](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3dd5b-172">For an introduction toomanaged applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="3dd5b-173">További információ a szolgáltatáskatalógus kezelt alkalmazás közzététele: [létrehozása és a szolgáltatáskatalógus kezelt alkalmazás közzététele](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="3dd5b-173">For information about publishing a Service Catalog managed application, see [Create and publish a Service Catalog managed application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="3dd5b-174">Közzétételi kezelt alkalmazások toohello Azure piactér kapcsolatos információkért lásd: [Azure felügyelt alkalmazások a piactér hello](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="3dd5b-174">For information about publishing managed applications toohello Azure Marketplace, see [Azure managed applications in hello Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="3dd5b-175">További információ a piactér hello a kezelt alkalmazás felhasználása: [felhasználásához Azure felügyelt alkalmazások a piactér hello](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="3dd5b-175">For information about consuming a managed application from hello Marketplace, see [Consume Azure managed applications in hello Marketplace](managed-application-consume-marketplace.md).</span></span>
