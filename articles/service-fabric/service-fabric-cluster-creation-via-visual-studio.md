---
title: "Visual Studio használatával a Service Fabric fürt aaaSetting |} Microsoft Docs"
description: "Ismerteti, hogyan tooset mentése a Service Fabric fürt egy Azure erőforráscsoport-projektet a Visual Studio által létrehozott Azure Resource Manager-sablon használatával"
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
ms.openlocfilehash: adb0dd2169a28b46e832c6f06c998cbed0c473f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>A Service Fabric-fürt beállítása a Visual Studio használatával
Ez a cikk ismerteti, hogyan tooset mentése az Azure Service Fabric fürt Visual Studio és az Azure Resource Manager-sablon használatával. A Visual Studio Azure resource csoport toocreate hello projektsablon használjuk. Hello sablon létrehozását követően központilag telepíthető közvetlenül a Visual Studio eszközből tooAzure. Azt is használható egy parancsfájlból származó, vagy a folyamatos integrációt (CI) létesítmény részeként.

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>A Service Fabric-fürt sablon az Azure erőforráscsoport-projekt létrehozása
tooget elindult, nyissa meg a Visual Studio és az Azure erőforráscsoport-projekt létrehozása (hello elérhető **felhő** mappa):

![Új projekt párbeszédpanel kijelölt Azure erőforráscsoport-projekt][1]

Hozzon létre egy új Visual Studio megoldás ebben a projektben, vagy adja hozzá tooan meglévő megoldással.

