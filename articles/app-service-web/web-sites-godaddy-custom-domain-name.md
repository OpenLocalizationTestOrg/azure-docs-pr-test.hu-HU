---
title: "Egyéni tartománynév beállítása az Azure App Service (GoDaddy)"
description: "A GoDaddy tartomány nevét az Azure Web Apps használata"
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
ms.openlocfilehash: 158c5dc06f83e16633d3c2fbb4eb27d3e8af030c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-purchased-directly-from-godaddy"></a><span data-ttu-id="b3644-103">Egyéni (közvetlenül a GoDaddytől vásárolt) tartománynév konfigurálása az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="b3644-103">Configure a custom domain name in Azure App Service (Purchased directly from GoDaddy)</span></span>
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro.md)]

<span data-ttu-id="b3644-104">Ha tartományi Azure App Service Web Apps keresztül vásárolt, akkor tekintse meg az utolsó lépésében [tartomány vásárlása Web Apps](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="b3644-104">If you have purchased domain through Azure App Service Web Apps then refer to the final step of [Buy Domain for Web Apps](custom-dns-web-site-buydomains-web-app.md).</span></span>

<span data-ttu-id="b3644-105">Ez a cikk útmutatás segítségével egy egyéni tartománynevet, közvetlenül a megvásárolt [GoDaddy](https://godaddy.com) rendelkező [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="b3644-105">This article provides instructions on using a custom domain name that was purchased directly from [GoDaddy](https://godaddy.com) with [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a><span data-ttu-id="b3644-106">DNS-rekordok ismertetése</span><span class="sxs-lookup"><span data-stu-id="b3644-106">Understanding DNS records</span></span>
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-raw.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a><span data-ttu-id="b3644-107">Az egyéni tartomány DNS-rekord hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b3644-107">Add a DNS record for your custom domain</span></span>
<span data-ttu-id="b3644-108">Az egyéni tartomány társítandó egy webalkalmazást az App Service-ben, hozzá kell adnia egy új bejegyzést a DNS-tábla az egyéni tartomány GoDaddy által biztosított eszközök segítségével.</span><span class="sxs-lookup"><span data-stu-id="b3644-108">To associate your custom domain with a web app in App Service, you must add a new entry in the DNS table for your custom domain by using tools provided by GoDaddy.</span></span> <span data-ttu-id="b3644-109">Az alábbi lépések segítségével keresse meg a DNS-eszközök a GoDaddy.com</span><span class="sxs-lookup"><span data-stu-id="b3644-109">Use the following steps to locate the DNS tools for GoDaddy.com</span></span>

1. <span data-ttu-id="b3644-110">Jelentkezzen be GoDaddy.com fiókját, és válassza ki **saját fiók** , majd **a tartományok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="b3644-110">Log on to your account with GoDaddy.com, and select **My Account** and then **Manage my domains**.</span></span> <span data-ttu-id="b3644-111">Válassza ki a tartománynév, amely az Azure-webalkalmazásban használ, és válassza ki a legördülő menü **kezelése DNS**.</span><span class="sxs-lookup"><span data-stu-id="b3644-111">Select the drop-down menu for the domain name that you wish to use with your Azure web app and select **Manage DNS**.</span></span>
   
    ![GoDaddy tartozó egyéni tartomány lap](./media/web-sites-godaddy-custom-domain-name/godaddy-customdomain.png)
2. <span data-ttu-id="b3644-113">Az a **tartományhoz részleteinek** lapon, görgessen a **DNS-zónafájlját** fülre.</span><span class="sxs-lookup"><span data-stu-id="b3644-113">From the **Domain details** page, scroll to the **DNS Zone File** tab.</span></span> <span data-ttu-id="b3644-114">Ez az a és a tartománynévhez tartozó DNS-rekordok módosításának használt szakasz.</span><span class="sxs-lookup"><span data-stu-id="b3644-114">This is the section used for adding and modifying DNS records for your domain name.</span></span>
   
    ![DNS-zónafájlját lap](./media/web-sites-godaddy-custom-domain-name/godaddy-zonetab.png)
   
    <span data-ttu-id="b3644-116">Válassza ki **rekord hozzáadása** hozzáadni egy meglévő rekordot.</span><span class="sxs-lookup"><span data-stu-id="b3644-116">Select **Add Record** to add an existing record.</span></span>
   
    <span data-ttu-id="b3644-117">A **szerkesztése** egy létező bejegyzést, válassza ki a toll & papír a bejegyzés mellett.</span><span class="sxs-lookup"><span data-stu-id="b3644-117">To **edit** an existing record, select the pen & paper icon beside the record.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b3644-118">Új rekord hozzáadása előtt vegye figyelembe, hogy a GoDaddy már hozott létre DNS-rekordjainak a népszerű altartományok (nevű **állomás** -szerkesztőben), mint **e-mail**, **fájlok**, **mail**, és mások számára.</span><span class="sxs-lookup"><span data-stu-id="b3644-118">Before adding new records, note that GoDaddy has already created DNS records for popular sub-domains (called **Host** in editor,) such as **email**, **files**, **mail**, and others.</span></span> <span data-ttu-id="b3644-119">Ha létezik-e már a használni kívánt nevet, módosítsa az új létrehozása helyett a meglévő rekord.</span><span class="sxs-lookup"><span data-stu-id="b3644-119">If the name you wish to use already exists, modify the existing record instead of creating a new one.</span></span>
   > 
   > 
3. <span data-ttu-id="b3644-120">Amikor felvesz egy bejegyzést, először ki kell választani a rekord típusát.</span><span class="sxs-lookup"><span data-stu-id="b3644-120">When adding a record, you must first select the record type.</span></span>
   
    ![Válassza ki a rekord típusa](./media/web-sites-godaddy-custom-domain-name/godaddy-selectrecordtype.png)
   
    <span data-ttu-id="b3644-122">Ezután meg kell adnia a **állomás** (az egyéni tartomány vagy altartomány) és amit a rendszergazda **mutat**.</span><span class="sxs-lookup"><span data-stu-id="b3644-122">Next, you must provide the **Host** (the custom domain or sub-domain) and what it **Points to**.</span></span>
   
    ![zóna rekord hozzáadása](./media/web-sites-godaddy-custom-domain-name/godaddy-addzonerecord.png)
   
   * <span data-ttu-id="b3644-124">Hozzáadásakor egy **(állomás) erőforrásrekord** -meg kell adni a **állomás** mező vagy  **@**  (gyökértartomány neve, mint például jelképez **contoso.com**,) * (több altartományok egyeztetéséhez helyettesítő) vagy a altartomány szeretné használni (például **www**.) Meg kell adni a **mutat** mezőről az Azure-webalkalmazásban IP-címét.</span><span class="sxs-lookup"><span data-stu-id="b3644-124">When adding an **A (host) record** - you must set the **Host** field to either **@** (this represents root domain name, such as **contoso.com**,) * (a wildcard for matching multiple sub-domains,) or the sub-domain you wish to use (for example, **www**.) You must set the **Points to** field to the IP address of your Azure web app.</span></span>
   * <span data-ttu-id="b3644-125">Hozzáadásakor egy **(aliast) CNAME rekord** -meg kell adni a **állomás** mezőről az altartomány szeretné használni.</span><span class="sxs-lookup"><span data-stu-id="b3644-125">When adding a **CNAME (alias) record** - you must set the **Host** field to the sub-domain you wish to use.</span></span> <span data-ttu-id="b3644-126">Például **www**.</span><span class="sxs-lookup"><span data-stu-id="b3644-126">For example, **www**.</span></span> <span data-ttu-id="b3644-127">Meg kell adni a **mutat** mezőről az **. azurewebsites.net** tartománynevét, az Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b3644-127">You must set the **Points to** field to the **.azurewebsites.net** domain name of your Azure web app.</span></span> <span data-ttu-id="b3644-128">Például **contoso.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="b3644-128">For example, **contoso.azurewebsites.net**.</span></span>
4. <span data-ttu-id="b3644-129">Kattintson a **hozzáadása egy másik**.</span><span class="sxs-lookup"><span data-stu-id="b3644-129">Click **Add Another**.</span></span>
5. <span data-ttu-id="b3644-130">Válassza ki **TXT** , a rekordtípust, majd adja meg a **állomás** értékének  **@**  és egy **mutat** értékének  **&lt;yourwebappname&gt;. azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="b3644-130">Select **TXT** as the record type, then specify a **Host** value of **@** and a **Points to** value of **&lt;yourwebappname&gt;.azurewebsites.net**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b3644-131">A TXT-rekord segítségével Azure ellenőrizze a tartomány az A rekord vagy az első TXT-rekord saját.</span><span class="sxs-lookup"><span data-stu-id="b3644-131">This TXT record is used by Azure to validate that you own the domain described by the A record or the first TXT record.</span></span> <span data-ttu-id="b3644-132">Miután a tartomány lett leképezve: a webalkalmazás az Azure portálon, a TXT-rekord bejegyzés lehet eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="b3644-132">Once the domain has been mapped to the web app in the Azure Portal, this TXT record entry can be removed.</span></span>
   > 
   > 
6. <span data-ttu-id="b3644-133">Hozzáadása után, vagy ha a bejegyzések módosítása, kattintson a **Befejezés** menti a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b3644-133">When you have finished adding or modifying records, click **Finish** to save changes.</span></span>

<a name="enabledomain"></a>

## <a name="enable-the-domain-name-on-your-web-app"></a><span data-ttu-id="b3644-134">A tartomány nevét a webalkalmazásban engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b3644-134">Enable the domain name on your web app</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

> [!NOTE]
> <span data-ttu-id="b3644-135">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="b3644-135">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="b3644-136">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="b3644-136">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="b3644-137">A változások</span><span class="sxs-lookup"><span data-stu-id="b3644-137">What's changed</span></span>
* <span data-ttu-id="b3644-138">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="b3644-138">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

