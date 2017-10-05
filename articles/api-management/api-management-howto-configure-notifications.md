---
title: "Értesítések konfigurálása és az e-mail sablonok az Azure API Management |} Microsoft Docs"
description: "Útmutató a értesítések konfigurálása és az e-mail sablonok az Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: ee25f26d-4752-433b-af9c-3817db38aed5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 3d8b74e32059cfc1a4c3a8fc7d3bd04676ee80c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a><span data-ttu-id="1412a-103">Az értesítések és e-mail sablonok konfigurálása az Azure API Management szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="1412a-103">How to configure notifications and email templates in Azure API Management</span></span>
<span data-ttu-id="1412a-104">API-kezelés lehetővé teszi bizonyos események értesítéseinek konfigurálásához, és konfigurálhatja az e-mail-sablonokkal a rendszergazdák és fejlesztők számára az API Management-példány folytatott kommunikációhoz használt.</span><span class="sxs-lookup"><span data-stu-id="1412a-104">API Management provides the ability to configure notifications for specific events, and to configure the email templates that are used to communicate with the administrators and developers of an API Management instance.</span></span> <span data-ttu-id="1412a-105">Ez a témakör bemutatja, hogyan használható eseményt az értesítések konfigurálása, és ezeket az eseményeket az e-mail-sablonokat konfigurálásának áttekintése.</span><span class="sxs-lookup"><span data-stu-id="1412a-105">This topic shows how to configure notifications for the available events, and provides an overview of configuring the email templates used for these events.</span></span>

## <span data-ttu-id="1412a-106"><a name="publisher-notifications"></a>Publisher értesítések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1412a-106"><a name="publisher-notifications"> </a>Configure publisher notifications</span></span>
<span data-ttu-id="1412a-107">Értesítések konfigurálásához kattintson **Publisher portal** az Azure portálon az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1412a-107">To configure notifications, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="1412a-108">Ezzel továbblép az API Management közzétevő portáljára.</span><span class="sxs-lookup"><span data-stu-id="1412a-108">This takes you to the API Management publisher portal.</span></span>

![Közzétevő portál][api-management-management-console]

> [!NOTE] 
> <span data-ttu-id="1412a-110">Ha még nem hozott létre API Management szolgáltatáspéldányt, tekintse meg az [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management] oktatóanyag [API Management szolgáltatáspéldány létrehozása][Create an API Management service instance] című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="1412a-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

<span data-ttu-id="1412a-111">Kattintson a **értesítések** a a **API Management** megtekintéséhez az elérhető értesítések a bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="1412a-111">Click **Notifications** from the **API Management** menu on the left to view the available notifications.</span></span>

![A Publisher értesítések][api-management-publisher-notifications]

<span data-ttu-id="1412a-113">Az alábbi listán szereplő események értesítések konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="1412a-113">The following list of events can be configured for notifications.</span></span>

* <span data-ttu-id="1412a-114">**Előfizetési kérelmek (jóváhagyásra van szükség)** -a megadott e-mail címzettek és a felhasználók az előfizetés kérelmekről API termékek jóváhagyásra van szükség az e-mail értesítéseket kap.</span><span class="sxs-lookup"><span data-stu-id="1412a-114">**Subscription requests (requiring approval)** - The specified email recipients and users will receive email notifications about subscription requests for API products requiring approval.</span></span>
* <span data-ttu-id="1412a-115">**Új előfizetések** -a megadott e-mail címzettek és a felhasználók új API termék előfizetések kapcsolatos e-mail értesítéseket kap.</span><span class="sxs-lookup"><span data-stu-id="1412a-115">**New subscriptions** - The specified email recipients and users will receive email notifications about new API product subscriptions.</span></span>
* <span data-ttu-id="1412a-116">**Gyűjteményelem alkalmazáskérelmeinek** -a megadott e-mail címzettek és a felhasználók kapnak értesítő e-mailek, amikor az alkalmazás gyűjteményébe új kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="1412a-116">**Application gallery requests** - The specified email recipients and users will receive email notifications when new applications are submitted to the application gallery.</span></span>
* <span data-ttu-id="1412a-117">**Titkos másolat** -a megadott e-mail címzettek és a felhasználók kapnak e-mailek Titkos másolatot a fejlesztők számára küldött összes e-maileket.</span><span class="sxs-lookup"><span data-stu-id="1412a-117">**BCC** - The specified email recipients and users will receive email blind carbon copies of all emails sent to developers.</span></span>
* <span data-ttu-id="1412a-118">**Új probléma vagy megjegyzés** – a megadott e-mail címzettek és a felhasználók kapnak értesítő e-mailek, amikor új probléma vagy megjegyzés küldése a fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="1412a-118">**New issue or comment** - The specified email recipients and users will receive email notifications when a new issue or comment is submitted on the developer portal.</span></span>
* <span data-ttu-id="1412a-119">**Zárja be a fiók üzenet** -a megadott e-mail címzettek és a felhasználók kapnak értesítő e-mailek, amikor egy fiók le van zárva.</span><span class="sxs-lookup"><span data-stu-id="1412a-119">**Close account message** - The specified email recipients and users will receive email notifications when an account is closed.</span></span>
* <span data-ttu-id="1412a-120">**Megközelíti előfizetés kvótakorlátot** -a következő e-mailek címzettjeinek és felhasználók kapnak értesítő e-mailek, amikor az előfizetés használatának lekérdezi megközelíti a memóriahasználati kvóta.</span><span class="sxs-lookup"><span data-stu-id="1412a-120">**Approaching subscription quota limit** - The following email recipients and users will receive email notifications when subscription usage gets close to usage quota.</span></span>

