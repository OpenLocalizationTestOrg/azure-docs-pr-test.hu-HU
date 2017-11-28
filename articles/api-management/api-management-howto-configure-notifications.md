---
title: "aaaConfigure értesítések és az e-mail sablonok az Azure API Management |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconfigure értesítések és az e-mail sablonok az Azure API Management."
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
ms.openlocfilehash: dc23289c25a1641992b73cb955099b3f207b6968
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-notifications-and-email-templates-in-azure-api-management"></a><span data-ttu-id="ae7fe-103">Hogyan tooconfigure értesítések és az e-mail sablonok az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="ae7fe-103">How tooconfigure notifications and email templates in Azure API Management</span></span>
<span data-ttu-id="ae7fe-104">API-kezelési biztosítja hello tooconfigure értesítéseket meghatározott események és tooconfigure hello e-mail sablonok, amelyek használt toocommunicate hello rendszergazdák és fejlesztők számára az API Management-példány.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-104">API Management provides hello ability tooconfigure notifications for specific events, and tooconfigure hello email templates that are used toocommunicate with hello administrators and developers of an API Management instance.</span></span> <span data-ttu-id="ae7fe-105">Ez a témakör bemutatja, hogyan tooconfigure értesítések hello használható eseményt, és ezeket az eseményeket használt hello e-mail sablonok konfigurálásának áttekintése.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-105">This topic shows how tooconfigure notifications for hello available events, and provides an overview of configuring hello email templates used for these events.</span></span>

## <span data-ttu-id="ae7fe-106"><a name="publisher-notifications"></a>Publisher értesítések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ae7fe-106"><a name="publisher-notifications"> </a>Configure publisher notifications</span></span>
<span data-ttu-id="ae7fe-107">tooconfigure értesítéseket, kattintson a **Publisher portal** a hello Azure portál, az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-107">tooconfigure notifications, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="ae7fe-108">Ezzel megnyitná toohello API Management publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-108">This takes you toohello API Management publisher portal.</span></span>

![Közzétevő portál][api-management-management-console]

> [!NOTE] 
> <span data-ttu-id="ae7fe-110">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

<span data-ttu-id="ae7fe-111">Kattintson a **értesítések** a hello **API Management** hello menüjének balra tooview hello elérhető értesítések.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-111">Click **Notifications** from hello **API Management** menu on hello left tooview hello available notifications.</span></span>

![A Publisher értesítések][api-management-publisher-notifications]

<span data-ttu-id="ae7fe-113">hello alábbi listán szereplő események konfigurálható az értesítésekhez.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-113">hello following list of events can be configured for notifications.</span></span>

* <span data-ttu-id="ae7fe-114">**Előfizetési kérelmek (jóváhagyásra van szükség)** – hello megadott e-mailek címzettjeinek és a felhasználók kapnak értesítő e-mailek kapcsolatos előfizetés kéréseinek API jóváhagyásra van szükség.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-114">**Subscription requests (requiring approval)** - hello specified email recipients and users will receive email notifications about subscription requests for API products requiring approval.</span></span>
* <span data-ttu-id="ae7fe-115">**Új előfizetések** – hello megadott e-mailek címzettjeinek és a felhasználók új API termék előfizetések kapcsolatos e-mail értesítéseket kap.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-115">**New subscriptions** - hello specified email recipients and users will receive email notifications about new API product subscriptions.</span></span>
* <span data-ttu-id="ae7fe-116">**Gyűjteményelem alkalmazáskérelmeinek** – hello megadott e-mailek címzettjeinek és a felhasználók e-mail értesítéseket kap, amikor új alkalmazások elküldött toohello alkalmazáskatalógusában.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-116">**Application gallery requests** - hello specified email recipients and users will receive email notifications when new applications are submitted toohello application gallery.</span></span>
* <span data-ttu-id="ae7fe-117">**Titkos másolat** – hello megadott e-mailek címzettjeinek és a felhasználók kapnak e-mailek Titkos másolatot minden küldött e-mailek toodevelopers.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-117">**BCC** - hello specified email recipients and users will receive email blind carbon copies of all emails sent toodevelopers.</span></span>
* <span data-ttu-id="ae7fe-118">**Új probléma vagy megjegyzés** – hello megadott e-mailek címzettjeinek és a felhasználók kapnak értesítő e-mailek, amikor új probléma vagy megjegyzés küldése hello fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-118">**New issue or comment** - hello specified email recipients and users will receive email notifications when a new issue or comment is submitted on hello developer portal.</span></span>
* <span data-ttu-id="ae7fe-119">**Zárja be a fiók üzenet** – hello megadott e-mailek címzettjeinek és a felhasználók e-mail értesítéseket kap, ha a fiók le van zárva.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-119">**Close account message** - hello specified email recipients and users will receive email notifications when an account is closed.</span></span>
* <span data-ttu-id="ae7fe-120">**Megközelíti előfizetés kvótakorlátot** - e-mailek címzettjeinek következő hello és a felhasználók e-mail értesítéseket kap, ha az előfizetés használatának lekérdezi Bezárás toousage kvóta.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-120">**Approaching subscription quota limit** - hello following email recipients and users will receive email notifications when subscription usage gets close toousage quota.</span></span>

