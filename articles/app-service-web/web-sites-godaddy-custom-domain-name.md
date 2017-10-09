---
title: "az Azure App Service (GoDaddy) egy egyéni tartománynevet aaaConfigure"
description: "Ismerje meg, hogyan toouse a tartomány nevére az Azure Web Apps GoDaddy"
services: app-service
documentationcenter: 
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 33233e30-5846-488f-83f3-b32e5c114564
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: 48158ee752f9833249bbf85adf80f572d1c68486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-purchased-directly-from-godaddy"></a><span data-ttu-id="c9387-103">Egyéni (közvetlenül a GoDaddytől vásárolt) tartománynév konfigurálása az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="c9387-103">Configure a custom domain name in Azure App Service (Purchased directly from GoDaddy)</span></span>
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro.md)]

<span data-ttu-id="c9387-104">Ha a tartomány Azure App Service Web Apps keresztül vásárolt, majd tekintse meg az utolsó lépésében toohello [tartomány vásárlása Web Apps](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="c9387-104">If you have purchased domain through Azure App Service Web Apps then refer toohello final step of [Buy Domain for Web Apps](custom-dns-web-site-buydomains-web-app.md).</span></span>

<span data-ttu-id="c9387-105">Ez a cikk útmutatás segítségével egy egyéni tartománynevet, közvetlenül a megvásárolt [GoDaddy](https://godaddy.com) rendelkező [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="c9387-105">This article provides instructions on using a custom domain name that was purchased directly from [GoDaddy](https://godaddy.com) with [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a><span data-ttu-id="c9387-106">DNS-rekordok ismertetése</span><span class="sxs-lookup"><span data-stu-id="c9387-106">Understanding DNS records</span></span>
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-raw.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a><span data-ttu-id="c9387-107">Az egyéni tartomány DNS-rekord hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c9387-107">Add a DNS record for your custom domain</span></span>
<span data-ttu-id="c9387-108">tooassociate egy webalkalmazást az App Service szolgáltatásban az egyéni tartományt, hozzá kell adnia egy új bejegyzést hello DNS-tábla az egyéni tartomány GoDaddy által biztosított eszközök segítségével.</span><span class="sxs-lookup"><span data-stu-id="c9387-108">tooassociate your custom domain with a web app in App Service, you must add a new entry in hello DNS table for your custom domain by using tools provided by GoDaddy.</span></span> <span data-ttu-id="c9387-109">Következő lépések toolocate hello DNS-eszközök a GoDaddy.com hello használata</span><span class="sxs-lookup"><span data-stu-id="c9387-109">Use hello following steps toolocate hello DNS tools for GoDaddy.com</span></span>

1. <span data-ttu-id="c9387-110">Jelentkezzen be GoDaddy.com tooyour fiókot, és válassza ki **saját fiók** , majd **a tartományok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="c9387-110">Log on tooyour account with GoDaddy.com, and select **My Account** and then **Manage my domains**.</span></span> <span data-ttu-id="c9387-111">Jelölje be hello legördülő menü hello tartománynév toouse kívánja az Azure-webalkalmazásban a, és válassza ki a **kezelése DNS**.</span><span class="sxs-lookup"><span data-stu-id="c9387-111">Select hello drop-down menu for hello domain name that you wish toouse with your Azure web app and select **Manage DNS**.</span></span>
   
    ![GoDaddy tartozó egyéni tartomány lap](./media/web-sites-godaddy-custom-domain-name/godaddy-customdomain.png)
2. <span data-ttu-id="c9387-113">A hello **tartományhoz részleteinek** párbeszédpanelen görgessen toohello **DNS-zónafájlját** fülre. Ez a hello szakasz hozzáadása és módosítása a tartománynévhez tartozó DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="c9387-113">From hello **Domain details** page, scroll toohello **DNS Zone File** tab. This is hello section used for adding and modifying DNS records for your domain name.</span></span>
   
    ![DNS-zónafájlját lap](./media/web-sites-godaddy-custom-domain-name/godaddy-zonetab.png)
   
    <span data-ttu-id="c9387-115">Válassza ki **rekord hozzáadása** tooadd meglévő bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="c9387-115">Select **Add Record** tooadd an existing record.</span></span>
   
    <span data-ttu-id="c9387-116">túl**szerkesztése** egy létező bejegyzést, jelölje be hello toll & papír ikon hello bejegyzés mellett.</span><span class="sxs-lookup"><span data-stu-id="c9387-116">too**edit** an existing record, select hello pen & paper icon beside hello record.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c9387-117">Új rekord hozzáadása előtt vegye figyelembe, hogy a GoDaddy már hozott létre DNS-rekordjainak a népszerű altartományok (nevű **állomás** -szerkesztőben), mint **e-mail**, **fájlok**, **mail**, és mások számára.</span><span class="sxs-lookup"><span data-stu-id="c9387-117">Before adding new records, note that GoDaddy has already created DNS records for popular sub-domains (called **Host** in editor,) such as **email**, **files**, **mail**, and others.</span></span> <span data-ttu-id="c9387-118">Ha hello neve toouse kívánja már létezik, módosítsa a hello meglévő rekord létrehozása egy új helyett.</span><span class="sxs-lookup"><span data-stu-id="c9387-118">If hello name you wish toouse already exists, modify hello existing record instead of creating a new one.</span></span>
   > 
   > 
3. <span data-ttu-id="c9387-119">A rekord felvételekor hello rekordtípus először ki kell választania.</span><span class="sxs-lookup"><span data-stu-id="c9387-119">When adding a record, you must first select hello record type.</span></span>
   
    ![Válassza ki a rekord típusa](./media/web-sites-godaddy-custom-domain-name/godaddy-selectrecordtype.png)
   
    <span data-ttu-id="c9387-121">Ezután meg kell adnia hello **állomás** (egyéni tartomány és altartomány hello) és amit a rendszergazda **mutat**.</span><span class="sxs-lookup"><span data-stu-id="c9387-121">Next, you must provide hello **Host** (hello custom domain or sub-domain) and what it **Points to**.</span></span>
   
    ![zóna rekord hozzáadása](./media/web-sites-godaddy-custom-domain-name/godaddy-addzonerecord.png)
   
   * <span data-ttu-id="c9387-123">Hozzáadásakor egy **(állomás) rekord** -meg kell adni a hello **állomás** mező tooeither  **@**  (gyökértartomány neve, mint például jelképez  **a contoso.com**,) * (több altartományok egyeztetéséhez helyettesítő) vagy hello altartomány toouse kívánja (például **www**.) Meg kell adni a hello **mutat** toohello IP-címét az Azure-webalkalmazásban mezőben.</span><span class="sxs-lookup"><span data-stu-id="c9387-123">When adding an **A (host) record** - you must set hello **Host** field tooeither **@** (this represents root domain name, such as **contoso.com**,) * (a wildcard for matching multiple sub-domains,) or hello sub-domain you wish toouse (for example, **www**.) You must set hello **Points to** field toohello IP address of your Azure web app.</span></span>
   * <span data-ttu-id="c9387-124">Hozzáadásakor egy **(aliast) CNAME rekord** -meg kell adni a hello **állomás** mező toohello altartomány toouse kívánja.</span><span class="sxs-lookup"><span data-stu-id="c9387-124">When adding a **CNAME (alias) record** - you must set hello **Host** field toohello sub-domain you wish toouse.</span></span> <span data-ttu-id="c9387-125">Például **www**.</span><span class="sxs-lookup"><span data-stu-id="c9387-125">For example, **www**.</span></span> <span data-ttu-id="c9387-126">Meg kell adni a hello **mutat** mező toohello **. azurewebsites.net** tartománynevét, az Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c9387-126">You must set hello **Points to** field toohello **.azurewebsites.net** domain name of your Azure web app.</span></span> <span data-ttu-id="c9387-127">Például **contoso.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="c9387-127">For example, **contoso.azurewebsites.net**.</span></span>
4. <span data-ttu-id="c9387-128">Kattintson a **hozzáadása egy másik**.</span><span class="sxs-lookup"><span data-stu-id="c9387-128">Click **Add Another**.</span></span>
5. <span data-ttu-id="c9387-129">Válassza ki **TXT** hello rekord típusa, majd adja meg a **állomás** értékének  **@**  és egy **mutat** értékének  **&lt;yourwebappname&gt;. azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="c9387-129">Select **TXT** as hello record type, then specify a **Host** value of **@** and a **Points to** value of **&lt;yourwebappname&gt;.azurewebsites.net**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c9387-130">A TXT-rekord hello rekord vagy hello első TXT rekord szerint hello tartomány saját Azure toovalidate használják.</span><span class="sxs-lookup"><span data-stu-id="c9387-130">This TXT record is used by Azure toovalidate that you own hello domain described by hello A record or hello first TXT record.</span></span> <span data-ttu-id="c9387-131">Miután hello tartományhoz már csatlakoztatott toohello webalkalmazás hello Azure portálon, a TXT-rekord bejegyzés lehet eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="c9387-131">Once hello domain has been mapped toohello web app in hello Azure Portal, this TXT record entry can be removed.</span></span>
   > 
   > 
6. <span data-ttu-id="c9387-132">Hozzáadása után, vagy ha a bejegyzések módosítása, kattintson a **Befejezés** toosave módosításokat.</span><span class="sxs-lookup"><span data-stu-id="c9387-132">When you have finished adding or modifying records, click **Finish** toosave changes.</span></span>

<a name="enabledomain"></a>

## <a name="enable-hello-domain-name-on-your-web-app"></a><span data-ttu-id="c9387-133">A webalkalmazásban hello tartománynév engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c9387-133">Enable hello domain name on your web app</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

> [!NOTE]
> <span data-ttu-id="c9387-134">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="c9387-134">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="c9387-135">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="c9387-135">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="c9387-136">A változások</span><span class="sxs-lookup"><span data-stu-id="c9387-136">What's changed</span></span>
* <span data-ttu-id="c9387-137">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="c9387-137">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

