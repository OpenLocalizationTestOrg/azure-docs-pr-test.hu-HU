---
title: "Az Azure AD Connect szinkronizálási szolgáltatás árnyékmásolat attribútumok |} Microsoft Docs"
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
ms.openlocfilehash: 0b6a7f22d744480a40a878c979986cdd7667109c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a><span data-ttu-id="9a565-103">Az Azure AD Connect szinkronizálási szolgáltatás árnyékmásolat attribútumok</span><span class="sxs-lookup"><span data-stu-id="9a565-103">Azure AD Connect sync service shadow attributes</span></span>
<span data-ttu-id="9a565-104">A legtöbb attribútumok vannak megadva ugyanúgy Azure AD-ben, mivel ezek a helyszíni Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="9a565-104">Most attributes are represented the same way in Azure AD as they are in your on-premises Active Directory.</span></span> <span data-ttu-id="9a565-105">Azonban néhány attribútum néhány különleges kezelést és az Azure AD-ben attribútumérték eltérhet az Azure AD Connect szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="9a565-105">But some attributes have some special handling and the attribute value in Azure AD might be different than what Azure AD Connect synchronizes.</span></span>

## <a name="introducing-shadow-attributes"></a><span data-ttu-id="9a565-106">Árnyékmásolat attribútumok bemutatása</span><span class="sxs-lookup"><span data-stu-id="9a565-106">Introducing shadow attributes</span></span>
<span data-ttu-id="9a565-107">Egyes attribútumok két felelősséget rendelkezik az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="9a565-107">Some attributes have two representations in Azure AD.</span></span> <span data-ttu-id="9a565-108">A helyszíni érték és a számított érték tárolja.</span><span class="sxs-lookup"><span data-stu-id="9a565-108">Both the on-premises value and a calculated value are stored.</span></span> <span data-ttu-id="9a565-109">A további attribútumok árnyékmásolat attribútumok nevezzük.</span><span class="sxs-lookup"><span data-stu-id="9a565-109">These extra attributes are called shadow attributes.</span></span> <span data-ttu-id="9a565-110">A két leggyakoribb attribútumok, ahol megjelenik ez a viselkedés **userPrincipalName** és **proxyAddress**.</span><span class="sxs-lookup"><span data-stu-id="9a565-110">The two most common attributes where you see this behavior are **userPrincipalName** and **proxyAddress**.</span></span> <span data-ttu-id="9a565-111">Az attribútum értékei módosítása történik, ha ezek az attribútumok nem ellenőrzött tartományok jelölő értékek vannak.</span><span class="sxs-lookup"><span data-stu-id="9a565-111">The change in attribute values happens when there are values in these attributes representing non-verified domains.</span></span> <span data-ttu-id="9a565-112">De a csatlakozás a szinkronizálási motor beolvassa az árnyék attribútum értéke, a szempontjából, az attribútum visszaigazolását Azure ad.</span><span class="sxs-lookup"><span data-stu-id="9a565-112">But the sync engine in Connect reads the value in the shadow attribute so from its perspective, the attribute has been confirmed by Azure AD.</span></span>

<span data-ttu-id="9a565-113">Nem található az árnyékmásolat-attribútumok az Azure portál vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="9a565-113">You cannot see the shadow attributes using the Azure portal or with PowerShell.</span></span> <span data-ttu-id="9a565-114">De a koncepció ismertetése segít bizonyos elhárításában, ha az attribútum helyszíni eltérő értékek tartoznak, és a felhőben.</span><span class="sxs-lookup"><span data-stu-id="9a565-114">But understanding the concept helps you to troubleshoot certain scenarios where the attribute has different values on-premises and in the cloud.</span></span>

<span data-ttu-id="9a565-115">Jobb megértése érdekében működését, tekintse meg ebben a példában a Fabrikam:</span><span class="sxs-lookup"><span data-stu-id="9a565-115">To better understand the behavior, look at this example from Fabrikam:</span></span>  
![Tartományok](./media/active-directory-aadconnectsyncservice-shadow-attributes/domains.png)  
<span data-ttu-id="9a565-117">A helyszíni Active Directoryban több egyszerű Felhasználónévi utótagot rendelkeznek, de azok csak ellenőrzését egy.</span><span class="sxs-lookup"><span data-stu-id="9a565-117">They have multiple UPN suffixes in their on-premises Active Directory, but they have only verified one.</span></span>