> [!NOTE]
> Ha nem látja hello Azure erőforráscsoport-projekt hello felhő csomópontja alatt, nincs hello Azure SDK telepítése. Indítsa el a Webplatform-telepítővel ([most telepíteni](http://www.microsoft.com/web/downloads/platform.aspx) Ha még nem tette meg), majd keresse meg a "Azure SDK a .NET" és a telepítés hello verziója, amely kompatibilis a Visual Studio verziójának.
> 
> 

Miután hello OK gombra kattint, a Visual Studio ekkor megkérdezi, hogy tooselect hello Resource Manager sablon toocreate:

![Azure-sablon párbeszédpanelen válassza ki a kijelölt Service Fabric-fürt sablonnal][2]

Válassza ki újra hello Service Fabric-fürt sablon és a találati hello OK gombra. hello projekt és hello Resource Manager-sablon már létrejött.

## <a name="prepare-hello-template-for-deployment"></a>Hello sablon üzembe helyezésének előkészítése
Mielőtt hello sablon telepített toocreate hello fürt, kell értékeket ad meg a szükséges hello Sablonparaméterek. A paraméterértékek pedig olvassa a hello `ServiceFabricCluster.parameters.json` fájl, amely hello `Templates` hello erőforráscsoport-projekt mappájából. Nyissa meg hello fájlt, és adja meg a következő értékek hello:

| Paraméter neve | Leírás |
| --- | --- |
| adminUserName |hello hello rendszergazdai fiók nevét a Service Fabric gépek (csomópontok). |
| certificateThumbprint |hello biztosítja a hello fürt hello tanúsítvány ujjlenyomata. |
| sourceVaultResourceId |Hello *erőforrás-azonosító* a hello kulcstároló hello tanúsítvány, amely biztosítja a hello fürt tárolására. |
| certificateUrlValue |hello hello fürt biztonsági tanúsítvány URL-címe |

hello Visual Studio Service Fabric Resource Manager-sablon védi egy tanúsítványt egy biztonságos fürtöt hoz létre. Ez a tanúsítvány hello azonosíthatók az utolsó három Sablonparaméterek (`certificateThumbprint`, `sourceVaultValue`, és `certificateUrlValue`), és azt már léteznie kell egy **Azure Key Vault**. Hogyan toocreate hello fürt biztonsági tanúsítvány további információkért lásd: [Service Fabric-fürt biztonsági forgatókönyvek](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) cikk.

## <a name="optional-change-hello-cluster-name"></a>Választható lehetőség: hello fürt nevének módosítása
Minden Service Fabric-fürt neve van. A Fabric-fürt létrehozása az Azure-ban fürt nevét határozza meg (együtt hello Azure-régió) hello fürt hello tartománynévrendszer (DNS) nevét. Például, ha a fürt nevezze el `myBigCluster`, és hello helyét (Azure-régió), amely helyt hello új fürt hello erőforráscsoport USA keleti régiója, hello fürt hello DNS-neve lesz `myBigCluster.eastus.cloudapp.azure.com`.

Alapértelmezés szerint hello fürtnév automatikusan jönnek létre, és egyedi végzett véletlenszerű utótag tooa "fürt" előtag csatolásával. Ez teszi, hogy rendkívül könnyen toouse hello sablon részeként egy **folyamatos integrációt** (CI) rendszer. Ha azt szeretné, hogy toouse egyedi nevet a fürtnek, ki, amely jelentéssel bíró tooyou, állítsa be hello hello `clusterName` változó hello Resource Manager sablon fájlban (`ServiceFabricCluster.json`) kiválasztott tooyour nevét. Hello első változót, az adott fájlban definiálva.

## <a name="optional-add-public-application-ports"></a>Választható lehetőség: nyilvános alkalmazás-portok hozzáadása
Is érdemes lehet toochange hello nyilvános alkalmazás portok hello fürt telepítése előtt. Alapértelmezés szerint hello sablon csak két nyilvános TCP-portot (80-as és 8081) megnyílik. Ha több, az alkalmazások, módosítsa a hello Azure Load Balancer definition hello sablonban. hello definition hello fő sablon fájl tárolja (`ServiceFabricCluster.json`). Nyissa meg a fájlt, és keresse meg `loadBalancedAppPort`. Minden port társítva három összetevők:

1. Hello port hello TCP port értékét meghatározó sablonváltozó:
   
    ```json
    "loadBalancedAppPort1": "80"
    ```
2. A *mintavételi* , amely meghatározza, milyen gyakran és mennyi ideig hello Azure terheléselosztó toouse egy adott Service Fabric-csomópont mielőtt hibát jelentene megpróbál egy tooanother keresztül. hello mintavételt a Load Balancer erőforrás hello részét képezik. Íme hello mintavételi definíciója hello első alapértelmezett portja:
   
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
3. A *terheléselosztási szabály* , amely kötelékek együtt hello port és hello mintavételi, mely lehetővé teszi, hogy terheléselosztás meg a Service Fabric-fürt csomópontok között:
   
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
   Hello megtervezni toodeploy toohello fürt szükséges további portok, adhat hozzá további mintavétel létrehozása és szabálydefiníciók terheléselosztás. További tájékoztatást a Resource Manager-sablonok segítségével az Azure terheléselosztó toowork lásd: [létrehozásához egy belső terheléselosztó-sablon használatával](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-hello-template-by-using-visual-studio"></a>Hello sablon telepítése a Visual Studio használatával
Mentése után minden hello-e a szükséges paraméterértékeket a`ServiceFabricCluster.param.dev.json` fájl, készen áll a toodeploy hello sablon és a Service Fabric-fürt létrehozása. Kattintson a jobb gombbal a hello erőforráscsoport-projekt a Visual Studio Solution Explorerben, és válassza a **telepítés |} Új központi telepítési...** . Ha szükséges, a Visual Studio megjeleníti hello **tooResource csoport telepítése** párbeszédpanelen tooauthenticate tooAzure kéri:

![Központi telepítése tooResource csoport párbeszédpanel][3]

hello párbeszédpanelen kiválaszthatja hello fürt-vagy hello beállítás toocreate egy új ad egy meglévő Resource Manager erőforráscsoportot. Általában teszi logika toouse egy külön erőforráscsoportot a Service Fabric-fürt.

Miután hello telepítés gombra kattint, a Visual Studio kérni fogja, tooconfirm hello sablon-paraméterértékek. Találati hello **mentése** gombra. Egy paraméter nincs a megőrzött értéke: hello fürt hello rendszergazdai fiók jelszavát. Ha a Visual Studio kéri egy szüksége tooprovide jelszó értéket.

> [!NOTE]
> Azure SDK 2.9-től kezdődően Visual Studio támogatja az olvasást jelszavakat **Azure Key Vault** üzembe helyezése során. Hello sablon paraméterek párbeszédpanelen figyelje meg, hogy hello `adminPassword` paraméter beviteli mező jobb hello rendelkezik egy kis "kulcsot" ikonra. Erre az ikonra lehetővé teszi egy meglévő kulcstároló titkos tooselect hello fürt hello felügyeleti jelszóként. Csak győződjön meg arról, hogy engedélyezze a hozzáférést a kulcstartót sablon-üzembehelyezés hello speciális hozzáférési házirendek az Azure Resource Manager toofirst. 
> 
> 

Kísérheti hello hello Visual Studio kimeneti ablakában hello központi telepítési folyamata során. Ha hello sablon telepítése befejeződött, az új fürthöz toouse készen áll!

> [!NOTE]
> Ha PowerShell soha nem volt használt tooadminister Azure most használt hello gépről, akkor toodo egy kis housekeeping.
> 
> 1. Engedélyezze a PowerShell parancsfájl-kezelési hello futtatásával [ `Set-ExecutionPolicy` ](https://technet.microsoft.com/library/hh849812.aspx) parancsot. A fejlesztési gépek "korlátlan" házirend általában elfogadható.
> 2. Döntse el, hogy Azure PowerShell-parancsokat, majd futtassa a diagnosztikai adatok gyűjtése tooallow [ `Enable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619303.aspx) vagy [ `Disable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619236.aspx) szükség szerint. Ezzel elkerüli a szükségtelen kér sablon üzembe helyezése során.
> 
> 

Ha hiba történik, lépjen a toohello [Azure-portálon](https://portal.azure.com/) és a nyitott hello erőforráscsoport számára központilag telepített. Kattintson a **összes beállítás**, majd kattintson a **központi telepítések** a hello-beállítások panelen. Sikertelen erőforráscsoport-telepítési hiba elhagyja részletes diagnosztikai adatokat.

> [!NOTE]
> Service Fabric-fürtök bizonyos számú csomópontok toobe toomaintain rendelkezésre állási be kell, és megőrzi állapotát - hivatkozott tooas "kvórum fenntartása." Ezért nem biztonságos tooshut összes hello fürt hello gépet le kivéve, ha először elvégezte a [teljes biztonsági mentés a állapot](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók: hello Azure-portál használatával a Service Fabric-fürt beállítása](service-fabric-cluster-creation-via-portal.md)
* [Megtudhatja, hogyan toomanage és a Visual Studio használatával a Service Fabric-alkalmazások központi telepítése](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
