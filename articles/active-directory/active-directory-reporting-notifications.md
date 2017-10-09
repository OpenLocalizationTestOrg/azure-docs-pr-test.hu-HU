---
title: "Active Directory jelentéskészítési értesítései aaaAzure"
description: "Hogyan toouse hello gyanús bejelentkezési az Azure Active Directory jelentéskészítési értesítéseket moduljainak."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ae6d4b0e-5931-4cb3-98bf-9be91b381c92
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.openlocfilehash: 3843c45eaf9d68e671943bfdbc7ab68933f38fbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-notifications"></a><span data-ttu-id="7f9df-103">Az Azure Active Directory jelentéskészítési értesítései</span><span class="sxs-lookup"><span data-stu-id="7f9df-103">Azure Active Directory Reporting Notifications</span></span>
## <a name="what-reports-generate-email-notifications"></a><span data-ttu-id="7f9df-104">Milyen jelentések készítése az értesítő e-mailek</span><span class="sxs-lookup"><span data-stu-id="7f9df-104">What reports generate email notifications</span></span>
<span data-ttu-id="7f9df-105">Jelenleg csak hello szabálytalan bejelentkezési tevékenység jelentés eseményindítók az e-mail értesítések.</span><span class="sxs-lookup"><span data-stu-id="7f9df-105">At this time, only hello Irregular Sign in Activity report triggers email notifications.</span></span>

## <a name="what-is-an-irregular-sign-in"></a><span data-ttu-id="7f9df-106">Mi az az "szabálytalan bejelentkezési"?</span><span class="sxs-lookup"><span data-stu-id="7f9df-106">What is an "Irregular Sign in"?</span></span>
<span data-ttu-id="7f9df-107">Szabálytalan bejelentkezések megegyeznek a gépi tanulási algoritmusok, hello alapon "lehetetlen odautazás" feltétel egy rendellenes bejelentkezési helye és az eszköz által azonosított.</span><span class="sxs-lookup"><span data-stu-id="7f9df-107">Irregular sign-ins are those that have been identified by our machine learning algorithms, on hello basis of an "impossible travel" condition combined with an anomalous sign-in location and device.</span></span> <span data-ttu-id="7f9df-108">Ez azt jelezheti, hogy egy támadó próbálkozott toosign az ezzel a fiókkal.</span><span class="sxs-lookup"><span data-stu-id="7f9df-108">This may indicate that a hacker has been trying toosign in using this account.</span></span>

## <a name="who-receives-hello-email-notifications"></a><span data-ttu-id="7f9df-109">Hello e-mail értesítéseket fogad számára?</span><span class="sxs-lookup"><span data-stu-id="7f9df-109">Who receives hello email notifications?</span></span>
<span data-ttu-id="7f9df-110">hello e-mailt küld tooall globális rendszergazdák számára egy Active Directory Premium licenc van rendelve.</span><span class="sxs-lookup"><span data-stu-id="7f9df-110">hello email is sent tooall global admins who have been assigned an Active Directory Premium license.</span></span> <span data-ttu-id="7f9df-111">tooensure biztosítását, hogy elküldi a toohello rendszergazdák másodlagos E-mail-cím is.</span><span class="sxs-lookup"><span data-stu-id="7f9df-111">tooensure it is delivered, we send it toohello admins Alternate Email Address as well.</span></span> <span data-ttu-id="7f9df-112">Tartalmaznia kell a rendszergazdák aad-alerts-noreply@mail.windowsazure.com a saját megbízható feladók listájához, így azok nem teljesíti az üdvözlő e-mail.</span><span class="sxs-lookup"><span data-stu-id="7f9df-112">Admins should include aad-alerts-noreply@mail.windowsazure.com in their safe senders list so they don’t miss hello email.</span></span>

## <a name="how-often-are-these-emails-sent"></a><span data-ttu-id="7f9df-113">Milyen gyakran történik a küldi az e-maileket?</span><span class="sxs-lookup"><span data-stu-id="7f9df-113">How often are these emails sent?</span></span>
<span data-ttu-id="7f9df-114">hello e-mailt küld, ha a 10 új szabálytalan bejelentkezési tevékenység történik hello a 30 napnál, vagy mert hello utolsó e-mailt küldték, attól függően kisebb.</span><span class="sxs-lookup"><span data-stu-id="7f9df-114">hello email is sent if 10 new irregular sign-in activities occur in hello last 30 days, or since hello last email was sent, whichever is less.</span></span>

