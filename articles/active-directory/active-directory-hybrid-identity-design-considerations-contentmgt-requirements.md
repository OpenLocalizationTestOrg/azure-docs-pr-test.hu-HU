---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - tartalomkezelési követelmények meghatározása |} Microsoft Docs"
description: "Hogyan toodetermine hello a vállalat követelményeinek tartalomkezelési betekintést nyújt. Általában amikor egy felhasználó rendelkezik-e a saját eszköz ő lehet is több hitelesítő adatokat, amelyek függően, hogy használó toohello alkalmazás váltakozó lesz. Fontos toodifferentiate is, hogy mely tartalmak lett létrehozva, és a vállalati hitelesítő adatok használatával létrehozott ők hello személyes hitelesítő adataival. A megoldást kell a felhőalapú szolgáltatások tooprovide képes toointeract egy zökkenőmentes élményt toohello végfelhasználói közben az adatvédelem biztosításához és az adatszivárgás elleni hello növelhető."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: dd1ef776-db4d-4ab8-9761-2adaa5a4f004
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 607d366633c37b65ec5cf8ae5c64d73ca1cc96b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="371dd-106">A hibrid identitáskezelési megoldás tartalomkezelési követelmények meghatározása</span><span class="sxs-lookup"><span data-stu-id="371dd-106">Determine content management requirements for your hybrid identity solution</span></span>
<span data-ttu-id="371dd-107">Hello tartalomkezelési követelményeinek ismertetése számára az üzleti előfordulhat, hogy közvetlen befolyásolhatják, mely hibrid identitáskezelési megoldás toouse meg.</span><span class="sxs-lookup"><span data-stu-id="371dd-107">Understanding hello content management requirements for your business may direct affect your decision on which hybrid identity solution toouse.</span></span> <span data-ttu-id="371dd-108">A hello elterjedése több eszközök és felhasználók toobring hello képességét a saját eszközeiket ([BYOD](http://aka.ms/byodcg)), hello vállalati védelmét kell beállítani, hogy hozzáadják saját adataikat, de azt is kell módosulna felhasználók adatait.</span><span class="sxs-lookup"><span data-stu-id="371dd-108">With hello proliferation of multiple devices and hello capability of users toobring their own devices ([BYOD](http://aka.ms/byodcg)), hello company must protect its own data but it also must keep user’s privacy intact.</span></span> <span data-ttu-id="371dd-109">Általában amikor egy felhasználó rendelkezik-e a saját eszköz ő lehet is több hitelesítő adatokat, amelyek függően, hogy használó toohello alkalmazás váltakozó lesz.</span><span class="sxs-lookup"><span data-stu-id="371dd-109">Usually when a user has his own device he might have also multiple credentials that will be alternating according toohello application that he uses.</span></span> <span data-ttu-id="371dd-110">Fontos toodifferentiate is, hogy mely tartalmak lett létrehozva, és a vállalati hitelesítő adatok használatával létrehozott ők hello személyes hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="371dd-110">It is important toodifferentiate what content was created using personal credentials versus hello ones created using corporate credentials.</span></span> <span data-ttu-id="371dd-111">A megoldást kell a felhőalapú szolgáltatások tooprovide képes toointeract egy zökkenőmentes élményt toohello végfelhasználói közben az adatvédelem biztosításához és az adatszivárgás elleni hello növelhető.</span><span class="sxs-lookup"><span data-stu-id="371dd-111">Your identity solution should be able toointeract with cloud services tooprovide a seamless experience toohello end user while ensure his privacy and increase hello protection against data leakage.</span></span> 

<span data-ttu-id="371dd-112">A megoldást fog javítható a rendelés tooprovide tartalomkezelési különböző technikai vezérlőkkel, az alábbi hello ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="371dd-112">Your identity solution will be leveraged by different technical controls in order tooprovide content management as shown in hello figure below:</span></span>

![](./media/hybrid-id-design-considerations/securitycontrols.png)

<span data-ttu-id="371dd-113">**Az identitás-kezelő rendszer használni fogja biztonsági vezérlők**</span><span class="sxs-lookup"><span data-stu-id="371dd-113">**Security controls that will be leveraging your identity management system**</span></span>

<span data-ttu-id="371dd-114">Tartalomkezelési követelményeit fogja használni, általában az identitás felügyeleti rendszer hello a következő területeken:</span><span class="sxs-lookup"><span data-stu-id="371dd-114">In general, content management requirements will leverage your identity management system in hello following areas:</span></span>

* <span data-ttu-id="371dd-115">Adatvédelem: olyan erőforráshoz és alkalmazása hello megfelelő vezérlők toomaintain integritási birtokló hello felhasználói azonosító.</span><span class="sxs-lookup"><span data-stu-id="371dd-115">Privacy: identifying hello user that owns a resource and applying hello appropriate controls toomaintain integrity.</span></span>
* <span data-ttu-id="371dd-116">Adatok besorolása: azonosítsa a hello felhasználót vagy csoportot és szintű hozzáférés tooan objektum tooits besorolás alapján történik.</span><span class="sxs-lookup"><span data-stu-id="371dd-116">Data Classification: identify hello user or group and level of access tooan object according tooits classification.</span></span> 
* <span data-ttu-id="371dd-117">Az Adatszivárgás adatvédelem: biztonsági vezérlők tooavoid adatszivárgás védelmét kell toointeract hello identitás rendszer toovalidate hello felhasználó identitásával.</span><span class="sxs-lookup"><span data-stu-id="371dd-117">Data Leakage Protection: security controls responsible for protecting data tooavoid leakage will need toointeract with hello identity system toovalidate hello user’s identity.</span></span> <span data-ttu-id="371dd-118">Ez fontos is ellenőrzési célból naplózását.</span><span class="sxs-lookup"><span data-stu-id="371dd-118">This is also important for auditing trail purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="371dd-119">Olvasási [felhő készítve az adatok besorolását](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) további információt az ajánlott eljárásokról és útmutatást az adatok besorolása érdekében.</span><span class="sxs-lookup"><span data-stu-id="371dd-119">Read [data classification for cloud readiness](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) for more information about best practices and guidelines for data classification.</span></span>
> 
> 

<span data-ttu-id="371dd-120">Ha a hibrid identitáskezelési megoldás tervezésének részeként győződjön meg arról, hogy a következő hello a kérdések és válaszok tooyour szervezet igényeinek megfelelően:</span><span class="sxs-lookup"><span data-stu-id="371dd-120">When planning your hybrid identity solution ensure that hello following questions are answered according tooyour organization’s requirements:</span></span>

* <span data-ttu-id="371dd-121">Nem rendelkezik a vállalata biztonsági vezérlők hely tooenforce adatvédelem?</span><span class="sxs-lookup"><span data-stu-id="371dd-121">Does your company have security controls in place tooenforce data privacy?</span></span>
  * <span data-ttu-id="371dd-122">Ha igen, azokat a vezérlőelemeket lesz a hello hibrid identitáskezelési megoldás, hogy-e folyamatban tooadopt képes toointegrate?</span><span class="sxs-lookup"><span data-stu-id="371dd-122">If yes, will those security controls be able toointegrate with hello hybrid identity solution that you are going tooadopt?</span></span>
* <span data-ttu-id="371dd-123">A vállalat használ az adatok besorolásával?</span><span class="sxs-lookup"><span data-stu-id="371dd-123">Does your company use data classification?</span></span>
  * <span data-ttu-id="371dd-124">Ha igen, van hello aktuális megoldás képes toointegrate hello hibrid identitáskezelési megoldás, hogy-e folyamatban tooadopt?</span><span class="sxs-lookup"><span data-stu-id="371dd-124">If yes, is hello current solution able toointegrate with hello hybrid identity solution that you are going tooadopt?</span></span>
* <span data-ttu-id="371dd-125">Rendelkezik a vállalata jelenleg bármely, a megoldás az adatok kiszivárgásának?</span><span class="sxs-lookup"><span data-stu-id="371dd-125">Does your company currently have any solution for data leakage?</span></span> 
  * <span data-ttu-id="371dd-126">Ha igen, van hello aktuális megoldás képes toointegrate hello hibrid identitáskezelési megoldás, hogy-e folyamatban tooadopt?</span><span class="sxs-lookup"><span data-stu-id="371dd-126">If yes, is hello current solution able toointegrate with hello hybrid identity solution that you are going tooadopt?</span></span>
* <span data-ttu-id="371dd-127">A vállalatának meg kell tooaudit hozzáférés tooresources?</span><span class="sxs-lookup"><span data-stu-id="371dd-127">Does your company need tooaudit access tooresources?</span></span>
  * <span data-ttu-id="371dd-128">Ha igen, milyen típusú erőforrásokat?</span><span class="sxs-lookup"><span data-stu-id="371dd-128">If yes, what type of resources?</span></span>
  * <span data-ttu-id="371dd-129">Ha igen, milyen szintű adatokat szükség?</span><span class="sxs-lookup"><span data-stu-id="371dd-129">If yes, what level of information is necessary?</span></span>
  * <span data-ttu-id="371dd-130">Ha igen, ahol hello napló kell lennie?</span><span class="sxs-lookup"><span data-stu-id="371dd-130">If yes, where hello audit log must reside?</span></span> <span data-ttu-id="371dd-131">A helyszíni vagy felhőben hello?</span><span class="sxs-lookup"><span data-stu-id="371dd-131">On-premises or in hello cloud?</span></span>
* <span data-ttu-id="371dd-132">A vállalatának meg kell tooencrypt a bizalmas adatok (taj számok esetében, hitelkártyaszámok stb) tartalmazó e-mailek?</span><span class="sxs-lookup"><span data-stu-id="371dd-132">Does your company need tooencrypt any emails that contain sensitive data (SSNs, credit card numbers, etc)?</span></span>
* <span data-ttu-id="371dd-133">A vállalatának meg kell tooencrypt összes dokumentumok/tartalma a külső üzleti partnerek megosztott?</span><span class="sxs-lookup"><span data-stu-id="371dd-133">Does your company need tooencrypt all documents/contents shared with external business partners?</span></span>
* <span data-ttu-id="371dd-134">A vállalatának meg kell tooenforce vállalati házirendek-e-mailek bizonyos típusú (nincs válasz mindenkinek végezni, ne továbbítsa)?</span><span class="sxs-lookup"><span data-stu-id="371dd-134">Does your company need tooenforce corporate policies on certain kinds of emails (do no reply all, do not forward)?</span></span>

> [!NOTE]
> <span data-ttu-id="371dd-135">Győződjön meg arról, hogy tootake megjegyzések minden válaszról és hello válasz hello logikája ismertetése.</span><span class="sxs-lookup"><span data-stu-id="371dd-135">Make sure tootake notes of each answer and understand hello rationale behind hello answer.</span></span> <span data-ttu-id="371dd-136">[Data Protection stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) hello lehetőségeit és az egyes lehetőségek előnyeit és hátrányait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="371dd-136">[Define Data Protection Strategy](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) will go over hello options available and advantages/disadvantages of each option.</span></span>  <span data-ttu-id="371dd-137">Ezen kérdések mely leginkább megfelelő lehetőséget az üzleti megválaszolása szükséges.</span><span class="sxs-lookup"><span data-stu-id="371dd-137">By having answered those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="371dd-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="371dd-138">Next steps</span></span>
[<span data-ttu-id="371dd-139">Hozzáférés-vezérlési követelményeinek meghatározása</span><span class="sxs-lookup"><span data-stu-id="371dd-139">Determine access control requirements</span></span>](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a><span data-ttu-id="371dd-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="371dd-140">See Also</span></span>
[<span data-ttu-id="371dd-141">Kialakítási szempontok áttekintése</span><span class="sxs-lookup"><span data-stu-id="371dd-141">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

