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
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a><span data-ttu-id="59a99-103">Az Azure App Service SSL-tanúsítvány kötés PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="59a99-103">Azure App Service SSL Certificate Binding using PowerShell</span></span>
<span data-ttu-id="59a99-104">Microsoft Azure PowerShell verziója 1.1.0-ás hozzá lett adva egy új parancsmagot hello kiadása, amely ad hello felhasználói hello képességét toobind meglévő vagy új SSL tanúsítványok tooan meglévő Web App.</span><span class="sxs-lookup"><span data-stu-id="59a99-104">With hello release of Microsoft Azure PowerShell version 1.1.0 a new cmdlet has been added that would give hello user hello ability toobind existing or new SSL certificates tooan existing Web App.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<span data-ttu-id="59a99-105">Azure Resource Manager használatával kapcsolatos toolearn alapú Azure PowerShell-parancsmagok toomanage a Web Apps ellenőrzés [Azure Resource Manager PowerShell-parancsok alapú Azure webalkalmazás számára](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="59a99-105">toolearn about using Azure Resource Manager based Azure PowerShell cmdlets toomanage your Web Apps check [Azure Resource Manager based PowerShell commands for Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span></span>

## <a name="uploading-and-binding-a-new-ssl-certificate"></a><span data-ttu-id="59a99-106">Feltöltését és egy új SSL-tanúsítvány kötésének</span><span class="sxs-lookup"><span data-stu-id="59a99-106">Uploading and Binding a new SSL certificate</span></span>
<span data-ttu-id="59a99-107">Forgatókönyv: hello felhasználói toobind egy SSL-tanúsítvány tooone a webes alkalmazások szeretné.</span><span class="sxs-lookup"><span data-stu-id="59a99-107">Scenario: hello user would like toobind an SSL certificate tooone of his web apps.</span></span>

<span data-ttu-id="59a99-108">Hello az erőforráscsoport neve, amely tartalmazza a hello webalkalmazás, hello webalkalmazás nevének, hello tanúsítvány .pfx fájl elérési útja a hello felhasználó, hogy tudnák hello hello tanúsítvány jelszava, hello az egyéni állomásnevet, a következő PowerShell-parancs toocreate hello is használhatók, amelyek SSL-kötés:</span><span class="sxs-lookup"><span data-stu-id="59a99-108">Knowing hello resource group name that contains hello web app, hello web app name, hello certificate .pfx file path on hello user machine, hello password for hello certificate, and hello custom hostname, we can use hello following PowerShell command toocreate that SSL binding:</span></span>

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

<span data-ttu-id="59a99-109">Vegye figyelembe, hogy mielőtt felvenne egy SSL-kötés tooa webalkalmazást, rendelkeznie kell egy állomásnevet (egyéni tartomány) már be van állítva.</span><span class="sxs-lookup"><span data-stu-id="59a99-109">Note that before adding a SSL binding tooa web app, you must have a host name (custom domain) already configured.</span></span> <span data-ttu-id="59a99-110">Ha hello állomás neve nincs beállítva, akkor hibaüzenetet kap "állomásnév" nem létezik a New-AzureRmWebAppSSLBinding futtatása során.</span><span class="sxs-lookup"><span data-stu-id="59a99-110">If hello host name is not configured , then you will get an error 'hostname' does not exist while running  New-AzureRmWebAppSSLBinding.</span></span> <span data-ttu-id="59a99-111">Az állomásnév-ről hello portálon vagy az Azure PowerShell használatával is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="59a99-111">You can add a hostname directly from hello portal or using Azure PowerShell.</span></span> <span data-ttu-id="59a99-112">hello következő PowerShell-részlet lehet tooconfigure hello állomásnév New-AzureRmWebAppSSLBinding futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="59a99-112">hello following PowerShell snippet can be tooconfigure hello hostname before running New-AzureRmWebAppSSLBinding.</span></span>   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

<span data-ttu-id="59a99-113">Fontos, hogy a Set-AzureRmWebApp parancsmag hello toounderstand hello állomásnevek hello webalkalmazás felülírja.</span><span class="sxs-lookup"><span data-stu-id="59a99-113">It is important toounderstand that hello Set-AzureRmWebApp cmdlet overwrites hello hostnames for hello web app.</span></span> <span data-ttu-id="59a99-114">Ezért a fent PowerShell részlet hello van fűznek toohello meglévő hello webalkalmazás hello állomásnevek listáját.</span><span class="sxs-lookup"><span data-stu-id="59a99-114">Hence hello above PowerShell snippet is appending toohello existing list of hello host names for hello web app.</span></span>  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a><span data-ttu-id="59a99-115">Feltöltését és a létező SSL-tanúsítvány kötésének</span><span class="sxs-lookup"><span data-stu-id="59a99-115">Uploading and Binding an existing SSL certificate</span></span>
<span data-ttu-id="59a99-116">Forgatókönyv: hello felhasználói toobind a webes alkalmazások korábban feltöltött SSL tanúsítvány tooone szeretné.</span><span class="sxs-lookup"><span data-stu-id="59a99-116">Scenario: hello user would like toobind a previously uploaded SSL certificate tooone of his web apps.</span></span>

<span data-ttu-id="59a99-117">Azt is beolvasása tanúsítványok listájának hello már feltöltött tooa adott erőforráscsoport hello a következő parancs használatával</span><span class="sxs-lookup"><span data-stu-id="59a99-117">We can get hello list of certificates already uploaded tooa specific resource group by using hello following command</span></span>

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

<span data-ttu-id="59a99-118">Vegye figyelembe, hogy hello tanúsítványok helyi tooa adott elhelyezés vagy erőforrás-csoport, hello felhasználói tanúsítványra van szükség toore-feltöltési hello Ha egy másik helyen konfigurált hello web app és erőforráscsoport más, hogy a hello tanúsítvány szükséges</span><span class="sxs-lookup"><span data-stu-id="59a99-118">Note that hello certificates are local tooa specific location and resource group, hello user need toore-upload hello certificate if hello configured web app is in a different location and resource group other that that of hello needed certificate</span></span> 

<span data-ttu-id="59a99-119">Hello az erőforráscsoport neve, amely tartalmazza a hello web app alkalmazásban ismerete hello webes alkalmazás neve, tanúsítvány-ujjlenyomat hello, és egyéni állomásnév hello, is használhatók a következő PowerShell-parancs toocreate hello adott SSL-kötés:</span><span class="sxs-lookup"><span data-stu-id="59a99-119">Knowing hello resource group name that contains hello web app, hello web app name, hello certificate thumbprint, and hello custom hostname, we can use hello following PowerShell command toocreate that SSL binding:</span></span>

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a><span data-ttu-id="59a99-120">Egy létező SSL-kötés törlése</span><span class="sxs-lookup"><span data-stu-id="59a99-120">Deleting an existing SSL binding</span></span>
<span data-ttu-id="59a99-121">Forgatókönyv: hello felhasználói szeretné toodelete egy létező SSL-kötést.</span><span class="sxs-lookup"><span data-stu-id="59a99-121">Scenario: hello user would like toodelete an existing SSL binding.</span></span>

<span data-ttu-id="59a99-122">Tudatában hello az erőforráscsoport neve, amely tartalmazza a hello web app alkalmazásban hello webes alkalmazás neve, és egyéni állomásnév hello, is használhatók a következő PowerShell-parancs tooremove hello adott SSL-kötés:</span><span class="sxs-lookup"><span data-stu-id="59a99-122">Knowing hello resource group name that contains hello web app, hello web app name, and hello custom hostname, we can use hello following PowerShell command tooremove that SSL binding:</span></span>

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

<span data-ttu-id="59a99-123">Vegye figyelembe, hogy ha hello eltávolítani az SSL-kötés lett hello utolsó azon a helyen, tanúsítvány használata kötelező alapértelmezett hello tanúsítvánnyal törlésre kerül, ha hello felhasználói szeretné tookeep hello tanúsítvány használhat hello DeleteCertificate beállítás tookeep hello tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="59a99-123">Note that if hello removed SSL binding was hello last binding using that certificate in that location, by default hello certificate will be deleted, if hello user want tookeep hello certificate he can use hello DeleteCertificate option tookeep hello certificate</span></span>

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a><span data-ttu-id="59a99-124">Referencia</span><span class="sxs-lookup"><span data-stu-id="59a99-124">References</span></span>
* [<span data-ttu-id="59a99-125">Az Azure Resource Manager-alapú PowerShell-parancsok Azure webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="59a99-125">Azure Resource Manager based PowerShell commands for Azure Web App</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
* [<span data-ttu-id="59a99-126">Bevezetés tooApp Service-környezet</span><span class="sxs-lookup"><span data-stu-id="59a99-126">Introduction tooApp Service Environment</span></span>](app-service-app-service-environment-intro.md)
* [<span data-ttu-id="59a99-127">Az Azure PowerShell használata az Azure Resource Managerrel</span><span class="sxs-lookup"><span data-stu-id="59a99-127">Using Azure PowerShell with Azure Resource Manager</span></span>](../powershell-azure-resource-manager.md)

