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
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a><span data-ttu-id="5d45a-103">Az Azure App Service SSL-tanúsítvány kötés PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="5d45a-103">Azure App Service SSL Certificate Binding using PowerShell</span></span>
<span data-ttu-id="5d45a-104">A Microsoft Azure PowerShell verziója 1.1.0-ás kiadása egy új parancsmagot, amely ad a felhasználó meglévő vagy új SSL-tanúsítványok kötése egy már meglévő webalkalmazás képes bővült.</span><span class="sxs-lookup"><span data-stu-id="5d45a-104">With the release of Microsoft Azure PowerShell version 1.1.0 a new cmdlet has been added that would give the user the ability to bind existing or new SSL certificates to an existing Web App.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<span data-ttu-id="5d45a-105">Azure Resource Manager-alapú kezelheti a Web Apps ellenőrzése az Azure PowerShell-parancsmagok használatával kapcsolatos további [Azure Resource Manager PowerShell-parancsok alapú Azure webalkalmazás számára](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="5d45a-105">To learn about using Azure Resource Manager based Azure PowerShell cmdlets to manage your Web Apps check [Azure Resource Manager based PowerShell commands for Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span></span>

## <a name="uploading-and-binding-a-new-ssl-certificate"></a><span data-ttu-id="5d45a-106">Feltöltését és egy új SSL-tanúsítvány kötésének</span><span class="sxs-lookup"><span data-stu-id="5d45a-106">Uploading and Binding a new SSL certificate</span></span>
<span data-ttu-id="5d45a-107">Forgatókönyv: A felhasználó szeretné egy SSL-tanúsítványt kötni a webes alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="5d45a-107">Scenario: The user would like to bind an SSL certificate to one of his web apps.</span></span>

<span data-ttu-id="5d45a-108">Az erőforráscsoport neve, amely tartalmazza a web app, a webes alkalmazás neve, a tanúsítvány .pfx fájl elérési útját a felhasználó számítógépén, a tanúsítványt, és az egyéni állomásnevet a jelszó ismerete, a következő PowerShell-parancsot, hogy SSL-kötés létrehozásához használhatja azt:</span><span class="sxs-lookup"><span data-stu-id="5d45a-108">Knowing the resource group name that contains the web app, the web app name, the certificate .pfx file path on the user machine, the password for the certificate, and the custom hostname, we can use the following PowerShell command to create that SSL binding:</span></span>

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

<span data-ttu-id="5d45a-109">Vegye figyelembe, hogy mielőtt felvenne egy SSL-kötés egy webalkalmazásba, rendelkeznie kell egy állomásnevet (egyéni tartomány) már be van állítva.</span><span class="sxs-lookup"><span data-stu-id="5d45a-109">Note that before adding a SSL binding to a web app, you must have a host name (custom domain) already configured.</span></span> <span data-ttu-id="5d45a-110">Ha az állomásnév nem úgy van beállítva, akkor hiba lép fel "állomásnév" nem létezik a New-AzureRmWebAppSSLBinding futtatása során.</span><span class="sxs-lookup"><span data-stu-id="5d45a-110">If the host name is not configured , then you will get an error 'hostname' does not exist while running  New-AzureRmWebAppSSLBinding.</span></span> <span data-ttu-id="5d45a-111">Az állomásnév-ről a portál vagy az Azure PowerShell használatával is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="5d45a-111">You can add a hostname directly from the portal or using Azure PowerShell.</span></span> <span data-ttu-id="5d45a-112">A következő PowerShell-részlet lehet az állomásnév konfigurálása a New-AzureRmWebAppSSLBinding futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="5d45a-112">The following PowerShell snippet can be to configure the hostname before running New-AzureRmWebAppSSLBinding.</span></span>   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

<span data-ttu-id="5d45a-113">Fontos megérteni, hogy a Set-AzureRmWebApp parancsmag felülírja a gazdagép neve a webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="5d45a-113">It is important to understand that the Set-AzureRmWebApp cmdlet overwrites the hostnames for the web app.</span></span> <span data-ttu-id="5d45a-114">Ezért a fenti PowerShell részlet van hozzáfűzése a meglévő lista tartalmazza az állomásnevek a webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="5d45a-114">Hence the above PowerShell snippet is appending to the existing list of the host names for the web app.</span></span>  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a><span data-ttu-id="5d45a-115">Feltöltését és a létező SSL-tanúsítvány kötésének</span><span class="sxs-lookup"><span data-stu-id="5d45a-115">Uploading and Binding an existing SSL certificate</span></span>
<span data-ttu-id="5d45a-116">Forgatókönyv: A felhasználó szeretné a korábban feltöltött SSL-tanúsítványt kötni a webes alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="5d45a-116">Scenario: The user would like to bind a previously uploaded SSL certificate to one of his web apps.</span></span>

<span data-ttu-id="5d45a-117">Azt is olvashatók a következő paranccsal egy adott erőforráscsoporthoz már feltöltött tanúsítványok listája</span><span class="sxs-lookup"><span data-stu-id="5d45a-117">We can get the list of certificates already uploaded to a specific resource group by using the following command</span></span>

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

<span data-ttu-id="5d45a-118">Vegye figyelembe, hogy a tanúsítványok helyi, egy adott helyen és az erőforráscsoport, a felhasználónak újra töltse fel a tanúsítványt, ha a konfigurált webes alkalmazás egy másik helyre, és erőforráscsoport más, meg kell, hogy a szükséges tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="5d45a-118">Note that the certificates are local to a specific location and resource group, the user need to re-upload the certificate if the configured web app is in a different location and resource group other that that of the needed certificate</span></span> 

<span data-ttu-id="5d45a-119">Az erőforráscsoport neve, amely tartalmazza a web app, a webes alkalmazás neve, a tanúsítvány ujjlenyomata és az egyéni állomásnevet ismerete, a következő PowerShell-parancsot, hogy SSL-kötés létrehozásához használhatja azt:</span><span class="sxs-lookup"><span data-stu-id="5d45a-119">Knowing the resource group name that contains the web app, the web app name, the certificate thumbprint, and the custom hostname, we can use the following PowerShell command to create that SSL binding:</span></span>

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a><span data-ttu-id="5d45a-120">Egy létező SSL-kötés törlése</span><span class="sxs-lookup"><span data-stu-id="5d45a-120">Deleting an existing SSL binding</span></span>
<span data-ttu-id="5d45a-121">Forgatókönyv: A felhasználó törölni kívánja egy létező SSL-kötést.</span><span class="sxs-lookup"><span data-stu-id="5d45a-121">Scenario: The user would like to delete an existing SSL binding.</span></span>

<span data-ttu-id="5d45a-122">Az erőforráscsoport neve, amely tartalmazza a web app, a webes alkalmazás neve és az egyéni állomásnevet ismeretében azt segítségével a következő PowerShell-parancs távolítsa el, hogy SSL-kötést:</span><span class="sxs-lookup"><span data-stu-id="5d45a-122">Knowing the resource group name that contains the web app, the web app name, and the custom hostname, we can use the following PowerShell command to remove that SSL binding:</span></span>

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

<span data-ttu-id="5d45a-123">Vegye figyelembe, hogy ha az eltávolított SSL-kötés lett az utolsó kötést adott abban, hogy a hely, a tanúsítvány alapértelmezés szerint törli, ha a felhasználó szeretné megtartani a tanúsítványt a DeleteCertificate beállítást használhat a tanúsítványra</span><span class="sxs-lookup"><span data-stu-id="5d45a-123">Note that if the removed SSL binding was the last binding using that certificate in that location, by default the certificate will be deleted, if the user want to keep the certificate he can use the DeleteCertificate option to keep the certificate</span></span>

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a><span data-ttu-id="5d45a-124">Referencia</span><span class="sxs-lookup"><span data-stu-id="5d45a-124">References</span></span>
* [<span data-ttu-id="5d45a-125">Az Azure Resource Manager-alapú PowerShell-parancsok Azure webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="5d45a-125">Azure Resource Manager based PowerShell commands for Azure Web App</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
* [<span data-ttu-id="5d45a-126">Az App Service Environment bemutatása</span><span class="sxs-lookup"><span data-stu-id="5d45a-126">Introduction to App Service Environment</span></span>](app-service-app-service-environment-intro.md)
* [<span data-ttu-id="5d45a-127">Az Azure PowerShell használata az Azure Resource Managerrel</span><span class="sxs-lookup"><span data-stu-id="5d45a-127">Using Azure PowerShell with Azure Resource Manager</span></span>](../powershell-azure-resource-manager.md)

