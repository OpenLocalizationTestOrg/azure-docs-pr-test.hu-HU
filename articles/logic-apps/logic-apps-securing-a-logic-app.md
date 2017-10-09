---
title: "aaaSecure hozzáférési tooAzure Logic Apps |} Microsoft Docs"
description: "Adja hozzá a biztonsági hozzáférési tootriggers, bemeneti adatokat és kimenetek, művelet paramétereinek és a munkafolyamatok az Azure Logic Apps szolgáltatás védelmére."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/22/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: abda2179e4cc2d2295cd8332ec017c848a456264
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-access-tooyour-logic-apps"></a>Biztonságos hozzáférés tooyour logic Apps alkalmazások

Nincsenek sok eszközök elérhető toohelp mindig lássa a logikai alkalmazást.

* Biztonságossá tétele a hozzáférés tootrigger logikai alkalmazás (HTTP kérelem eseményindító)
* Biztonságossá tétele a hozzáférés toomanage, szerkesztése, vagy olvassa el a logikai alkalmazás
* Hozzáférés toocontents bemeneti és kimeneti futtató biztonságossá tétele
* Paraméterek vagy a műveletek a munkafolyamat belül biztonságossá tétele
* Biztonságossá tétele a hozzáférés tooservices, amely kéréseket fogadni munkafolyamat

## <a name="secure-access-tootrigger"></a>Biztonságos hozzáférés tootrigger

Használata esetén a logikai alkalmazás, amely akkor következik be, a HTTP-kérelem ([kérelem](../connectors/connectors-native-reqres.md) vagy [Webhook](../connectors/connectors-native-webhook.md)), korlátozhatja a hozzáférést, hogy csak az arra jogosult ügyfelek is érvényesítést hello logikai alkalmazást. Logikai alkalmazás az összes kérelem titkosítja és védi SSL.

### <a name="shared-access-signature"></a>Közös hozzáférésű Jogosultságkódot