### <a name="userprincipalname"></a><span data-ttu-id="9a565-118">UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="9a565-118">userPrincipalName</span></span>
<span data-ttu-id="9a565-119">A felhasználó a következő attribútumértékeit a nem ellenőrzött tartomány van:</span><span class="sxs-lookup"><span data-stu-id="9a565-119">A user has the following attribute values in a non-verified domain:</span></span>

| <span data-ttu-id="9a565-120">Attribútum</span><span class="sxs-lookup"><span data-stu-id="9a565-120">Attribute</span></span> | <span data-ttu-id="9a565-121">Érték</span><span class="sxs-lookup"><span data-stu-id="9a565-121">Value</span></span> |
| --- | --- |
| <span data-ttu-id="9a565-122">a helyszíni userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="9a565-122">on-premises userPrincipalName</span></span> | lee.sperry@fabrikam.com |
| <span data-ttu-id="9a565-123">Az Azure AD-shadowUserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="9a565-123">Azure AD shadowUserPrincipalName</span></span> | lee.sperry@fabrikam.com |
| <span data-ttu-id="9a565-124">Az Azure AD userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="9a565-124">Azure AD userPrincipalName</span></span> | lee.sperry@fabrikam.onmicrosoft.com |

<span data-ttu-id="9a565-125">A userPrincipalName attribútum a értéke megjelenik, amikor a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="9a565-125">The userPrincipalName attribute is the value you see when using PowerShell.</span></span>

<span data-ttu-id="9a565-126">Mivel a tényleges helyszíni attribútumérték rendszer az Azure AD, ha ellenőrizte, fabrikam.com tartomány, az Azure AD frissíti a userPrincipalName attribútum a shadowUserPrincipalName értékével.</span><span class="sxs-lookup"><span data-stu-id="9a565-126">Since the real on-premises attribute value is stored in Azure AD, when you verify the fabrikam.com domain, Azure AD updates the userPrincipalName attribute with the value from the shadowUserPrincipalName.</span></span> <span data-ttu-id="9a565-127">Nincs a módosításokat az frissítenie kell ezeket az értékeket az Azure AD Connect szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="9a565-127">You do not have to synchronize any changes from Azure AD Connect for these values to be updated.</span></span>

### <a name="proxyaddresses"></a><span data-ttu-id="9a565-128">proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="9a565-128">proxyAddresses</span></span>
<span data-ttu-id="9a565-129">Ugyanezt az eljárást csak beleértve az ellenőrzött tartományok esetében is a proxyAddresses, de néhány további logikával következik be.</span><span class="sxs-lookup"><span data-stu-id="9a565-129">The same process for only including verified domains also occurs for proxyAddresses, but with some additional logic.</span></span> <span data-ttu-id="9a565-130">Az ellenőrzött tartományok jelölőnégyzet csak akkor zajlik le postaláda-felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="9a565-130">The check for verified domains only happens for mailbox users.</span></span> <span data-ttu-id="9a565-131">Egy levelezési felhasználó vagy az ügyfél felel meg a felhasználó egy másik Exchange-szervezetben, és adhat hozzá az értékeket a proxyAddresses ezeket az objektumokat.</span><span class="sxs-lookup"><span data-stu-id="9a565-131">A mail-enabled user or contact represent a user in another Exchange organization and you can add any values in proxyAddresses to these objects.</span></span>

<span data-ttu-id="9a565-132">Postaláda-felhasználónak, a helyszíni vagy az Exchange Online esetén csak az ellenőrzött tartományok értékek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="9a565-132">For a mailbox user, either on-premises or in Exchange Online, only values for verified domains appear.</span></span> <span data-ttu-id="9a565-133">Az nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="9a565-133">It could look like this:</span></span>

| <span data-ttu-id="9a565-134">Attribútum</span><span class="sxs-lookup"><span data-stu-id="9a565-134">Attribute</span></span> | <span data-ttu-id="9a565-135">Érték</span><span class="sxs-lookup"><span data-stu-id="9a565-135">Value</span></span> |
| --- | --- |
| <span data-ttu-id="9a565-136">a helyszíni proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="9a565-136">on-premises proxyAddresses</span></span> | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| <span data-ttu-id="9a565-137">Exchange Online proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="9a565-137">Exchange Online proxyAddresses</span></span> | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

