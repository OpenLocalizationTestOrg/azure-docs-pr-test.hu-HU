---
title: "Az Azure AD Connect: Deklaratív kiépítés kifejezéseinek |} Microsoft Docs"
description: "A deklaratív kiépítés kifejezéseinek ismerteti."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: e3ea53c8-3801-4acf-a297-0fb9bb1bf11d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: e3a03a97b10e04fb85261620879b2102e1db8465
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a><span data-ttu-id="970ef-103">Azure AD Connect szinkronizálása: deklaratív kiépítés kifejezések ismertetése</span><span class="sxs-lookup"><span data-stu-id="970ef-103">Azure AD Connect sync: Understanding Declarative Provisioning Expressions</span></span>
<span data-ttu-id="970ef-104">Azure AD Connect szinkronizálása épít deklaratív kiépítés először a Forefront Identity Manager 2010 verzióban jelent.</span><span class="sxs-lookup"><span data-stu-id="970ef-104">Azure AD Connect sync builds on declarative provisioning first introduced in Forefront Identity Manager 2010.</span></span> <span data-ttu-id="970ef-105">Lehetővé teszi a teljes identity integration üzleti logika lefordított kód írása nélkül végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="970ef-105">It allows you to implement your complete identity integration business logic without the need to write compiled code.</span></span>

<span data-ttu-id="970ef-106">Nagyon fontos részét képezik deklaratív kiépítés megegyezik kifejezés attribútumfolyamok szerepel.</span><span class="sxs-lookup"><span data-stu-id="970ef-106">An essential part of declarative provisioning is the expression language used in attribute flows.</span></span> <span data-ttu-id="970ef-107">A használt nyelv része a Microsoft® Visual Basic® Applications (VBA).</span><span class="sxs-lookup"><span data-stu-id="970ef-107">The language used is a subset of Microsoft® Visual Basic® for Applications (VBA).</span></span> <span data-ttu-id="970ef-108">Ezen a nyelven a Microsoft Office szolgál, és a VBScript élménye rendelkező felhasználók is felismeri azt.</span><span class="sxs-lookup"><span data-stu-id="970ef-108">This language is used in Microsoft Office and users with experience of VBScript will also recognize it.</span></span> <span data-ttu-id="970ef-109">A deklaratív kiépítés kifejezés nyelvi csak funkciókat használ, és nem strukturált nyelvet.</span><span class="sxs-lookup"><span data-stu-id="970ef-109">The Declarative Provisioning Expression Language is only using functions and is not a structured language.</span></span> <span data-ttu-id="970ef-110">Nincsenek módszerek vagy utasításokat.</span><span class="sxs-lookup"><span data-stu-id="970ef-110">There are no methods or statements.</span></span> <span data-ttu-id="970ef-111">Funkciók helyette beágyazott express program folyamokhoz.</span><span class="sxs-lookup"><span data-stu-id="970ef-111">Functions are instead nested to express program flow.</span></span>

<span data-ttu-id="970ef-112">További részletekért lásd: [üdvözli a Visual Basic for Applications nyelvi referencia az Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).</span><span class="sxs-lookup"><span data-stu-id="970ef-112">For more details, see [Welcome to the Visual Basic for Applications language reference for Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).</span></span>

<span data-ttu-id="970ef-113">Az attribútumok vannak erős típusmegadású.</span><span class="sxs-lookup"><span data-stu-id="970ef-113">The attributes are strongly typed.</span></span> <span data-ttu-id="970ef-114">A függvény csak a megfelelő típusú attribútumokat fogad el.</span><span class="sxs-lookup"><span data-stu-id="970ef-114">A function only accepts attributes of the correct type.</span></span> <span data-ttu-id="970ef-115">Egyúttal kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="970ef-115">It is also case-sensitive.</span></span> <span data-ttu-id="970ef-116">Mind a nevét, és az attribútumok nevében kell rendelkeznie a megfelelő kis-és nagybetűk, vagy hiba történt.</span><span class="sxs-lookup"><span data-stu-id="970ef-116">Both function names and attribute names must have proper casing or an error is thrown.</span></span>

