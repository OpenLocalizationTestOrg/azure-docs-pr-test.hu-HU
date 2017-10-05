---
title: "Az Azure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - határozza meg a címtár-szinkronizálás követelményeinek |} Microsoft Docs"
description: "Milyen követelmények szükségesek szinkronizálásához között a felhasználók azonosítása az = a helyszíni és a vállalati felhő."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 593eaa71-17eb-4c16-8c98-43cc62987e65
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 5ef87e606f055359ca325befd6048353ce57ca2b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="determine-directory-synchronization-requirements"></a><span data-ttu-id="8703d-103">Címtár-szinkronizálás követelmények meghatározása</span><span class="sxs-lookup"><span data-stu-id="8703d-103">Determine directory synchronization requirements</span></span>
<span data-ttu-id="8703d-104">Szinkronizálás legyen biztosítható a felhasználók a felhőben, a helyszíni identitás alapján identitást.</span><span class="sxs-lookup"><span data-stu-id="8703d-104">Synchronization is all about providing users an identity in the cloud based on their on-premises identity.</span></span> <span data-ttu-id="8703d-105">-E a hitelesítés és az összevont hitelesítés szinkronizált fiókkal fogja használják, a felhasználók továbbra is kell rendelkeznie az identitás a felhőben.</span><span class="sxs-lookup"><span data-stu-id="8703d-105">Whether or not they will use synchronized account for authentication or federated authentication, the users will still need to have an identity in the cloud.</span></span>  <span data-ttu-id="8703d-106">Ezzel az identitással kell tartani és rendszeresen frissülnek.</span><span class="sxs-lookup"><span data-stu-id="8703d-106">This identity will need to be maintained and updated periodically.</span></span>  <span data-ttu-id="8703d-107">A frissítések számos formája, a jelszó-változtatásának cím módosításai.</span><span class="sxs-lookup"><span data-stu-id="8703d-107">The updates can take many forms, from title changes to password changes.</span></span>  

<span data-ttu-id="8703d-108">Indítsa el a szervezet helyszíni identitás megoldás és a felhasználó szükséges követelmények értékelésekor.</span><span class="sxs-lookup"><span data-stu-id="8703d-108">Start by evaluating the organizations on-premises identity solution and user requirements.</span></span> <span data-ttu-id="8703d-109">Ez a kiértékelés fontos, hogy milyen felhasználói identitások létrejön és adatait a felhőben a műszaki követelményeinek meghatározását.</span><span class="sxs-lookup"><span data-stu-id="8703d-109">This evaluation is important to define the technical requirements for how user identities will be created and maintained in the cloud.</span></span>  <span data-ttu-id="8703d-110">A szervezetek többsége, az Active Directory a helyszíni és a helyszíni címtár, hogy a felhasználók által rendszer szinkronizálja a, azonban néhány esetben ez nem lesz az eset.</span><span class="sxs-lookup"><span data-stu-id="8703d-110">For a majority of organizations, Active Directory is on-premises and will be the on-premises directory that users will by synchronized from, however in some cases this will not be the case.</span></span>  

<span data-ttu-id="8703d-111">Győződjön meg arról, hogy válaszoljon a következő kérdésekre:</span><span class="sxs-lookup"><span data-stu-id="8703d-111">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="8703d-112">Rendelkezik egy AD-erdő, több vagy nincs?</span><span class="sxs-lookup"><span data-stu-id="8703d-112">Do you have one AD forest, multiple, or none?</span></span>
  
  * <span data-ttu-id="8703d-113">Hány Azure AD-címtártól fog meg kell szinkronizálja a névjegyadatokat?</span><span class="sxs-lookup"><span data-stu-id="8703d-113">How many Azure AD directories will you be synchronizing to?</span></span>
    
    1. <span data-ttu-id="8703d-114">Használ szűrés?</span><span class="sxs-lookup"><span data-stu-id="8703d-114">Are you using filtering?</span></span>
    2. <span data-ttu-id="8703d-115">Van több Azure AD Connect-kiszolgálók tervezett?</span><span class="sxs-lookup"><span data-stu-id="8703d-115">Do you have multiple Azure AD Connect servers planned?</span></span>
* <span data-ttu-id="8703d-116">Jelenleg rendelkezik a szinkronizálás a helyszíni eszköze?</span><span class="sxs-lookup"><span data-stu-id="8703d-116">Do you currently have a synchronization tool on-premises?</span></span>
  
  * <span data-ttu-id="8703d-117">Ha igen, ha a felhasználók egy virtuális könyvtárat és-integráció identitások használ a felhasználók?</span><span class="sxs-lookup"><span data-stu-id="8703d-117">If yes, does your users if users have a virtual directory/integration of identities?</span></span>
