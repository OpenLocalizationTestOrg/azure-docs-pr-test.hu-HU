---
title: "aaaAzure AD Connect szinkronizálási szolgáltatás árnyékmásolat attribútumok |} Microsoft Docs"
description: "Ismerteti az árnyékmásolat attribútumok működése az Azure AD Connect szinkronizálási szolgáltatást."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 1b8665e7488c6078b655f8a3e35519145bacd898
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a><span data-ttu-id="af1a8-103">Az Azure AD Connect szinkronizálási szolgáltatás árnyékmásolat attribútumok</span><span class="sxs-lookup"><span data-stu-id="af1a8-103">Azure AD Connect sync service shadow attributes</span></span>
<span data-ttu-id="af1a8-104">A legtöbb attribútumokat hello azonos módon az Azure AD mivel ezek a helyszíni Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="af1a8-104">Most attributes are represented hello same way in Azure AD as they are in your on-premises Active Directory.</span></span> <span data-ttu-id="af1a8-105">De néhány attribútum rendelkezik néhány különleges kezelést és az Azure AD hello attribútumérték eltérhet az Azure AD Connect szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="af1a8-105">But some attributes have some special handling and hello attribute value in Azure AD might be different than what Azure AD Connect synchronizes.</span></span>

## <a name="introducing-shadow-attributes"></a><span data-ttu-id="af1a8-106">Árnyékmásolat attribútumok bemutatása</span><span class="sxs-lookup"><span data-stu-id="af1a8-106">Introducing shadow attributes</span></span>
<span data-ttu-id="af1a8-107">Egyes attribútumok két felelősséget rendelkezik az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="af1a8-107">Some attributes have two representations in Azure AD.</span></span> <span data-ttu-id="af1a8-108">Hello helyszíni érték és a számított érték tárolja.</span><span class="sxs-lookup"><span data-stu-id="af1a8-108">Both hello on-premises value and a calculated value are stored.</span></span> <span data-ttu-id="af1a8-109">A további attribútumok árnyékmásolat attribútumok nevezzük.</span><span class="sxs-lookup"><span data-stu-id="af1a8-109">These extra attributes are called shadow attributes.</span></span> <span data-ttu-id="af1a8-110">Ez a viselkedés megtapasztalhatja hello két leggyakoribb attribútumok **userPrincipalName** és **proxyAddress**.</span><span class="sxs-lookup"><span data-stu-id="af1a8-110">hello two most common attributes where you see this behavior are **userPrincipalName** and **proxyAddress**.</span></span> <span data-ttu-id="af1a8-111">Attribútumértékek hello változás történik, ha ezek az attribútumok nem ellenőrzött tartományok jelölő értékek vannak.</span><span class="sxs-lookup"><span data-stu-id="af1a8-111">hello change in attribute values happens when there are values in these attributes representing non-verified domains.</span></span> <span data-ttu-id="af1a8-112">De hello szinkronizálási motor a csatlakozás hello értéket olvasó hello árnyékmásolat attribútumban, a szempontjából, hello attribútum visszaigazolását Azure ad.</span><span class="sxs-lookup"><span data-stu-id="af1a8-112">But hello sync engine in Connect reads hello value in hello shadow attribute so from its perspective, hello attribute has been confirmed by Azure AD.</span></span>

<span data-ttu-id="af1a8-113">Nem található hello árnyékmásolat attribútumok hello Azure portál vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="af1a8-113">You cannot see hello shadow attributes using hello Azure portal or with PowerShell.</span></span> <span data-ttu-id="af1a8-114">De ismertetése hello koncepció segít meg tootroubleshoot bizonyos esetekben ha hello attribútum különböző érték is a helyszíni és hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="af1a8-114">But understanding hello concept helps you tootroubleshoot certain scenarios where hello attribute has different values on-premises and in hello cloud.</span></span>

<span data-ttu-id="af1a8-115">toobetter hello viselkedésének megértése, nézze meg ebben a példában a Fabrikam:</span><span class="sxs-lookup"><span data-stu-id="af1a8-115">toobetter understand hello behavior, look at this example from Fabrikam:</span></span>  
![Tartományok](./media/active-directory-aadconnectsyncservice-shadow-attributes/domains.png)  
<span data-ttu-id="af1a8-117">A helyszíni Active Directoryban több egyszerű Felhasználónévi utótagot rendelkeznek, de azok csak ellenőrzését egy.</span><span class="sxs-lookup"><span data-stu-id="af1a8-117">They have multiple UPN suffixes in their on-premises Active Directory, but they have only verified one.</span></span>

