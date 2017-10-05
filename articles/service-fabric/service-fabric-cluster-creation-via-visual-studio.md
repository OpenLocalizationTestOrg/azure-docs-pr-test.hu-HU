---
title: "Egy Visual Studio használatával a Service Fabric-fürt beállítása |} Microsoft Docs"
description: "Ismerteti, hogyan lehet egy Azure erőforráscsoport-projektet a Visual Studio által létrehozott Azure Resource Manager-sablon használatával a Service Fabric-fürt beállítása"
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: adegeo
editor: 
ms.assetid: bd2c0511-36c9-4828-8dc3-69e4b6a70567
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/21/2017
ms.author: mikhegn
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.openlocfilehash: c43145b96cdbdfaa7e1893e50d027321fe4c0510
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>A Service Fabric-fürt beállítása a Visual Studio használatával
A cikkből megtudhatja, hogyan állíthat be egy Azure Service Fabric-fürt Visual Studio és az Azure Resource Manager-sablon használatával. A Visual Studio Azure erőforráscsoport-projekt használjuk a sablon létrehozásához. A sablon létrehozása után telepíthető közvetlenül az Azure-bA a Visual Studio eszközből. Azt is használható egy parancsfájlból származó, vagy a folyamatos integrációt (CI) létesítmény részeként.

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>A Service Fabric-fürt sablon az Azure erőforráscsoport-projekt létrehozása
Első lépésként nyissa meg a Visual Studio, és hozzon létre egy Azure erőforráscsoport-projekt (érhető el a **felhő** mappa):

![Új projekt párbeszédpanel kijelölt Azure erőforráscsoport-projekt][1]

Hozzon létre egy új Visual Studio megoldás ebben a projektben, vagy adja hozzá egy meglévő megoldás.

