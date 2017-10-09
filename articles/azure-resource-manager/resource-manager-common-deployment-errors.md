---
title: "aaaTroubleshoot gyakran az Azure-telepítés hibákat |} Microsoft Docs"
description: "Ismerteti, hogyan tooresolve gyakori hibák Azure Resource Manager használatával erőforrások tooAzure központi telepítésekor."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "központi telepítési hiba, az azure-telepítés tooazure központi telepítése"
ms.assetid: c002a9be-4de5-4963-bd14-b54aa3d8fa59
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: tomfitz
ms.openlocfilehash: 8571e9941879eb5586e4258a785b6a09247da771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Hibaelhárítás általános az Azure-telepítés az Azure Resource Manager eszközzel
Ez a témakör ismerteti, hogyan oldhatja meg néhány gyakori merülhetnek fel az Azure-telepítés hibáit.

Ez a témakör ismerteti a következő hibakódok hello:

* [AccountNameInvalid](#accountnameinvalid)
* [A bejelentkezés nem sikerült](#authorization-failed)
* [BadRequest](#badrequest)
* [Sikertelen](#deploymentfailed)
* [DisallowedOperation](#disallowedoperation)
* [InvalidContentLink](#invalidcontentlink)
* [InvalidTemplate](#invalidtemplate)
* [MissingSubscriptionRegistration](#noregisteredproviderfound)
* [NotFound](#notfound)
* [NoRegisteredProviderFound](#noregisteredproviderfound)
* [OperationNotAllowed](#quotaexceeded)
* [ParentResourceNotFound](#parentresourcenotfound)
* [QuotaExceeded](#quotaexceeded)
* [RequestDisallowedByPolicy](#requestdisallowedbypolicy)
* [ResourceNotFound](#notfound)
* [SkuNotAvailable](#skunotavailable)
* [StorageAccountAlreadyExists](#storagenamenotunique)
* [StorageAccountAlreadyTaken](#storagenamenotunique)

## <a name="deploymentfailed"></a>Sikertelen

Ez a hibakód azt jelzi, általános központi telepítési hiba, de nincs szüksége toostart hibaelhárítási hello hibakód. hello segítő ténylegesen hello elhárításához hibakód általában ez a hiba alatt egy szinttel. Például hello következő kép bemutatja hello **RequestDisallowedByPolicy** hello központi telepítési hiba alá tartozó hibakód.

![hibakód megjelenítése](./media/resource-manager-common-deployment-errors/error-code.png)

## <a name="skunotavailable"></a>SkuNotAvailable

Egy erőforrás (általában egy virtuális gép) telepítésekor a következő kód és a hiba hibaüzenet hello jelenhet meg:

```
Code: SkuNotAvailable
Message: hello requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy tooa different location.
```

Hibaüzenet jelenik a hello erőforrás (például a Virtuálisgép-méretet) kiválasztott Termékváltozat nem érhető el a kiválasztott hello helyéhez. tooresolve probléma kell toodetermine termékváltozatok rendelkezésre álló régióban. Használhatja a PowerShell, hello portál vagy a REST művelet toofind elérhető termékváltozatok.

- A PowerShell, használjon [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) és hely szerint szűrő. Hello legújabb verziójának PowerShell ennél a parancsnál kell rendelkeznie.

  ```powershell
  Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
  ```

  hello eredmények tartalmazzák a hello hely SKU listáját, valamint, hogy termékváltozat korlátozások.

  ```powershell
  ResourceType                Name      Locations Restriction                      Capability Value
  ------------                ----      --------- -----------                      ---------- -----
  availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
  availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
  virtualMachines      Standard_A0 southcentralus
  virtualMachines      Standard_A1 southcentralus
  virtualMachines      Standard_A2 southcentralus
  ```

- toouse hello [portal](https://portal.azure.com), toohello portal be, és vegyen fel egy erőforrást hello felületen keresztül. Hello értékeket állíthat be, megjelenik a hello elérhető termékváltozatok az adott erőforráshoz. Nem kell toocomplete hello központi telepítés.

    ![elérhető termékváltozatok](./media/resource-manager-common-deployment-errors/view-sku.png)

- toouse hello REST API-t a virtuális gépek küldése hello kérelem a következő:

  ```HTTP 
  GET
  https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
  ```

  Elérhető termékváltozatok és régiók visszaküldi hello a következő formátumban:

  ```json
  {
    "value": [
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A0",
        "tier": "Standard",
        "size": "A0",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A1",
        "tier": "Standard",
        "size": "A1",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      ...
    ]
  }    
  ```

Ha egy adott régióban vagy egy másik régióban, az üzleti igényeinek megfelelő megfelelő Termékváltozat nem toofind, küldje el a [SKU kérelem](https://aka.ms/skurestriction) tooAzure támogatása.

## <a name="disallowedoperation"></a>DisallowedOperation

```
Code: DisallowedOperation
Message: hello current subscription type is not permitted tooperform operations on any provider 
namespace. Please use a different subscription.
```

Ez a hibaüzenet jelenik meg, ha az előfizetés, amely nem megengedett tooaccess bármely Azure-szolgáltatások nem Azure Active Directory. Lehetséges, hogy az ilyen típusú előfizetés tooaccess hello klasszikus portál hiszen toodeploy erőforrások nem engedélyezett. tooresolve probléma előfizetést kell használnia egy engedély toodeploy erőforrásokat tartalmazó.  

tooview a PowerShell használatával, az elérhető előfizetések használata:

```powershell
Get-AzureRmSubscription
```

És tooset hello előfizetésben, használja:

```powershell
Set-AzureRmContext -SubscriptionName {subscription-name}
```

tooview Azure CLI 2.0, az elérhető előfizetések használata:

```azurecli
az account list
```

És tooset hello előfizetésben, használja:

```azurecli
az account set --subscription {subscription-name}
```

## <a name="invalidtemplate"></a>InvalidTemplate
Ez a hiba oka lehet több különböző típusú hibák.

- Szintaktikai hiba

   Ha hello sablont nem sikerült érvényesítési jelző hibaüzenetet kapja, előfordulhat, hogy szintaktikai hiba a sablonban.

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed
  ```

   Ez a hiba oka az, könnyen toomake sablon kifejezések bonyolult lehet. Például hello következő hozzárendelést egy tárfiók zárójelek közé egy készletét, három funkció, három különböző kerek zárójeleket tartalmazhatnak, szimpla idézőjelben egy készletét és tartalmaz egy tulajdonság:

  ```json
  "name": "[concat('storage', uniqueString(resourceGroup().id))]",
  ```

   Ha nem ad meg hello megfelelő szintaxist, hello sablon egy érték, amely eltér attól a levelezéshez hoz létre.

   Hiba az ilyen típusú megjelenésekor gondosan tekintse át a hello kifejezési szintaxist. Érdemes lehet például egy JSON-szerkesztővel [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) vagy [Visual Studio Code](resource-manager-vs-code.md), amely képes figyelmeztessen a szintaktikai hibákat.

- Helytelen szegmens hosszának

   Egy másik érvénytelen a sablon-hiba akkor fordul elő, ha hello erőforrás neve nem hello megfelelő formátumban.

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed: 'hello template resource {resource-name}'
  for type {resource-type} has incorrect segment lengths.
  ```

   Egy legfelső szintű erőforrás hello neve eltér az hello erőforrás típusa egy kisebb szegmenst kell rendelkeznie. Minden szegmensben perjellel különbözteti meg van. A következő példa hello, hello típus két szegmensek és hello neve rendelkezik egy szegmens, akkor egy **érvényes**.

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "myHostingPlanName",
    ...
  }
  ```

   De a következő példa hello **nem egy érvényes nevet** mert hello szegmensek azonos számú hello típusként.

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "appPlan/myHostingPlanName",
    ...
  }
  ```

   Az alsóbb szintű erőforrásai, hello típusa és neve rendelkezik hello szegmensek azonos számú. A létrehozható szegmensek számát az így, mert hello teljes nevét és típusát hello gyermek tartalmazza hello szülő nevét és típusát. Ezért hello teljes név még egy szegmens kevesebb mint hello teljes típusa.

  ```json
  "resources": [
      {
          "type": "Microsoft.KeyVault/vaults",
          "name": "contosokeyvault",
          ...
          "resources": [
              {
                  "type": "secrets",
                  "name": "appPassword",
                  ...
              }
          ]
      }
  ]
  ```

   Első hello szegmensek jobb erőforrás-kezelő típusú erőforrás-szolgáltató között alkalmazott megkapni. Például az erőforrás zárolási tooa webhely alkalmazása szükséges négy szegmensek típus. Ezért hello neve a következő három szegmensek:

  ```json
  {
      "type": "Microsoft.Web/sites/providers/locks",
      "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
      ...
  }
  ```

- Másolás index nem várt.

   Ez tapasztal **InvalidTemplate** hiba a hello alkalmazott **másolási** elem tooa részét hello sablont, amely nem támogatja ezt az elemet. Hello másolási elem tooa erőforrás típusa csak alkalmazhatók. Másolás tooa tulajdonság erőforrástípus belül nem tudja alkalmazni. Például másolás tooa virtuális gép telepítését, de a virtuális gép operációs rendszer toohello lemezek nem alkalmazható. Bizonyos esetekben átválthat egy gyermek erőforrás tooa szülő erőforrás toocreate a másolási ciklust. Másolás használatával kapcsolatos további információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).

- Érvénytelen paraméter

   Ha hello sablon határozza meg a paraméter megengedett értékei, és megad egy értéket, amely nem egy ezeket az értékeket, egy üzenet hasonló toohello, a következő hiba jelenhet meg:

  ```
  Code=InvalidTemplate;
  Message=Deployment template validation failed: 'hello provided value {parameter value}
  for hello template parameter {parameter name} is not valid. hello parameter value is not
  part of hello allowed values
  ``` 

   Ellenőrizze hello hello sablon az engedélyezett értékek, és adjon meg egy központi telepítése során.

- Körkörös függőséget észlelt

   Hibaüzenet az erőforrások lehetnek egymással függőségi viszonyban oly módon, hogy megakadályozza, hogy a hello telepítési indítását. Egy egymástól függő szolgáltatásainak összevonás a két vagy több erőforrás, várjon, amíg más erőforrások, amelyek is várnak. Például resource1 resource3 függ, resource2 resource1 függ, és resource3 resource2 függ. Eltávolítja a szükségtelen függőségek általában megoldhatja a problémát. 

<a id="notfound" />
### <a name="notfound-and-resourcenotfound"></a>NotFound és ResourceNotFound
Ha a sablon tartalmazza, amely nem oldható fel erőforrás hello neve, hasonló hibaüzenetet kap:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Ha a kívánt toodeploy hello hello sablonban erőforrás hiányzik, ellenőrizze, hogy szükséges-e tooadd függőség. Erőforrás-kezelő optimalizálja a központi telepítési erőforrások létrehozásával párhuzamosan, amikor lehetséges. Ha egy erőforrást egy másik erőforrás után kell telepíteni, akkor kell-e toouse hello **dependsOn** elem található a sablon toocreate a függőség hello egyéb erőforrásokat. Például a webes alkalmazás telepítésekor hello App Service-csomag léteznie kell. Ha nem adott meg, hogy hello webalkalmazás hello App Service-csomag függ, erőforrás-kezelő létrehoz-e az hello mindkét erőforrás ugyanannyi időt vesz igénybe. Hibaüzenet jelenik meg, amely meghatározza, hogy adott hello App Service-csomag erőforrás nem található, mert nem létezik még tooset hello webalkalmazásban tulajdonság megkísérlése során. Megakadályozható, hogy ezt a hibát úgy, hogy hello függőségi hello web app alkalmazásban.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Függőségi hiba elhárításával kapcsolatos javaslatok, lásd: [ellenőrizze a telepítési sorrendjét](#check-deployment-sequence).

Ez a hiba sem hello erőforrás létezik egy másik erőforráscsoportban található, mint egy telepítendő hello látni. Ebben az esetben használja a hello [resourceId függvény](resource-group-template-functions-resource.md#resourceid) tooget hello teljesen minősített hello erőforrás nevét.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

Ha úgy próbálja toouse hello [hivatkozás](resource-group-template-functions-resource.md#reference) vagy [listKeys](resource-group-template-functions-resource.md#listkeys) működik együtt a erőforrása, amely nem oldható fel, megjelenik a következő hiba hello:

```
Code=ResourceNotFound;
Message=hello Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

Egy kifejezés, amely magában foglalja a hello keressen **hivatkozás** függvény. A ellenőrizze, hogy helyesek-e a hello paraméterértékeket.

## <a name="parentresourcenotfound"></a>ParentResourceNotFound

Amikor egy erőforrást egy szülő tooanother erőforrás, hello szülő erőforrás hello gyermek-erőforrás létrehozása előtt léteznie kell. Ha még nem létezik, hello a következő hiba jelenhet meg:

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

hello gyermek-erőforrás nevét hello hello szülő nevének tartalmazza. Például egy SQL-adatbázis előfordulhat, hogy meghatározása:

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

De ha nincs megadva a függőség hello szülő erőforrás, hello gyermek erőforrás előfordulhat, hogy telepítve előtt hello szülő. tooresolve Ez a hiba egy függőséget tartalmaz.

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

<a id="storagenamenotunique" />

## <a name="storageaccountalreadyexists-and-storageaccountalreadytaken"></a>StorageAccountAlreadyExists és StorageAccountAlreadyTaken
Storage-fiókok meg kell adnia egy nevet, amely egyedi egész Azure hello erőforrás. Ha nem ad meg egy egyedi nevet, például hibaüzenetet kap:

```
Code=StorageAccountAlreadyTaken
Message=hello storage account named mystorage is already taken.
```

Létrehozhat egy egyedi nevet az elnevezési hello hello eredményét a szereplő [uniqueString](resource-group-template-functions-string.md#uniquestring) függvény.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Ha telepít egy tárfiók hello azonos nevet, amint egy meglévő tárfiók az előfizetésben, de adjon meg egy másik helyre, jelző hello tárfiók már létezik egy másik helyen lévő hibaüzenetet kap. Vagy törölje a meglévő tárfiók hello, vagy adjon meg hello ugyanazon a helyen, a meglévő tárfiók hello.

## <a name="accountnameinvalid"></a>AccountNameInvalid
Megjelenik a hello **AccountNameInvalid** hiba a tárolási fiók nevét, amely tartalmazza az toogive kísérlet tiltott karakter. Tárfiókok neve 3 – 24 karakter hosszúságúnak kell és számokat és kisbetűket tartalmazhatnak csak. Hello [uniqueString](resource-group-template-functions-string.md#uniquestring) függvény 13 karaktert adja vissza. Ha Ön egy előtag toohello összefűzésére **uniqueString** vezethet, adjon meg egy előtag 11 karakter vagy kevesebb.

## <a name="badrequest"></a>BadRequest

Ha egy tulajdonság érvénytelen értéket ad meg BadRequest állapot merülhetnek fel. Például ha egy tárfiók Termékváltozata helytelen értéket ad meg, hello központi telepítés sikertelen lesz. érvényes értékei toodetermine tulajdonság, nézze meg hello [REST API](/rest/api) hello erőforrástípus telepít.

<a id="noregisteredproviderfound" />

## <a name="noregisteredproviderfound-and-missingsubscriptionregistration"></a>NoRegisteredProviderFound és MissingSubscriptionRegistration
Erőforrás telepítésekor hibakód a következő hello kap, és üzenet:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Vagy hasonló üzenet jelenhet meg:

```
Code: MissingSubscriptionRegistration
Message: hello subscription is not registered toouse namespace {resource-provider-namespace}
```

Ezek a hibák három okok egyike jelenhet meg:

1. hello erőforrás-szolgáltató nincs regisztrálva az előfizetéséhez
2. Hello erőforrástípus nem támogatott API-verzió
3. Hely hello erőforrástípus nem támogatott

hello hibaüzenet hello támogatott helyek és API-verziók javaslatokat adjon meg. Módosíthatja a sablon tooone hello a javasolt értékek. A legtöbb szolgáltatók által hello Azure portal vagy hello parancssori felületet használ, de nem mindegyik automatikusan regisztrált. Ha még nem használta az egy adott erőforrás-szolgáltató előtt, szükség lehet tooregister ezt a szolgáltatót. További információ a PowerShell vagy Azure parancssori felületen keresztül erőforrás-szolgáltatók felderíthetők.

**Portál**

Hello regisztrációs állapotát tekintheti meg, és regisztrálja egy erőforrás-szolgáltató névtere hello portálon keresztül.

1. Az előfizetéshez, válasszon **erőforrás-szolgáltató**.

   ![Válassza ki az erőforrás-szolgáltató](./media/resource-manager-common-deployment-errors/select-resource-provider.png)

2. Nézze meg hello erőforrás-szolgáltatók listáját, és szükség esetén válasszon ki hello **regisztrálása** hivatkozás tooregister hello erőforrás-szolgáltató hello típusú toodeploy próbált.

   ![erőforrás-szolgáltatók felsorolása](./media/resource-manager-common-deployment-errors/list-resource-providers.png)

**PowerShell**

toosee a regisztrációs állapotát, használja **Get-AzureRmResourceProvider**.

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

a szolgáltató tooregister használja **Register-AzureRmResourceProvider** és hello nevezze hello erőforrás-szolgáltató tooregister kívánja.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

egy adott típusú erőforrás, tooget hello támogatott helyek használja:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

tooget hello támogatott API-verziók egy adott típusú erőforrás használata:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

**Azure CLI**

toosee hello szolgáltató regisztrálva van, hogy használja-hello `azure provider list` parancsot.

```azurecli
az provider list
```

egy erőforrás-szolgáltató tooregister hello használata `azure provider register` parancsot, és adja meg a hello *névtér* tooregister.

```azurecli
az provider register --namespace Microsoft.Cdn
```

toosee hello támogatott helyek és egy erőforrástípusra API-verziók használata:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

<a id="quotaexceeded" />

## <a name="quotaexceeded-and-operationnotallowed"></a>QuotaExceeded és OperationNotAllowed
Problémák állhatnak, ha a központi telepítés meghaladja a kvótát, legyen az erőforráscsoportot, előfizetések, fiókok és egyéb hatókörök. Az előfizetés lehet például a régió magok száma toolimit hello konfigurálva. Ha úgy próbálja toodeploy engedélyezett összeg hello mint, több maggal rendelkező virtuális gép, hibaüzenet azzal hello kvóta túl lett lépve.
Teljes kvóta információkért lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).

tooexamine az előfizetése magokra vonatkozó kvótákat mag, hello használhatja `azure vm list-usage` hello Azure CLI parancsot. a következő példa hello mutatja be, hogy hello core kvóta, egy ingyenes próbafiók érték 4:

```azurecli
az vm list-usage --location "South Central US"
```

Amely adja vissza:

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

Ha négynél több maggal hello USA nyugati régiója régióban egy sablont, kap egy központi telepítési hiba, amely a következőhöz hasonló:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Vagy a PowerShellben használható hello **Get-AzureRmVMUsage** parancsmag.

```powershell
Get-AzureRmVMUsage
```

Amely adja vissza:

```powershell
...
CurrentValue : 0
Limit        : 4
Name         : {
                 "value": "cores",
                 "localizedValue": "Total Regional Cores"
               }
Unit         : null
...
```

Ezekben az esetekben toohello portálon lépjen és a fájl egy támogatási probléma tooraise hello régió toodeploy szeretné a kvótát.

> [!NOTE]
> Ne feledje, hogy az erőforráscsoportok, hello kvóta minden egyes régió, nem a teljes előfizetés hello. Toodeploy 30 magok az USA nyugati régiója van szüksége, az USA nyugati régiója a 30 erőforrás-kezelő magok tooask lesz. Ha toodeploy 30 magok hello régiók toowhich valamelyikében kell rendelkezik hozzáféréssel, kérdezze meg a 30 erőforrás-kezelő magok minden régióban.
>
>

## <a name="invalidcontentlink"></a>InvalidContentLink
Ha a hello hibaüzenetet kapja:

```
Code=InvalidContentLink
Message=Unable toodownload deployment content from ...
```

Valószínűleg próbált toolink tooa beágyazott sablont, amely nem érhető el. Ellenőrizze hello hello beágyazott sablonhoz megadott URI. Ha hello sablon szerepel egy tárfiókot, ellenőrizze, hogy elérhető-e a hello URI. Szükség lehet egy SAS-jogkivonat toopass. További információ: [Kapcsolt sablonok használata az Azure Resource Manager eszközben](resource-group-linked-templates.md).

## <a name="requestdisallowedbypolicy"></a>RequestDisallowedByPolicy
Hibaüzenet az előfizetése tartalmaz egy erőforrás-házirendet, amely megakadályozza a művelet kívánt tooperform üzembe helyezése során. Hello hibaüzenet jelenik meg keresse meg hello házirend-azonosító.

```
Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'
```

A **PowerShell**, adja meg, hogy a házirend az azonosítója, mint hello **azonosító** hello-szabályzatot, amely blokkolja a telepítési paraméter tooretrieve részleteit.

```powershell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

A **Azure CLI**, adja meg a házirend-definíció hello hello nevét:

```azurecli
az policy definition show --name regionPolicyAssignment
```

További információkért tekintse meg a következő cikkek hello:

- [RequestDisallowedByPolicy hiba](resource-manager-policy-requestdisallowedbypolicy-error.md)
- [Házirend toomanage erőforrásainak használatához, és hozzáférést](resource-manager-policy.md).

## <a name="authorization-failed"></a>A bejelentkezés nem sikerült
Hiba jelenhet üzembe helyezése során, mert hello fiók vagy egyszerű toodeploy hello erőforrások próbált szolgáltatás nem rendelkezik hozzáférési tooperform ezeket a műveleteket. Az Azure Active Directory lehetővé teszi, hogy Ön vagy a rendszergazda toocontrol, mely identitások nagy fokú pontosságot milyen erőforrások eléréséhez. Például toohello olvasó szerepkört az Ön fiókjához van társítva, ha még nem tud toocreate erőforrásokat. Ebben az esetben megjelenik egy hibaüzenet üzent arról, hogy engedélyezése nem sikerült.

Szerepköralapú hozzáférés-vezérléssel kapcsolatos további információkért lásd: [átruházásához hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).


## <a name="next-steps"></a>Következő lépések
* toolearn kapcsolatos naplózási műveletek, lásd: [naplózási műveletek a Resource Manager](resource-group-audit.md).
* toolearn kapcsolatos műveletek toodetermine hello hibák központi telepítése során lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).