### <a name="userprincipalname"></a><span data-ttu-id="af1a8-118">UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="af1a8-118">userPrincipalName</span></span>
<span data-ttu-id="af1a8-119">A felhasználó rendelkezik-e a következő attribútum értékei nem ellenőrzött tartomány hello:</span><span class="sxs-lookup"><span data-stu-id="af1a8-119">A user has hello following attribute values in a non-verified domain:</span></span>

| <span data-ttu-id="af1a8-120">Attribútum</span><span class="sxs-lookup"><span data-stu-id="af1a8-120">Attribute</span></span> | <span data-ttu-id="af1a8-121">Érték</span><span class="sxs-lookup"><span data-stu-id="af1a8-121">Value</span></span> |
| --- | --- |
| <span data-ttu-id="af1a8-122">a helyszíni userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="af1a8-122">on-premises userPrincipalName</span></span> | lee.sperry@fabrikam.com |
| <span data-ttu-id="af1a8-123">Az Azure AD-shadowUserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="af1a8-123">Azure AD shadowUserPrincipalName</span></span> | lee.sperry@fabrikam.com |
| <span data-ttu-id="af1a8-124">Az Azure AD userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="af1a8-124">Azure AD userPrincipalName</span></span> | lee.sperry@fabrikam.onmicrosoft.com |

<span data-ttu-id="af1a8-125">hello userPrincipalName attribútum értéke hello megjelenik, amikor a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="af1a8-125">hello userPrincipalName attribute is hello value you see when using PowerShell.</span></span>

<span data-ttu-id="af1a8-126">Mivel a hello valós helyszíni attribútumérték rendszer az Azure AD, ha ellenőrizte, fabrikam.com tartomány hello, az Azure AD hello userPrincipalName attribútum hello shadowUserPrincipalName hello értéket frissíti.</span><span class="sxs-lookup"><span data-stu-id="af1a8-126">Since hello real on-premises attribute value is stored in Azure AD, when you verify hello fabrikam.com domain, Azure AD updates hello userPrincipalName attribute with hello value from hello shadowUserPrincipalName.</span></span> <span data-ttu-id="af1a8-127">Nincs toosynchronize az Azure AD Connect ezen értékek toobe frissítése a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="af1a8-127">You do not have toosynchronize any changes from Azure AD Connect for these values toobe updated.</span></span>

### <a name="proxyaddresses"></a><span data-ttu-id="af1a8-128">proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="af1a8-128">proxyAddresses</span></span>
<span data-ttu-id="af1a8-129">hello azonos folyamata csak beleértve az ellenőrzött tartományok is következik be, a proxyAddresses, de néhány további logikát.</span><span class="sxs-lookup"><span data-stu-id="af1a8-129">hello same process for only including verified domains also occurs for proxyAddresses, but with some additional logic.</span></span> <span data-ttu-id="af1a8-130">az ellenőrzött tartományok hello ellenőrzése csak a postaláda felhasználóknak zavartalan.</span><span class="sxs-lookup"><span data-stu-id="af1a8-130">hello check for verified domains only happens for mailbox users.</span></span> <span data-ttu-id="af1a8-131">Egy levelezési felhasználó vagy az ügyfél felel meg a felhasználó Exchange-szervezet egy másik, és proxyAddresses toothese objektumok minden olyan értéket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="af1a8-131">A mail-enabled user or contact represent a user in another Exchange organization and you can add any values in proxyAddresses toothese objects.</span></span>

<span data-ttu-id="af1a8-132">Postaláda-felhasználónak, a helyszíni vagy az Exchange Online esetén csak az ellenőrzött tartományok értékek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="af1a8-132">For a mailbox user, either on-premises or in Exchange Online, only values for verified domains appear.</span></span> <span data-ttu-id="af1a8-133">Az nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="af1a8-133">It could look like this:</span></span>

| <span data-ttu-id="af1a8-134">Attribútum</span><span class="sxs-lookup"><span data-stu-id="af1a8-134">Attribute</span></span> | <span data-ttu-id="af1a8-135">Érték</span><span class="sxs-lookup"><span data-stu-id="af1a8-135">Value</span></span> |
| --- | --- |
| <span data-ttu-id="af1a8-136">a helyszíni proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="af1a8-136">on-premises proxyAddresses</span></span> | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| <span data-ttu-id="af1a8-137">Exchange Online proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="af1a8-137">Exchange Online proxyAddresses</span></span> | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