<span data-ttu-id="9a565-138">Ebben az esetben  **smtp:abbie.spencer@fabrikam.com**  el lett távolítva, mert az adott tartomány nincs ellenőrizve.</span><span class="sxs-lookup"><span data-stu-id="9a565-138">In this case **smtp:abbie.spencer@fabrikam.com** was removed since that domain has not been verified.</span></span> <span data-ttu-id="9a565-139">De adott Exchange  **SIP:abbie.spencer@fabrikamonline.com** .</span><span class="sxs-lookup"><span data-stu-id="9a565-139">But Exchange also added **SIP:abbie.spencer@fabrikamonline.com**.</span></span> <span data-ttu-id="9a565-140">Fabrikam nem használt Lync/Skype a helyszínen, de az Azure AD és az Exchange Online előkészítése.</span><span class="sxs-lookup"><span data-stu-id="9a565-140">Fabrikam has not used Lync/Skype on-premises, but Azure AD and Exchange Online prepare for it.</span></span>

<span data-ttu-id="9a565-141">Ez a módszer a proxyAddresses nevezzük **ProxyCalc**.</span><span class="sxs-lookup"><span data-stu-id="9a565-141">This logic for proxyAddresses is referred to as **ProxyCalc**.</span></span> <span data-ttu-id="9a565-142">ProxyCalc minden változás, a felhasználó meghívása során:</span><span class="sxs-lookup"><span data-stu-id="9a565-142">ProxyCalc is invoked with every change on a user when:</span></span>

- <span data-ttu-id="9a565-143">A felhasználói azonosítót, amely tartalmazza az Exchange Online, még akkor is, ha a felhasználó nem rendelkezik licenccel az Exchange service-csomag.</span><span class="sxs-lookup"><span data-stu-id="9a565-143">The user has been assigned a service plan that includes Exchange Online even if the user was not licensed for Exchange.</span></span> <span data-ttu-id="9a565-144">Ha például a felhasználó hozzá van rendelve a Office E3 SKU, de csak hozzá volt rendelve a SharePoint Online.</span><span class="sxs-lookup"><span data-stu-id="9a565-144">For example, if the user is assigned the Office E3 SKU, but only was assigned SharePoint Online.</span></span> <span data-ttu-id="9a565-145">Ez igaz, akkor is, ha a postaláda továbbra is a helyszíni.</span><span class="sxs-lookup"><span data-stu-id="9a565-145">This is true even if your mailbox is still on-premises.</span></span>
- <span data-ttu-id="9a565-146">Az attribútum msExchRecipientTypeDetails értéke.</span><span class="sxs-lookup"><span data-stu-id="9a565-146">The attribute msExchRecipientTypeDetails has a value.</span></span>
- <span data-ttu-id="9a565-147">Módosítja a proxyAddresses vagy a userPrincipalName.</span><span class="sxs-lookup"><span data-stu-id="9a565-147">You make a change to proxyAddresses or userPrincipalName.</span></span>

<span data-ttu-id="9a565-148">ProxyCalc eltarthat egy ideig, a felhasználó változás feldolgozni, és nem az Azure AD Connect exportálási folyamat szinkronban.</span><span class="sxs-lookup"><span data-stu-id="9a565-148">ProxyCalc might take some time to process a change on a user and is not synchronous with the Azure AD Connect export process.</span></span>

> [!NOTE]
> <span data-ttu-id="9a565-149">A ProxyCalc logika van néhány további eszközöket nem ebben a témakörben leírt speciális forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="9a565-149">The ProxyCalc logic has some additional behaviors for advanced scenarios not documented in this topic.</span></span> <span data-ttu-id="9a565-150">Ez a témakör átláthatja viselkedését, és nem az összes belső logikai dokumentum valósul meg.</span><span class="sxs-lookup"><span data-stu-id="9a565-150">This topic is provided for you to understand the behavior and not document all internal logic.</span></span>

### <a name="quarantined-attribute-values"></a><span data-ttu-id="9a565-151">Karanténba helyezett attribútumértékek</span><span class="sxs-lookup"><span data-stu-id="9a565-151">Quarantined attribute values</span></span>
<span data-ttu-id="9a565-152">Árnyékmásolat attribútumok is használják, amikor ismétlődő attribútum-érték.</span><span class="sxs-lookup"><span data-stu-id="9a565-152">Shadow attributes are also used when there are duplicate attribute values.</span></span> <span data-ttu-id="9a565-153">További információkért lásd: [ismétlődő attribútum rugalmassági](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span><span class="sxs-lookup"><span data-stu-id="9a565-153">For more information, see [duplicate attribute resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="9a565-154">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="9a565-154">See also</span></span>
* [<span data-ttu-id="9a565-155">Az Azure AD Connect szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="9a565-155">Azure AD Connect sync</span></span>](active-directory-aadconnectsync-whatis.md)
* <span data-ttu-id="9a565-156">[A helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="9a565-156">[Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