<span data-ttu-id="1412a-121">Az egyes eseményekhez megadhatja az e-mailek címzettjeinek az e-mail cím beviteli mezőbe, vagy választhat a felhasználók listájából.</span><span class="sxs-lookup"><span data-stu-id="1412a-121">For each event, you can specify email recipients using the email address text box or you can select users from a list.</span></span>

<span data-ttu-id="1412a-122">Adja meg az e-mail címeket, adja meg azokat az e-mail cím szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="1412a-122">To specify the email addresses to be notified, enter them in the email address text box.</span></span> <span data-ttu-id="1412a-123">Ha az e-mail címeket, külön azokat vesszővel válassza el őket.</span><span class="sxs-lookup"><span data-stu-id="1412a-123">If you have multiple email addresses, separate them using commas.</span></span>

![Értesítés címzettjeinek][api-management-email-addresses]

<span data-ttu-id="1412a-125">Adja meg a felhasználóknak szeretne értesítést kapni, kattintson a **címzettet adjon hozzá**, jelölje be a jelölőnégyzetet, kattintson a felhasználók értesítést kapnak, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="1412a-125">To specify the users to be notified, click **add recipient**, check the box beside the users to be notified, and click **OK**.</span></span>

> [!NOTE] 
> <span data-ttu-id="1412a-126">Csak a rendszergazdák a listában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="1412a-126">Only administrators are displayed in the list.</span></span>


<span data-ttu-id="1412a-127">Miután az értesítési címzettek, kattintson a **mentése** a frissített értesítés címzettjeinek alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="1412a-127">After configuring the notification recipients, click **Save** to apply the updated notification recipients.</span></span>

> [!NOTE] 
> <span data-ttu-id="1412a-128">Ha manuálisan lép távolabb a **Publisher értesítések** lapon a közzétevő portal riasztást küld a hiba nem mentett módosítások vannak.</span><span class="sxs-lookup"><span data-stu-id="1412a-128">If you navigate away from the **Publisher Notifications** tab the publisher portal alerts you if there are unsaved changes.</span></span>


## <span data-ttu-id="1412a-129"><a name="email-templates"></a>E-mail sablonok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1412a-129"><a name="email-templates"> </a>Configure email templates</span></span>
<span data-ttu-id="1412a-130">Az API Management e-mail sablonok biztosít a felügyelete, és a szolgáltatás használata során küldött e-mailek.</span><span class="sxs-lookup"><span data-stu-id="1412a-130">API Management provides email templates for the email messages that are sent in the course of administering and using the service.</span></span> <span data-ttu-id="1412a-131">A következő e-mail sablonok találhatók.</span><span class="sxs-lookup"><span data-stu-id="1412a-131">The following email templates are provided.</span></span>

