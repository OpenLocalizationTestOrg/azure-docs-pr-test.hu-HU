---
title: egy Azure Application Gateway - sablonok aaaCreate |} Microsoft Docs
description: "Ezen a lapon nyújt útmutatást toocreate Azure Alkalmazásátjáró hello Azure Resource Manager-sablon használatával"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: fc18e553852551326d6a302abe2c7f8a08c2eb6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-resource-manager-template"></a>Alkalmazásátjáró létrehozása hello Azure Resource Manager-sablon használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Klasszikus Azure PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sablon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Az Azure Application Gateway egy 7. rétegbeli terheléselosztó. Azt is biztosít feladatátvételi és HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között hello felhőalapú vagy helyszíni legyenek. Az Application Gateway számos alkalmazáskézbesítési vezérlőszolgáltatást (ADC) biztosít, beleértve a HTTP-terheléselosztást, a cookie-alapú munkamenet-affinitást, a Secure Sockets Layer- (SSL-) alapú kiszervezést, az egyéni állapotmintákat, a többhelyes támogatást és még sok mást. látogasson el a támogatott funkciók teljes listáját toofind [Alkalmazásátjáró áttekintése](application-gateway-introduction.md)

Ez a cikk útmutatást nyújt a letöltés és módosítani egy meglévő Azure Resource Manager-sablont a Githubból, és hello sablont a Githubból, PowerShell és hello Azure CLI telepítése.

Egyszerűen telepítése Azure Resource Manager-sablon hello közvetlenül a Githubból módosítások nélkül, ugorjon a toodeploy egy sablont a Githubból.

## <a name="scenario"></a>Forgatókönyv

Ebben a forgatókönyvben az alábbiakat fogja tenni:

* Hozzon létre egy alkalmazás webalkalmazási tűzfal.
* Létrehoz egy VirtualNetwork1 nevű virtuális hálózatot a 10.0.0.0/16 egy fenntartott CIDR-blokkjával.
* Létrehoz egy Appgatewaysubnet nevű alhálózatot, amelynek a CIDR-blokkja 10.0.0.0/28 lesz.
* Két korábban konfigurált háttér IP-címek hello webkiszolgálókhoz beállítani kívánt tooload egyenleg hello forgalom. A sablon példában hello háttér IP-címek 10.0.1.10 és 10.0.1.11.

> [!NOTE]
> Ezek a beállítások olyan hello paraméterek ehhez a sablonhoz. toocustomize hello sablon, módosíthatja szabályok, hello figyelő, SSL és egyéb beállítások hello azuredeploy.json fájl.

![Forgatókönyv](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-hello-azure-resource-manager-template"></a>Töltse le és hello Azure Resource Manager sablon ismertetése

Töltse le a hello meglévő Azure Resource Manager sablon toocreate virtuális hálózat és két alhálózat a Githubból, hajtsa végre a módosításokat, előfordulhat, hogy szeretné, és újra felhasználhatja. toodo Igen, használja a lépéseket követve hello:

1. Keresse meg a túl[Alkalmazásátjáró létrehozása webalkalmazási tűzfal engedélyezve van a](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).
1. Kattintson az **azuredeploy.json**, majd a **RAW** elemre.
1. Mentés hello fájl tooa helyi mappába a számítógépen.
1. Ha ismeri az Azure Resource Manager-sablonok, ugorjon a toostep 7.
1. Nyissa meg a mentett hello fájlt, és nézze meg alatt látható tartalmakat hello **paraméterek** sorban
1. Az Azure Resource Manager-sablonparaméterek az üzembe helyezés során kitölthető paraméterek helyőrzőiként működnek.

  | Paraméter | Leírás |
  | --- | --- |
  | **subnetPrefix** |Hello alkalmazás átjáró-alhálózat CIDR-blokkja. |
  | **applicationGatewaySize** | Hello Alkalmazásátjáró mérete.  WAF csak akkor engedélyezett, közepes és nagy méretű. |
  | **backendIpaddress1** |IP-címe hello első webkiszolgálón. |
  | **backendIpaddress2** |Hello második webalkalmazás-kiszolgáló IP-címe. |
  | **wafEnabled** | Toodetermine a beállítást, ha WAF engedélyezve van.|
  | **wafMode** | Webalkalmazási tűzfal hello módját.  Elérhető lehetőségek **megelőzési** vagy **észlelési**.|
  | **wafRuleSetType** | WAF szabálykészletben típusa.  Jelenleg OWASP hello csak akkor támogatja a beállítást. |
  | **wafRuleSetVersion** |Szabálykészletben verziója. Program 2.2.9-es és 3.0 OWASP CRS opció jelenleg hello támogatott. |

1. Hello tartalom alapján ellenőrizze **erőforrások** és a hirdetmény hello következő tulajdonságai:

   * **type**. Hello sablon által létrehozott erőforrás típusát. Ebben az esetben a hello típus: `Microsoft.Network/applicationGateways`, amely olyan átjárót jelöli.
   * **Név** Hello erőforrás nevét. Értesítés hello használata `[parameters('applicationGatewayName')]`, ami azt jelenti, hogy hello neve valósul meg bemeneti adatként, vagy egy paraméterfájl üzembe helyezése során.
   * **properties**. Hello erőforrás tulajdonságainak listája. Ez a sablon által hello virtuális hálózat és a nyilvános IP-cím application gateway létrehozása során.

   > [!NOTE]
   > További információt a sablonok: [Resource Manager-sablonok referenciája](/templates/)

1. Lépjen vissza túl[https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).
1. Kattintson a **azuredeploy-parameters.json**, és kattintson a **RAW**.
1. Mentés hello fájl tooa helyi mappába a számítógépen.
1. Nyissa meg a mentett hello fájlt, és módosítsa a hello hello paraméter értékét. A következő értékek toodeploy hello Alkalmazásátjáró leírt hello használata.

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "addressPrefix": {
            "value": "10.0.0.0/16"
            },
            "subnetPrefix": {
            "value": "10.0.0.0/28"
            },
            "applicationGatewaySize": {
            "value": "WAF_Medium"
            },
            "capacity": {
            "value": 2
            },
            "backendIpAddress1": {
            "value": "10.0.1.10"
            },
            "backendIpAddress2": {
            "value": "10.0.1.11"
            },
            "wafEnabled": {
            "value": true
            },
            "wafMode": {
            "value": "Detection"
            },
            "wafRuleSetType": {
            "value": "OWASP"
            },
            "wafRuleSetVersion": {
            "value": "3.0"
            }
        }
    }
    ```

1. Hello fájl mentéséhez. Tesztelheti hello JSON-sablon és a paraméter sablon JSON érvényesítési online eszközökkel, például a [JSlint.com](http://www.jslint.com/).

## <a name="deploy-hello-azure-resource-manager-template-by-using-powershell"></a>Hello Azure Resource Manager-sablon üzembe helyezése a PowerShell használatával

Ha még sosem használta az Azure PowerShell, látogasson el: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) és kövesse az utasításokat toosign hello az Azure, és jelölje ki az előfizetését.

1. Bejelentkezési tooPowerShell

    ```powershell
    Login-AzureRmAccount
    ```

1. Hello előfizetések hello fiók ellenőrzése.

    ```powershell
    Get-AzureRmSubscription
    ```

    A hitelesítő adataival felszólító tooauthenticate áll.

1. Válassza ki, amely az Azure-előfizetések toouse.

    ```powershell
    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
    ```

1. Szükség esetén hozzon létre egy erőforráscsoportot hello segítségével **New-használják** parancsmag. A következő példa hello hozzon létre egy erőforráscsoportot AppgatewayRG nevű USA keleti régiója helyen.

    ```powershell
    New-AzureRmResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. Futtassa a hello **New-AzureRmResourceGroupDeployment** parancsmag toodeploy hello új virtuális hálózat használatával a letöltött és módosított sablonnal és paraméterfájlokkal fájlok megelőző hello.
    
    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-hello-azure-resource-manager-template-by-using-hello-azure-cli"></a>Hello Azure Resource Manager sablon üzembe helyezése hello Azure parancssori felület használatával

