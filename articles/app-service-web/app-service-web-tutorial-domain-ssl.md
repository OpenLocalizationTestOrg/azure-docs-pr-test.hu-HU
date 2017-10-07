---
title: "aaaAdd egyéni tartomány- és SSL tooan Azure web app |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprepare az Azure web app toogo éles adja hozzá a vállalati arculat megjelenítése. Hello egyéni tartomány nevét (kreatív tartomány) tooyour webalkalmazás rendelve, és biztosíthatja egy egyéni SSL-tanúsítvánnyal."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 03/29/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2679ed8b2dbbeba0b128c1a3ec01148f97c35342
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-domain-and-ssl-tooan-azure-web-app"></a>Adja hozzá az egyéni tartomány- és SSL tooan Azure-webalkalmazásban

Az oktatóanyag bemutatja, hogyan tooquickly egy egyéni tartomány nevét tooyour Azure-webalkalmazásban rendelve, és ezután biztonságos egyéni SSL-tanúsítvánnyal. 

## <a name="before-you-begin"></a>Előkészületek

Ez a minta futtatásához telepítse a hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) helyileg.

Szükség rendszergazdai hozzáférés toohello DNS konfigurálása lapon a megfelelő tartomány szolgáltatóhoz. Például tooadd `www.contoso.com`, toobe képes tooconfigure DNS-bejegyzéseket kell `contoso.com`.

Végül kell egy érvényes. PFX-fájl _és_ a kívánt tooupload hello SSL-tanúsítvány jelszavát, majd kötést. Az SSL-tanúsítvány konfigurált toosecure hello egyéni tartománynevet kell lennie. A fenti példa hello, az SSL-tanúsítvány kell biztonságossá tenni `www.contoso.com`. 

## <a name="step-1---create-an-azure-web-app"></a>1. lépés – Azure-webalkalmazás létrehozása

### <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Folyamatban van, most a folyamatban toouse hello Azure CLI 2.0 egy terminálablakot toocreate hello erőforrások a szükséges toohost a Node.js-alkalmazás az Azure-ban.  Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat. 

```azurecli 
az login 
``` 
   
### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot   
Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create). Az Azure-erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat (például webappokat, adatbázisokat és tárfiókokat). 

```azurecli
az group create --name myResourceGroup --location westeurope 
```

toosee milyen lehetséges értékei, akkor is használhat `---location`, használja a hello `az appservice list-locations` Azure CLI parancs.

## <a name="create-an-app-service-plan"></a>App Service-csomag létrehozása

Hozzon létre egy App Service-csomag hello [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) parancsot. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

hello alábbi példakód létrehozza az App Service-csomag nevű `myAppServicePlan` hello segítségével **alapvető** tarifacsomagra vált.

az App Service-csomagot hozzon létre--name myAppServicePlan--B1 erőforráscsoport myResourceGroup--termékváltozat

Hello App Service-csomag létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg. 

```json 
{ 
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
    "kind": "app", 
    "location": "West Europe", 
    "sku": { 
    "capacity": 1, 
    "family": "B", 
    "name": "B1", 
    "tier": "Basic" 
    }, 
    "status": "Ready", 
    "type": "Microsoft.Web/serverfarms" 
} 
``` 

## <a name="create-a-web-app"></a>Webalkalmazás létrehozása

