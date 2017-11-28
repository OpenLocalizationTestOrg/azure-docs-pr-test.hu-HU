---
title: "App Service üzembe helyezési hitelesítő adatok aaaAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure App Service üzembe helyezési hitelesítő adatokat."
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
ms.openlocfilehash: d6f9f5cc1b62a17c42643266f4c9490f827c63f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a><span data-ttu-id="572ff-103">Telepítési hitelesítő adatok beállítása az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="572ff-103">Configure deployment credentials for Azure App Service</span></span>
<span data-ttu-id="572ff-104">[Az Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) támogatja a hitelesítő adatok kétféle [helyi Git-telepítésének](app-service-deploy-local-git.md) és [FTP/S telepítési](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="572ff-104">[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) supports two types of credentials for [local Git deployment](app-service-deploy-local-git.md) and [FTP/S deployment](app-service-deploy-ftp.md).</span></span> <span data-ttu-id="572ff-105">A rendszer nem hello ugyanaz, mint az Azure Active Directorybeli hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="572ff-105">These are not hello same as your Azure Active Directory credentials.</span></span>

* <span data-ttu-id="572ff-106">**Felhasználói szintű hitelesítő adatokat**: egy készletét hello teljes Azure-fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="572ff-106">**User-level credentials**: one set of credentials for hello entire Azure account.</span></span> <span data-ttu-id="572ff-107">A bármely alkalmazás bármely előfizetés, amely az Azure-fiók hello van engedélye tooaccess használt toodeploy tooApp szolgáltatás lehet.</span><span class="sxs-lookup"><span data-stu-id="572ff-107">It can be used toodeploy tooApp Service for any app, in any subscription, that hello Azure account has permission tooaccess.</span></span> <span data-ttu-id="572ff-108">Ezek a hello alapértelmezett hitelesítő adatok beállítása, amelyet megadtak **alkalmazásszolgáltatások** > **&lt;alkalmazás_neve >** > **üzembe helyezési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="572ff-108">These are hello default credentials set that you configure in **App Services** > **&lt;app_name>** > **Deployment credentials**.</span></span> <span data-ttu-id="572ff-109">Ez az alapértelmezés szerint, amely az illesztett hello portálon grafikus felhasználói Felülettel is hello (például hello **áttekintése** és **tulajdonságok** az alkalmazás [erőforráspanelen](../azure-resource-manager/resource-group-portal.md#manage-resources)).</span><span class="sxs-lookup"><span data-stu-id="572ff-109">This is also hello default set that's surfaced in hello portal GUI (such as hello **Overview** and **Properties** of your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources)).</span></span>

    > [!NOTE]
    > <span data-ttu-id="572ff-110">Delegálja a szerepköralapú hozzáférés vezérlés (RBAC) vagy társadminisztrátornak engedélyek tooAzure erőforrások eléréséhez, ha minden Azure felhasználói, amely megkapja a hozzáférési tooan alkalmazás képes személyes felhasználói szintű hitelesítő adatokat használhatna, amíg hozzáférését visszavonja.</span><span class="sxs-lookup"><span data-stu-id="572ff-110">When you delegate access tooAzure resources via Role Based Access Control (RBAC) or co-admin permissions, each Azure user that receives access tooan app can use his/her personal user-level credentials until access is revoked.</span></span> <span data-ttu-id="572ff-111">A központi telepítési hitelesítő adatokat nem lehet megosztva Azure másokkal.</span><span class="sxs-lookup"><span data-stu-id="572ff-111">These deployment credentials should not be shared with other Azure users.</span></span>
    >
    >

* <span data-ttu-id="572ff-112">**Alkalmazási szintű hitelesítő adatokat**: hitelesítő adatok az egyes alkalmazásokhoz egy készletét.</span><span class="sxs-lookup"><span data-stu-id="572ff-112">**App-level credentials**: one set of credentials for each app.</span></span> <span data-ttu-id="572ff-113">Lehet, hogy a használt toodeploy toothat alkalmazás hátterére van hatással.</span><span class="sxs-lookup"><span data-stu-id="572ff-113">It can be used toodeploy toothat app only.</span></span> <span data-ttu-id="572ff-114">hello hitelesítő adatokat az egyes alkalmazások a alkalmazás létrehozásakor automatikusan létrejön, és hello alkalmazásban található közzétételi profil.</span><span class="sxs-lookup"><span data-stu-id="572ff-114">hello credentials for each app is generated automatically at app creation, and is found in hello app's publish profile.</span></span> <span data-ttu-id="572ff-115">Nem konfigurálhatja manuálisan hello hitelesítő adatokat, de visszaállíthatja őket egy olyan alkalmazáshoz bármikor.</span><span class="sxs-lookup"><span data-stu-id="572ff-115">You cannot manually configure hello credentials, but you can reset them for an app anytime.</span></span>

    > [!NOTE]
    > <span data-ttu-id="572ff-116">A sorrend toogive valaki hozzáférési toothese hitelesítő adatokat keresztül szerepköralapú hozzáférés vezérlés (RBAC), toomake kell őket közreműködő vagy nagyobb hello Web App.</span><span class="sxs-lookup"><span data-stu-id="572ff-116">In order toogive someone access toothese credentials via Role Based Access Control (RBAC), you need toomake them contributor or higher on hello Web App.</span></span> <span data-ttu-id="572ff-117">Olvasók toopublish nem engedélyezett, és ezért nem tudja elérni ezeket a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="572ff-117">Readers are not allowed toopublish, and hence can't access those credentials.</span></span>
    >
    >

## <span data-ttu-id="572ff-118"><a name="userscope"></a>Állítsa be, és a felhasználói szintű hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="572ff-118"><a name="userscope"></a>Set and reset user-level credentials</span></span>

<span data-ttu-id="572ff-119">Konfigurálhatja a felhasználói szintű hitelesítő adatokat a bármely alkalmazás [erőforráspanelen](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="572ff-119">You can configure your user-level credentials in any app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span> <span data-ttu-id="572ff-120">Attól függetlenül történik melyik alkalmazás konfigurálja ezeket a hitelesítő adatokat, tooall alkalmazások vonatkozik, és az Azure-előfizetések minden fiókot.</span><span class="sxs-lookup"><span data-stu-id="572ff-120">Regardless in which app you configure these credentials, it applies tooall apps and for all subscriptions in your Azure account.</span></span> 

<span data-ttu-id="572ff-121">tooconfigure a felhasználói szintű hitelesítő adatait:</span><span class="sxs-lookup"><span data-stu-id="572ff-121">tooconfigure your user-level credentials:</span></span>

1. <span data-ttu-id="572ff-122">A hello [Azure-portálon](https://portal.azure.com), kattintson az App Service >  **&lt;any_app >** > **üzembe helyezési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="572ff-122">In hello [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Deployment credentials**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="572ff-123">Hello portálon rendelkeznie kell legalább egy alkalmazás eléréséhez hello telepítési hitelesítő adatok panelt.</span><span class="sxs-lookup"><span data-stu-id="572ff-123">In hello portal, you must have at least one app before you can access hello deployment credentials blade.</span></span> <span data-ttu-id="572ff-124">Azonban a hello [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md), konfigurálhatja a felhasználói szintű hitelesítő adatok nélkül egy meglévő alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="572ff-124">However, with hello [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md), you can configure user-level credentials without an existing app.</span></span>

2. <span data-ttu-id="572ff-125">Konfigurálja a hello felhasználónevét és jelszavát, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="572ff-125">Configure hello user name and password, and then click **Save**.</span></span>

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

<span data-ttu-id="572ff-126">Az üzembe helyezési hitelesítő adatok megadása után hello található *Git* az alkalmazás központi telepítési felhasználónév **áttekintése**,</span><span class="sxs-lookup"><span data-stu-id="572ff-126">Once you have set your deployment credentials, you can find hello *Git* deployment username in your app's **Overview**,</span></span>

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

<span data-ttu-id="572ff-127">és és *FTP* az alkalmazás központi telepítési felhasználónév **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="572ff-127">and and *FTP* deployment username in your app's **Properties**.</span></span>

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> <span data-ttu-id="572ff-128">Azure nem jeleníti meg a felhasználói szintű telepítési jelszavát.</span><span class="sxs-lookup"><span data-stu-id="572ff-128">Azure does not show your user-level deployment password.</span></span> <span data-ttu-id="572ff-129">Ha hello jelszavát, nem lehet beolvasni.</span><span class="sxs-lookup"><span data-stu-id="572ff-129">If you forget hello password, you can't retrieve it.</span></span> <span data-ttu-id="572ff-130">Azonban, alaphelyzetbe állíthatja a hitelesítő adatok hello ebben a szakaszban ismertetett lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="572ff-130">However, you can reset your credentials by following hello steps in this section.</span></span>
>
>  

## <span data-ttu-id="572ff-131"><a name="appscope"></a>Első és az alkalmazási szintű hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="572ff-131"><a name="appscope"></a>Get and reset app-level credentials</span></span>
<span data-ttu-id="572ff-132">Minden alkalmazás az App Service-ben, az alkalmazási szintű tárolt hitelesítő adatok vannak hello XML közzétételi profil.</span><span class="sxs-lookup"><span data-stu-id="572ff-132">For each app in App Service, its app-level credentials are stored in hello XML publish profile.</span></span>

<span data-ttu-id="572ff-133">tooget hello alkalmazási szintű hitelesítő adatokat:</span><span class="sxs-lookup"><span data-stu-id="572ff-133">tooget hello app-level credentials:</span></span>

1. <span data-ttu-id="572ff-134">A hello [Azure-portálon](https://portal.azure.com), kattintson az App Service >  **&lt;any_app >** > **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="572ff-134">In hello [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Overview**.</span></span>

2. <span data-ttu-id="572ff-135">Kattintson a **... További** > **Get közzétételi profil**, és elindítja a letöltési egy. PublishSettings-fájl.</span><span class="sxs-lookup"><span data-stu-id="572ff-135">Click **...More** > **Get publish profile**, and download starts for a .PublishSettings file.</span></span>

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. <span data-ttu-id="572ff-136">Nyissa meg hello. PublishSettings fájlt, és megkeresi hello `<publishProfile>` hello attribútummal rendelkező címke `publishMethod="FTP"`.</span><span class="sxs-lookup"><span data-stu-id="572ff-136">Open hello .PublishSettings file and find hello `<publishProfile>` tag with hello attribute `publishMethod="FTP"`.</span></span> <span data-ttu-id="572ff-137">Ezt követően az beszerzése a `userName` és `password` attribútumok.</span><span class="sxs-lookup"><span data-stu-id="572ff-137">Then, get its `userName` and `password` attributes.</span></span>
<span data-ttu-id="572ff-138">Ezek a hello alkalmazási szintű hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="572ff-138">These are hello app-level credentials.</span></span>

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    <span data-ttu-id="572ff-139">Hasonló toohello felhasználói szintű hitelesítő adatokat, hello FTP telepítési felhasználónév hello formátumban kell `<app_name>\<username>`, és csak akkor lesz hello Git telepítési felhasználónév `<username>` nélkül előző hello `<app_name>\`.</span><span class="sxs-lookup"><span data-stu-id="572ff-139">Similar toohello user-level credentials, hello FTP deployment username is in hello format of `<app_name>\<username>`, and hello Git deployment username is just `<username>` without hello preceding `<app_name>\`.</span></span>

<span data-ttu-id="572ff-140">tooreset hello alkalmazási szintű hitelesítő adatokat:</span><span class="sxs-lookup"><span data-stu-id="572ff-140">tooreset hello app-level credentials:</span></span>

1. <span data-ttu-id="572ff-141">A hello [Azure-portálon](https://portal.azure.com), kattintson az App Service >  **&lt;any_app >** > **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="572ff-141">In hello [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Overview**.</span></span>

2. <span data-ttu-id="572ff-142">Kattintson a **... További** > **a közzétételi profil alaphelyzetbe állítása**.</span><span class="sxs-lookup"><span data-stu-id="572ff-142">Click **...More** > **Reset publish profile**.</span></span> <span data-ttu-id="572ff-143">Kattintson a **Igen** tooconfirm hello alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="572ff-143">Click **Yes** tooconfirm hello reset.</span></span>

    <span data-ttu-id="572ff-144">a korábban letöltött hello a reset művelet érvénytelenné válik. PublishSettings-fájlok.</span><span class="sxs-lookup"><span data-stu-id="572ff-144">hello reset action invalidates any previously-downloaded .PublishSettings files.</span></span>

## <a name="next-steps"></a><span data-ttu-id="572ff-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="572ff-145">Next steps</span></span>

<span data-ttu-id="572ff-146">Megtudhatja, hogyan toouse ezen hitelesítő adatok toodeploy az alkalmazását [helyi Git](app-service-deploy-local-git.md) vagy [FTP/S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="572ff-146">Find out how toouse these credentials toodeploy your app from [local Git](app-service-deploy-local-git.md) or using [FTP/S](app-service-deploy-ftp.md).</span></span>