<span data-ttu-id="ae7fe-121">Az egyes eseményekhez megadhatja az e-mailek címzettjeinek hello e-mail cím beviteli mezőbe, vagy választhat a felhasználók listájából.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-121">For each event, you can specify email recipients using hello email address text box or you can select users from a list.</span></span>

<span data-ttu-id="ae7fe-122">toospecify hello e-mail címek toobe értesítést kap, mezőben adja meg őket hello e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-122">toospecify hello email addresses toobe notified, enter them in hello email address text box.</span></span> <span data-ttu-id="ae7fe-123">Ha az e-mail címeket, külön azokat vesszővel válassza el őket.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-123">If you have multiple email addresses, separate them using commas.</span></span>

![Értesítés címzettjeinek][api-management-email-addresses]

<span data-ttu-id="ae7fe-125">toospecify hello felhasználók toobe értesítést kap, kattintson a **címzettet adjon hozzá**, hello hello felhasználók toobe értesítés melletti jelölőnégyzetet, majd kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-125">toospecify hello users toobe notified, click **add recipient**, check hello box beside hello users toobe notified, and click **OK**.</span></span>

> [!NOTE] 
> <span data-ttu-id="ae7fe-126">Csak a rendszergazdák hello listában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-126">Only administrators are displayed in hello list.</span></span>


<span data-ttu-id="ae7fe-127">Miután hello értesítés címzettjeinek, kattintson a **mentése** tooapply hello értesítés címzettjeinek frissítése.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-127">After configuring hello notification recipients, click **Save** tooapply hello updated notification recipients.</span></span>

> [!NOTE] 
> <span data-ttu-id="ae7fe-128">Ha elnavigál hello **Publisher értesítések** lapon hello publisher portal riasztást küld a hiba nem mentett módosítások vannak.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-128">If you navigate away from hello **Publisher Notifications** tab hello publisher portal alerts you if there are unsaved changes.</span></span>


## <span data-ttu-id="ae7fe-129"><a name="email-templates"></a>E-mail sablonok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ae7fe-129"><a name="email-templates"> </a>Configure email templates</span></span>
<span data-ttu-id="ae7fe-130">Az API Management biztosít e-mail sablonok hello hello működés során felügyelete és hello szolgáltatás használatával küldött e-mail üzenetek.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-130">API Management provides email templates for hello email messages that are sent in hello course of administering and using hello service.</span></span> <span data-ttu-id="ae7fe-131">a következő e-mail sablonok hello vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-131">hello following email templates are provided.</span></span>