Minden logikai alkalmazás kérés végpontja tartalmaz egy [közös hozzáférésű Jogosultságkód (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) hello URL-cím részeként. Minden egyes URL-cím tartalmaz egy `sp`, `sv`, és `sig` lekérdezési paraméter. Engedélyek vannak megadva `sp`, és meg tooHTTP módszer engedélyezett, `sv` van hello használt verzió toogenerate, és `sig` használt tooauthenticate hozzáférés tootrigger van. hello aláírás generálta hello SHA-256 algoritmus hello URL-cím elérési út és a tulajdonságok a titkos kulccsal. hello titkos kulcs soha nem közzétett és közzé, és a titkosított és hello logikai alkalmazás tárolt. A Logic Apps alkalmazást csak hello titkos kulccsal létrehozott érvényes aláírást tartalmazó eseményindítók engedélyezi.

#### <a name="regenerate-access-keys"></a>Tárelérési kulcsok újragenerálása

Új biztonsági kulcsot következő bármikor hello REST API-t vagy az Azure portálon keresztül is generálja újra. Az aktuális URL-címet korábban kulccsal hello régi létrehozott érvénytelenített és már nem jogosult toofire hello logikai alkalmazás.

1. Hello Azure-portálon nyissa meg azt szeretné, hogy a kulcs tooregenerate hello logikai alkalmazás
1. Kattintson a hello **hívóbetűk** menüpont alatt **beállítások**
1. Válassza ki a kulcs tooregenerate hello és teljes hello folyamat

URL-címek lekérése után hello új elérési kulcs újragenerálása aláírt esetén.

#### <a name="creating-callback-urls-with-an-expiration-date"></a>A lejárati dátum visszahívási URL-címek létrehozása

Hello URL-címet oszt meg más felek, ha a URL-címek meghatározott kulcsokkal és a lejárati dátumok igény szerint is létrehozhat. Ezután zökkenőmentesen kulcsok visszaállítani, vagy hozzáférési toofire biztosítása az alkalmazások korlátozott tooa bizonyos timespan van. Adott URL esetén hello keresztül is megadhat a lejárati dátumot [REST API-t a logic apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers):

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Hello törzsében, tartalmazza a hello tulajdonságot `NotAfter` karakterláncként JSON dátum, amely adja vissza egy visszahívási URL-címet, amely csak akkor érvényes, amíg hello `NotAfter` dátuma és időpontja.

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a>Elsődleges vagy másodlagos titkos kulccsal URL-címek létrehozása

Létrehozni, vagy a visszahívási kérelem-alapú eseményindítók URL-listában, mely kulcs toouse toosign hello URL-cím is megadható.  Létrehozhat egy URL-cím, egy adott kulcs keresztül hello által aláírt [REST API-t a logic apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers) az alábbiak szerint:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Hello törzsében, tartalmazza a hello tulajdonságot `KeyType` , vagyis `Primary` vagy `Secondary`.  Ez visszaadja a megadott hello biztonságos kulcs által aláírt URL-címet.

### <a name="restrict-incoming-ip-addresses"></a>Bejövő IP-címek korlátozása

Továbbá a közös hozzáférésű Jogosultságkód toohello, érdemes toorestrict logikai alkalmazás hívása csak az adott ügyfelektől.  Például ha Ön kezeli a végpont Azure API Management keresztül, korlátozhatja a hello logic app tooonly hello kérés elfogadása, amikor hello kérelem hello API Management példány IP-cím származik.

Ez a beállítás konfigurálható hello logic app beállítások:

1. A hello Azure-portálon nyissa meg a kívánt tooadd IP-címkorlátozások hello logikai alkalmazás
1. Kattintson a hello **hozzáférés-vezérlési konfiguráció** menüpont alatt **beállítások**
1. Adja meg az IP cím tartományokhoz toobe hello eseményindító által elfogadott hello listája

Egy érvényes IP-címtartomány hello formátumban veszi `192.168.1.1/255`. Ha hello logic app tooonly tűz beágyazott logika alkalmazásként, jelölje be a hello **csak más logic apps** lehetőséget. Ez a beállítás egy üres tömb toohello erőforrás, ami azt jelenti, csak hívást hello szolgáltatás magát (szülő logic apps) tűz sikeresen ír.

> [!NOTE]
> Továbbra is futtatható a logikai alkalmazást az kérelem eseményindító hello REST API-n keresztül / felügyeleti `/triggers/{triggerName}/run` IP függetlenül. Ebben a forgatókönyvben hello Azure REST API hitelesítést igényel, és az összes esemény hello Azure napló jelenik meg. Ennek megfelelően beállított hozzáférés-vezérlési házirendek.

#### <a name="setting-ip-ranges-on-hello-resource-definition"></a>IP-címtartományok hello erőforrás-definíció beállítása

Ha használ egy [központi telepítési sablont](logic-apps-create-deploy-template.md) a központi telepítések, hello IP-címtartomány beállításokat lehet megadni a hello erőforrás sablon tooautomate.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "triggers": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}

