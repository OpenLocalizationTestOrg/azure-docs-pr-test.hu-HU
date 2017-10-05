---
title: "Az Azure Active Directory B2C: Az önkiszolgáló jelszó-átállítási |} Microsoft Docs"
description: "A témakör bemutatja, hogyan lehet az önkiszolgáló jelszó-visszaállítást a felhasználók az Azure Active Directory B2C beállítása"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 0508868e3b00c5771cc26038a3dd71fde6625a84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a><span data-ttu-id="e3d34-103">Az Azure Active Directory B2C: Az önkiszolgáló jelszó-visszaállítást a felhasználók beállítása</span><span class="sxs-lookup"><span data-stu-id="e3d34-103">Azure Active Directory B2C: Set up self-service password reset for your consumers</span></span>
<span data-ttu-id="e3d34-104">Az önkiszolgáló jelszó-visszaállítási szolgáltatással (akiknek regisztráltak-e helyi fiókok esetében) a felhasználók alaphelyzetbe állíthatja a saját maguk a jelszavukat.</span><span class="sxs-lookup"><span data-stu-id="e3d34-104">With the self-service password reset feature, your consumers (who have signed up for local accounts) can reset their passwords on their own.</span></span> <span data-ttu-id="e3d34-105">Ez jelentősen csökkenti a jelentős terhet róhat a támogató személyzete számára, különösen akkor, ha az alkalmazás rendelkezik több millió fogyasztók rendszeresen használja azt.</span><span class="sxs-lookup"><span data-stu-id="e3d34-105">This significantly reduces the burden on your support staff, especially if your application has millions of consumers using it on a regular basis.</span></span> <span data-ttu-id="e3d34-106">Jelenleg csak támogatjuk egy helyreállítási módszer az egy ellenőrzött és érvényes e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="e3d34-106">Currently, we only support using a verified email address as a recovery method.</span></span> <span data-ttu-id="e3d34-107">A jövőben további helyreállítási módszerek (ellenőrzött telefonszám, a biztonsági kérdések stb.) adunk hozzá.</span><span class="sxs-lookup"><span data-stu-id="e3d34-107">We will add additional recovery methods (verified phone number, security questions, etc.) in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="e3d34-108">Ez a cikk vonatkozik az önkiszolgáló jelszó alaphelyzetbe állítása a bejelentkezési házirend környezetben használják.</span><span class="sxs-lookup"><span data-stu-id="e3d34-108">This article applies to self-service password reset used in the context of a sign-in policy.</span></span> <span data-ttu-id="e3d34-109">Ha módosítania kell meghívni a az alkalmazás teljes mértékben testreszabható jelszó alaphelyzetbe állítása házirendek, lásd: [Ez a cikk](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).</span><span class="sxs-lookup"><span data-stu-id="e3d34-109">If you need fully customizable password reset policies invoked from your app, see [this article](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).</span></span>
> 
> 

<span data-ttu-id="e3d34-110">Alapértelmezés szerint a címtárban nincs önkiszolgáló jelszó-visszaállítási-e kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="e3d34-110">By default, your directory will not have self-service password reset turned on.</span></span> <span data-ttu-id="e3d34-111">Az alábbi lépések segítségével kapcsolja be:</span><span class="sxs-lookup"><span data-stu-id="e3d34-111">Use the following steps to turn it on:</span></span>

1. <span data-ttu-id="e3d34-112">Jelentkezzen be a [klasszikus Azure-portálra](https://manage.windowsazure.com/) előfizetés-rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e3d34-112">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) as the Subscription Administrator.</span></span> <span data-ttu-id="e3d34-113">Ez az ugyanaz a munkahelyi vagy iskolai fiók vagy a Microsoft-fiók, mint amellyel a címtárban.</span><span class="sxs-lookup"><span data-stu-id="e3d34-113">This is the same work or school account or the same Microsoft account that you used to create your directory.</span></span>
2. <span data-ttu-id="e3d34-114">Lépjen a bal oldalon található navigációs sávban az Active Directory bővítményre.</span><span class="sxs-lookup"><span data-stu-id="e3d34-114">Navigate to the Active Directory extension on the navigation bar on the left side.</span></span>
3. <span data-ttu-id="e3d34-115">A könyvtár alatt található a **Directory** fülre, és kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="e3d34-115">Find your directory under the **Directory** tab and click it.</span></span>
4. <span data-ttu-id="e3d34-116">Kattintson a **Configure** (Konfigurálás) lapra.</span><span class="sxs-lookup"><span data-stu-id="e3d34-116">Click the **Configure** tab.</span></span>
5. <span data-ttu-id="e3d34-117">Görgessen le a **felhasználói jelszó-visszaállítási házirend** szakasz és váltása a **jelszó-visszaállításhoz engedélyezett felhasználók** lehetőséggel **Igen**.</span><span class="sxs-lookup"><span data-stu-id="e3d34-117">Scroll down to the **User password reset policy** section and toggle the **Users enabled for password reset** option to **YES**.</span></span> <span data-ttu-id="e3d34-118">Figyelje meg, hogy a **másodlagos E-mail-cím** beállítást; hagyja, mert az.</span><span class="sxs-lookup"><span data-stu-id="e3d34-118">Notice that the **Alternate Email Address** option is checked; leave it as it is.</span></span>
   
    ![Új jelszó önkiszolgáló kérése](./media/active-directory-b2c-reference-sspr/sspr.png)
6. <span data-ttu-id="e3d34-120">Kattintson a lap alján található **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="e3d34-120">Click **Save** at the bottom of the page.</span></span> <span data-ttu-id="e3d34-121">Elkészült!</span><span class="sxs-lookup"><span data-stu-id="e3d34-121">You're done!</span></span>

<span data-ttu-id="e3d34-122">Teszteléséhez használja a "Futtatás most" a szolgáltatás bármely bejelentkezési házirend, amely rendelkezik a helyi fiókok identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="e3d34-122">To test, use the "Run now" feature on any sign-in policy that has local accounts as an identity provider.</span></span> <span data-ttu-id="e3d34-123">A helyi fiók bejelentkezési a lapon (ha ad meg egy e-mail címet és jelszót, vagy felhasználónév és jelszó), kattintson **nem fér hozzá a fiókjához?** ellenőrizheti a felhasználói élmény.</span><span class="sxs-lookup"><span data-stu-id="e3d34-123">On the local account sign-in page (where you enter an email address and password, or a username and password), click **Can't access your account?** to verify the consumer experience.</span></span>

> [!NOTE]
> <span data-ttu-id="e3d34-124">Az önkiszolgáló jelszó-visszaállítási lapok használatával testre szabható a [vállalati arculat megjelenítése a szolgáltatás](../active-directory/active-directory-add-company-branding.md).</span><span class="sxs-lookup"><span data-stu-id="e3d34-124">The self-service password reset pages can be customized by using the [company branding feature](../active-directory/active-directory-add-company-branding.md).</span></span>
> 
> 

