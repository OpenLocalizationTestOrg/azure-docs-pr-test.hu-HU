---
title: aaaResource Manager REST API-k |} Microsoft Docs
description: "Hello Resource Manager REST API-k hitelesítés és használati példák áttekintése"
services: azure-resource-manager
documentationcenter: na
author: navalev
manager: timlt
editor: 
ms.assetid: e8d7a1d2-1e82-4212-8288-8697341408c5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/13/2017
ms.author: navale;tomfitz;
ms.openlocfilehash: 3ccc3575c5e06c41f2fdc5317711980fc6a2f649
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resource-manager-rest-apis"></a>Erőforrás-kezelői REST API-k
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Portál](resource-group-portal.md) 
> * [REST API](resource-manager-rest-api.md)
> 
> 

Minden hívás tooAzure erőforrás-kezelő mögött minden telepített sablon mögött minden konfigurált tárfiók mögött van egy vagy több hívások toohello Azure Resource Manager RESTful API. Ez a témakör az fordított toothose API-t, és hogyan hívása őket bármely SDK minden használata nélkül. Ezt a módszert akkor hasznos, ha azt szeretné, hogy teljes körű hozzáférést engedélyezzenek kérelmek tooAzure, vagy ha az elsődleges nyelvhez SDK hello nem érhető el, vagy nem támogatja a szükséges hello műveletek.