```

### <a name="adding-azure-active-directory-oauth-or-other-security"></a>Azure Active Directory, az OAuth és az egyéb biztonsági

további hitelesítési protokollok egy logikai alkalmazás fölött tooadd [Azure API Management](https://azure.microsoft.com/services/api-management/) kínál gazdag figyelési, biztonsági, házirend, és bármely végpont hello funkció tooexpose dokumentációja, az API-k logikai alkalmazás. Az Azure API Management is elérhetővé teheti a saját vagy nyilvános végpontja hello logikai alkalmazás, hogy az Azure Active Directory, a tanúsítványt, az OAuth vagy a más biztonsági szabványok használ. Amikor egy kérelem érkezik, az Azure API Management hello kérelem toohello logikai alkalmazás (hajt végre semmilyen szükséges átalakítások vagy üzenetsoroktól korlátozások) továbbítja. Hello beállításai hello logic app tooonly lehetővé teszik hello logic app toobe elindul az API Management bejövő IP-címtartomány is használhatja.

## <a name="secure-access-toomanage-or-edit-logic-apps"></a>Biztonságos hozzáférés a logic apps toomanage vagy szerkesztése

Hozzáférési toomanagement műveleteket logikai alkalmazás korlátozhatja, hogy csak bizonyos felhasználók vagy csoportok hello erőforrás képes tooperform műveleteket. A Logic apps használata hello Azure [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md) szolgáltatás, és testre szabható a hello ugyanazokat az eszközöket.  Az előfizetés tooas tagjai is rendelhet néhány beépített szerepkörök állnak rendelkezésre:

* **Logic App közreműködői** -hozzáférés tooview, szerkesztése és a logikai alkalmazás frissítése biztosít.  Nem lehet eltávolítani a hello erőforrás, vagy felügyeleti műveleteket.
* **Logic App operátor** – megtekintheti hello logikai alkalmazás és futtatási előzményei, és engedélyezését vagy letiltását.  Nem szerkeszthetők vagy hello definíciójának frissítése.

Is [Azure erőforrás-zárolás](../azure-resource-manager/resource-group-lock-resources.md) tooprevent módosítása vagy törlése a logic Apps alkalmazásokat. Ez a lehetőség akkor értékes tooprevent éles erőforrásokat módosítások vagy törlésekhez.

## <a name="secure-access-toocontents-of-hello-run-history"></a>Biztonságos hozzáférés toocontents a hello futtatási előzményei

A bemeneti vagy kimeneti hozzáférés toocontents előző futtatása toospecific IP-címtartományok korlátozhatja.  

Egy munkafolyamat sorozaton belül összes adata titkosításra kerül, az átvitel során, és inaktív. Hívás toorun előzményeit történik, amikor hello szolgáltatás hitelesíti hello kérelmet, és hivatkozások toohello kérés és válasz bemenetekhez és kimenetekhez biztosít. Ez a hivatkozás lehet védett, csak kérelmek tooview tartalmából áll a kijelölt IP-címtartomány hello tartalmát adja vissza. Ez a funkció további hozzáférés-vezérléshez is használhatja. Akkor kell megadni egy IP-címet, például `0.0.0.0` , nem sikerült elérni a bemenetek/kimenetek. Csak a személy rendszergazdai jogosultságokkal tudta eltávolítani ezt a korlátozást, és hello lehetőséget biztosít a "just-in-time" hozzáférési tooworkflow tartalmát.

Ez a beállítás konfigurálható hello erőforrás beállításainak hello Azure-portálon:

1. A hello Azure-portálon nyissa meg a kívánt tooadd IP-címkorlátozások hello logikai alkalmazás
1. Kattintson a hello **hozzáférés-vezérlési konfiguráció** menüpont alatt **beállítások**
1. Adja meg a hozzáférés toocontent IP-címtartományát hello listája

#### <a name="setting-ip-ranges-on-hello-resource-definition"></a>IP-címtartományok hello erőforrás-definíció beállítása

Ha használ egy [központi telepítési sablont](logic-apps-create-deploy-template.md) a központi telepítések, hello IP-címtartomány beállításokat lehet megadni a hello erőforrás sablon tooautomate.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "contents": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}
```

## <a name="secure-parameters-and-inputs-within-a-workflow"></a>Biztonságos paraméterek és a bemenetek egy munkafolyamaton belül

