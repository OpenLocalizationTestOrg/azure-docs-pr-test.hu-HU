---
title: "aaaAzure AD rétegzett biztonsági jelszó |} Microsoft Docs"
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
ms.openlocfilehash: 10d8b600d9f4c02355b2cd8c5dccf8505aaf210d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="a-multi-tiered-approach-tooazure-ad-password-security"></a><span data-ttu-id="a7d54-103">A többrétegű megközelítés tooAzure AD biztonsági jelszó</span><span class="sxs-lookup"><span data-stu-id="a7d54-103">A multi-tiered approach tooAzure AD password security</span></span>

<span data-ttu-id="a7d54-104">Ez a cikk ismerteti, amelyek néhány ajánlott eljárás követésével egy olyan felhasználó nevében, vagy egy rendszergazda tooprotect az Azure Active Directory (Azure AD) vagy a Microsoft Account.</span><span class="sxs-lookup"><span data-stu-id="a7d54-104">This article discusses some best practices you can follow as a user or as an administrator tooprotect your Azure Active Directory (Azure AD) or Microsoft Account.</span></span>

 > [!NOTE]
 > <span data-ttu-id="a7d54-105">Az Azure AD-rendszergazdák alaphelyzetbe állíthatja a felhasználói jelszavakat hello útmutatásának hello cikkben [az Azure Active Directory felhasználó jelszavának alaphelyzetbe állítása hello](active-directory-users-reset-password-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a7d54-105">Azure AD administrators can reset user passwords using hello guidance in hello article [Reset hello password for a user in Azure Active Directory](active-directory-users-reset-password-azure-portal.md).</span></span>
 >
 > <span data-ttu-id="a7d54-106">Felhasználók visszaállíthatják a saját jelszavát hello útmutatásának hello cikkben [elfelejtettem a saját Azure AD-jelszó súgó](active-directory-passwords-update-your-own-password.md).</span><span class="sxs-lookup"><span data-stu-id="a7d54-106">Users can reset their own password using hello guidance in hello article [Help I forgot my Azure AD password](active-directory-passwords-update-your-own-password.md).</span></span>
 >

## <a name="password-requirements"></a><span data-ttu-id="a7d54-107">Jelszavakra vonatkozó követelmények</span><span class="sxs-lookup"><span data-stu-id="a7d54-107">Password requirements</span></span>

<span data-ttu-id="a7d54-108">Az Azure AD magában foglalja a következő közös megközelítések toosecuring jelszavak hello:</span><span class="sxs-lookup"><span data-stu-id="a7d54-108">Azure AD incorporates hello following common approaches toosecuring passwords:</span></span>

* <span data-ttu-id="a7d54-109">Jelszóhosszra vonatkozó követelmények</span><span class="sxs-lookup"><span data-stu-id="a7d54-109">Password length requirements</span></span>
* <span data-ttu-id="a7d54-110">Összetettségi követelményeknek</span><span class="sxs-lookup"><span data-stu-id="a7d54-110">Password complexity requirements</span></span>
* <span data-ttu-id="a7d54-111">Normál és időszakos jelszólejárat</span><span class="sxs-lookup"><span data-stu-id="a7d54-111">Regular and periodic password expiration</span></span>

<span data-ttu-id="a7d54-112">Jelszó alaphelyzetbe állítása, az Azure Active Directoryban kapcsolatos információkért lásd: hello témakör [az Azure AD az önkiszolgáló jelszó-változtatási az informatikai szakembereknek szóló hello](active-directory-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="a7d54-112">For information about password reset in Azure Active Directory, see hello topic [Azure AD self-service password reset for hello IT professional](active-directory-passwords.md).</span></span>

## <a name="azure-ad-password-protections"></a><span data-ttu-id="a7d54-113">Az Azure AD-jelszó védelmet</span><span class="sxs-lookup"><span data-stu-id="a7d54-113">Azure AD password protections</span></span>

<span data-ttu-id="a7d54-114">Az Azure AD és a Microsoft-Fiókrendszerben használja bizonyítása iparági hello megközelíti tooensure biztonságos védelmét, beleértve a felhasználói és rendszergazdai jelszavakat:</span><span class="sxs-lookup"><span data-stu-id="a7d54-114">Azure AD and hello Microsoft Account System use industry proven approaches tooensure secure protection of user and administrator passwords including:</span></span>

* <span data-ttu-id="a7d54-115">Dinamikusan letiltott jelszavak</span><span class="sxs-lookup"><span data-stu-id="a7d54-115">Dynamically banned passwords</span></span>
* <span data-ttu-id="a7d54-116">Intelligens jelszózárolás</span><span class="sxs-lookup"><span data-stu-id="a7d54-116">Smart Password Lockout</span></span>

<span data-ttu-id="a7d54-117">Aktuális eredményei alapján jelszó-kezeléssel kapcsolatos további információkért lásd: hello tanulmány [jelszó útmutatást](http://aka.ms/passwordguidance).</span><span class="sxs-lookup"><span data-stu-id="a7d54-117">For information about password management based on current research, see hello whitepaper [Password Guidance](http://aka.ms/passwordguidance).</span></span>

### <a name="dynamically-banned-passwords"></a><span data-ttu-id="a7d54-118">Dinamikusan letiltott jelszavak</span><span class="sxs-lookup"><span data-stu-id="a7d54-118">Dynamically banned passwords</span></span>

<span data-ttu-id="a7d54-119">Az Azure AD és a Microsoft Accounts megakadályozhatja a jelszavas védelem által dinamikusan tiltó a gyakran használt jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="a7d54-119">Azure AD and Microsoft Accounts safeguard password protection by dynamically banning commonly used passwords.</span></span> <span data-ttu-id="a7d54-120">hello Azure ID Identity Protection-csapatnak rendszeresen elemzi tiltott jelszavak listája, meggátolja, hogy a felhasználók kiválasztása a gyakran használt jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="a7d54-120">hello Azure ID Identity Protection team routinely analyzes banned password lists, preventing users from selecting commonly used passwords.</span></span> <span data-ttu-id="a7d54-121">Ez a szolgáltatás elérhető tooAzure AD és hello Microsoft-fiók szolgáltatás felhasználói.</span><span class="sxs-lookup"><span data-stu-id="a7d54-121">This service is available tooAzure AD and hello Microsoft Account Service customers.</span></span>

<span data-ttu-id="a7d54-122">Jelszavak létrehozása, ha fontos a rendszergazdák tooencourage felhasználók toochoose jelszó kifejezések, amely tartalmazza az betűket, számokat, karakterek vagy szavak egyedi kombinációja.</span><span class="sxs-lookup"><span data-stu-id="a7d54-122">When creating passwords, it is important for administrators tooencourage users toochoose password phrases that include a unique combination of letters, numbers, characters, or words.</span></span> <span data-ttu-id="a7d54-123">Ez a megközelítés segít toomake felhasználói jelszavak majdnem lehetetlen toobe biztonsága sérült, de megkönnyíthető a felhasználóknak tooremember.</span><span class="sxs-lookup"><span data-stu-id="a7d54-123">This approach helps toomake user passwords nearly impossible toobe compromised but easier for users tooremember.</span></span>

#### <a name="password-breaches"></a><span data-ttu-id="a7d54-124">Jelszó megszegése</span><span class="sxs-lookup"><span data-stu-id="a7d54-124">Password breaches</span></span>

<span data-ttu-id="a7d54-125">Microsoft toostay egy lépésben előre számítógépes-bűnözők mindig működik.</span><span class="sxs-lookup"><span data-stu-id="a7d54-125">Microsoft is always working toostay one step ahead of cyber-criminals.</span></span>

<span data-ttu-id="a7d54-126">az Azure AD Identity Protection-csapatnak hello folyamatosan elemzi általánosan használt jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="a7d54-126">hello Azure AD Identity Protection team continually analyzes passwords that are commonly used.</span></span> <span data-ttu-id="a7d54-127">Számítógépes-bűnözők is használhatja hasonló stratégiák tooinform azok támadásokkal, például a épület egy [rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) repedés jelszókivonatait a.</span><span class="sxs-lookup"><span data-stu-id="a7d54-127">Cyber-criminals also use similar strategies tooinform their attacks, such as building a [rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) for cracking password hashes.</span></span>

<span data-ttu-id="a7d54-128">Folyamatosan elemzi a Microsoft [adatszivárgáshoz](https://www.privacyrights.org/data-breaches) toomaintain dinamikusan frissített tiltott jelszó listáját, amely biztosítja, hogy így elkerülhetők a valódi tilos sebezhető jelszavak fenyegetés tooAzure AD felhasználók.</span><span class="sxs-lookup"><span data-stu-id="a7d54-128">Microsoft continually analyzes [data breaches](https://www.privacyrights.org/data-breaches) toomaintain a dynamically updated banned password list, which ensures that vulnerable passwords are banned before they become a real threat tooAzure AD customers.</span></span> <span data-ttu-id="a7d54-129">Az aktuális biztonsági erőfeszítéseket kapcsolatos további információkért lásd: hello [Microsoft biztonsági az Eszközintelligencia-jelentés](https://www.microsoft.com/security/sir/default.aspx).</span><span class="sxs-lookup"><span data-stu-id="a7d54-129">For more information about our current security efforts, see hello [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx).</span></span>

### <a name="smart-password-lockout"></a><span data-ttu-id="a7d54-130">Intelligens jelszózárolás</span><span class="sxs-lookup"><span data-stu-id="a7d54-130">Smart Password Lockout</span></span>

<span data-ttu-id="a7d54-131">Ha az Azure AD a felhasználói jelszó egy potenciális számítógépes-büntetőjogi közben toohack észlel, azt rendelkező intelligens jelszó fiókzárolási hello felhasználói fiókot a zárolása.</span><span class="sxs-lookup"><span data-stu-id="a7d54-131">When Azure AD detects a potential cyber-criminal trying toohack into a user password, we lock hello user account with Smart Password Lockout.</span></span> <span data-ttu-id="a7d54-132">Az Azure AD úgy van kialakítva, toodetermine hello kockázatának adott bejelentkezési munkameneteket.</span><span class="sxs-lookup"><span data-stu-id="a7d54-132">Azure AD is designed toodetermine hello risk associated with specific login sessions.</span></span> <span data-ttu-id="a7d54-133">Hello legfrissebb biztonsági adatok, majd érvénybe lépni fiókzárolási szemantikáját toostop számítógépes fenyegetéseket.</span><span class="sxs-lookup"><span data-stu-id="a7d54-133">Then using hello most up-to-date security data, we apply lockout semantics toostop cyber threats.</span></span>

<span data-ttu-id="a7d54-134">Ha a felhasználó le van tiltva az Azure AD ki, a képernyő következőhöz hasonló toohello egy, a következő:</span><span class="sxs-lookup"><span data-stu-id="a7d54-134">If a user is locked out of Azure AD, their screen looks similar toohello one that follows:</span></span>

  ![Kizárva az Azure AD-ből](./media/active-directory-secure-passwords/locked-out-azuread.png)

<span data-ttu-id="a7d54-136">Más Microsoft-fiók a képernyő következőhöz hasonló toohello egy, a következő:</span><span class="sxs-lookup"><span data-stu-id="a7d54-136">For other Microsoft accounts, their screen looks similar toohello one that follows:</span></span>

  ![Kizárva egy Microsoft-fiókból](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

<span data-ttu-id="a7d54-138">Jelszó alaphelyzetbe állítása, az Azure Active Directoryban kapcsolatos információkért lásd: hello témakör [az Azure AD az önkiszolgáló jelszó-változtatási az informatikai szakembereknek szóló hello](active-directory-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="a7d54-138">For information about password reset in Azure Active Directory, see hello topic [Azure AD self-service password reset for hello IT professional](active-directory-passwords.md).</span></span>

  >[!NOTE]
  ><span data-ttu-id="a7d54-139">Ha az Azure AD-rendszergazdaként, érdemes lehet a toouse [Windows Hello](https://www.microsoft.com/windows/windows-hello) tooavoid, hogy a felhasználók regisztrálását hagyományos jelszót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a7d54-139">If you are an Azure AD administrator, you may want toouse [Windows Hello](https://www.microsoft.com/windows/windows-hello) tooavoid having your users create traditional passwords altogether.</span></span>
  >

## <a name="next-steps"></a><span data-ttu-id="a7d54-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a7d54-140">Next steps</span></span>

* [<span data-ttu-id="a7d54-141">Hogyan tooupdate a saját jelszavát</span><span class="sxs-lookup"><span data-stu-id="a7d54-141">How tooupdate your own password</span></span>](active-directory-passwords-update-your-own-password.md)
* [<span data-ttu-id="a7d54-142">Azure-identitás felügyeleti hello alapjai</span><span class="sxs-lookup"><span data-stu-id="a7d54-142">hello fundamentals of Azure identity management</span></span>](fundamentals-identity.md)
* [<span data-ttu-id="a7d54-143">A jelentés a jelszó-visszaállítási tevékenység</span><span class="sxs-lookup"><span data-stu-id="a7d54-143">Report on password reset activity</span></span>](active-directory-passwords-reporting.md)