* <span data-ttu-id="8703d-118">Van bármilyen más címtár a helyszínen (pl. LDAP-címtárhoz, személyzeti adatbázis stb) szinkronizálni kívánt?</span><span class="sxs-lookup"><span data-stu-id="8703d-118">Do you have any other directory on-premises that you want to synchronize (e.g. LDAP Directory, HR database, etc)?</span></span>
  * <span data-ttu-id="8703d-119">Lesz, aki a GALSync?</span><span class="sxs-lookup"><span data-stu-id="8703d-119">Are you going to be doing any GALSync?</span></span>
  * <span data-ttu-id="8703d-120">Mi az a szervezet UPN-EK aktuális állapotát?</span><span class="sxs-lookup"><span data-stu-id="8703d-120">What is the current state of UPNs in your organization?</span></span> 
  * <span data-ttu-id="8703d-121">Van egy másik címtárat, hogy a felhasználók hitelesítése?</span><span class="sxs-lookup"><span data-stu-id="8703d-121">Do you have a different directory that users authenticate against?</span></span>
  * <span data-ttu-id="8703d-122">A vállalat használ a Microsoft Exchange?</span><span class="sxs-lookup"><span data-stu-id="8703d-122">Does your company use Microsoft Exchange?</span></span>
    * <span data-ttu-id="8703d-123">Ezek fogja, hogy az exchange hibrid telepítés?</span><span class="sxs-lookup"><span data-stu-id="8703d-123">Do they plan of having a hybrid exchange deployment?</span></span>

<span data-ttu-id="8703d-124">Most, hogy segítse az szinkronizálási követelményekről, meg kell határoznia, melyik eszközt, a megfelelő egy ezek a követelmények teljesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8703d-124">Now that you have an idea about your synchronization requirements, you need to determine which tool is the correct one to meet these requirements.</span></span>  <span data-ttu-id="8703d-125">Microsoft címtár-integráció és a szinkronizálás elvégzéséhez eszközöket biztosít.</span><span class="sxs-lookup"><span data-stu-id="8703d-125">Microsoft provides several tools to accomplish directory integration and synchronization.</span></span>  <span data-ttu-id="8703d-126">Tekintse meg a [hibrid Identity directory integration eszközök összehasonlító táblázatában](active-directory-hybrid-identity-design-considerations-tools-comparison.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="8703d-126">See the [Hybrid Identity directory integration tools comparison table](active-directory-hybrid-identity-design-considerations-tools-comparison.md) for more information.</span></span> 

<span data-ttu-id="8703d-127">Most, hogy a szinkronizálási követelmények és az eszközt, amely a rendszer ehhez a vállalat, fel kell mérnie az alkalmazásokat, amelyek a címtárszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="8703d-127">Now that you have your synchronization requirements and the tool that will accomplish this for your company, you need to evaluate the applications that use these directory services.</span></span> <span data-ttu-id="8703d-128">Ez a kiértékelés fontos, hogy ezeket az alkalmazásokat a felhőben való integrálásához műszaki követelményeinek meghatározása.</span><span class="sxs-lookup"><span data-stu-id="8703d-128">This evaluation is important to define the technical requirements to integrate these applications to the cloud.</span></span> <span data-ttu-id="8703d-129">Győződjön meg arról, hogy válaszoljon a következő kérdésekre:</span><span class="sxs-lookup"><span data-stu-id="8703d-129">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="8703d-130">Ezek az alkalmazások kerül a felhőben és a címtár?</span><span class="sxs-lookup"><span data-stu-id="8703d-130">Will these applications be moved to the cloud and use the directory?</span></span>
* <span data-ttu-id="8703d-131">Vannak-e különleges attribútumok, amelyek a felhőbe szinkronizálni kell, így ezek az alkalmazások használni tudják őket sikeresen?</span><span class="sxs-lookup"><span data-stu-id="8703d-131">Are there special attributes that need to be synchronized to the cloud so these applications can use them successfully?</span></span>
* <span data-ttu-id="8703d-132">Felhőalapú hitelesítés előnyeinek kihasználása érdekében újra írandó kell ezeket az alkalmazásokat?</span><span class="sxs-lookup"><span data-stu-id="8703d-132">Will these applications need to be re-written to take advantage of cloud auth?</span></span>
* <span data-ttu-id="8703d-133">Ezek az alkalmazások továbbra is a helyszíni élő, amíg a felhasználók férhetnek hozzá, azokat a felhőalapú identitás?</span><span class="sxs-lookup"><span data-stu-id="8703d-133">Will these applications continue to live on-premises while users access them using the cloud identity?</span></span>