Most, hogy az App Service-csomag létrehozása után hozzon létre egy webalkalmazás hello `myAppServicePlan` App Service-csomag. hello web app ad meg egy üzemeltetési terület toodeploy még a kódot, valamint tooview hello biztosítja egy URL-CÍMÉT, központilag telepített alkalmazás. Használjon hello [az App Service webalkalmazás létrehozása](/cli/azure/appservice/web#create) parancs toocreate hello webalkalmazás. 

Hello parancs az alábbi, a adjon helyettesítse a saját egyedi alkalmazásnévvel hello megtapasztalhatja `<app_name>` helyőrző. A egyedi név, hello nevének kell toobe egyedi az Azure-ban minden alkalmazások között használható hello alapértelmezett tartomány nevét hello webalkalmazás, hello részeként. Ki kell alakítani az tooyour felhasználók bármilyen egyéni DNS bejegyzés toohello webalkalmazás később is leképezheti. 

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan 
```

Hello webalkalmazás létrehozásakor hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg. 

```json 
{ 
    "clientAffinityEnabled": true, 
    "defaultHostName": "<app_name>.azurewebsites.net", 
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>", 
    "isDefaultContainer": null, 
    "kind": "app", 
    "location": "West Europe", 
    "name": "<app_name>", 
    "repositorySiteName": "<app_name>", 
    "reserved": true, 
    "resourceGroup": "myResourceGroup", 
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
    "state": "Running", 
    "type": "Microsoft.Web/sites", 
} 
```

A JSON-kimenetét hello `defaultHostName` a webalkalmazás alapértelmezett tartomány nevét jeleníti meg. A böngészőben nyissa meg a toothis cím.

```
http://<app_name>.azurewebsites.net 
``` 
 
![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-created.png)  

## <a name="step-2---configure-dns-mapping"></a>2. lépés – konfigurálja a DNS-hozzárendelése

Ebben a lépésben ad hozzá egy leképezése az egy egyéni tartomány tooyour webes alkalmazás alapértelmezett tartomány nevét, `<app_name>.azurewebsites.net`. Általában akkor hajtsa végre ezt a lépést a tartomány szolgáltató webhelyéről. Minden tartományregisztráló webhely némileg eltér, nézze át a szolgáltató dokumentációját. Azonban az alábbiakban néhány általános irányelveket. 

### <a name="navigate-tootoodns-management-page"></a>Keresse meg a tootooDNS kezelése lap

Első lépésként jelentkezzen be tooyour tartomány regisztráló webhelyén.  

Ezt követően hello lapon található DNS-rekordok kezelése. Keressen hivatkozásokat vagy feliratú hello hely területek **tartománynév**, **DNS**, vagy **neve kezelő**. Gyakran, megtalálhatja hello hivatkozás megtekintésével a fiók adatait, és ezután keres, mint egy hivatkozást **a tartományok**.

Ezen a lapon talál, keresse meg a hivatkozást, amellyel lehetővé teszi, hogy hozzáadása vagy szerkesztése a DNS-rekordokat. Lehetséges, hogy egy **zónafájl** vagy **DNS-rekordok** hivatkozásra, vagy egy **speciális konfigurációs** hivatkozásra.

### <a name="create-a-cname-record"></a>Hozzon létre egy CNAME rekordot

Adjon hozzá egy CNAME rekordot, amely leképezhető hello kívánt altartomány neve tooyour webes alkalmazás alapértelmezett tartomány nevét (`<app_name>.azurewebsites.net`, ahol `<app_name>` egyedi név az alkalmazásban).

A hello `www.contoso.com` például létrehozhat egy olyan CNAME REKORDOT, amely leképezhető hello `www` állomásnév túl`<app_name>.azurewebsites.net`.

## <a name="step-3---configure-hello-custom-domain-on-your-web-app"></a>3. lépés - a webalkalmazás hello egyéni tartománynév konfigurálása

Amikor befejezte a hello állomásnév leképezés konfigurálása a tartomány szolgáltató webhelyéről, most készen áll a tooconfigure hello egyéni tartomány a webalkalmazásban. Használjon hello [hozzáadása az App Service web config állomásnév](/cli/azure/appservice/web/config/hostname#add) parancs tooadd ezt a konfigurációt. 

Hello parancs az alábbi, adjon helyettesítse `<app_name>` egyedi alkalmazásnévvel és < your_custom_domain > hello teljesen minősített egyéni tartomány nevét (pl. `www.contoso.com`). 

```azurecli
az appservice web config hostname add --webapp <app_name> --resource-group myResourceGroup --name <your_custom_domain>
```

hello egyéni tartomány most már teljes mértékben csatlakoztatott tooyour webalkalmazás áll. A böngészőben nyissa meg a toohello egyéni tartománynevet. Példa:

```
http://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-custom-domain.png)  

## <a name="step-4---bind-a-custom-ssl-certificate-tooyour-web-app"></a>4. lépés: a kötés SSL tanúsítvány tooyour egyéni webalkalmazás

Most már rendelkezik egy Azure webalkalmazás hello tartománynévvel kívánt hello böngésző címsorába. Azonban ha manuálisan lép toohello `https://<your_custom_domain>` , akkor a hibaüzenet egy tanúsítvány. 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-cert-error.png)  

Ez a hiba akkor fordul elő, mert a webalkalmazás még nem rendelkezik SSL-tanúsítvány kötést, amely megfelel az egyéni tartománynevet. Azonban nem kérek hiba, ha manuálisan lép túl`https://<app_name>.azurewebsites.net`. Ennek az az oka az alkalmazást, valamint az összes Azure App Service apps védett hello SSL-tanúsítvánnyal hello `*.azurewebsites.net` helyettesítő tartomány alapértelmezés szerint. 

A webalkalmazás tooaccess sorrendben az egyéni tartománynév alapján, az egyéni tartomány toohello webalkalmazás toobind hello SSL-tanúsítvány szükséges. Az ebben a lépésben fogja végrehajtani. 

### <a name="upload-hello-ssl-certificate"></a>Hello SSL-tanúsítvány feltöltése

