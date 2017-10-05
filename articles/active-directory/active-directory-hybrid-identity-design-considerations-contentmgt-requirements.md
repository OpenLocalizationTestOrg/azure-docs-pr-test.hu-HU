---
title: "Az Azure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - tartalomkezelési követelmények meghatározása |} Microsoft Docs"
description: "Annak megállapítása, az üzleti tartalomkezelési követelményeit betekintést nyújt. Általában amikor egy felhasználó rendelkezik-e a saját eszköz ő lehet is több hitelesítő adatokat, amelyek alapján az alkalmazás, amely használja a fog váltakozó. Fontos megkülönböztetéséhez tartalmat lett létrehozva, és a vállalati hitelesítő adatok használatával létrehozott megfelelően személyes hitelesítő adataival. A megoldást kell is használhassák a felhőalapú szolgáltatások zökkenőmentes élményt biztosíthat a végfelhasználó közben az adatvédelem biztosításához a, és növeli az adatszivárgás elleni védelem."
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
ms.openlocfilehash: 840de1e1fcba74285788d51d8f544375f0affa77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="5adf9-106">A hibrid identitáskezelési megoldás tartalomkezelési követelmények meghatározása</span><span class="sxs-lookup"><span data-stu-id="5adf9-106">Determine content management requirements for your hybrid identity solution</span></span>
<span data-ttu-id="5adf9-107">A Tartalomkezelés felméréséről a vállalati közvetlen hatással lehet a döntést mely hibrid identitáskezelési megoldás használata.</span><span class="sxs-lookup"><span data-stu-id="5adf9-107">Understanding the content management requirements for your business may direct affect your decision on which hybrid identity solution to use.</span></span> <span data-ttu-id="5adf9-108">A több eszközre és a felhasználók a saját eszközeik elterjedése rendelkező ([BYOD](http://aka.ms/byodcg)), a vállalat a saját adatok kell védeni, de azt is kell módosulna felhasználók adatait.</span><span class="sxs-lookup"><span data-stu-id="5adf9-108">With the proliferation of multiple devices and the capability of users to bring their own devices ([BYOD](http://aka.ms/byodcg)), the company must protect its own data but it also must keep user’s privacy intact.</span></span> <span data-ttu-id="5adf9-109">Általában amikor egy felhasználó rendelkezik-e a saját eszköz ő lehet is több hitelesítő adatokat, amelyek alapján az alkalmazás, amely használja a fog váltakozó.</span><span class="sxs-lookup"><span data-stu-id="5adf9-109">Usually when a user has his own device he might have also multiple credentials that will be alternating according to the application that he uses.</span></span> <span data-ttu-id="5adf9-110">Fontos megkülönböztetéséhez tartalmat lett létrehozva, és a vállalati hitelesítő adatok használatával létrehozott megfelelően személyes hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="5adf9-110">It is important to differentiate what content was created using personal credentials versus the ones created using corporate credentials.</span></span> <span data-ttu-id="5adf9-111">A megoldást kell is használhassák a felhőalapú szolgáltatások zökkenőmentes élményt biztosíthat a végfelhasználó közben az adatvédelem biztosításához a, és növeli az adatszivárgás elleni védelem.</span><span class="sxs-lookup"><span data-stu-id="5adf9-111">Your identity solution should be able to interact with cloud services to provide a seamless experience to the end user while ensure his privacy and increase the protection against data leakage.</span></span> 

<span data-ttu-id="5adf9-112">A megoldást fogja javítható, különböző technikai vezérlőkkel ahhoz, hogy adja meg a tartalom kezelésére, az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="5adf9-112">Your identity solution will be leveraged by different technical controls in order to provide content management as shown in the figure below:</span></span>

![](./media/hybrid-id-design-considerations/securitycontrols.png)

<span data-ttu-id="5adf9-113">**Az identitás-kezelő rendszer használni fogja biztonsági vezérlők**</span><span class="sxs-lookup"><span data-stu-id="5adf9-113">**Security controls that will be leveraging your identity management system**</span></span>

<span data-ttu-id="5adf9-114">A Tartalomkezelés követelmények fogja használni, általában az identitás-kezelő rendszer a következő területeken:</span><span class="sxs-lookup"><span data-stu-id="5adf9-114">In general, content management requirements will leverage your identity management system in the following areas:</span></span>

* <span data-ttu-id="5adf9-115">Adatvédelem: erőforrás birtokló felhasználó azonosítására, és alkalmazza a megfelelő vezérlők egységének fenntartására szolgáló módszert.</span><span class="sxs-lookup"><span data-stu-id="5adf9-115">Privacy: identifying the user that owns a resource and applying the appropriate controls to maintain integrity.</span></span>
* <span data-ttu-id="5adf9-116">Adatok besorolása: azonosítsa a felhasználót vagy csoportot, és megfelelően a besorolását objektumhoz hozzáférési szintjét.</span><span class="sxs-lookup"><span data-stu-id="5adf9-116">Data Classification: identify the user or group and level of access to an object according to its classification.</span></span> 
* <span data-ttu-id="5adf9-117">Az Adatszivárgás adatvédelem: biztonsági vezérlők védelmét az adatok kiszivárgásának elkerülésére kell interakciót folytatni a rendszer ellenőrzi a felhasználó identitását.</span><span class="sxs-lookup"><span data-stu-id="5adf9-117">Data Leakage Protection: security controls responsible for protecting data to avoid leakage will need to interact with the identity system to validate the user’s identity.</span></span> <span data-ttu-id="5adf9-118">Ez fontos is ellenőrzési célból naplózását.</span><span class="sxs-lookup"><span data-stu-id="5adf9-118">This is also important for auditing trail purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="5adf9-119">Olvasási [felhő készítve az adatok besorolását](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) további információt az ajánlott eljárásokról és útmutatást az adatok besorolása érdekében.</span><span class="sxs-lookup"><span data-stu-id="5adf9-119">Read [data classification for cloud readiness](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) for more information about best practices and guidelines for data classification.</span></span>
> 
> 

<span data-ttu-id="5adf9-120">Ha a hibrid identitáskezelési megoldás tervezésének részeként győződjön meg arról, hogy a szervezet igényeinek megfelelően a következő kérdéseket válaszok:</span><span class="sxs-lookup"><span data-stu-id="5adf9-120">When planning your hybrid identity solution ensure that the following questions are answered according to your organization’s requirements:</span></span>

* <span data-ttu-id="5adf9-121">A vállalat rendelkezik biztonsági vezérlő érvényesítését adatvédelem?</span><span class="sxs-lookup"><span data-stu-id="5adf9-121">Does your company have security controls in place to enforce data privacy?</span></span>
  * <span data-ttu-id="5adf9-122">Ha igen, azokat a vezérlőelemeket lesz képes a hibrid identitáskezelési megoldás, amely fogad el kívánja integrálni?</span><span class="sxs-lookup"><span data-stu-id="5adf9-122">If yes, will those security controls be able to integrate with the hybrid identity solution that you are going to adopt?</span></span>
* <span data-ttu-id="5adf9-123">A vállalat használ az adatok besorolásával?</span><span class="sxs-lookup"><span data-stu-id="5adf9-123">Does your company use data classification?</span></span>
  * <span data-ttu-id="5adf9-124">Ha igen, van az aktuális megoldás képes a hibrid identitáskezelési megoldás, amely fogad el kívánja integrálni?</span><span class="sxs-lookup"><span data-stu-id="5adf9-124">If yes, is the current solution able to integrate with the hybrid identity solution that you are going to adopt?</span></span>
* <span data-ttu-id="5adf9-125">Rendelkezik a vállalata jelenleg bármely, a megoldás az adatok kiszivárgásának?</span><span class="sxs-lookup"><span data-stu-id="5adf9-125">Does your company currently have any solution for data leakage?</span></span> 
  * <span data-ttu-id="5adf9-126">Ha igen, van az aktuális megoldás képes a hibrid identitáskezelési megoldás, amely fogad el kívánja integrálni?</span><span class="sxs-lookup"><span data-stu-id="5adf9-126">If yes, is the current solution able to integrate with the hybrid identity solution that you are going to adopt?</span></span>
* <span data-ttu-id="5adf9-127">A vállalati erőforrásokhoz való hozzáférés naplózása nem kell?</span><span class="sxs-lookup"><span data-stu-id="5adf9-127">Does your company need to audit access to resources?</span></span>
  * <span data-ttu-id="5adf9-128">Ha igen, milyen típusú erőforrásokat?</span><span class="sxs-lookup"><span data-stu-id="5adf9-128">If yes, what type of resources?</span></span>
  * <span data-ttu-id="5adf9-129">Ha igen, milyen szintű adatokat szükség?</span><span class="sxs-lookup"><span data-stu-id="5adf9-129">If yes, what level of information is necessary?</span></span>
  * <span data-ttu-id="5adf9-130">Ha igen, ahol a biztonsági naplóban találhatók kell?</span><span class="sxs-lookup"><span data-stu-id="5adf9-130">If yes, where the audit log must reside?</span></span> <span data-ttu-id="5adf9-131">A helyszíni vagy a felhőben?</span><span class="sxs-lookup"><span data-stu-id="5adf9-131">On-premises or in the cloud?</span></span>
* <span data-ttu-id="5adf9-132">Vállalatának meg kell a bizalmas adatok (taj számok esetében, hitelkártyaszámok stb) tartalmazó e-mailek titkosításához?</span><span class="sxs-lookup"><span data-stu-id="5adf9-132">Does your company need to encrypt any emails that contain sensitive data (SSNs, credit card numbers, etc)?</span></span>
* <span data-ttu-id="5adf9-133">Nem a vállalat titkosítania kell az összes dokumentumok/tartalma a külső üzleti partnerek megosztott?</span><span class="sxs-lookup"><span data-stu-id="5adf9-133">Does your company need to encrypt all documents/contents shared with external business partners?</span></span>
* <span data-ttu-id="5adf9-134">A vállalatának meg kell bizonyos típusú e-maileket a vállalati házirendeknek az érvényesítését (nincs válasz mindenkinek végezni, ne továbbítsa)?</span><span class="sxs-lookup"><span data-stu-id="5adf9-134">Does your company need to enforce corporate policies on certain kinds of emails (do no reply all, do not forward)?</span></span>

> [!NOTE]
> <span data-ttu-id="5adf9-135">Ügyeljen arra, hogy minden válaszról jegyzeteket, és megismerheti a válaszok indokait.</span><span class="sxs-lookup"><span data-stu-id="5adf9-135">Make sure to take notes of each answer and understand the rationale behind the answer.</span></span> <span data-ttu-id="5adf9-136">[Data Protection stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) ismerteti a rendelkezésre álló lehetőségeket, illetve az egyes lehetőségek előnyeit és hátrányait.</span><span class="sxs-lookup"><span data-stu-id="5adf9-136">[Define Data Protection Strategy](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) will go over the options available and advantages/disadvantages of each option.</span></span>  <span data-ttu-id="5adf9-137">Ezen kérdések mely leginkább megfelelő lehetőséget az üzleti megválaszolása szükséges.</span><span class="sxs-lookup"><span data-stu-id="5adf9-137">By having answered those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="5adf9-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5adf9-138">Next steps</span></span>
[<span data-ttu-id="5adf9-139">Hozzáférés-vezérlési követelményeinek meghatározása</span><span class="sxs-lookup"><span data-stu-id="5adf9-139">Determine access control requirements</span></span>](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a><span data-ttu-id="5adf9-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5adf9-140">See Also</span></span>
[<span data-ttu-id="5adf9-141">Kialakítási szempontok áttekintése</span><span class="sxs-lookup"><span data-stu-id="5adf9-141">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