## <a name="language-definitions-and-identifiers"></a><span data-ttu-id="970ef-117">Nyelvi definíciókat és -azonosítók</span><span class="sxs-lookup"><span data-stu-id="970ef-117">Language definitions and Identifiers</span></span>
* <span data-ttu-id="970ef-118">Funkciók argumentumokat szögletes zárójelbe követ nevet adni: (1 argumentum, N argumentum) függvénynév.</span><span class="sxs-lookup"><span data-stu-id="970ef-118">Functions have a name followed by arguments in brackets: FunctionName(argument 1, argument N).</span></span>
* <span data-ttu-id="970ef-119">Attribútumok szögletes zárójelbe azonosítja: [attributeName]</span><span class="sxs-lookup"><span data-stu-id="970ef-119">Attributes are identified by square brackets: [attributeName]</span></span>
* <span data-ttu-id="970ef-120">Paraméterek százalékjelek azonosítja: % ParameterName %</span><span class="sxs-lookup"><span data-stu-id="970ef-120">Parameters are identified by percent signs: %ParameterName%</span></span>
* <span data-ttu-id="970ef-121">A karakterlánckonstansokat ajánlatok tette: például "Contoso" (Megjegyzés: Egyenes idézőjel kell használnia. "", illetve nem idézőjelek között "")</span><span class="sxs-lookup"><span data-stu-id="970ef-121">String constants are surrounded by quotes: For example, "Contoso" (Note: must use straight quotes "" and not smart quotes “”)</span></span>
* <span data-ttu-id="970ef-122">Numerikus értékek nélkül idézőjelek között kifejezett és várhatóan decimális.</span><span class="sxs-lookup"><span data-stu-id="970ef-122">Numeric values are expressed without quotes and expected to be decimal.</span></span> <span data-ttu-id="970ef-123">Hexadecimális értékek fűzve előtagként & H.</span><span class="sxs-lookup"><span data-stu-id="970ef-123">Hexadecimal values are prefixed with &H.</span></span> <span data-ttu-id="970ef-124">Például a 98052 & HFF</span><span class="sxs-lookup"><span data-stu-id="970ef-124">For example, 98052, &HFF</span></span>
* <span data-ttu-id="970ef-125">Logikai értékek szerint van megadva, az állandókat: True, False.</span><span class="sxs-lookup"><span data-stu-id="970ef-125">Boolean values are expressed with constants: True, False.</span></span>
* <span data-ttu-id="970ef-126">Beépített állandók és literálok szerint van megadva. csak a nevükkel: NULL, CRLF, IgnoreThisFlow</span><span class="sxs-lookup"><span data-stu-id="970ef-126">Built-in constants and literals are expressed with only their name: NULL, CRLF, IgnoreThisFlow</span></span>

### <a name="functions"></a><span data-ttu-id="970ef-127">Functions</span><span class="sxs-lookup"><span data-stu-id="970ef-127">Functions</span></span>
<span data-ttu-id="970ef-128">Deklaratív kiépítés használja sok funkciók engedélyezéséhez attribútumértékek átalakítás lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="970ef-128">Declarative provisioning uses many functions to enable the possibility to transform attribute values.</span></span> <span data-ttu-id="970ef-129">Ezek a funkciók egymásba, az eredmény egy függvényből másik függvény kerül átadásra.</span><span class="sxs-lookup"><span data-stu-id="970ef-129">These functions can be nested so the result from one function is passed in to another function.</span></span>

`Function1(Function2(Function3()))`

