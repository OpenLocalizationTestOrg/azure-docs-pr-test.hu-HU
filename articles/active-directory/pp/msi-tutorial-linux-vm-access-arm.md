---
title: "A Linux virtuális gép felhasználó által hozzárendelt MSI Azure Resource Manager eléréséhez használja"
description: "Ez az oktatóanyag végigvezeti egy User-Assigned felügyelt szolgáltatás identitásának (MSI) használata a Linux virtuális gép, Azure Resource Manager eléréséhez."
services: active-directory
documentationcenter: 
author: bryanLa
manager: mtillman
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: arluca
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: bebdccb616a4677fdf36ac257ac36f1827958af7
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/09/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-resource-manager"></a>Linux virtuális gép, egy felhasználó által hozzárendelt felügyelt szolgáltatás identitásának (MSI) használatával férjenek hozzá az Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Ez az oktatóanyag azt ismerteti, hogyan hozzon létre egy felhasználó által hozzárendelt felügyelt szolgáltatás identitásának (MSI), rendelje hozzá a Linux virtuális gép (VM) és az Azure Resource Manager API eléréséhez majd használja az identitásukat. 

Felügyelt szolgáltatás-identitások Azure automatikusan kezeli. Az Azure AD-alapú hitelesítés, anélkül, hogy a hitelesítő adatok beágyazása a kódot támogató szolgáltatások lehetővé teszik.

Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * A felhasználó által hozzárendelt MSI-fájl létrehozása
> * Az MSI-fájl kiosztása Linux virtuális gép 
> * Az MSI-hozzáférési jogot egy erőforráscsoportot az Azure Resource Manager 
> * Szereznie egy hozzáférési jogkivonatot az MSI-fájl használatával, és hívja az Azure Resource Manager használatával 

## <a name="prerequisites"></a>Előfeltételek

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Ebben az oktatóanyagban a CLI-parancsfájlt példák futtatásához két lehetőség közül választhat:

- Használjon [Azure Cloud rendszerhéj](~/articles/cloud-shell/overview.md) vagy Azure-portálról, vagy a "próbálja" gombra, keresztül minden kódblokk jobb felső sarkában található.
- [Telepítse a legújabb verziót a CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 vagy újabb verzió) Ha a helyi CLI-konzollal szeretné.

## <a name="sign-in-to-azure"></a>Bejelentkezés az Azure-ba

