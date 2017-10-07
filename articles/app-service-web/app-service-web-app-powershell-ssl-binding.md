---
title: "aaaSSL tanúsítványok kötése a PowerShell használatával"
description: "Ismerje meg, hogyan toobind SSL-tanúsítványok tooyour web app PowerShell használatával."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 8ce32508-e982-48d3-b878-0e526afda537
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: aelnably
ms.openlocfilehash: 82f0e7c796da99ab50f69f3638ef64d55a94fc8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Az Azure App Service SSL-tanúsítvány kötés PowerShell használatával
Microsoft Azure PowerShell verziója 1.1.0-ás hozzá lett adva egy új parancsmagot hello kiadása, amely ad hello felhasználói hello képességét toobind meglévő vagy új SSL tanúsítványok tooan meglévő Web App.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Azure Resource Manager használatával kapcsolatos toolearn alapú Azure PowerShell-parancsmagok toomanage a Web Apps ellenőrzés [Azure Resource Manager PowerShell-parancsok alapú Azure webalkalmazás számára](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Feltöltését és egy új SSL-tanúsítvány kötésének
Forgatókönyv: hello felhasználói toobind egy SSL-tanúsítvány tooone a webes alkalmazások szeretné.

Hello az erőforráscsoport neve, amely tartalmazza a hello webalkalmazás, hello webalkalmazás nevének, hello tanúsítvány .pfx fájl elérési útja a hello felhasználó, hogy tudnák hello hello tanúsítvány jelszava, hello az egyéni állomásnevet, a következő PowerShell-parancs toocreate hello is használhatók, amelyek SSL-kötés:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Vegye figyelembe, hogy mielőtt felvenne egy SSL-kötés tooa webalkalmazást, rendelkeznie kell egy állomásnevet (egyéni tartomány) már be van állítva. Ha hello állomás neve nincs beállítva, akkor hibaüzenetet kap "állomásnév" nem létezik a New-AzureRmWebAppSSLBinding futtatása során. Az állomásnév-ről hello portálon vagy az Azure PowerShell használatával is hozzáadhat. hello következő PowerShell-részlet lehet tooconfigure hello állomásnév New-AzureRmWebAppSSLBinding futtatása előtt.   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

Fontos, hogy a Set-AzureRmWebApp parancsmag hello toounderstand hello állomásnevek hello webalkalmazás felülírja. Ezért a fent PowerShell részlet hello van fűznek toohello meglévő hello webalkalmazás hello állomásnevek listáját.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Feltöltését és a létező SSL-tanúsítvány kötésének
Forgatókönyv: hello felhasználói toobind a webes alkalmazások korábban feltöltött SSL tanúsítvány tooone szeretné.

Azt is beolvasása tanúsítványok listájának hello már feltöltött tooa adott erőforráscsoport hello a következő parancs használatával

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Vegye figyelembe, hogy hello tanúsítványok helyi tooa adott elhelyezés vagy erőforrás-csoport, hello felhasználói tanúsítványra van szükség toore-feltöltési hello Ha egy másik helyen konfigurált hello web app és erőforráscsoport más, hogy a hello tanúsítvány szükséges 

Hello az erőforráscsoport neve, amely tartalmazza a hello web app alkalmazásban ismerete hello webes alkalmazás neve, tanúsítvány-ujjlenyomat hello, és egyéni állomásnév hello, is használhatók a következő PowerShell-parancs toocreate hello adott SSL-kötés:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Egy létező SSL-kötés törlése
Forgatókönyv: hello felhasználói szeretné toodelete egy létező SSL-kötést.

Tudatában hello az erőforráscsoport neve, amely tartalmazza a hello web app alkalmazásban hello webes alkalmazás neve, és egyéni állomásnév hello, is használhatók a következő PowerShell-parancs tooremove hello adott SSL-kötés:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Vegye figyelembe, hogy ha hello eltávolítani az SSL-kötés lett hello utolsó azon a helyen, tanúsítvány használata kötelező alapértelmezett hello tanúsítvánnyal törlésre kerül, ha hello felhasználói szeretné tookeep hello tanúsítvány használhat hello DeleteCertificate beállítás tookeep hello tanúsítvány

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Referencia
* [Az Azure Resource Manager-alapú PowerShell-parancsok Azure webalkalmazás számára](app-service-web-app-azure-resource-manager-powershell.md)
* [Bevezetés tooApp Service-környezet](app-service-app-service-environment-intro.md)
* [Az Azure PowerShell használata az Azure Resource Managerrel](../powershell-azure-resource-manager.md)