Az egyéni tartomány tooyour webalkalmazás hello SSL-tanúsítvány feltöltése hello segítségével [az App Service web config ssl feltöltés](/cli/azure/appservice/web/config/ssl#upload) parancsot.

Hello parancs az alábbi, adjon helyettesítse `<app_name>` az egyedi alkalmazás nevű `<path_to_ptx_file>` hello elérési tooyour együtt. PFX-fájlt, és `<password>` a tanúsítvány jelszóval. 

```azurecli
az appservice web config ssl upload --name <app_name> --resource-group myResourceGroup --certificate-file <path_to_pfx_file> --certificate-password <password> 
```

Hello tanúsítvány töltheti fel, ha a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:

```json
{
  "cerBlob": null,
  "expirationDate": "2018-03-29T14:12:57+00:00",
  "friendlyName": "",
  "hostNames": [
    "www.contoso.com"
  ],
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/cert
ificates/9FD1D2D06E2293673E2A8D1CA484A092BD016D00__West Europe_myResourceGroup",
  "issueDate": "2017-03-29T14:12:57+00:00",
  "issuer": "www.contoso.com",
  "keyVaultId": null,
  "keyVaultSecretName": null,
  "keyVaultSecretStatus": "Initialized",
  "kind": null,
  "location": "West Europe",
  "name": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00__West Europe_myResourceGroup",
  "password": null,
 "pfxBlob": null,
  "publicKeyHash": null,
  "resourceGroup": "myResourceGroup",
  "selfLink": null,
  "serverFarmId": null,
  "siteName": null,
  "subjectName": "www.contoso.com",
  "tags": null,
  "thumbprint": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00",
  "type": "Microsoft.Web/certificates",
  "valid": null
}
```

A JSON-kimenetét hello `thumbprint` a feltöltött tanúsítvány ujjlenyomata látható. Másolja a következő lépéshez hello értéke.

### <a name="bind-hello-uploaded-ssl-certificate-toohello-web-app"></a>A kötés hello feltöltött SSL-tanúsítvány toohello webalkalmazás

A webalkalmazás már hello egyéni tartománynevet, és egy SSL-tanúsítvány, amely biztosítja, hogy egyéni tartományt is rendelkezik. hello csak dolog bal oldali toodo toobind hello feltöltött tanúsítvány toohello webalkalmazás. Hello használatával teheti [az App Service web config ssl-kötés](/cli/azure/appservice/web/config/ssl#bind) parancsot.

Hello parancs az alábbi, adjon helyettesítse `<app_name>` saját egyedi alkalmazás nevét és `<thumbprint-from-previous-output>` az előző parancs hello kapott hello tanúsítvány ujjlenyomata. 

az App Service web config ssl-kötés--name < alkalmazásnév >--erőforráscsoport myResourceGroup – tanúsítvány-ujjlenyomata < ujjlenyomat-a-előző-kimeneti >--ssl-típus SNI

Ha hello tanúsítvány kötött tooyour webalkalmazás, hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:

{"availabilityState": "Normál", "clientAffinityEnabled": true, "clientCertEnabled": hamis, "cloningInfo": null, "containerSize": 0, "dailyMemoryTimeQuota": 0, "defaultHostName": "< alkalmazásnév >. azurewebsites.net", "enabled": true, "enabledHostNames": ["www.contoso.com", "< alkalmazásnév >. azurewebsites.net", "< alkalmazásnév >. scm.azurewebsites.net"], "gatewaySiteName": null, a "hostNameSslStates": [{"hostType": "Standard", "name": "< alkalmazásnév >. azurewebsites.net", "sslState": "Letiltott", "ujjlenyomata": null, "toUpdate": null, "virtuális IP-cím": null}, {"hostType": "Tárház", "name": "< alkalmazásnév >. scm.azurewebsites.net", "sslState": "Letiltva" "ujjlenyomata": null, "toUpdate": null, "virtuális IP-cím": null}, {"hostType": "Standard", "name": "www.contoso.com", "sslState": "SniEnabled", "ujjlenyomata": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00", "toUpdate": null, "virtuális IP-cím": null}], "hostNames": ["www.contoso.com", "< alkalmazásnév >. azurewebsites.net"], "hostNamesDisabled": false, "hostingEnvironmentProfile": null, "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s / < alkalmazásnév >", "isDefaultContainer": null, a "fajta": "Webalkalmazás", "lastModifiedTimeUtc": "2017-03-29T14:36:18.803333", "hely": "Nyugati Európa", "maxNumberOfWorkers": null, "Mikroszolgáltatási": "false", "név": "< alkalmazásnév >" "outboundIpAddresses": "13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1", "premiumAppDeployed": null, "repositorySiteName": "< alkalmazásnév >", "fenntartott": false, "erőforráscsoport": "contoso.com", "scmSiteAlsoStopped": hamis, "serverFarmId": "/ előfizetések/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/szolgáltatók/Microsof t.Web/serverfarms/myAppServicePlan", "siteConfig": null, a "slotSwapStatus": null, a "állapot": "Fut", "suspendedTill": null, a "címkék": null, "targetSwapSlot": null, a "trafficManagerHostNames": null, "type": "Microsoft.Web/sites", "usageState": "Normál"}

A böngészőben nyissa meg az egyéni tartománynevet tooHTTPS végpontja. Példa:

```
https://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-ssl-success.png)  

## <a name="more-resources"></a>További erőforrások

[Vásároljon és egyéni tartománynév beállítása az Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[vásárolnia és SSL-tanúsítvány konfigurálása az Azure App Service](web-sites-purchase-ssl-web-site.md)