toodeploy hello Azure Resource Manager sablon Azure CLI használatával kövesse a lépéseket követve hello:

1. Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](/cli/azure/install-azure-cli) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.

1. Ha szükséges, futtatási hello `az group create` parancs toocreate egy erőforráscsoport, ahogy az a következő kódrészletet hello. Figyelje meg hello hello parancs kimenetét. hello kimenet után látható hello lista hello paramétereket ismerteti. További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    **-n (vagy --name)**. Hello új erőforráscsoport neve. A mi esetünkben *appgatewayRG*.
    
    **-l (vagy --location)**. Azure-régió, ahol hello új erőforráscsoport létrejön. A mi esetünkben rendelkezik *westus*.

1. Futtassa a hello `az group deployment create` parancsmag toodeploy hello új virtuális hálózat hello sablonnal és paraméterfájlokkal letöltött és módosított lépést megelőző hello a fájlokat. hello kimenet után látható hello lista hello paramétereket ismerteti.

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-hello-azure-resource-manager-template-by-using-click-to-deploy"></a>Hello Azure Resource Manager sablon telepíteni a kattintson a központi telepítése

Egy másik módja toouse kattintson a központi telepítése az Azure Resource Manager-sablonok. Egy egyszerűen toouse sablonokat hello Azure-portálon.

1. Nyissa meg túl[hozzon létre egy alkalmazás webalkalmazási tűzfal](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).

1. Kattintson a **tooAzure telepítése**.

    ![TooAzure telepítése](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)
    
1. Töltse ki a hello paraméterek hello központi telepítési sablon hello portálon, és kattintson a **OK**.

    ![Paraméterek](./media/application-gateway-create-gateway-arm-template/ibiza1.png)
    
1. Válassza ki **toohello feltételek és kikötések fenti elfogadom** kattintson **beszerzési**.

1. Hello egyéni telepítési paneljén kattintson **létrehozása**.

## <a name="providing-certificate-data-tooresource-manager-templates"></a>Biztosító tanúsítvány adatok tooResource Manager-sablonok

Ha SSL-t egy sablont használ, a hello tanúsítványt kell toobe megadott helyett feltöltendő Base64 kódolású karakterlánc. tooconvert egy .pfx vagy .cer tooa Base64 kódolású karakterlánc hello a következő parancsok egyikét használhatja. hello következő parancsokat az átalakítás hello tanúsítvány tooa base64 karakterláncot, amely toohello sablon megadható. hello várt kimeneti karakterlánc, amely egy változó tárolja, és a beillesztett hello sablonban.

### <a name="macos"></a>macOS
```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a>Windows
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a>Az összes erőforrás törlése

toodelete összes erőforrás létrehozása ebben a cikkben a lépéseket követve hello teljes egyikét:

### <a name="powershell"></a>PowerShell

```powershell
Remove-AzureRmResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a>Következő lépések

Ha azt szeretné, hogy az SSL-kiszervezés tooconfigure, látogasson el: [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl.md).

Ha azt szeretné, hogy egy alkalmazás átjáró toouse belső terheléselosztással tooconfigure, látogasson el: [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).

Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

