---
title: "Az Azure Active Directory B2C: E-mail ellenőrzése során a felhasználói regisztrációs letiltása |} Microsoft Docs"
description: "A témakör bemutatásához, hogyan toodisable e-mail-ellenőrzés során az Azure Active Directory B2C-előfizetési fogyasztói"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 433f32b8-96d2-4113-aa82-efcf42fa9827
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/06/2017
ms.author: parakhj
ms.openlocfilehash: a8a42eddcb577725f04d70e1b1ebbebf10b5937c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a><span data-ttu-id="dee29-103">Az Azure Active Directory B2C: Letiltása e-mail ellenőrzése felhasználói regisztráció során</span><span class="sxs-lookup"><span data-stu-id="dee29-103">Azure Active Directory B2C: Disable email verification during consumer sign-up</span></span>
<span data-ttu-id="dee29-104">Ha engedélyezve van, Azure Active Directory (Azure AD) B2C által biztosított a fogyasztói hello képességét toosign feliratkozott alkalmazások megadása az e-mail címet és egy helyi fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="dee29-104">When enabled, Azure Active Directory (Azure AD) B2C gives a consumer hello ability toosign up for applications by providing an email address and creating a local account.</span></span> <span data-ttu-id="dee29-105">Az Azure AD B2C garantálja érvényes e-mail címet azzal, hogy a fogyasztók tooverify őket hello regisztrációs folyamat során.</span><span class="sxs-lookup"><span data-stu-id="dee29-105">Azure AD B2C ensures valid email addresses by requiring consumers tooverify them during hello sign-up process.</span></span> <span data-ttu-id="dee29-106">Is megakadályozza, hogy egy rosszindulatú automatikus folyamat hello alkalmazások hamis fiókok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="dee29-106">It also prevents a malicious automated process from generating fake accounts for hello applications.</span></span>

<span data-ttu-id="dee29-107">Néhány alkalmazásfejlesztők inkább tooskip e-mail ellenőrzése hello regisztrációs folyamat során, és helyette a fogyasztók hello e-mail cím ellenőrzése később rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="dee29-107">Some application developers prefer tooskip email verification during hello sign-up process and instead have consumers verify hello email address later.</span></span> <span data-ttu-id="dee29-108">Ez az Azure AD B2C segítségével toosupport konfigurált toodisable e-mail ellenőrzése sikerült.</span><span class="sxs-lookup"><span data-stu-id="dee29-108">toosupport this, Azure AD B2C can be configured toodisable email verification.</span></span> <span data-ttu-id="dee29-109">Ezzel létrehoz egy gördülékenyebb bejelentkezési folyamatot, és lehetőséget nyújt a fejlesztőknek hello rugalmasságot toodifferentiate hello fogyasztók, hogy ellenőrizte az e-mail címüket a fogyasztók, amelyek nem rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="dee29-109">Doing so creates a smoother sign-up process and gives developers hello flexibility toodifferentiate hello consumers that have verified their email address from those consumers that have not.</span></span>

<span data-ttu-id="dee29-110">Alapértelmezés szerint előfizetési házirendek rendelkeznek-e kapcsolva e-mail ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="dee29-110">By default, sign-up policies have email verification turned on.</span></span> <span data-ttu-id="dee29-111">Használjon hello következő lépések tooturn azt ki:</span><span class="sxs-lookup"><span data-stu-id="dee29-111">Use hello following steps tooturn it off:</span></span>

1. <span data-ttu-id="dee29-112">[Kövesse a lépéseket toonavigate toohello B2C funkciók panelje hello Azure-portálon](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="dee29-112">[Follow these steps toonavigate toohello B2C features blade on hello Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="dee29-113">Kattintson a **előfizetési házirendek** vagy **regisztráció vagy bejelentkezés házirendek** attól függően, hogy a konfigurált előfizetési.</span><span class="sxs-lookup"><span data-stu-id="dee29-113">Click **Sign-up policies** or **Sign-up or sign-in policies** depending on what you configured for sign-up.</span></span>
3. <span data-ttu-id="dee29-114">Kattintson a házirendet (például "B2C_1_SiUp") tooopen azt.</span><span class="sxs-lookup"><span data-stu-id="dee29-114">Click your policy (for example, "B2C_1_SiUp") tooopen it.</span></span> <span data-ttu-id="dee29-115">Kattintson a **szerkesztése** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="dee29-115">Click **Edit** at hello top of hello blade.</span></span>
4. <span data-ttu-id="dee29-116">Kattintson a **lap felhasználói felületének testreszabása**.</span><span class="sxs-lookup"><span data-stu-id="dee29-116">Click **Page UI Customization**.</span></span>
5. <span data-ttu-id="dee29-117">Kattintson a **helyi fiók előfizetéshez**.</span><span class="sxs-lookup"><span data-stu-id="dee29-117">Click **Local account sign-up page**.</span></span>
6. <span data-ttu-id="dee29-118">Kattintson a **E-mail cím** a hello **neve** alatti hello **regisztrációs attribútumokat** szakasz.</span><span class="sxs-lookup"><span data-stu-id="dee29-118">Click **Email Address** in hello **Name** column under hello **Sign-up attributes** section.</span></span>
7. <span data-ttu-id="dee29-119">Váltás hello **ellenőrzés megkövetelése** beállítás túl**nem**.</span><span class="sxs-lookup"><span data-stu-id="dee29-119">Toggle hello **Require verification** option too**No**.</span></span>
8. <span data-ttu-id="dee29-120">Kattintson a **OK** hello alsó hello addig **házirend szerkesztése** panelen.</span><span class="sxs-lookup"><span data-stu-id="dee29-120">Click **OK** at hello bottom until you reach hello **Edit policy** blade.</span></span>
9. <span data-ttu-id="dee29-121">Kattintson a **mentése** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="dee29-121">Click **Save** at hello top of hello blade.</span></span> <span data-ttu-id="dee29-122">Elkészült!</span><span class="sxs-lookup"><span data-stu-id="dee29-122">You're done!</span></span>

> [!NOTE]
> <span data-ttu-id="dee29-123">E-mail ellenőrzése a regisztrációs folyamat hello letiltása toospam vezethet.</span><span class="sxs-lookup"><span data-stu-id="dee29-123">Disabling email verification in hello sign-up process may lead toospam.</span></span> <span data-ttu-id="dee29-124">Ha letiltja a hello alapértelmezett, ajánlott hozzáadása a saját ellenőrzési rendszerétől.</span><span class="sxs-lookup"><span data-stu-id="dee29-124">If you disable hello default one, we recommend adding your own verification system.</span></span>
> 
> 

<span data-ttu-id="dee29-125">Azt a rendszer mindig megnyitott toofeedback és javaslatok!</span><span class="sxs-lookup"><span data-stu-id="dee29-125">We are always open toofeedback and suggestions!</span></span> <span data-ttu-id="dee29-126">Ha bármilyen nehézségbe ütközik az ebben a témakörben, vagy az ehhez a tartalomhoz javítására állnak, azt fogadjuk visszajelzéseit hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="dee29-126">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at hello bottom of hello page.</span></span> <span data-ttu-id="dee29-127">A szolgáltatás kéréseket, vegye fel őket túl[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="dee29-127">For feature requests, add them too[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>