<span data-ttu-id="970ef-130">A funkciók teljes listája megtalálható a [hivatkozás működéséhez](active-directory-aadconnectsync-functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="970ef-130">The complete list of functions can be found in the [function reference](active-directory-aadconnectsync-functions-reference.md).</span></span>

### <a name="parameters"></a><span data-ttu-id="970ef-131">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="970ef-131">Parameters</span></span>
<span data-ttu-id="970ef-132">A paraméter PowerShell használó rendszergazda vagy egy összekötő van definiálva.</span><span class="sxs-lookup"><span data-stu-id="970ef-132">A parameter is defined either by a Connector or by an administrator using PowerShell.</span></span> <span data-ttu-id="970ef-133">Paraméterek általában tartalmaznak, amelyek különböző operációs rendszerek közötti értékek, például található, a tartomány a felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="970ef-133">Parameters usually contain values that are different from system to system, for example the name of the domain the user is located in.</span></span> <span data-ttu-id="970ef-134">Ezek a paraméterek attribútumfolyamok használható.</span><span class="sxs-lookup"><span data-stu-id="970ef-134">These parameters can be used in attribute flows.</span></span>

<span data-ttu-id="970ef-135">Az Active Directory-összekötő előírt bejövő szinkronizálási szabályok a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="970ef-135">The Active Directory Connector provided the following parameters for inbound Synchronization Rules:</span></span>

| <span data-ttu-id="970ef-136">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="970ef-136">Parameter Name</span></span> | <span data-ttu-id="970ef-137">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="970ef-137">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="970ef-138">Domain.Netbios</span><span class="sxs-lookup"><span data-stu-id="970ef-138">Domain.Netbios</span></span> |<span data-ttu-id="970ef-139">A tartomány jelenleg importált, például FABRIKAMSALES NetBIOS formátuma</span><span class="sxs-lookup"><span data-stu-id="970ef-139">Netbios format of the domain currently being imported, for example FABRIKAMSALES</span></span> |
| <span data-ttu-id="970ef-140">Domain.FQDN</span><span class="sxs-lookup"><span data-stu-id="970ef-140">Domain.FQDN</span></span> |<span data-ttu-id="970ef-141">A tartomány jelenleg importált, például sales.fabrikam.com FQDN-formátumban</span><span class="sxs-lookup"><span data-stu-id="970ef-141">FQDN format of the domain currently being imported, for example sales.fabrikam.com</span></span> |
| <span data-ttu-id="970ef-142">Domain.LDAP</span><span class="sxs-lookup"><span data-stu-id="970ef-142">Domain.LDAP</span></span> |<span data-ttu-id="970ef-143">LDAP-formátum jelenleg importált, a tartomány például DC értékesítés, DC = fabrikam, DC = = com</span><span class="sxs-lookup"><span data-stu-id="970ef-143">LDAP format of the domain currently being imported, for example DC=sales,DC=fabrikam,DC=com</span></span> |
| <span data-ttu-id="970ef-144">Forest.Netbios</span><span class="sxs-lookup"><span data-stu-id="970ef-144">Forest.Netbios</span></span> |<span data-ttu-id="970ef-145">Az erdő nevének jelenleg importált, például FABRIKAMCORP NetBIOS formátuma</span><span class="sxs-lookup"><span data-stu-id="970ef-145">Netbios format of the forest name currently being imported, for example FABRIKAMCORP</span></span> |
| <span data-ttu-id="970ef-146">Forest.FQDN</span><span class="sxs-lookup"><span data-stu-id="970ef-146">Forest.FQDN</span></span> |<span data-ttu-id="970ef-147">Az erdő nevének jelenleg importált, fabrikam.com például az FQDN-formátumban</span><span class="sxs-lookup"><span data-stu-id="970ef-147">FQDN format of the forest name currently being imported, for example fabrikam.com</span></span> |
| <span data-ttu-id="970ef-148">Forest.LDAP</span><span class="sxs-lookup"><span data-stu-id="970ef-148">Forest.LDAP</span></span> |<span data-ttu-id="970ef-149">LDAP formátuma az erdő nevének jelenleg importált, például: DC = fabrikam, DC = com</span><span class="sxs-lookup"><span data-stu-id="970ef-149">LDAP format of the forest name currently being imported, for example DC=fabrikam,DC=com</span></span> |

<span data-ttu-id="970ef-150">A rendszer biztosítja a következő paraméter, amely lekérni a folyamatban az összekötő azonosítója:</span><span class="sxs-lookup"><span data-stu-id="970ef-150">The system provides the following parameter, which is used to get the identifier of the Connector currently running:</span></span>  
`Connector.ID`

<span data-ttu-id="970ef-151">Íme egy példa a metaverse attribútum tartomány a felhasználó a tartomány netbios-nevét feltöltő:</span><span class="sxs-lookup"><span data-stu-id="970ef-151">Here is an example that populates the metaverse attribute domain with the netbios name of the domain where the user is located:</span></span>  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a><span data-ttu-id="970ef-152">Operátorok</span><span class="sxs-lookup"><span data-stu-id="970ef-152">Operators</span></span>
<span data-ttu-id="970ef-153">Az alábbi operátorok használhatók:</span><span class="sxs-lookup"><span data-stu-id="970ef-153">The following operators can be used:</span></span>

* <span data-ttu-id="970ef-154">**Összehasonlítás**: <, < =, <>, =, >, > =</span><span class="sxs-lookup"><span data-stu-id="970ef-154">**Comparison**: <, <=, <>, =, >, >=</span></span>
* <span data-ttu-id="970ef-155">**Matematikai**: +, -, \*, -</span><span class="sxs-lookup"><span data-stu-id="970ef-155">**Mathematics**: +, -, \*, -</span></span>
* <span data-ttu-id="970ef-156">**Karakterlánc**: & (ÖSSZEFŰZ)</span><span class="sxs-lookup"><span data-stu-id="970ef-156">**String**: & (concatenate)</span></span>
* <span data-ttu-id="970ef-157">**Logikai**: & & (és). (vagy)</span><span class="sxs-lookup"><span data-stu-id="970ef-157">**Logical**: && (and), || (or)</span></span>
* <span data-ttu-id="970ef-158">**Kiértékelési sorrend**:)</span><span class="sxs-lookup"><span data-stu-id="970ef-158">**Evaluation order**: ( )</span></span>