* <span data-ttu-id="ae7fe-132">Application gallery küldésének jóváhagyott</span><span class="sxs-lookup"><span data-stu-id="ae7fe-132">Application gallery submission approved</span></span>
* <span data-ttu-id="ae7fe-133">Fejlesztői farewell levél</span><span class="sxs-lookup"><span data-stu-id="ae7fe-133">Developer farewell letter</span></span>
* <span data-ttu-id="ae7fe-134">Értesítési megközelítő fejlesztői kvótakorlátot</span><span class="sxs-lookup"><span data-stu-id="ae7fe-134">Developer quota limit approaching notification</span></span>
* <span data-ttu-id="ae7fe-135">Felhasználó meghívása</span><span class="sxs-lookup"><span data-stu-id="ae7fe-135">Invite user</span></span>
* <span data-ttu-id="ae7fe-136">Új megjegyzés hozzá tooan probléma</span><span class="sxs-lookup"><span data-stu-id="ae7fe-136">New comment added tooan issue</span></span>
* <span data-ttu-id="ae7fe-137">Kapott új probléma</span><span class="sxs-lookup"><span data-stu-id="ae7fe-137">New issue received</span></span>
* <span data-ttu-id="ae7fe-138">Új előfizetés aktiválása</span><span class="sxs-lookup"><span data-stu-id="ae7fe-138">New subscription activated</span></span>
* <span data-ttu-id="ae7fe-139">Előfizetés megújítása megerősítése</span><span class="sxs-lookup"><span data-stu-id="ae7fe-139">Subscription renewed confirmation</span></span>
* <span data-ttu-id="ae7fe-140">Előfizetés visszautasítja</span><span class="sxs-lookup"><span data-stu-id="ae7fe-140">Subscription request declines</span></span>
* <span data-ttu-id="ae7fe-141">Előfizetés kérelem érkezett</span><span class="sxs-lookup"><span data-stu-id="ae7fe-141">Subscription request received</span></span>

<span data-ttu-id="ae7fe-142">Ezek a sablonok módosítható igény szerint.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-142">These templates can be modified as desired.</span></span>

<span data-ttu-id="ae7fe-143">tooview és hello e-mail sablonok az API Management-példány beállításához kattintson a gombra **értesítések** a hello **API Management** hello balra, és jelölje be hello menüjének **E-mail sablonok**  fülre.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-143">tooview and configure hello email templates for your API Management instance, click **Notifications** from hello **API Management** menu on hello left, and select hello **Email Templates** tab.</span></span>

![E-mail-sablonok][api-management-email-templates]

<span data-ttu-id="ae7fe-145">tooview vagy egy adott sablon módosításához válassza ki azt a hello **sablonok** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-145">tooview or modify a specific template, select it from hello **Templates** drop-down list.</span></span>

![E-mail sablonok listája][api-management-email-templates-list]

<span data-ttu-id="ae7fe-147">Minden e-mail sablon egyszerű szöveges tárgy és törzs HTML formátumban definícióját rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-147">Each email template has a subject in plain text, and a body definition in HTML format.</span></span> <span data-ttu-id="ae7fe-148">Minden elem testre szabható, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-148">Each item can be customized as desired.</span></span>

![E-mail sablon szerkesztő][api-management-email-template]

<span data-ttu-id="ae7fe-150">Hello **paraméterek** lista felsorolja azokat a paraméterek, amikor hello tárgya és törzse beszúrt, kijelölt kicserélt hello érték lesz üdvözlő e-mail küldésekor.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-150">hello **Parameters** list contains a list of parameters, which when inserted into hello subject or body, will be replaced hello designated value when hello email is sent.</span></span> <span data-ttu-id="ae7fe-151">a paramétert, tooinsert kurzorral hello ahol hello paraméter toogo kívánja, majd kattintson a hello toohello balra nyíl hello paraméternév.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-151">tooinsert a parameter, place hello cursor where you wish hello parameter toogo, and click hello arrow toohello left of hello parameter name.</span></span>

<span data-ttu-id="ae7fe-152">Kattintson a **előzetes** vagy **egy teszt küldése** toosee hogyan hello e-mail fog meg, vagy egy teszt e-mailek küldése.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-152">Click **Preview** or **Send a test** toosee how hello email will look or send a test email.</span></span>

> [!NOTE] 
> <span data-ttu-id="ae7fe-153">hello paraméterek nem cserélhető le tényleges értékek előnézet vagy egy teszt küldése.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-153">hello parameters are not replaced with actual values when previewing or sending a test.</span></span>

<span data-ttu-id="ae7fe-154">toosave hello módosítások toohello e-mail sablont, kattintson a **mentése**, vagy toocancel hello kattintson **Mégse**.</span><span class="sxs-lookup"><span data-stu-id="ae7fe-154">toosave hello changes toohello email template, click **Save**, or toocancel hello changes click **Cancel**.</span></span>
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
