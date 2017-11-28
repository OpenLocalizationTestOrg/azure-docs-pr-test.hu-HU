---
title: "Az Azure Active Directory B2C: Az önkiszolgáló jelszó-átállítási |} Microsoft Docs"
description: "A témakör bemutatásához, hogyan tooset fel az önkiszolgáló jelszó alaphelyzetbe állítása a felhasználók az Azure Active Directory B2C"
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
ms.openlocfilehash: ff87eea42259b610702da73af35d0a38eb7dd33d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a><span data-ttu-id="10314-103">Az Azure Active Directory B2C: Az önkiszolgáló jelszó-visszaállítást a felhasználók beállítása</span><span class="sxs-lookup"><span data-stu-id="10314-103">Azure Active Directory B2C: Set up self-service password reset for your consumers</span></span>
<span data-ttu-id="10314-104">Hello az önkiszolgáló jelszó-átállítási funkció, a felhasználók (akiknek regisztráltak-e helyi fiókok esetében) alaphelyzetbe állíthatja a saját maguk a jelszavukat.</span><span class="sxs-lookup"><span data-stu-id="10314-104">With hello self-service password reset feature, your consumers (who have signed up for local accounts) can reset their passwords on their own.</span></span> <span data-ttu-id="10314-105">Ez jelentősen csökkenti a hello nehezedő a támogató személyzete számára, különösen akkor, ha az alkalmazás rendelkezik több millió fogyasztók rendszeresen használja azt.</span><span class="sxs-lookup"><span data-stu-id="10314-105">This significantly reduces hello burden on your support staff, especially if your application has millions of consumers using it on a regular basis.</span></span> <span data-ttu-id="10314-106">Jelenleg csak támogatjuk egy helyreállítási módszer az egy ellenőrzött és érvényes e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="10314-106">Currently, we only support using a verified email address as a recovery method.</span></span> <span data-ttu-id="10314-107">Hello jövőben további helyreállítási módszerek (ellenőrzött telefonszám, a biztonsági kérdések stb.) adunk hozzá.</span><span class="sxs-lookup"><span data-stu-id="10314-107">We will add additional recovery methods (verified phone number, security questions, etc.) in hello future.</span></span>

> [!NOTE]
> <span data-ttu-id="10314-108">Ez a cikk vonatkozik tooself-jelszavának alaphelyzetbe állítása a bejelentkezési házirend hello környezetben használják.</span><span class="sxs-lookup"><span data-stu-id="10314-108">This article applies tooself-service password reset used in hello context of a sign-in policy.</span></span> <span data-ttu-id="10314-109">Ha módosítania kell meghívni a az alkalmazás teljes mértékben testreszabható jelszó alaphelyzetbe állítása házirendek, lásd: [Ez a cikk](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).</span><span class="sxs-lookup"><span data-stu-id="10314-109">If you need fully customizable password reset policies invoked from your app, see [this article](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).</span></span>
> 
> 

<span data-ttu-id="10314-110">Alapértelmezés szerint a címtárban nincs önkiszolgáló jelszó-visszaállítási-e kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="10314-110">By default, your directory will not have self-service password reset turned on.</span></span> <span data-ttu-id="10314-111">Használjon hello következő lépések tooturn azt a:</span><span class="sxs-lookup"><span data-stu-id="10314-111">Use hello following steps tooturn it on:</span></span>

1. <span data-ttu-id="10314-112">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) hello előfizetési rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="10314-112">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) as hello Subscription Administrator.</span></span> <span data-ttu-id="10314-113">Ez az azonos munkahelyi vagy iskolai fiókkal, vagy ugyanazon Microsoft-fiókkal, amellyel toocreate a címtár hello hello.</span><span class="sxs-lookup"><span data-stu-id="10314-113">This is hello same work or school account or hello same Microsoft account that you used toocreate your directory.</span></span>
2. <span data-ttu-id="10314-114">Keresse meg a hello bal oldali navigációs sávban hello toohello Active Directory-bővítményt.</span><span class="sxs-lookup"><span data-stu-id="10314-114">Navigate toohello Active Directory extension on hello navigation bar on hello left side.</span></span>
3. <span data-ttu-id="10314-115">A címtár hello alatt található **Directory** fülre, és kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="10314-115">Find your directory under hello **Directory** tab and click it.</span></span>
4. <span data-ttu-id="10314-116">Kattintson a hello **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="10314-116">Click hello **Configure** tab.</span></span>
5. <span data-ttu-id="10314-117">Görgessen lefelé toohello **felhasználói jelszó-visszaállítási házirend** szakasz és váltása hello **jelszó-visszaállításhoz engedélyezett felhasználók** beállítás túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="10314-117">Scroll down toohello **User password reset policy** section and toggle hello **Users enabled for password reset** option too**YES**.</span></span> <span data-ttu-id="10314-118">Figyelje meg, hogy hello **másodlagos E-mail-cím** beállítást; hagyja, mert az.</span><span class="sxs-lookup"><span data-stu-id="10314-118">Notice that hello **Alternate Email Address** option is checked; leave it as it is.</span></span>
   
    ![Új jelszó önkiszolgáló kérése](./media/active-directory-b2c-reference-sspr/sspr.png)
6. <span data-ttu-id="10314-120">Kattintson a **mentése** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="10314-120">Click **Save** at hello bottom of hello page.</span></span> <span data-ttu-id="10314-121">Elkészült!</span><span class="sxs-lookup"><span data-stu-id="10314-121">You're done!</span></span>

<span data-ttu-id="10314-122">tootest, hello "Futtatás most" szolgáltatás bármely bejelentkezési házirend, amely rendelkezik a helyi fiókok identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="10314-122">tootest, use hello "Run now" feature on any sign-in policy that has local accounts as an identity provider.</span></span> <span data-ttu-id="10314-123">Hello helyi fiók bejelentkezhet a lapon (ha ad meg egy e-mail címet és jelszót, vagy felhasználónév és jelszó), kattintson **nem fér hozzá a fiókjához?** tooverify hello végfelhasználói élmény.</span><span class="sxs-lookup"><span data-stu-id="10314-123">On hello local account sign-in page (where you enter an email address and password, or a username and password), click **Can't access your account?** tooverify hello consumer experience.</span></span>

> [!NOTE]
> <span data-ttu-id="10314-124">hello önkiszolgáló jelszó-visszaállítási oldal testreszabható hello segítségével [vállalati arculat megjelenítése a szolgáltatás](../active-directory/active-directory-add-company-branding.md).</span><span class="sxs-lookup"><span data-stu-id="10314-124">hello self-service password reset pages can be customized by using hello [company branding feature](../active-directory/active-directory-add-company-branding.md).</span></span>
> 
> 