* <span data-ttu-id="1412a-132">Application gallery küldésének jóváhagyott</span><span class="sxs-lookup"><span data-stu-id="1412a-132">Application gallery submission approved</span></span>
* <span data-ttu-id="1412a-133">Fejlesztői farewell levél</span><span class="sxs-lookup"><span data-stu-id="1412a-133">Developer farewell letter</span></span>
* <span data-ttu-id="1412a-134">Értesítési megközelítő fejlesztői kvótakorlátot</span><span class="sxs-lookup"><span data-stu-id="1412a-134">Developer quota limit approaching notification</span></span>
* <span data-ttu-id="1412a-135">Felhasználó meghívása</span><span class="sxs-lookup"><span data-stu-id="1412a-135">Invite user</span></span>
* <span data-ttu-id="1412a-136">Új megjegyzés hozzáadása egy problémához</span><span class="sxs-lookup"><span data-stu-id="1412a-136">New comment added to an issue</span></span>
* <span data-ttu-id="1412a-137">Kapott új probléma</span><span class="sxs-lookup"><span data-stu-id="1412a-137">New issue received</span></span>
* <span data-ttu-id="1412a-138">Új előfizetés aktiválása</span><span class="sxs-lookup"><span data-stu-id="1412a-138">New subscription activated</span></span>
* <span data-ttu-id="1412a-139">Előfizetés megújítása megerősítése</span><span class="sxs-lookup"><span data-stu-id="1412a-139">Subscription renewed confirmation</span></span>
* <span data-ttu-id="1412a-140">Előfizetés visszautasítja</span><span class="sxs-lookup"><span data-stu-id="1412a-140">Subscription request declines</span></span>
* <span data-ttu-id="1412a-141">Előfizetés kérelem érkezett</span><span class="sxs-lookup"><span data-stu-id="1412a-141">Subscription request received</span></span>

<span data-ttu-id="1412a-142">Ezek a sablonok módosítható igény szerint.</span><span class="sxs-lookup"><span data-stu-id="1412a-142">These templates can be modified as desired.</span></span>

<span data-ttu-id="1412a-143">Tekintheti meg és konfigurálhatja az e-mail sablonok az API Management-példány, kattintson a **értesítések** a a **API Management** menüt, és válassza ki a **E-mail sablonok**fülre.</span><span class="sxs-lookup"><span data-stu-id="1412a-143">To view and configure the email templates for your API Management instance, click **Notifications** from the **API Management** menu on the left, and select the **Email Templates** tab.</span></span>

![E-mail-sablonok][api-management-email-templates]

<span data-ttu-id="1412a-145">Megtekintése vagy módosítása egy adott sablon, válassza ki azt a **sablonok** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="1412a-145">To view or modify a specific template, select it from the **Templates** drop-down list.</span></span>

![E-mail sablonok listája][api-management-email-templates-list]

<span data-ttu-id="1412a-147">Minden e-mail sablon egyszerű szöveges tárgy és törzs HTML formátumban definícióját rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1412a-147">Each email template has a subject in plain text, and a body definition in HTML format.</span></span> <span data-ttu-id="1412a-148">Minden elem testre szabható, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="1412a-148">Each item can be customized as desired.</span></span>

![E-mail sablon szerkesztő][api-management-email-template]

<span data-ttu-id="1412a-150">A **paraméterek** lista felsorolja azokat a paramétereket, amikor a tárgya és törzse beszúrt fogja cserélni a kijelölt érték, ha az e-mailt küld.</span><span class="sxs-lookup"><span data-stu-id="1412a-150">The **Parameters** list contains a list of parameters, which when inserted into the subject or body, will be replaced the designated value when the email is sent.</span></span> <span data-ttu-id="1412a-151">Paraméter beszúrása, vigye a kurzort, ahol szeretné nyissa meg a paramétert, majd kattintson a a paraméternév balra mutató nyílra.</span><span class="sxs-lookup"><span data-stu-id="1412a-151">To insert a parameter, place the cursor where you wish the parameter to go, and click the arrow to the left of the parameter name.</span></span>

<span data-ttu-id="1412a-152">Kattintson a **előzetes** vagy **egy teszt küldése** hogyan az e-mailt fog keresse meg vagy egy teszt e-mailek küldése.</span><span class="sxs-lookup"><span data-stu-id="1412a-152">Click **Preview** or **Send a test** to see how the email will look or send a test email.</span></span>

> [!NOTE] 
> <span data-ttu-id="1412a-153">A paraméterek nem cserélhető le tényleges értékek előnézet vagy egy teszt küldése.</span><span class="sxs-lookup"><span data-stu-id="1412a-153">The parameters are not replaced with actual values when previewing or sending a test.</span></span>

<span data-ttu-id="1412a-154">Az e-mail sablon a módosítások mentéséhez kattintson **mentése**, vagy visszavonhatja a módosításokat kattintson **Mégse**.</span><span class="sxs-lookup"><span data-stu-id="1412a-154">To save the changes to the email template, click **Save**, or to cancel the changes click **Cancel**.</span></span>
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