<span data-ttu-id="8703d-134">Meg kell határoznia a biztonsági követelmények és korlátozások címtár-szinkronizálás is.</span><span class="sxs-lookup"><span data-stu-id="8703d-134">You also need to determine the security requirements and constraints directory synchronization.</span></span> <span data-ttu-id="8703d-135">Ez a kiértékelés fontos, hogy lesz szükség ahhoz, hogy létrehozása és karbantartása a felhő a felhasználói identitások követelmények listáját.</span><span class="sxs-lookup"><span data-stu-id="8703d-135">This evaluation is important to get a list of the requirements that will be needed in order to create and maintain user’s identities in the cloud.</span></span> <span data-ttu-id="8703d-136">Győződjön meg arról, hogy válaszoljon a következő kérdésekre:</span><span class="sxs-lookup"><span data-stu-id="8703d-136">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="8703d-137">Ha a szinkronizálási kiszolgáló kerülnek?</span><span class="sxs-lookup"><span data-stu-id="8703d-137">Where will the synchronization server be located?</span></span>
* <span data-ttu-id="8703d-138">Lesz tartományhoz csatlakoztatott?</span><span class="sxs-lookup"><span data-stu-id="8703d-138">Will it be domain joined?</span></span>
* <span data-ttu-id="8703d-139">A kiszolgáló helyezkednek el a korlátozott jogú hálózatban, például egy Szegélyhálózaton tűzfal mögött található?</span><span class="sxs-lookup"><span data-stu-id="8703d-139">Will the server be located on a restricted network behind a firewall, such as a DMZ?</span></span>
  * <span data-ttu-id="8703d-140">Meg fogja tudni a szinkronizálás támogatásához szükséges portok megnyitása?</span><span class="sxs-lookup"><span data-stu-id="8703d-140">Will you be able to open the required firewall ports to support synchronization?</span></span>
* <span data-ttu-id="8703d-141">A vész-helyreállítási terv a szinkronizálási kiszolgáló van?</span><span class="sxs-lookup"><span data-stu-id="8703d-141">Do you have a disaster recovery plan for the synchronization server?</span></span>
* <span data-ttu-id="8703d-142">Rendelkezik egy fiókot az összes erdőben megfelelő engedélyekkel a synch szeretné?</span><span class="sxs-lookup"><span data-stu-id="8703d-142">Do you have an account with the correct permissions for all forests you want to synch with?</span></span>
  * <span data-ttu-id="8703d-143">Ha a vállalat nem tudja, hogy a válasz a kérdésre, tekintse át a "Jelszó-szinkronizálás engedélyeinek" című cikk [telepítse az Azure Active Directory szinkronizáló szolgáltatás](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) és határozza meg, ha már rendelkezik ezekkel a jogosultságokkal rendelkező fiók, vagy ha kell létrehoznia egyet.</span><span class="sxs-lookup"><span data-stu-id="8703d-143">If your company doesn’t know the answer for this question, review the section “Permissions for password synchronization” in the article [Install the Azure Active Directory Sync Service](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) and determine if you already have an account with these permissions or if you need to create one.</span></span>
* <span data-ttu-id="8703d-144">Ha rendelkezik a mutli-erdő szinkronizálási kérhető le a mindkét erdőben a szinkronizálási kiszolgálót?</span><span class="sxs-lookup"><span data-stu-id="8703d-144">If you have mutli-forest sync is the sync server able to get to each forest?</span></span>

> [!NOTE]
> <span data-ttu-id="8703d-145">Ügyeljen arra, hogy minden válaszról jegyzeteket, és megismerheti a válaszok indokait.</span><span class="sxs-lookup"><span data-stu-id="8703d-145">Make sure to take notes of each answer and understand the rationale behind the answer.</span></span> <span data-ttu-id="8703d-146">[Incidensválasz-követelmények meghatározása](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) az elérhető lehetőségeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="8703d-146">[Determine incident response requirements](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) will go over the options available.</span></span> <span data-ttu-id="8703d-147">Ezen kérdések mely leginkább megfelelő lehetőséget az üzleti megválaszolása szükséges.</span><span class="sxs-lookup"><span data-stu-id="8703d-147">By having answered those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="8703d-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8703d-148">Next steps</span></span>
[<span data-ttu-id="8703d-149">A multi-factor authentication követelmények meghatározása</span><span class="sxs-lookup"><span data-stu-id="8703d-149">Determine multi-factor authentication requirements</span></span>](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a><span data-ttu-id="8703d-150">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="8703d-150">See also</span></span>
[<span data-ttu-id="8703d-151">Kialakítási szempontok áttekintése</span><span class="sxs-lookup"><span data-stu-id="8703d-151">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