> [!NOTE]
> Ha nem látja az Azure erőforráscsoport-projekt felhő csomópontja alatt, nincs az Azure SDK telepítése. Indítsa el a Webplatform-telepítővel ([most telepíteni](http://www.microsoft.com/web/downloads/platform.aspx) Ha még nem tette meg), és keressen a "Azure SDK a .NET", és telepítse a Visual Studio verziója kompatibilis verziót.
> 
> 

Miután az OK gombra kattint, a Visual Studio felszólítja, hogy válassza ki az erőforrás-kezelő sablont szeretne létrehozni:

![Azure-sablon párbeszédpanelen válassza ki a kijelölt Service Fabric-fürt sablonnal][2]

Válassza ki a Service Fabric-fürt sablont, majd ismét kattintson az OK gombra. Most már létrejött a projekt és a Resource Manager-sablon.

## <a name="prepare-the-template-for-deployment"></a>A sablon üzembe helyezésének előkészítése
A sablon telepítése a fürt létrehozásához, előtt a sablonhoz szükséges paraméterek értékeket kell megadnia. A paraméterértékek pedig olvassa a rendszer a `ServiceFabricCluster.parameters.json` fájl, amely a `Templates` az erőforráscsoport-projekt mappájából. Nyissa meg a fájlt, és adja meg a következő értékeket:

| Paraméter neve | Leírás |
| --- | --- |
| adminUserName |A Service Fabric gépek (csomópontok) a rendszergazdai fiók nevét. |
| certificateThumbprint |Biztonságossá teszi a fürt tanúsítvány ujjlenyomata. |
| sourceVaultResourceId |A *erőforrás-azonosító* a kulcstároló, a tanúsítványt, amely biztosítja a fürt tárolására. |
| certificateUrlValue |A fürt biztonsági tanúsítvány URL-CÍMÉT. |

A Visual Studio Service Fabric Resource Manager-sablon védi egy tanúsítványt egy biztonságos fürtöt hoz létre. Ez a tanúsítvány az utolsó három Sablonparaméterek azonosít (`certificateThumbprint`, `sourceVaultValue`, és `certificateUrlValue`), és azt már léteznie kell egy **Azure Key Vault**. A fürt biztonsági tanúsítvány létrehozásával kapcsolatos további információkért lásd: [Service Fabric-fürt biztonsági forgatókönyvek](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) cikk.

## <a name="optional-change-the-cluster-name"></a>Választható lehetőség: a fürt nevének módosítása
Minden Service Fabric-fürt neve van. A Fabric-fürt létrehozása az Azure-fürt nevét határozza meg (együtt az Azure-régió) a tartománynévrendszer (DNS) a fürt nevét. Például, ha a fürt nevezze el `myBigCluster`, és a hely (Azure-régió), amely az új fürt üzemelteti az erőforráscsoportban USA keleti régiója, és a fürt DNS-neve lesz `myBigCluster.eastus.cloudapp.azure.com`.

Alapértelmezés szerint a fürt neve automatikusan létrehozza és egyedi tett egy véletlenszerű utótagot csatolása egy "fürt" előtag. Ez megkönnyíti a sablon használata részeként egy **folyamatos integrációt** (CI) rendszer. Ha azt szeretné, a fürt számára megadott név, amelyet kifejező, állítsa a `clusterName` változó a Resource Manager sablon fájlban (`ServiceFabricCluster.json`) a kiválasztott névre. Az első, az adott fájlban definiált változó.

## <a name="optional-add-public-application-ports"></a>Választható lehetőség: nyilvános alkalmazás-portok hozzáadása
Érdemes módosítani a nyilvános alkalmazás által használt portokat a fürt telepítése előtt is. Alapértelmezés szerint a sablon csak két nyilvános TCP-portot (80-as és 8081) megnyílik. Ha több, az alkalmazások, módosítsa a sablon az Azure Load Balancer-definíciójában. A fő sablon fájl tárolja a definition (`ServiceFabricCluster.json`). Nyissa meg a fájlt, és keresse meg `loadBalancedAppPort`. Minden port társítva három összetevők:

1. Egy sablon változó, amely meghatározza a port, az TCP-port értékének:
   
    ```json
    "loadBalancedAppPort1": "80"
    ```
2. A *mintavételi* , mely meghatározza, hogy milyen gyakran és mennyi ideig a Azure Load terheléselosztó próbálja meg használni egy adott Service Fabric-csomópont másikra feladatátvételét előtt. A mintavételt a Load Balancer erőforrás részét képezik. Ez az első alapértelmezett portja mintavételi definíciója:
   
    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```
3. A *terheléselosztási szabály* , amely kötelékek együtt a port és a mintavételi, mely lehetővé teszi, hogy terheléselosztás meg a Service Fabric-fürt csomópontok között:
   
    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
   Alkalmazásokat szeretne telepíteni a fürt további portokat kell, adhat hozzá további mintavétel létrehozása és szabálydefiníciók terheléselosztás. A Resource Manager-sablonok segítségével az Azure terheléselosztó munkavégzés további információkért lásd: [létrehozásához egy belső terheléselosztó-sablon használatával](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-the-template-by-using-visual-studio"></a>A sablon telepítése a Visual Studio használatával
Az összes kötelező paraméter értékét mentése után a`ServiceFabricCluster.param.dev.json` fájl, készen áll a sablon telepítéséhez és a Service Fabric-fürt létrehozása. Kattintson a jobb gombbal az erőforráscsoport-projekt a Visual Studio Solution Explorerben, és válassza a **telepítés |} Új központi telepítési...** . Ha szükséges, a Visual Studio megjeleníti a **telepítés erőforráscsoportra** párbeszédpanelen kéri, hogy hitelesítésre:

![Erőforráscsoport párbeszédpanel telepítése][3]

A párbeszédpanelen kiválaszthatja egy erőforrás-kezelő meglévő erőforráscsoportot, a fürt, és felajánlja a hozzon létre egy újat. Általában érdemes külön erőforráscsoport használata a Service Fabric-fürt.

Miután a telepítés gombra kattint, a Visual Studio kérni fogja, hogy erősítse meg a sablon-paraméterértékek. Elérte a **mentése** gombra. Egy paraméter nincs a megőrzött értéke: a fürt rendszergazdai fiók jelszavát. Adjon meg egy jelszót értéket, ha a Visual Studio kéri egy kell.

> [!NOTE]
> Azure SDK 2.9-től kezdődően Visual Studio támogatja az olvasást jelszavakat **Azure Key Vault** üzembe helyezése során. Figyelje meg, hogy a sablon paraméterek párbeszédpanelen a `adminPassword` paraméter szövegmező rendelkezik egy kis "kulcsot" ikonra a jobb oldali. Ez az ikon jelöljön ki egy meglévő kulcstároló titkos kulcsot, a fürt rendszergazdai jelszavának teszi lehetővé. Ne feledje első engedélyezése a kulcstartót sablon-üzembehelyezés a speciális hozzáférési házirendek az Azure Resource Manager hozzáférését. 
> 
> 

Megfigyelheti, hogy a Visual Studio kimeneti ablakában a telepítési folyamat előrehaladását. Ha a sablon telepítése befejeződött, az új fürt használatra készen áll!

> [!NOTE]
> Ha a PowerShell soha nem szerepel a számítógépről, most használt Azure felügyeletéhez, kell tennie egy kis housekeeping.
> 
> 1. Engedélyezze a PowerShell-parancsprogramok futtatásával a [ `Set-ExecutionPolicy` ](https://technet.microsoft.com/library/hh849812.aspx) parancsot. A fejlesztési gépek "korlátlan" házirend általában elfogadható.
> 2. Határozza meg, hogy az Azure PowerShell-parancsok diagnosztikai adatok gyűjtését, és futtassa [ `Enable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619303.aspx) vagy [ `Disable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619236.aspx) szükség szerint. Ezzel elkerüli a szükségtelen kér sablon üzembe helyezése során.
> 
> 

Ha hiba történik, akkor folytassa a [Azure-portálon](https://portal.azure.com/) , és nyissa meg az erőforráscsoport számára központilag telepített. Kattintson a **összes beállítás**, majd kattintson a **központi telepítések** a beállítások panelen. Sikertelen erőforráscsoport-telepítési hiba elhagyja részletes diagnosztikai adatokat.

> [!NOTE]
> Service Fabric-fürtök rendelkezésre álljon, és állapot - néven "kvórum fenntartása." megőrzése be kell csomópontok bizonyos számú megkövetelése Ezért már nem biztonságos állítsa le a gépeket a fürt összes, kivéve, ha először elvégezte a [teljes biztonsági mentés a állapot](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók az Azure portál használatával a Service Fabric-fürt beállítása](service-fabric-cluster-creation-via-portal.md)
* [Ismerje meg, hogyan kezelheti és a Visual Studio használatával a Service Fabric-alkalmazások központi telepítése](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