<span data-ttu-id="af1a8-138">Ebben az esetben  **smtp:abbie.spencer@fabrikam.com**  el lett távolítva, mert az adott tartomány nincs ellenőrizve.</span><span class="sxs-lookup"><span data-stu-id="af1a8-138">In this case **smtp:abbie.spencer@fabrikam.com** was removed since that domain has not been verified.</span></span> <span data-ttu-id="af1a8-139">De adott Exchange  **SIP:abbie.spencer@fabrikamonline.com** . Fabrikam nem használt Lync/Skype a helyszínen, de az Azure AD és az Exchange Online előkészítése.</span><span class="sxs-lookup"><span data-stu-id="af1a8-139">But Exchange also added **SIP:abbie.spencer@fabrikamonline.com**. Fabrikam has not used Lync/Skype on-premises, but Azure AD and Exchange Online prepare for it.</span></span>

<span data-ttu-id="af1a8-140">Ez a módszer a proxyAddresses hivatkozott tooas **ProxyCalc**.</span><span class="sxs-lookup"><span data-stu-id="af1a8-140">This logic for proxyAddresses is referred tooas **ProxyCalc**.</span></span> <span data-ttu-id="af1a8-141">ProxyCalc minden változás, a felhasználó meghívása során:</span><span class="sxs-lookup"><span data-stu-id="af1a8-141">ProxyCalc is invoked with every change on a user when:</span></span>

- <span data-ttu-id="af1a8-142">hello felhasználói azonosítót, amely tartalmazza az Exchange Online, még akkor is, ha hello felhasználó nem rendelkezik licenccel az Exchange service-csomag.</span><span class="sxs-lookup"><span data-stu-id="af1a8-142">hello user has been assigned a service plan that includes Exchange Online even if hello user was not licensed for Exchange.</span></span> <span data-ttu-id="af1a8-143">Például ha hello felhasználó hozzá van rendelve hello Office E3 SKU, de csak SharePoint online-hoz lett rendelve.</span><span class="sxs-lookup"><span data-stu-id="af1a8-143">For example, if hello user is assigned hello Office E3 SKU, but only was assigned SharePoint Online.</span></span> <span data-ttu-id="af1a8-144">Ez igaz, akkor is, ha a postaláda továbbra is a helyszíni.</span><span class="sxs-lookup"><span data-stu-id="af1a8-144">This is true even if your mailbox is still on-premises.</span></span>
- <span data-ttu-id="af1a8-145">hello attribútum msExchRecipientTypeDetails értéke.</span><span class="sxs-lookup"><span data-stu-id="af1a8-145">hello attribute msExchRecipientTypeDetails has a value.</span></span>
- <span data-ttu-id="af1a8-146">A módosítás tooproxyAddresses vagy a userPrincipalName elvégezte.</span><span class="sxs-lookup"><span data-stu-id="af1a8-146">You make a change tooproxyAddresses or userPrincipalName.</span></span>

<span data-ttu-id="af1a8-147">ProxyCalc is igénybe vehet néhány alkalommal tooprocess módosítását a felhasználó, ezért nem szinkron hello Azure AD Connect exportálási folyamat során.</span><span class="sxs-lookup"><span data-stu-id="af1a8-147">ProxyCalc might take some time tooprocess a change on a user and is not synchronous with hello Azure AD Connect export process.</span></span>

> [!NOTE]
> <span data-ttu-id="af1a8-148">hello ProxyCalc logika van néhány további eszközöket nem ebben a témakörben leírt speciális forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="af1a8-148">hello ProxyCalc logic has some additional behaviors for advanced scenarios not documented in this topic.</span></span> <span data-ttu-id="af1a8-149">Ez a témakör kerül meg toounderstand hello viselkedését, és nem az összes belső logikai dokumentum.</span><span class="sxs-lookup"><span data-stu-id="af1a8-149">This topic is provided for you toounderstand hello behavior and not document all internal logic.</span></span>

### <a name="quarantined-attribute-values"></a><span data-ttu-id="af1a8-150">Karanténba helyezett attribútumértékek</span><span class="sxs-lookup"><span data-stu-id="af1a8-150">Quarantined attribute values</span></span>
<span data-ttu-id="af1a8-151">Árnyékmásolat attribútumok is használják, amikor ismétlődő attribútum-érték.</span><span class="sxs-lookup"><span data-stu-id="af1a8-151">Shadow attributes are also used when there are duplicate attribute values.</span></span> <span data-ttu-id="af1a8-152">További információkért lásd: [ismétlődő attribútum rugalmassági](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span><span class="sxs-lookup"><span data-stu-id="af1a8-152">For more information, see [duplicate attribute resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="af1a8-153">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="af1a8-153">See also</span></span>
* [<span data-ttu-id="af1a8-154">Az Azure AD Connect szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="af1a8-154">Azure AD Connect sync</span></span>](active-directory-aadconnectsync-whatis.md)
* <span data-ttu-id="af1a8-155">[A helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="af1a8-155">[Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
