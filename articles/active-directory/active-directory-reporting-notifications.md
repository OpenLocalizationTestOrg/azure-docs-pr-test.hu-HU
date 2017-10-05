---
title: "Az Azure Active Directory jelentéskészítési értesítései"
description: "Hogyan használható az Azure Active Directory reporting gyanús bejelentkezési értesítéseket moduljainak."
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
ms.openlocfilehash: f4632bd2af802b10c8c64972e8c605d7ad7c0eaf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-reporting-notifications"></a><span data-ttu-id="9ea8d-103">Az Azure Active Directory jelentéskészítési értesítései</span><span class="sxs-lookup"><span data-stu-id="9ea8d-103">Azure Active Directory Reporting Notifications</span></span>
## <a name="what-reports-generate-email-notifications"></a><span data-ttu-id="9ea8d-104">Milyen jelentések készítése az értesítő e-mailek</span><span class="sxs-lookup"><span data-stu-id="9ea8d-104">What reports generate email notifications</span></span>
<span data-ttu-id="9ea8d-105">Jelenleg csak a szabálytalan bejelentkezési tevékenység jelentés eseményindítók e-mail értesítések.</span><span class="sxs-lookup"><span data-stu-id="9ea8d-105">At this time, only the Irregular Sign in Activity report triggers email notifications.</span></span>

## <a name="what-is-an-irregular-sign-in"></a><span data-ttu-id="9ea8d-106">Mi az az "szabálytalan bejelentkezési"?</span><span class="sxs-lookup"><span data-stu-id="9ea8d-106">What is an "Irregular Sign in"?</span></span>
<span data-ttu-id="9ea8d-107">Szabálytalan bejelentkezések megegyeznek a gépi tanulási algoritmusok, alapján nem "lehetetlen odautazás" állapotot egy rendellenes bejelentkezési helye és az eszköz által azonosított.</span><span class="sxs-lookup"><span data-stu-id="9ea8d-107">Irregular sign-ins are those that have been identified by our machine learning algorithms, on the basis of an "impossible travel" condition combined with an anomalous sign-in location and device.</span></span> <span data-ttu-id="9ea8d-108">Ez azt jelezheti, hogy egy támadó megpróbálja ezzel a fiókkal bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="9ea8d-108">This may indicate that a hacker has been trying to sign in using this account.</span></span>

## <a name="who-receives-the-email-notifications"></a><span data-ttu-id="9ea8d-109">Ki kapja meg az e-mail értesítések?</span><span class="sxs-lookup"><span data-stu-id="9ea8d-109">Who receives the email notifications?</span></span>
<span data-ttu-id="9ea8d-110">Az e-mailt küld összes globális rendszergazda, aki van rendelve egy Active Directory Premium licenc.</span><span class="sxs-lookup"><span data-stu-id="9ea8d-110">The email is sent to all global admins who have been assigned an Active Directory Premium license.</span></span> <span data-ttu-id="9ea8d-111">Annak érdekében, hogy biztosítását, kapni, a rendszergazdáknak másodlagos E-mail-cím is.</span><span class="sxs-lookup"><span data-stu-id="9ea8d-111">To ensure it is delivered, we send it to the admins Alternate Email Address as well.</span></span> <span data-ttu-id="9ea8d-112">Tartalmaznia kell a rendszergazdák aad-alerts-noreply@mail.windowsazure.com a saját megbízható feladók listájához, így azok nem hagy ki az e-mailt.</span><span class="sxs-lookup"><span data-stu-id="9ea8d-112">Admins should include aad-alerts-noreply@mail.windowsazure.com in their safe senders list so they don’t miss the email.</span></span>

