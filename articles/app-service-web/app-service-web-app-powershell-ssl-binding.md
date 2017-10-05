---
title: "SSL-tanúsítványok kötése PowerShell használatával"
description: "Megtudhatja, hogyan lehet kötést létrehozni az SSL-tanúsítványok a PowerShell használatával webes alkalmazást."
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
ms.openlocfilehash: a1fcc618fb0c68778e39cc227368a60b008f9401
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Az Azure App Service SSL-tanúsítvány kötés PowerShell használatával
A Microsoft Azure PowerShell verziója 1.1.0-ás kiadása egy új parancsmagot, amely ad a felhasználó meglévő vagy új SSL-tanúsítványok kötése egy már meglévő webalkalmazás képes bővült.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Azure Resource Manager-alapú kezelheti a Web Apps ellenőrzése az Azure PowerShell-parancsmagok használatával kapcsolatos további [Azure Resource Manager PowerShell-parancsok alapú Azure webalkalmazás számára](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Feltöltését és egy új SSL-tanúsítvány kötésének
Forgatókönyv: A felhasználó szeretné egy SSL-tanúsítványt kötni a webes alkalmazások.

Az erőforráscsoport neve, amely tartalmazza a web app, a webes alkalmazás neve, a tanúsítvány .pfx fájl elérési útját a felhasználó számítógépén, a tanúsítványt, és az egyéni állomásnevet a jelszó ismerete, a következő PowerShell-parancsot, hogy SSL-kötés létrehozásához használhatja azt:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Vegye figyelembe, hogy mielőtt felvenne egy SSL-kötés egy webalkalmazásba, rendelkeznie kell egy állomásnevet (egyéni tartomány) már be van állítva. Ha az állomásnév nem úgy van beállítva, akkor hiba lép fel "állomásnév" nem létezik a New-AzureRmWebAppSSLBinding futtatása során. Az állomásnév-ről a portál vagy az Azure PowerShell használatával is hozzáadhat. A következő PowerShell-részlet lehet az állomásnév konfigurálása a New-AzureRmWebAppSSLBinding futtatása előtt.   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

Fontos megérteni, hogy a Set-AzureRmWebApp parancsmag felülírja a gazdagép neve a webalkalmazás számára. Ezért a fenti PowerShell részlet van hozzáfűzése a meglévő lista tartalmazza az állomásnevek a webalkalmazás számára.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Feltöltését és a létező SSL-tanúsítvány kötésének
Forgatókönyv: A felhasználó szeretné a korábban feltöltött SSL-tanúsítványt kötni a webes alkalmazások.

Azt is olvashatók a következő paranccsal egy adott erőforráscsoporthoz már feltöltött tanúsítványok listája

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Vegye figyelembe, hogy a tanúsítványok helyi, egy adott helyen és az erőforráscsoport, a felhasználónak újra töltse fel a tanúsítványt, ha a konfigurált webes alkalmazás egy másik helyre, és erőforráscsoport más, meg kell, hogy a szükséges tanúsítvány 

Az erőforráscsoport neve, amely tartalmazza a web app, a webes alkalmazás neve, a tanúsítvány ujjlenyomata és az egyéni állomásnevet ismerete, a következő PowerShell-parancsot, hogy SSL-kötés létrehozásához használhatja azt:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Egy létező SSL-kötés törlése
Forgatókönyv: A felhasználó törölni kívánja egy létező SSL-kötést.

Az erőforráscsoport neve, amely tartalmazza a web app, a webes alkalmazás neve és az egyéni állomásnevet ismeretében azt segítségével a következő PowerShell-parancs távolítsa el, hogy SSL-kötést:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Vegye figyelembe, hogy ha az eltávolított SSL-kötés lett az utolsó kötést adott abban, hogy a hely, a tanúsítvány alapértelmezés szerint törli, ha a felhasználó szeretné megtartani a tanúsítványt a DeleteCertificate beállítást használhat a tanúsítványra

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Referencia
* [Az Azure Resource Manager-alapú PowerShell-parancsok Azure webalkalmazás számára](app-service-web-app-azure-resource-manager-powershell.md)
* [Az App Service Environment bemutatása](app-service-app-service-environment-intro.md)
* [Az Azure PowerShell használata az Azure Resource Managerrel](../powershell-azure-resource-manager.md)

