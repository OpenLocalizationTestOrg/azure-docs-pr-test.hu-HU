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
# <a name="consume-an-internal-managed-application"></a>Belső felügyelt alkalmazás felhasználása

Azure felhasználhat [kezelt alkalmazások](managed-application-overview.md) szolgálnak, hogy a szervezet tagjaira. Kiválaszthatja például a kezelt alkalmazások az informatikai részleg, amely a vállalati szabványoknak való megfelelés biztosítása. A kezelt alkalmazások hello szolgáltatáskatalógus, nem hello Azure piactéren keresztül érhetők el.

Ez a cikk folytatása előtt az előfizetéshez tartozó hello szolgáltatáskatalógus elérhető kezelt alkalmazás kell rendelkeznie. Ha valaki van a szervezet rendelkezik nem hozott létre egy felügyelt alkalmazást, lásd: [– belső használat a kezelt alkalmazás közzététele](managed-application-publishing.md).

Jelenleg az Azure parancssori felület vagy hello Azure portál tooconsume egy felügyelt alkalmazást is használhatja.

## <a name="create-hello-managed-application-by-using-hello-portal"></a>Hello portál használatával felügyelt hello alkalmazás létrehozása

kezelt alkalmazás hello portálon keresztül toodeploy kövesse az alábbi lépéseket:

1. Nyissa meg az Azure portál toohello. Keresse meg **szolgáltatáskatalógus felügyelt alkalmazás**.

   ![Szolgáltatáskatalógus felügyelt alkalmazás](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. Jelölje be hello elérhető megoldások listájából hello toocreate kívánt alkalmazás kezeli. Kattintson a **Létrehozás** gombra.

   ![Kezelt alkalmazás kiválasztása](./media/managed-application-consumption/select-offer.png)

1. Adja meg, amely szükséges tooprovision hello erőforrás hello paraméterek értékét. Válassza ki **nyugati középső Régiójában** helyéhez. Kattintson az **OK** gombra.

   ![A felügyelt alkalmazási paraméterek](./media/managed-application-consumption/input-parameters.png)

1. hello sablon hello értékektől ellenőrzi. Ha az érvényesítés sikeres, válassza ki a **OK** toostart hello központi telepítés.

   ![A felügyelt alkalmazási érvényesítése](./media/managed-application-consumption/validation.png)

Hello központi telepítés befejezése után hello megfelelő erőforrásokkal hello sablonban meghatározott hello felügyelt erőforráscsoportban megadott törlődnek.

## <a name="create-hello-managed-application-by-using-azure-cli"></a>Hello felügyelt alkalmazás létrehozása az Azure parancssori felület használatával

Nincsenek két módon toocreate kezelt alkalmazás Azure parancssori felület használatával:

* Hello paranccsal kezelt alkalmazások létrehozásához.
* Hello rendszeres sablon telepítési paranccsal.

### <a name="use-hello-template-deployment-command"></a>Hello sablon telepítési parancs

Hello applianceMainTemplate.json fájl, amely létre szállító hello telepítése.

Ezután hozzon létre két erőforráscsoport. hello első erőforráscsoport egy hello kezelt alkalmazás erőforrás létrehozási helyének: Microsoft.Solutions/appliances. hello második erőforráscsoport összes hello erőforrásai mainTemplate.json definiálva. Ez az erőforráscsoport hello ISV kezeli.

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> Használjon `westcentralus` hello erőforráscsoport hello helyként.
>

a mainResourceGroup, a következő parancs használata hello applianceMainTemplate.json toodeploy:

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

Hello megelőző sablon fut, után megkérdezi hello hello sablonban meghatározott hello paraméterek értékeit. Ezenkívül toohello paramétereket szükséges tooprovision erőforrások sablonban, két fő paraméterértékek van szüksége:

- **managedResourceGroupId**: hello hello erőforrás csoportot tartalmazó hello erőforrások applianceMainTemplate.json definiált azonosítója. hello azonosító: hello űrlap `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`. A fenti példa hello, hello azonosítója tartozó `managedResourceGroup`.
- **applianceDefinitionId**: hello hello azonosítója felügyelt alkalmazás erőforrás-definícióban. Ez az érték hello ISV biztosítja.

> [!NOTE]
> hello publisher hozzáférés toohello erőforráscsoport hello felügyelt alkalmazás definícióját tartalmazó kell biztosítania. hello definition erőforrás hello publisher előfizetés jön létre. Ezért egy felhasználó, a felhasználói csoport vagy az alkalmazás ügyfél-bérlőben hello szükséges olvasási hozzáférés toothis erőforrás.

Hello központi telepítés sikeres befejezése után megjelenik a felügyelt hello alkalmazás mainResourceGroup jön létre. hello storageAccount erőforrás managedResourceGroup jön létre.

### <a name="use-hello-create-command"></a>A create parancs használata hello

Használhatja a hello `az managedapp create` parancs toocreate hello a kezelt alkalmazás felügyelt alkalmazás definícióját.

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* **készülék Meghatározásazonosítóra**: hello erőforrás-azonosítója, hello által felügyelt hello az előző lépésben létrehozott alkalmazás-definíciót. tooobtain ezt az Azonosítót, futtassa a következő parancs hello:

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  Ez a parancs visszaadja a felügyelt hello definíciót. Hello azonosító tulajdonság értékének hello van szüksége.

* **felügyelt rg-azonosító**: hello erőforrás csoportot tartalmazó hello erőforrások applianceMainTemplate.json definiált hello nevét. Ez az erőforráscsoport hello felügyelt erőforráscsoportban. Hello publisher kezeli. Ha még nem létezik, az Ön létrejön.
* **Erőforráscsoport**: ahol hello felügyelt alkalmazás erőforrás hello erőforráscsoportban jön létre. Ez az erőforráscsoport él hello Microsoft.Solutions/appliance erőforrás.
* **paraméterek**: hello applianceMainTemplate.json definiált hello erőforrások szükséges paraméterek.

## <a name="known-issues"></a>Ismert problémák

Az előzetes kiadás tartalmazza a következő problémák hello:

* 500 belső kiszolgálóhiba hello felügyelt alkalmazás hello létrehozása során jelenik meg. Ha ezt a problémát tapasztal, valószínűleg átmeneti toobe is. Ismételje meg a hello műveletet.
* Új erőforráscsoport hello felügyelt erőforráscsoportban van szükség. Ha egy meglévő erőforráscsoportot használ, hello telepítése sikertelen.
* hello hello Microsoft.Solutions/appliances erőforrás tartalmazó erőforráscsoportot kell létrehozni hello **westcentralus** helyét.

## <a name="next-steps"></a>Következő lépések

* Egy bevezető toomanaged alkalmazások, lásd: [felügyelt használatát áttekintő cikkben](managed-application-overview.md).
* További információ a szolgáltatáskatalógus kezelt alkalmazás közzététele: [létrehozása és a szolgáltatáskatalógus kezelt alkalmazás közzététele](managed-application-publishing.md).
* Közzétételi kezelt alkalmazások toohello Azure piactér kapcsolatos információkért lásd: [Azure felügyelt alkalmazások a piactér hello](managed-application-author-marketplace.md).
* További információ a piactér hello a kezelt alkalmazás felhasználása: [felhasználásához Azure felügyelt alkalmazások a piactér hello](managed-application-consume-marketplace.md).