Ez a cikk nem lép az Azure-ban van közzétéve, de ahelyett, hogy használja az egyes műveletek példa arra, hogyan kapcsolódnak a toothem minden API-n keresztül. Hello alapok elsajátítása után el tudja olvasni hello [Azure Resource Manager REST API-referencia](https://docs.microsoft.com/rest/api/resources/) toofind részletes információk hogyan toouse hello részeinek hello API-k.

## <a name="authentication"></a>Authentication
Hitelesítés az erőforrás-kezelő által Azure Active Directory (AD) kell kezelni. tooconnect tooany API, először az Azure AD tooreceive egy hitelesítési jogkivonatot, amelyek átadhatók tooevery kérésre tooauthenticate. Mivel azt egy tiszta hívás olvashat részletesen, közvetlenül a REST API-k toohello, feltételezzük, hogy nem szeretné, hogy tooauthenticate által meg kell adni egy felhasználónevet és jelszót. Azt is feltételezzük, hogy nem használ két többtényezős hitelesítési mechanizmus. Ezért létrehozhatunk egy Azure AD-alkalmazást és egy egyszerű, amelyek a használt toolog úgynevezett. Ne feledje, hogy az Azure AD támogatja a több hitelesítési eljárás, és az összes, de lehet használt tooretrieve, hogy további kérelmeknél API szükséges hitelesítési tokent.
Hajtsa végre a [létrehozása Azure AD-alkalmazást és egy egyszerű szolgáltatást](resource-group-create-service-principal-portal.md) részletes útmutatást.

### <a name="generating-an-access-token"></a>Egy hozzáférési jogkivonat létrehozása
Hitelesítés engedélyezésébe az Azure AD létesítő tooAzure AD, login.microsoftonline.com helyen történik. tooauthenticate, kell toohave hello a következő információkat:

* Az Azure AD bérlő azonosítója (hello nevét, hogy az Azure AD a toolog használ, gyakran hello ugyanaz, mint a vállalat, de nem szükséges)
* Alkalmazásazonosító (hello Azure AD-alkalmazás létrehozása lépés során végrehajtott)
* Jelszó (hello Azure AD-alkalmazás létrehozása során kiválasztott)

A következő HTTP-kérelem hello győződjön meg arról, hogy tooreplace "Azure AD bérlő azonosítója", "Alkalmazásazonosító" és a "Password" hello megfelelő értékekkel.

**Általános HTTP-kérelem:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... (Ha a hitelesítés sikeres) lesz a válasz a következő válasz hasonló toohello eredményezi:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(a válasz megelőző hello hello access_token rövidített tooincrease olvashatóság volt)

**Létrehozása a Bash használatával:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Létrehozása a PowerShell használatával:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

hello válasz egy hozzáférési jogkivonatot, információ arról, hogy mennyi ideig, hogy a jogkivonat érvényes, és milyen erőforrás is használhatja, hogy a jogkivonat kapcsolatos információkat tartalmazza.
az összes kérelem toohello Resource Manager API hello kapott hozzáférési jogkivonat segítségével hello előző HTTP hívásban kell átadni. "Engedélyezés" hello "Tulajdonosi YOUR_ACCESS_TOKEN" értékű nevű fejléc értékeként adja át. Figyelje meg a "Tulajdonos" és a hozzáférési token hello térköze.

Ahogy fent HTTP hello látja, hello lexikális elem érvénytelen a meghatározott időn időintervalluma, gyorsítótárba, és használja fel, hogy ugyanezt a tokent. Akkor is, ha lehetséges tooauthenticate szemben Azure ad-val minden API-hívás, nagyon hatékony lenne.

## <a name="calling-resource-manager-rest-apis"></a>Hívása Resource Manager REST API-k
Ez a témakör csak néhány API-k tooexplain hello használatának alapjaival hello REST műveleteinek használja. Minden hello műveletekkel kapcsolatos információkért lásd: [Azure Resource Manager REST API-k](https://docs.microsoft.com/rest/api/resources/).

### <a name="list-all-subscriptions"></a>Minden előfizetés felsorolása
Hello teheti legegyszerűbb műveletek egyike toolist hello az elérhető előfizetések keresztül elérhető. A kérelem a következő hello láthatja, hogyan hello hozzáférési jogkivonat érték az átadott fejléc:

(Cserélje YOUR_ACCESS_TOKEN a tényleges hozzáférési jogkivonat.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... és eredményeként kapott előfizetések listáját, hogy a szolgáltatás egyszerű tooaccess engedélyezett

(Előfizetés-azonosítók rendelkezik lettek rövidítve olvashatóság érdekében)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Egy adott előfizetés összes erőforráscsoportok felsorolása
Hello Resource Manager API-k elérhető összes erőforrás egy erőforráscsoportban van beágyazva. Meglévő erőforráscsoport erőforrás-kezelő lekérheti az előfizetéshez a következő HTTP GET kérést hello segítségével. Figyelje meg, hogyan hello előfizetés-azonosító érték az átadott hello URL-cím része az időt.

(YOUR_ACCESS_TOKEN és lecserélése ELŐFIZETÉS_AZONOSÍTÓJA a tényleges access token és előfizetés-azonosító)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

hello választ kapott attól függ, hogy van-e olyan meghatározott erőforráscsoportokat, és ha igen, hogy hány.

(Előfizetés-azonosítók rendelkezik lettek rövidítve olvashatóság érdekében)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot
Az eddigi jelenleg már csak lett lekérdezése hello Resource Manager API-k információt. Azt bizonyos erőforrások létrehozza, és azok minden, egy erőforráscsoport legegyszerűbb hello először. hello következő HTTP-kérelem hoz létre egy erőforráscsoportot a régió/hely az Ön által választott, és hozzáad egy címke tooit.

(Cserélje YOUR_ACCESS_TOKEN, ELŐFIZETÉS_AZONOSÍTÓJA, RESOURCE_GROUP_NAME a tényleges hozzáférési jogkivonat, előfizetés-azonosító és hello toocreate kívánt erőforráscsoport neve)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Sikeres ellenőrzés esetén töltse le a válaszhoz, amely hasonló toohello válasz a következő:

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Sikeresen létrehozott egy erőforráscsoportot az Azure-ban. Gratulálunk!

### <a name="deploy-resources-tooa-resource-group-using-a-resource-manager-template"></a>A Resource Manager sablonnal erőforrások tooa erőforráscsoport telepítése
A Resource Manager telepítheti az erőforrásokat sablonok használatával. A sablon meghatározza a több erőforrás és függőségi viszonyaikat. Az ebben a szakaszban feltételezzük, hogy ismeri a Resource Manager-sablonok, és csak megmutatjuk, hogyan toomake hello API hívása toostart központi telepítés. Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).

Központi telepítési sablon sok más API-k hívása toohow nem különböznek. Egy fontos eleme a központi telepítési sablon elég hosszú időt vehet igénybe. hello API-hívás csak adja vissza, és azt fel tooyou fejlesztői tooquery állapotához tartozó hello telepítési toofind ki, amikor hello a telepítés befejezése. További információkért lásd: [nyomon követheti a aszinkron Azure műveleteket](resource-manager-async-operations.md).

A jelen példában használjuk elérhető egy nyilvánosan elérhetővé sablon [GitHub](https://github.com/Azure/azure-quickstart-templates). hello sablon használjuk telepíti egy Linux virtuális gép toohello USA nyugati régiója területet. Annak ellenére, hogy ebben a példában egy nyilvános tárházban, például a Githubon elérhető sablont használ, ehelyett átadhatók hello teljes sablon hello kérelem részeként. Vegye figyelembe, hogy az általunk paraméterértékek hello kérelem hello belül használt sablon üzembe.

(A név felülírandó ELŐFIZETÉS_AZONOSÍTÓJA, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD és DNS_NAME_FOR_PUBLIC_IP toovalues kérelmét megfelelő)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

hello hosszú JSON-válasz ehhez a kérelemhez nincs megadva tooimprove olvashatóság érdekében a jelen dokumentáció le lett. hello válasz hello sablonalapú telepítés létrehozott adatait tartalmazza.

## <a name="next-steps"></a>Következő lépések

- toolearn aszinkron REST műveleteinek, kezelésével kapcsolatban lásd: [nyomon követheti a aszinkron Azure műveleteket](resource-manager-async-operations.md).