Érdemes lehet tooparameterize telepítési munkafolyamat definícióját az egyes funkcióit környezetek között. Egyes paraméterek előfordulhat, hogy is biztonságos paraméterek nem szeretné, hogy tooappear egy munkafolyamatot, például egy ügyfél-azonosító és a titkos ügyfélkulcs szerkesztésekor [Azure Active Directory hitelesítési](../connectors/connectors-native-http.md#authentication) egy HTTP-művelet.

### <a name="using-parameters-and-secure-parameters"></a>Paraméterek és biztonságos paraméterek

tooaccess hello futásidőben, egy erőforrás paraméter értékének hello [munkafolyamatdefiníciós nyelve](http://aka.ms/logicappsdocs) biztosít egy `@parameters()` műveletet. Akkor is, [adja meg a paraméterek hello erőforrás központi telepítési sablont a](../azure-resource-manager/resource-group-authoring-templates.md#parameters). De ha hello paraméter típusa, adja meg `securestring`, hello paraméter hello többi hello erőforrás-definíció nem adható vissza, és nem lesznek elérhetők a telepítés utáni hello erőforrás megtekintésével.

> [!NOTE]
> Ha a paraméter hello fejlécek vagy egy kérelem törzse hello paraméter is láthatják őket futtatása hello előzmények és kimenő HTTP-kérelem elérésével. Győződjön meg arról, hogy tooset a tartalom-hozzáférési házirendek ennek megfelelően.
> Engedélyezési fejléceket keresztül bemeneti vagy kimeneti soha nem láthatók el. Ezért ha hello titkos kulcs használatban van ott, hello titkos kulcs nincs lekérhető.

#### <a name="resource-deployment-template-with-secrets"></a>Erőforrás központi telepítési sablont a titkos kulcsok

hello alábbi példa azt mutatja a központi telepítés egy biztonságos paramétere hivatkozó `secret` futásidőben. Egy külön paraméterek fájlban hello hello környezet értéket kell megadni `secret`, vagy használjon [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) tooretrieve titkok, idő központi telepítése.

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "secretDeploymentParam": {
      "type": "securestring"
    }
  },
  "variables": {},
  "resources": [
    {
      "name": "secret-deploy",
      "type": "Microsoft.Logic/workflows",
      "location": "westus",
      "tags": {
        "displayName": "LogicApp"
      },
      "apiVersion": "2016-06-01",
      "properties": {
        "definition": {
          "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "actions": {
            "Call_External_API": {
              "type": "http",
              "inputs": {
                "headers": {
                  "Authorization": "@parameters('secret')"
                },
                "body": "This is hello request"
              },
              "runAfter": {}
            }
          },
          "parameters": {
            "secret": {
              "type": "SecureString"
            }
          },
          "triggers": {
            "manual": {
              "type": "Request",
              "kind": "Http",
              "inputs": {
                "schema": {}
              }
            }
          },
          "contentVersion": "1.0.0.0",
          "outputs": {}
        },
        "parameters": {
          "secret": {
            "value": "[parameters('secretDeploymentParam')]"
          }
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="secure-access-tooservices-receiving-requests-from-a-workflow"></a>Biztonságos hozzáférés tooservices fogadó kérelmeinek munkafolyamat

Nincsenek biztonságos bármely végpont hello logic app kell tooaccess számos módon toohelp.

### <a name="using-authentication-on-outbound-requests"></a>A kimenő kérelmek-hitelesítéssel

Amikor egy HTTP-, HTTP + Swagger (nyílt API-t), vagy a Webhook művelet olyan hitelesítési toohello kérelmet küldött is hozzáadhat. Alapszintű hitelesítés, tanúsítvány vagy Azure Active Directory-hitelesítés tartalmazhatnak. Hogyan tooconfigure a hitelesítés található részleteinek [ebben a cikkben](../connectors/connectors-native-http.md#authentication).

### <a name="restricting-access-toologic-app-ip-addresses"></a>Hozzáférés toologic app IP-címek korlátozása

A logic Apps alkalmazásokból összes hívások régiónként eltérő IP-címek meghatározott készletének határozza meg. Hozzáadhat további szűrési tooonly azoktól, amelyeket a kijelölt IP-címek kérelmek fogadására. Egy adott IP-címek listáját lásd: [logic app korlátozásai és konfigurációja](logic-apps-limits-and-config.md#configuration).

### <a name="on-premises-connectivity"></a>Helyszíni kapcsolatok

A Logic apps több szolgáltatások tooprovide biztonságos és megbízható integrálva a helyszíni kommunikációt biztosítanak.

#### <a name="on-premises-data-gateway"></a>Helyi adatátjáró

Sok felügyelt összekötők logic apps a tooon helyszíni rendszerekre, például File System, a SQL, a SharePoint, a DB2 és egyéb biztonságos kapcsolatot biztosít. hello átjáró továbbítja a titkosított csatornákon keresztül hello Azure Service Bus helyszíni forrásból származó adatokat. Az összes forgalom származik, mint hello átjáró ügynök biztonságos kimenő forgalmát. További információ [hello adatátjáró működése](logic-apps-gateway-install.md#gateway-cloud-service).

#### <a name="azure-api-management"></a>Azure API Management

[Az Azure API Management](https://azure.microsoft.com/services/api-management/) rendelkezik a helyszíni kapcsolati lehetőségek, többek között a pont-pont VPN- és ExpressRoute integráció biztonságos proxy és a kommunikáció tooon helyszíni rendszerekben. Hello Logic App Designer az API-k egy munkafolyamaton belül az Azure API Management kitett gyors hozzáférést biztosító tooon helyszíni rendszerekben gyorsan kiválaszthatja.

#### <a name="hybrid-connections-from-azure-app-service"></a>Hibrid kapcsolatok az Azure App Service

Azure API és a Web apps toocommunicate helyszíni hello helyszíni hibrid kapcsolat funkció használható.  Hibrid kapcsolatok és hogyan tooconfigure található [ebben a cikkben](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="next-steps"></a>Következő lépések
[Központi telepítési sablon létrehozása](logic-apps-create-deploy-template.md)  
[Kivételkezelés](logic-apps-exception-handling.md)  
[Logikai alkalmazások figyelése](logic-apps-monitor-your-logic-apps.md)  
[Logic app hibák és problémák diagnosztizálása](logic-apps-diagnosing-failures.md)  