## <a name="how-do-i-access-hello-report-mentioned-in-hello-email"></a><span data-ttu-id="7f9df-115">Hogyan érhetem el az üdvözlő e-mail említett hello jelentés?</span><span class="sxs-lookup"><span data-stu-id="7f9df-115">How do I access hello report mentioned in hello email?</span></span>
<span data-ttu-id="7f9df-116">Amikor hello hivatkozásra kattint, lehetősége lesz átirányított toohello jelentés oldalról hello a klasszikus Azure portálon belül.</span><span class="sxs-lookup"><span data-stu-id="7f9df-116">When you click on hello link, you will be redirected toohello report page within hello Azure classic portal.</span></span> <span data-ttu-id="7f9df-117">Sorrendben tooaccess hello jelentés, mindkét toobe van szüksége:</span><span class="sxs-lookup"><span data-stu-id="7f9df-117">In order tooaccess hello report, you need toobe both:</span></span>

* <span data-ttu-id="7f9df-118">Rendszergazdaként vagy az Azure-előfizetéshez társadminisztrátornak</span><span class="sxs-lookup"><span data-stu-id="7f9df-118">An admin or co-admin of your Azure subscription</span></span>
* <span data-ttu-id="7f9df-119">Hello címtár globális rendszergazdája és az Active Directory Premium licenc hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="7f9df-119">A global administrator in hello directory, and assigned an Active Directory Premium license.</span></span> <span data-ttu-id="7f9df-120">További információk: [Azure Active Directory editions](active-directory-editions.md) (Azure Active Directory-kiadások).</span><span class="sxs-lookup"><span data-stu-id="7f9df-120">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

## <a name="can-i-turn-off-these-emails"></a><span data-ttu-id="7f9df-121">Lehet kikapcsolni az e-maileket?</span><span class="sxs-lookup"><span data-stu-id="7f9df-121">Can I turn off these emails?</span></span>
<span data-ttu-id="7f9df-122">Igen, rendszerértesítőket tooturn kapcsolódó tooanomalous bejelentkezések hello a klasszikus Azure portálon belül, kattintson a **konfigurálása**, majd válassza ki **letiltott** alatt hello **értesítések**szakasz.</span><span class="sxs-lookup"><span data-stu-id="7f9df-122">Yes, tooturn off notifications related tooanomalous sign-ins within hello Azure classic portal, click **Configure**, and then select **Disabled** under hello **Notifications** section.</span></span>

## <a name="whats-next"></a><span data-ttu-id="7f9df-123">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="7f9df-123">What's next</span></span>
* <span data-ttu-id="7f9df-124">Fejezetét milyen biztonsági, naplózási és tevékenységjelentéseket állnak rendelkezésre?</span><span class="sxs-lookup"><span data-stu-id="7f9df-124">Curious about what security, audit, and activity reports are available?</span></span> <span data-ttu-id="7f9df-125">Tekintse meg [az Azure AD biztonsági, naplózási és tevékenységjelentéseket](active-directory-view-access-usage-reports.md)</span><span class="sxs-lookup"><span data-stu-id="7f9df-125">Check out [Azure AD Security, Audit, and Activity Reports](active-directory-view-access-usage-reports.md)</span></span>
* [<span data-ttu-id="7f9df-126">Bevezetés a Prémium szintű Azure Active Directory használatába</span><span class="sxs-lookup"><span data-stu-id="7f9df-126">Getting started with Azure Active Directory Premium</span></span>](active-directory-get-started-premium.md)
* [<span data-ttu-id="7f9df-127">Vállalati arculat megjelenítése a tooyour bejelentkezés és a hozzáférési Panel oldalon</span><span class="sxs-lookup"><span data-stu-id="7f9df-127">Add company branding tooyour Sign In and Access Panel pages</span></span>](active-directory-add-company-branding.md)

