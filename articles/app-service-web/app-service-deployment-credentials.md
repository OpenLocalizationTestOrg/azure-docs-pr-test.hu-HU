---
title: "Az Azure App Service telepítési hitelesítő adatok |} Microsoft Docs"
description: "Ismerje meg, hogyan használható az Azure App Service üzembe helyezési hitelesítő adatokat."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/05/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 86a2cd8ae9f97c606a378452e44eec8941700531
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a><span data-ttu-id="4d76d-103">Telepítési hitelesítő adatok beállítása az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4d76d-103">Configure deployment credentials for Azure App Service</span></span>
<span data-ttu-id="4d76d-104">[Az Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) támogatja a hitelesítő adatok kétféle [helyi Git-telepítésének](app-service-deploy-local-git.md) és [FTP/S telepítési](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="4d76d-104">[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) supports two types of credentials for [local Git deployment](app-service-deploy-local-git.md) and [FTP/S deployment](app-service-deploy-ftp.md).</span></span> <span data-ttu-id="4d76d-105">Ezek olyan nem ugyanaz, mint az Azure Active Directorybeli hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="4d76d-105">These are not the same as your Azure Active Directory credentials.</span></span>

* <span data-ttu-id="4d76d-106">**Felhasználói szintű hitelesítő adatokat**: a teljes Azure-fiók hitelesítő adatait egy készletét.</span><span class="sxs-lookup"><span data-stu-id="4d76d-106">**User-level credentials**: one set of credentials for the entire Azure account.</span></span> <span data-ttu-id="4d76d-107">Az App Service a telepíteni kívánt alkalmazást, minden előfizetést, az Azure-fiókra van hozzáférése a használható.</span><span class="sxs-lookup"><span data-stu-id="4d76d-107">It can be used to deploy to App Service for any app, in any subscription, that the Azure account has permission to access.</span></span> <span data-ttu-id="4d76d-108">Ezek az alapértelmezett hitelesítő adatok beállítása, amelyet megadtak a **alkalmazásszolgáltatások** > **&lt;alkalmazás_neve >** > **üzembe helyezési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="4d76d-108">These are the default credentials set that you configure in **App Services** > **&lt;app_name>** > **Deployment credentials**.</span></span> <span data-ttu-id="4d76d-109">Ez egyben az alapértelmezett, amely a grafikus felhasználói Felülettel portálon illesztett van (például a **áttekintése** és **tulajdonságok** az alkalmazás [erőforráspanelen](../azure-resource-manager/resource-group-portal.md#manage-resources)).</span><span class="sxs-lookup"><span data-stu-id="4d76d-109">This is also the default set that's surfaced in the portal GUI (such as the **Overview** and **Properties** of your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources)).</span></span>

    > [!NOTE]
    > <span data-ttu-id="4d76d-110">Azure-erőforrások szerepköralapú hozzáférés vezérlés (RBAC) vagy társadminisztrátornak engedélyek hozzáférés delegálásához, ha minden Azure felhasználói, amely fogad egy alkalmazás hozzáférési jogának képes személyes felhasználói szintű hitelesítő adatokat használhatna, amíg hozzáférését visszavonja.</span><span class="sxs-lookup"><span data-stu-id="4d76d-110">When you delegate access to Azure resources via Role Based Access Control (RBAC) or co-admin permissions, each Azure user that receives access to an app can use his/her personal user-level credentials until access is revoked.</span></span> <span data-ttu-id="4d76d-111">A központi telepítési hitelesítő adatokat nem lehet megosztva Azure másokkal.</span><span class="sxs-lookup"><span data-stu-id="4d76d-111">These deployment credentials should not be shared with other Azure users.</span></span>
    >
    >

* <span data-ttu-id="4d76d-112">**Alkalmazási szintű hitelesítő adatokat**: hitelesítő adatok az egyes alkalmazásokhoz egy készletét.</span><span class="sxs-lookup"><span data-stu-id="4d76d-112">**App-level credentials**: one set of credentials for each app.</span></span> <span data-ttu-id="4d76d-113">Csak az alkalmazás telepítéséhez használható.</span><span class="sxs-lookup"><span data-stu-id="4d76d-113">It can be used to deploy to that app only.</span></span> <span data-ttu-id="4d76d-114">A hitelesítő adatokat az egyes alkalmazások a alkalmazás létrehozásakor automatikusan létrejön, és az alkalmazás megtalálható közzétételi profil.</span><span class="sxs-lookup"><span data-stu-id="4d76d-114">The credentials for each app is generated automatically at app creation, and is found in the app's publish profile.</span></span> <span data-ttu-id="4d76d-115">Nem lehet manuálisan konfigurálnia a hitelesítő adatokat, de visszaállíthatja őket egy olyan alkalmazáshoz bármikor.</span><span class="sxs-lookup"><span data-stu-id="4d76d-115">You cannot manually configure the credentials, but you can reset them for an app anytime.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4d76d-116">Annak érdekében, hogy illetéktelen személyek számára hozzáférést a hitelesítő adatokat keresztül szerepköralapú hozzáférés vezérlés (RBAC), meg kell győződnie őket közreműködő vagy magasabb a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4d76d-116">In order to give someone access to these credentials via Role Based Access Control (RBAC), you need to make them contributor or higher on the Web App.</span></span> <span data-ttu-id="4d76d-117">Olvasók nem engedélyezett a közzététel, és ezért nem tudja elérni ezeket a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="4d76d-117">Readers are not allowed to publish, and hence can't access those credentials.</span></span>
    >
    >

## <span data-ttu-id="4d76d-118"><a name="userscope"></a>Állítsa be, és a felhasználói szintű hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="4d76d-118"><a name="userscope"></a>Set and reset user-level credentials</span></span>

<span data-ttu-id="4d76d-119">Konfigurálhatja a felhasználói szintű hitelesítő adatokat a bármely alkalmazás [erőforráspanelen](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="4d76d-119">You can configure your user-level credentials in any app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span> <span data-ttu-id="4d76d-120">Minden alkalmazás és az Azure-fiókjával a előfizetéseket függetlenül melyik alkalmazás konfigurálja ezeket a hitelesítő adatokat, vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="4d76d-120">Regardless in which app you configure these credentials, it applies to all apps and for all subscriptions in your Azure account.</span></span> 

<span data-ttu-id="4d76d-121">A felhasználói szintű hitelesítő adatok konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="4d76d-121">To configure your user-level credentials:</span></span>

1. <span data-ttu-id="4d76d-122">Az a [Azure-portálon](https://portal.azure.com), kattintson az App Service >  **&lt;any_app >** > **üzembe helyezési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="4d76d-122">In the [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Deployment credentials**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4d76d-123">A portálon rendelkeznie kell legalább egy alkalmazást a központi telepítési hitelesítő adatok panelt elérése előtt.</span><span class="sxs-lookup"><span data-stu-id="4d76d-123">In the portal, you must have at least one app before you can access the deployment credentials blade.</span></span> <span data-ttu-id="4d76d-124">Azonban a a [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md), konfigurálhatja a felhasználói szintű hitelesítő adatok nélkül egy meglévő alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4d76d-124">However, with the [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md), you can configure user-level credentials without an existing app.</span></span>

2. <span data-ttu-id="4d76d-125">Konfigurálja a felhasználónevet és jelszót, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="4d76d-125">Configure the user name and password, and then click **Save**.</span></span>

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

<span data-ttu-id="4d76d-126">Az üzembe helyezési hitelesítő adatok megadása után található a *Git* az alkalmazás központi telepítési felhasználónév **áttekintése**,</span><span class="sxs-lookup"><span data-stu-id="4d76d-126">Once you have set your deployment credentials, you can find the *Git* deployment username in your app's **Overview**,</span></span>

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

<span data-ttu-id="4d76d-127">és és *FTP* az alkalmazás központi telepítési felhasználónév **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="4d76d-127">and and *FTP* deployment username in your app's **Properties**.</span></span>

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> <span data-ttu-id="4d76d-128">Azure nem jeleníti meg a felhasználói szintű telepítési jelszavát.</span><span class="sxs-lookup"><span data-stu-id="4d76d-128">Azure does not show your user-level deployment password.</span></span> <span data-ttu-id="4d76d-129">Ha elfelejti a jelszavát, nem lehet beolvasni.</span><span class="sxs-lookup"><span data-stu-id="4d76d-129">If you forget the password, you can't retrieve it.</span></span> <span data-ttu-id="4d76d-130">Azonban, alaphelyzetbe állíthatja a hitelesítő adatok ebben a szakaszban ismertetett lépések szerint.</span><span class="sxs-lookup"><span data-stu-id="4d76d-130">However, you can reset your credentials by following the steps in this section.</span></span>
>
>  

## <span data-ttu-id="4d76d-131"><a name="appscope"></a>Első és az alkalmazási szintű hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="4d76d-131"><a name="appscope"></a>Get and reset app-level credentials</span></span>
<span data-ttu-id="4d76d-132">Minden alkalmazás az App Service-ben, az alkalmazási szintű tárolják a hitelesítő adatokat az XML-közzétételi profil.</span><span class="sxs-lookup"><span data-stu-id="4d76d-132">For each app in App Service, its app-level credentials are stored in the XML publish profile.</span></span>

<span data-ttu-id="4d76d-133">Az alkalmazási szintű hitelesítő adatainak lekéréséhez:</span><span class="sxs-lookup"><span data-stu-id="4d76d-133">To get the app-level credentials:</span></span>

1. <span data-ttu-id="4d76d-134">Az a [Azure-portálon](https://portal.azure.com), kattintson az App Service >  **&lt;any_app >** > **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="4d76d-134">In the [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Overview**.</span></span>

2. <span data-ttu-id="4d76d-135">Kattintson a **... További** > **Get közzétételi profil**, és elindítja a letöltési egy. PublishSettings-fájl.</span><span class="sxs-lookup"><span data-stu-id="4d76d-135">Click **...More** > **Get publish profile**, and download starts for a .PublishSettings file.</span></span>

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. <span data-ttu-id="4d76d-136">Nyissa meg a. PublishSettings fájlt, és keresse a `<publishProfile>` attribútummal rendelkező címke `publishMethod="FTP"`.</span><span class="sxs-lookup"><span data-stu-id="4d76d-136">Open the .PublishSettings file and find the `<publishProfile>` tag with the attribute `publishMethod="FTP"`.</span></span> <span data-ttu-id="4d76d-137">Ezt követően az beszerzése a `userName` és `password` attribútumok.</span><span class="sxs-lookup"><span data-stu-id="4d76d-137">Then, get its `userName` and `password` attributes.</span></span>
<span data-ttu-id="4d76d-138">Ezek azok az alkalmazási szintű hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="4d76d-138">These are the app-level credentials.</span></span>

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    <span data-ttu-id="4d76d-139">A felhasználói szintű hitelesítő adatokat hasonló, az FTP telepítési felhasználónév az formátumban `<app_name>\<username>`, és a Git telepítési felhasználónév az imént `<username>` anélkül, hogy az előző `<app_name>\`.</span><span class="sxs-lookup"><span data-stu-id="4d76d-139">Similar to the user-level credentials, the FTP deployment username is in the format of `<app_name>\<username>`, and the Git deployment username is just `<username>` without the preceding `<app_name>\`.</span></span>

<span data-ttu-id="4d76d-140">Az alkalmazási szintű hitelesítő adatok alaphelyzetbe állítása:</span><span class="sxs-lookup"><span data-stu-id="4d76d-140">To reset the app-level credentials:</span></span>

1. <span data-ttu-id="4d76d-141">Az a [Azure-portálon](https://portal.azure.com), kattintson az App Service >  **&lt;any_app >** > **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="4d76d-141">In the [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Overview**.</span></span>

2. <span data-ttu-id="4d76d-142">Kattintson a **... További** > **a közzétételi profil alaphelyzetbe állítása**.</span><span class="sxs-lookup"><span data-stu-id="4d76d-142">Click **...More** > **Reset publish profile**.</span></span> <span data-ttu-id="4d76d-143">Kattintson a **Igen** az alaphelyzetbe állítás megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4d76d-143">Click **Yes** to confirm the reset.</span></span>

    <span data-ttu-id="4d76d-144">A reset művelet érvényteleníti a korábban letöltött. PublishSettings-fájlok.</span><span class="sxs-lookup"><span data-stu-id="4d76d-144">The reset action invalidates any previously-downloaded .PublishSettings files.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d76d-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4d76d-145">Next steps</span></span>

<span data-ttu-id="4d76d-146">Megtudhatja, hogyan telepítse az alkalmazást a rendszer ezen hitelesítő adatok segítségével [helyi Git](app-service-deploy-local-git.md) vagy [FTP/S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="4d76d-146">Find out how to use these credentials to deploy your app from [local Git](app-service-deploy-local-git.md) or using [FTP/S](app-service-deploy-ftp.md).</span></span>