## <a name="how-often-are-these-emails-sent"></a><span data-ttu-id="9ea8d-113">Milyen gyakran történik a küldi az e-maileket?</span><span class="sxs-lookup"><span data-stu-id="9ea8d-113">How often are these emails sent?</span></span>
<span data-ttu-id="9ea8d-114">Az e-mailt küld, ha a 10 új szabálytalan bejelentkezési tevékenység történik az elmúlt 30 napban, vagy mert a legutóbbi e-mailben küldték, attól függően kisebb.</span><span class="sxs-lookup"><span data-stu-id="9ea8d-114">The email is sent if 10 new irregular sign-in activities occur in the last 30 days, or since the last email was sent, whichever is less.</span></span>

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a><span data-ttu-id="9ea8d-115">Hogyan érhető el a jelentésben szerepel az e-mailt?</span><span class="sxs-lookup"><span data-stu-id="9ea8d-115">How do I access the report mentioned in the email?</span></span>
<span data-ttu-id="9ea8d-116">Ha a hivatkozásra kattint, a klasszikus Azure portálon belül a jelentés oldalra irányítja.</span><span class="sxs-lookup"><span data-stu-id="9ea8d-116">When you click on the link, you will be redirected to the report page within the Azure classic portal.</span></span> <span data-ttu-id="9ea8d-117">A jelentés eléréséhez kell lennie mindkét:</span><span class="sxs-lookup"><span data-stu-id="9ea8d-117">In order to access the report, you need to be both:</span></span>

* <span data-ttu-id="9ea8d-118">Rendszergazdaként vagy az Azure-előfizetéshez társadminisztrátornak</span><span class="sxs-lookup"><span data-stu-id="9ea8d-118">An admin or co-admin of your Azure subscription</span></span>
* <span data-ttu-id="9ea8d-119">A címtár globális rendszergazdája és az Active Directory Premium licenc hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="9ea8d-119">A global administrator in the directory, and assigned an Active Directory Premium license.</span></span> <span data-ttu-id="9ea8d-120">További információk: [Azure Active Directory editions](active-directory-editions.md) (Azure Active Directory-kiadások).</span><span class="sxs-lookup"><span data-stu-id="9ea8d-120">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

## <a name="can-i-turn-off-these-emails"></a><span data-ttu-id="9ea8d-121">Lehet kikapcsolni az e-maileket?</span><span class="sxs-lookup"><span data-stu-id="9ea8d-121">Can I turn off these emails?</span></span>
<span data-ttu-id="9ea8d-122">Igen, a rendellenes bejelentkezések a klasszikus Azure portálon belül kapcsolatos értesítések kikapcsolásához kattintson **konfigurálása**, majd válassza ki **letiltott** alatt a **értesítések** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9ea8d-122">Yes, to turn off notifications related to anomalous sign-ins within the Azure classic portal, click **Configure**, and then select **Disabled** under the **Notifications** section.</span></span>

## <a name="whats-next"></a><span data-ttu-id="9ea8d-123">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ea8d-123">What's next</span></span>
* <span data-ttu-id="9ea8d-124">Fejezetét milyen biztonsági, naplózási és tevékenységjelentéseket állnak rendelkezésre?</span><span class="sxs-lookup"><span data-stu-id="9ea8d-124">Curious about what security, audit, and activity reports are available?</span></span> <span data-ttu-id="9ea8d-125">Tekintse meg [az Azure AD biztonsági, naplózási és tevékenységjelentéseket](active-directory-view-access-usage-reports.md)</span><span class="sxs-lookup"><span data-stu-id="9ea8d-125">Check out [Azure AD Security, Audit, and Activity Reports](active-directory-view-access-usage-reports.md)</span></span>
* [<span data-ttu-id="9ea8d-126">Bevezetés a Prémium szintű Azure Active Directory használatába</span><span class="sxs-lookup"><span data-stu-id="9ea8d-126">Getting started with Azure Active Directory Premium</span></span>](active-directory-get-started-premium.md)
* [<span data-ttu-id="9ea8d-127">Vállalati arculat megjelenítése a bejelentkezési és a hozzáférési panel oldalon</span><span class="sxs-lookup"><span data-stu-id="9ea8d-127">Add company branding to your Sign In and Access Panel pages</span></span>](active-directory-add-company-branding.md)