<span data-ttu-id="970ef-159">Operátorok értékeli ki a rendszer balról jobbra, és értékelési azonos prioritású.</span><span class="sxs-lookup"><span data-stu-id="970ef-159">Operators are evaluated left to right and have the same evaluation priority.</span></span> <span data-ttu-id="970ef-160">Ez azt jelenti, hogy a \* (szorzója) nem kerül kiértékelésre, mielőtt - (kivonás).</span><span class="sxs-lookup"><span data-stu-id="970ef-160">That is, the \* (multiplier) is not evaluated before - (subtraction).</span></span> <span data-ttu-id="970ef-161">2\*(5 + 3) célja nem ugyanaz, mint 2\*5 + 3.</span><span class="sxs-lookup"><span data-stu-id="970ef-161">2\*(5+3) is not the same as 2\*5+3.</span></span> <span data-ttu-id="970ef-162">A zárójelek () segítségével a kiértékelési sorrend módosításakor balról jobbra kiértékelési sorrend nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="970ef-162">The brackets ( ) are used to change the evaluation order when left to right evaluation order isn't appropriate.</span></span>

## <a name="multi-valued-attributes"></a><span data-ttu-id="970ef-163">Többértékű attribútumok</span><span class="sxs-lookup"><span data-stu-id="970ef-163">Multi-valued attributes</span></span>
<span data-ttu-id="970ef-164">A funkciók is egyértékű és többértékű attribútumok is működik.</span><span class="sxs-lookup"><span data-stu-id="970ef-164">The functions can operate on both single-valued and multi-valued attributes.</span></span> <span data-ttu-id="970ef-165">Többértékű attribútumok a függvény minden egyes értékhez keresztül működik, és ugyanazt a funkciót alkalmazza minden egyes értékhez.</span><span class="sxs-lookup"><span data-stu-id="970ef-165">For multi-valued attributes, the function operates over every value and applies the same function to every value.</span></span>

<span data-ttu-id="970ef-166">Példa:</span><span class="sxs-lookup"><span data-stu-id="970ef-166">For example:</span></span>  
<span data-ttu-id="970ef-167">`Trim([proxyAddresses])`Minden a proxyAddress attribútum értékének Trim tegye.</span><span class="sxs-lookup"><span data-stu-id="970ef-167">`Trim([proxyAddresses])` Do a Trim of every value in the proxyAddress attribute.</span></span>  
<span data-ttu-id="970ef-168">`Word([proxyAddresses],1,"@") & "@contoso.com"`Minden értékekre, egy @-sign, cserélje le a tartomány @contoso.com.</span><span class="sxs-lookup"><span data-stu-id="970ef-168">`Word([proxyAddresses],1,"@") & "@contoso.com"` For every value with an @-sign, replace the domain with @contoso.com.</span></span>  
<span data-ttu-id="970ef-169">`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Keresse meg a SIP-cím, és távolítsa el az értékeket.</span><span class="sxs-lookup"><span data-stu-id="970ef-169">`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])` Look for the SIP-address and remove it from the values.</span></span>

## <a name="next-steps"></a><span data-ttu-id="970ef-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="970ef-170">Next steps</span></span>
* <span data-ttu-id="970ef-171">További információk a konfigurációs modell [ismertetése deklaratív kiépítés](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="970ef-171">Read more about the configuration model in [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span></span>
* <span data-ttu-id="970ef-172">Lásd: hogyan deklaratív kiépítés használt out-of-box a [az alapértelmezett konfiguráció ismertetése](active-directory-aadconnectsync-understanding-default-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="970ef-172">See how declarative provisioning is used out-of-box in [Understanding the default configuration](active-directory-aadconnectsync-understanding-default-configuration.md).</span></span>
* <span data-ttu-id="970ef-173">Lásd: how to gyakorlati módosítja a deklaratív kiépítés használatával [hogyan lehet módosítani az alapértelmezett konfiguráció](active-directory-aadconnectsync-change-the-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="970ef-173">See how to make a practical change using declarative provisioning in [How to make a change to the default configuration](active-directory-aadconnectsync-change-the-configuration.md).</span></span>

<span data-ttu-id="970ef-174">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="970ef-174">**Overview topics**</span></span>

* [<span data-ttu-id="970ef-175">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="970ef-175">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="970ef-176">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="970ef-176">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

<span data-ttu-id="970ef-177">**Referencia-témakörei**</span><span class="sxs-lookup"><span data-stu-id="970ef-177">**Reference topics**</span></span>

* [<span data-ttu-id="970ef-178">Azure AD Connect szinkronizálása: funkciók referencia</span><span class="sxs-lookup"><span data-stu-id="970ef-178">Azure AD Connect sync: Functions Reference</span></span>](active-directory-aadconnectsync-functions-reference.md)