Jelentkezzen be az Azure Portalra a [https://portal.azure.com](https://portal.azure.com) webhelyen.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Egy új erőforráscsoportot a Linux virtuális gép létrehozása

A jelen oktatóanyag esetében először létrehozhat egy új Linux virtuális Gépet. Úgy is dönthet, hogy egy meglévő virtuális gép használja.

1. Kattintson az Azure Portal bal felső sarkában található **Új** gombra.
2. Válassza a **Számítás**, majd az **Ubuntu Server 16.04 LTS** elemet.
3. Adja meg a virtuális gép adatait. A **hitelesítési típus**, jelölje be **nyilvános SSH-kulcs** vagy **jelszó**. A létrehozott hitelesítő adatok lehetővé teszik-e jelentkezni a virtuális gép.

    ![Linux virtuális gép létrehozása](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Válasszon egy **előfizetés** a virtuális gép meg a legördülő listában.
5. Jelölje be egy új **erőforráscsoport** szeretne létrehozni, válassza a virtuális gép **hozzon létre új**. Amikor végzett, kattintson az **OK** gombra.
6. Adja meg a virtuális gép számára. További méretek megtekintéséhez válasszon **összes** , vagy módosítsa a lemez típusát támogatott szűrő. A Beállítások panelen hagyja változatlanul az alapértelmezett beállításokat, és kattintson az **OK** gombra.

## <a name="create-a-user-assigned-msi"></a>A felhasználó által hozzárendelt MSI-fájl létrehozása

1. A parancssori felület konzol (helyett egy Azure-felhő rendszerhéj munkamenet) használata, ha először jelentkezik be az Azure-bA. Használjon olyan fiókot, amelybe szeretné létrehozni az új MSI Azure-előfizetéssel társított:

    ```azurecli
    az login
    ```

2. Hozzon létre egy felhasználó által hozzárendelt MSI az [az identitás létrehozása](/cli/azure/identity#az_identity_create). A `-g` paraméter határozza meg az erőforráscsoportot, ahol az MSI-fájl jön létre, és a `-n` paraméter határozza meg a nevét. Ügyeljen arra, hogy cserélje le a `<RESOURCE GROUP>` és `<MSI NAME>` paraméterértékeket a saját értékekkel:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    A választ a felhasználó által hozzárendelt MSI létrehozott, az alábbi példához hasonló részleteit tartalmazza. Megjegyzés: a `id` értékét az MSI-fájl, a következő lépésben fogja használni:

    ```json
    {
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=12344643-8088-4d70-9532-c3a0fdc190fz",
    "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
    "location": "westcentralus",
    "name": "<MSI NAME>",
    "principalId": "9012",
    "resourceGroup": "<RESOURCE GROUP>",
    "tags": {},
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
    }
    ```

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>A felhasználó által hozzárendelt MSI hozzárendelése a Linux virtuális gép

Egy alapértelmezett MSI eltérően a felhasználó által hozzárendelt MSI több Azure-erőforrások az ügyfelek által használható. Ebben az oktatóanyagban, rendelje hozzá egy virtuális. Is hozzárendelheti az egynél több virtuális géphez.

A felhasználó által hozzárendelt MSI hozzárendelése a Linux virtuális gép [az vm hozzárendelése-identitás](/cli/azure/vm#az_vm_assign_identity). Ügyeljen arra, hogy cserélje le a `<RESOURCE GROUP>` és `<VM NAME>` paraméterértékeket a saját értékekkel. Használja a `id` tulajdonságot adott vissza az előző lépésben a `--identities` paraméter értékét:

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="grant-your-user-assigned-msi-access-to-a-resource-group-in-azure-resource-manager"></a>A felhasználó által hozzárendelt MSI hozzáférési jogot egy erőforráscsoportot az Azure Resource Manager 

MSI-fájl a kódot egy hozzáférési jogkivonatot, hogy az erőforrás API-k, amelyek támogatják az Azure AD-alapú hitelesítés hitelesítést nyújt. Ebben az oktatóanyagban a kód éri el az Azure Resource Manager API-t. 

A kódot, ha hozzáférni az API-t, a MSI identitás hozzáférést egy erőforráshoz az Azure Resource Manager szeretné. Ebben az esetben az erőforráscsoportot, amelyben a virtuális gép található. Ügyeljen arra, hogy cserélje le a `<CLIENT ID>`, `<SUBSCRIPTION ID>`, és `<RESOURCE GROUP>` paraméterértékeket a saját értékekkel. Cserélje le `<CLIENT ID>` rendelkező a `clientId` tulajdonság által visszaadott a `az identity create` parancsot [hozzon létre egy felhasználó által hozzárendelt MSI](#create-a-user-assigned-msi): 

```azurecli-interactive
az role assignment create --assignee <CLIENT ID> --role ‘Reader’ --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP> "
```

A válasz tartalmazza a létrehozott, az alábbi példához hasonló szerepkör-hozzárendelés részletei:

```json
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Authorization/roleAssignments/b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "name": "b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "properties": {
    "principalId": "f5fdfdc1-ed84-4d48-8551-999fb9dedfbl",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}

```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Szereznie egy hozzáférési jogkivonatot, a virtuális gép azonosítójának használatával, és hívja az erőforrás-kezelő használatával 

Az oktatóanyag a hátralévő azt fog működni a korábban létrehozott virtuális gépről.

Lépések elvégzéséhez szüksége van egy SSH-ügyfél. Windows használ, ha az SSH-ügyfél a használhatja a [Linux rendszerhez készült Windows alrendszer](https://msdn.microsoft.com/commandline/wsl/about). Ha az SSH-ügyfél kulcsok konfigurálása segítségre van szüksége, tekintse meg [a Windows Azure használatára SSH-kulcsok hogyan](~/articles/virtual-machines/linux/ssh-from-windows.md), vagy [létrehozása, és az SSH nyilvános és titkos kulcsból álló kulcspárt használata a Linux virtuális gépek Azure-ban](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Az Azure-portálon lépjen a **virtuális gépek**, keresse fel a Linux virtuális gépet, majd a a **áttekintése** kattintson **Connect** tetején. Másolja a karakterláncot, amellyel a virtuális Géphez csatlakozik.
2. **Csatlakozás** a virtuális géphez a az SSH-ügyfél az Ön által választott.  
3. A Terminálszolgáltatások ablakban CURL, használatával indítson egy lekérdezést a helyi MSI-végpont megszerezni egy hozzáférési jogkivonatot az Azure Resource Manager.  

   A CURL kérelem olyan hozzáférési jogkivonatot szerezni az alábbi példában látható. Ügyeljen arra, hogy a csere `<CLIENT ID>` rendelkező a `clientId` tulajdonság által visszaadott a `az identity create` parancsot [hozzon létre egy felhasználó által hozzárendelt MSI](#create-a-user-assigned-msi): 
    
   ```bash
   curl -H Metadata:true "http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<CLIENT ID>"   
   ```
    
    > [!NOTE]
    > Értékét a `resource` paraméter az Azure AD által várt pontosan egyeznie kell. Az erőforrás-kezelő erőforrás-azonosító használata esetén meg kell adni a záró perjelet URI-n. 
    
    A válasz tartalmazza az Azure Resource Manager eléréséhez szükséges jogkivonat. 
    
    Válasz példa:  

    ```bash
    {
    "access_token":"eyJ0eXAiOi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"
    } 
    ```

4. Most már Azure Resource Manager eléréséhez használja a jogkivonatot, és az erőforráscsoportot, amelyhez Ön korábban hozzáférést a felhasználó által hozzárendelt MSI tulajdonságainak olvasása. Ügyeljen arra, hogy a csere `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` a korábban megadott értékeket, és `<ACCESS TOKEN>` a jogkivonatok az előző lépés eredményeképpen visszakapott.

    > [!NOTE]
    > Az URL-címe: kis-és nagybetűket, ezért ügyeljen arra, hogy a használt korábban az erőforráscsoportot és a nagybetűk "G" nevű pontos nagybetűket `resourceGroups`.  

    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    A válasz az adott erőforráscsoport információkat tartalmaz, a következőhöz hasonló: 

    ```bash
    {
    "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/DevTest",
    "name":"DevTest",
    "location":"westus",
    "properties":{"provisioningState":"Succeeded"}
    } 
    ```
    
## <a name="next-steps"></a>További lépések

- MSI áttekintését lásd: [Szolgáltatásidentitás felügyelete – áttekintés](msi-overview.md).

Az alábbi Megjegyzések szakasz segítségével visszajelzést, és segítsen pontosítsa és a tartalom.
