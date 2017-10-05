---
title: "Az Azure AD rétegzett biztonsági jelszó |} Microsoft Docs"
description: "Azt ismerteti, hogyan az Azure AD kikényszeríti a erős jelszavak és védje a felhasználók jelszavát a számítógépes bűnözők"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: joflore
ms.openlocfilehash: 32464307ccb082b25538eaa522c1cdedef1ca555
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="a-multi-tiered-approach-to-azure-ad-password-security"></a><span data-ttu-id="a0666-103">Az Azure AD-jelszó biztonsági többrétegű megközelítése</span><span class="sxs-lookup"><span data-stu-id="a0666-103">A multi-tiered approach to Azure AD password security</span></span>

<span data-ttu-id="a0666-104">Ez a cikk ismerteti, amelyek néhány ajánlott eljárás követésével egy olyan felhasználó nevében, vagy arra, hogy az Azure Active Directory (Azure AD) vagy a Microsoft Account rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a0666-104">This article discusses some best practices you can follow as a user or as an administrator to protect your Azure Active Directory (Azure AD) or Microsoft Account.</span></span>

 > [!NOTE]
 > <span data-ttu-id="a0666-105">Az Azure AD-rendszergazdák alaphelyzetbe állíthatja a felhasználói jelszavakat útmutatásának megfelelően történt a cikkben található [az Azure Active Directoryban a felhasználó jelszavának visszaállítása](active-directory-users-reset-password-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a0666-105">Azure AD administrators can reset user passwords using the guidance in the article [Reset the password for a user in Azure Active Directory](active-directory-users-reset-password-azure-portal.md).</span></span>
 >
 > <span data-ttu-id="a0666-106">Felhasználók visszaállíthatják a saját jelszavát útmutatásának megfelelően történt a cikkben található [elfelejtettem a saját Azure AD-jelszó súgó](active-directory-passwords-update-your-own-password.md).</span><span class="sxs-lookup"><span data-stu-id="a0666-106">Users can reset their own password using the guidance in the article [Help I forgot my Azure AD password](active-directory-passwords-update-your-own-password.md).</span></span>
 >

## <a name="password-requirements"></a><span data-ttu-id="a0666-107">Jelszavakra vonatkozó követelmények</span><span class="sxs-lookup"><span data-stu-id="a0666-107">Password requirements</span></span>

<span data-ttu-id="a0666-108">Az Azure AD a következő általános megközelítéseket tartalmazza a jelszavak védelme érdekében:</span><span class="sxs-lookup"><span data-stu-id="a0666-108">Azure AD incorporates the following common approaches to securing passwords:</span></span>

* <span data-ttu-id="a0666-109">Jelszóhosszra vonatkozó követelmények</span><span class="sxs-lookup"><span data-stu-id="a0666-109">Password length requirements</span></span>
* <span data-ttu-id="a0666-110">Összetettségi követelményeknek</span><span class="sxs-lookup"><span data-stu-id="a0666-110">Password complexity requirements</span></span>
* <span data-ttu-id="a0666-111">Normál és időszakos jelszólejárat</span><span class="sxs-lookup"><span data-stu-id="a0666-111">Regular and periodic password expiration</span></span>

<span data-ttu-id="a0666-112">Jelszó alaphelyzetbe állítása, az Azure Active Directoryban kapcsolatos információkért lásd a témakör [az Azure AD az önkiszolgáló jelszó-változtatási az informatikai szakembereknek szóló](active-directory-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="a0666-112">For information about password reset in Azure Active Directory, see the topic [Azure AD self-service password reset for the IT professional](active-directory-passwords.md).</span></span>

## <a name="azure-ad-password-protections"></a><span data-ttu-id="a0666-113">Az Azure AD-jelszó védelmet</span><span class="sxs-lookup"><span data-stu-id="a0666-113">Azure AD password protections</span></span>

<span data-ttu-id="a0666-114">Az Azure AD és a Microsoft-fiókrendszer megközelítések bizonyítása iparági használatával biztosíthatja a felhasználói és rendszergazdai jelszavakat, beleértve a biztonságos védelméről:</span><span class="sxs-lookup"><span data-stu-id="a0666-114">Azure AD and the Microsoft Account System use industry proven approaches to ensure secure protection of user and administrator passwords including:</span></span>

* <span data-ttu-id="a0666-115">Dinamikusan letiltott jelszavak</span><span class="sxs-lookup"><span data-stu-id="a0666-115">Dynamically banned passwords</span></span>
* <span data-ttu-id="a0666-116">Intelligens jelszózárolás</span><span class="sxs-lookup"><span data-stu-id="a0666-116">Smart Password Lockout</span></span>

<span data-ttu-id="a0666-117">A jelszókezelés aktuális eredményei alapján kapcsolatos információkért lásd [jelszó útmutatást](http://aka.ms/passwordguidance).</span><span class="sxs-lookup"><span data-stu-id="a0666-117">For information about password management based on current research, see the whitepaper [Password Guidance](http://aka.ms/passwordguidance).</span></span>

### <a name="dynamically-banned-passwords"></a><span data-ttu-id="a0666-118">Dinamikusan letiltott jelszavak</span><span class="sxs-lookup"><span data-stu-id="a0666-118">Dynamically banned passwords</span></span>

<span data-ttu-id="a0666-119">Az Azure AD és a Microsoft Accounts megakadályozhatja a jelszavas védelem által dinamikusan tiltó a gyakran használt jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="a0666-119">Azure AD and Microsoft Accounts safeguard password protection by dynamically banning commonly used passwords.</span></span> <span data-ttu-id="a0666-120">Az Azure ID Identity Protection csapat rendszeresen elemzi a letiltott jelszavak listáit, amely segít meggátolni, hogy a felhasználók gyakran használt jelszavakat válasszanak.</span><span class="sxs-lookup"><span data-stu-id="a0666-120">The Azure ID Identity Protection team routinely analyzes banned password lists, preventing users from selecting commonly used passwords.</span></span> <span data-ttu-id="a0666-121">Ez a szolgáltatás az Azure AD és a Microsoft-fiókszolgáltatás ügyfeleinek érhető el.</span><span class="sxs-lookup"><span data-stu-id="a0666-121">This service is available to Azure AD and the Microsoft Account Service customers.</span></span>

<span data-ttu-id="a0666-122">Jelszavak létrehozása, ha fontos a rendszergazdák számára ösztönözzék a felhasználókat válassza ki a jelszó kifejezések, amely tartalmazza az betűket, számokat, karakterek vagy szavak egyedi kombinációja.</span><span class="sxs-lookup"><span data-stu-id="a0666-122">When creating passwords, it is important for administrators to encourage users to choose password phrases that include a unique combination of letters, numbers, characters, or words.</span></span> <span data-ttu-id="a0666-123">Ez a megközelítés segít lehetetlenné felhasználói jelszavak majdnem sérült biztonságú, de jegyezze meg a felhasználók könnyebben.</span><span class="sxs-lookup"><span data-stu-id="a0666-123">This approach helps to make user passwords nearly impossible to be compromised but easier for users to remember.</span></span>

#### <a name="password-breaches"></a><span data-ttu-id="a0666-124">Jelszó megszegése</span><span class="sxs-lookup"><span data-stu-id="a0666-124">Password breaches</span></span>

<span data-ttu-id="a0666-125">A Microsoft mindig dolgozik egy lépésben előre számítógépes-bűnözők marad.</span><span class="sxs-lookup"><span data-stu-id="a0666-125">Microsoft is always working to stay one step ahead of cyber-criminals.</span></span>

<span data-ttu-id="a0666-126">Az Azure AD Identity Protection csapata folyamatosan elemzi a gyakran használt jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="a0666-126">The Azure AD Identity Protection team continually analyzes passwords that are commonly used.</span></span> <span data-ttu-id="a0666-127">A számítógépes bűnözők is hasonló stratégiákat használnak a támadásaikhoz, például [szivárványtáblát](https://en.wikipedia.org/wiki/Rainbow_table) hoznak létre a jelszókivonatok feltörése érdekében.</span><span class="sxs-lookup"><span data-stu-id="a0666-127">Cyber-criminals also use similar strategies to inform their attacks, such as building a [rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) for cracking password hashes.</span></span>

<span data-ttu-id="a0666-128">A Microsoft folyamatosan elemzi az [adatszivárgásokat](https://www.privacyrights.org/data-breaches) a tiltott jelszavak dinamikusan frissített listájának fenntartása érdekében, így a sebezhető jelszavak még azelőtt le lesznek tiltva, hogy valódi fenyegetést jelenthetnének az Azure AD ügyfelei számára.</span><span class="sxs-lookup"><span data-stu-id="a0666-128">Microsoft continually analyzes [data breaches](https://www.privacyrights.org/data-breaches) to maintain a dynamically updated banned password list, which ensures that vulnerable passwords are banned before they become a real threat to Azure AD customers.</span></span> <span data-ttu-id="a0666-129">Az aktuális biztonsági erőfeszítéseinkről további információt a [Microsoft biztonsági intelligenciáról szóló jelentésében](https://www.microsoft.com/security/sir/default.aspx) talál.</span><span class="sxs-lookup"><span data-stu-id="a0666-129">For more information about our current security efforts, see the [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx).</span></span>

### <a name="smart-password-lockout"></a><span data-ttu-id="a0666-130">Intelligens jelszózárolás</span><span class="sxs-lookup"><span data-stu-id="a0666-130">Smart Password Lockout</span></span>

<span data-ttu-id="a0666-131">Amikor az Azure AD érzékeli, hogy egy lehetséges számítógépes bűnöző megpróbál feltörni egy felhasználói jelszót, az Intelligens jelszózárolás segítségével zároljuk a felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="a0666-131">When Azure AD detects a potential cyber-criminal trying to hack into a user password, we lock the user account with Smart Password Lockout.</span></span> <span data-ttu-id="a0666-132">Az Azure AD-t arra tervezték, hogy meghatározza az egyes bejelentkezési munkamenetekkel társított kockázatokat.</span><span class="sxs-lookup"><span data-stu-id="a0666-132">Azure AD is designed to determine the risk associated with specific login sessions.</span></span> <span data-ttu-id="a0666-133">Segítségével a lehető legfrissebb biztonsági adatokat, majd érvénybe lépni fiókzárolási szemantikáját, hogy számítógépes fenyegetéseket.</span><span class="sxs-lookup"><span data-stu-id="a0666-133">Then using the most up-to-date security data, we apply lockout semantics to stop cyber threats.</span></span>

<span data-ttu-id="a0666-134">Ha a felhasználó le van tiltva az Azure AD ki, a képernyő hasonlít, amely a következő:</span><span class="sxs-lookup"><span data-stu-id="a0666-134">If a user is locked out of Azure AD, their screen looks similar to the one that follows:</span></span>

  ![Kizárva az Azure AD-ből](./media/active-directory-secure-passwords/locked-out-azuread.png)

<span data-ttu-id="a0666-136">Más Microsoft-fiók a képernyő hasonlít, amely a következő:</span><span class="sxs-lookup"><span data-stu-id="a0666-136">For other Microsoft accounts, their screen looks similar to the one that follows:</span></span>

  ![Kizárva egy Microsoft-fiókból](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

<span data-ttu-id="a0666-138">Jelszó alaphelyzetbe állítása, az Azure Active Directoryban kapcsolatos információkért lásd a témakör [az Azure AD az önkiszolgáló jelszó-változtatási az informatikai szakembereknek szóló](active-directory-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="a0666-138">For information about password reset in Azure Active Directory, see the topic [Azure AD self-service password reset for the IT professional](active-directory-passwords.md).</span></span>

  >[!NOTE]
  ><span data-ttu-id="a0666-139">Ha Ön Azure AD-rendszergazda, javasolt a [Windows Hello](https://www.microsoft.com/windows/windows-hello) használata, hogy a felhasználók egyáltalán ne is hozhassanak létre hagyományos jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="a0666-139">If you are an Azure AD administrator, you may want to use [Windows Hello](https://www.microsoft.com/windows/windows-hello) to avoid having your users create traditional passwords altogether.</span></span>
  >

## <a name="next-steps"></a><span data-ttu-id="a0666-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0666-140">Next steps</span></span>

* [<span data-ttu-id="a0666-141">Saját jelszó frissítése</span><span class="sxs-lookup"><span data-stu-id="a0666-141">How to update your own password</span></span>](active-directory-passwords-update-your-own-password.md)
* [<span data-ttu-id="a0666-142">Az Azure-identitáskezelés alapjai</span><span class="sxs-lookup"><span data-stu-id="a0666-142">The fundamentals of Azure identity management</span></span>](fundamentals-identity.md)
* [<span data-ttu-id="a0666-143">A jelentés a jelszó-visszaállítási tevékenység</span><span class="sxs-lookup"><span data-stu-id="a0666-143">Report on password reset activity</span></span>](active-directory-passwords-reporting.md)


