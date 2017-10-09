---
title: "Az Azure AD Connect: Deklaratív kiépítés kifejezéseinek |} Microsoft Docs"
description: "Hello deklaratív kiépítés kifejezéseinek ismerteti."
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
ms.openlocfilehash: 516bcf1991c608d33aefc19551254d8b2bfc024f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a><span data-ttu-id="64b85-103">Azure AD Connect szinkronizálása: deklaratív kiépítés kifejezések ismertetése</span><span class="sxs-lookup"><span data-stu-id="64b85-103">Azure AD Connect sync: Understanding Declarative Provisioning Expressions</span></span>
<span data-ttu-id="64b85-104">Azure AD Connect szinkronizálása épít deklaratív kiépítés először a Forefront Identity Manager 2010 verzióban jelent.</span><span class="sxs-lookup"><span data-stu-id="64b85-104">Azure AD Connect sync builds on declarative provisioning first introduced in Forefront Identity Manager 2010.</span></span> <span data-ttu-id="64b85-105">Tooimplement lehetővé teszi a teljes identity integration üzleti logika nélkül hello kell toowrite lefordított kódot.</span><span class="sxs-lookup"><span data-stu-id="64b85-105">It allows you tooimplement your complete identity integration business logic without hello need toowrite compiled code.</span></span>

<span data-ttu-id="64b85-106">Nagyon fontos részét képezik deklaratív kiépítés hello kifejezés nyelvi attribútumfolyamok használatban.</span><span class="sxs-lookup"><span data-stu-id="64b85-106">An essential part of declarative provisioning is hello expression language used in attribute flows.</span></span> <span data-ttu-id="64b85-107">hello nyelvét része a Microsoft® Visual Basic® Applications (VBA).</span><span class="sxs-lookup"><span data-stu-id="64b85-107">hello language used is a subset of Microsoft® Visual Basic® for Applications (VBA).</span></span> <span data-ttu-id="64b85-108">Ezen a nyelven a Microsoft Office szolgál, és a VBScript élménye rendelkező felhasználók is felismeri azt.</span><span class="sxs-lookup"><span data-stu-id="64b85-108">This language is used in Microsoft Office and users with experience of VBScript will also recognize it.</span></span> <span data-ttu-id="64b85-109">Deklaratív kiépítés kifejezés nyelvi hello csak funkciókat használ, és nem strukturált nyelvet.</span><span class="sxs-lookup"><span data-stu-id="64b85-109">hello Declarative Provisioning Expression Language is only using functions and is not a structured language.</span></span> <span data-ttu-id="64b85-110">Nincsenek módszerek vagy utasításokat.</span><span class="sxs-lookup"><span data-stu-id="64b85-110">There are no methods or statements.</span></span> <span data-ttu-id="64b85-111">Ehelyett beágyazott függvények tooexpress program folyamata.</span><span class="sxs-lookup"><span data-stu-id="64b85-111">Functions are instead nested tooexpress program flow.</span></span>

<span data-ttu-id="64b85-112">További részletekért lásd: [üdvözli a Visual Basic toohello az alkalmazások nyelvi referencia az Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).</span><span class="sxs-lookup"><span data-stu-id="64b85-112">For more details, see [Welcome toohello Visual Basic for Applications language reference for Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).</span></span>

<span data-ttu-id="64b85-113">hello attribútumok vannak erős típusmegadású.</span><span class="sxs-lookup"><span data-stu-id="64b85-113">hello attributes are strongly typed.</span></span> <span data-ttu-id="64b85-114">Egy függvény csak a megfelelő típusú hello attribútumokat fogad el.</span><span class="sxs-lookup"><span data-stu-id="64b85-114">A function only accepts attributes of hello correct type.</span></span> <span data-ttu-id="64b85-115">Egyúttal kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="64b85-115">It is also case-sensitive.</span></span> <span data-ttu-id="64b85-116">Mind a nevét, és az attribútumok nevében kell rendelkeznie a megfelelő kis-és nagybetűk, vagy hiba történt.</span><span class="sxs-lookup"><span data-stu-id="64b85-116">Both function names and attribute names must have proper casing or an error is thrown.</span></span>

## <a name="language-definitions-and-identifiers"></a><span data-ttu-id="64b85-117">Nyelvi definíciókat és -azonosítók</span><span class="sxs-lookup"><span data-stu-id="64b85-117">Language definitions and Identifiers</span></span>
* <span data-ttu-id="64b85-118">Funkciók argumentumokat szögletes zárójelbe követ nevet adni: (1 argumentum, N argumentum) függvénynév.</span><span class="sxs-lookup"><span data-stu-id="64b85-118">Functions have a name followed by arguments in brackets: FunctionName(argument 1, argument N).</span></span>
* <span data-ttu-id="64b85-119">Attribútumok szögletes zárójelbe azonosítja: [attributeName]</span><span class="sxs-lookup"><span data-stu-id="64b85-119">Attributes are identified by square brackets: [attributeName]</span></span>
* <span data-ttu-id="64b85-120">Paraméterek százalékjelek azonosítja: % ParameterName %</span><span class="sxs-lookup"><span data-stu-id="64b85-120">Parameters are identified by percent signs: %ParameterName%</span></span>
* <span data-ttu-id="64b85-121">A karakterlánckonstansokat ajánlatok tette: például "Contoso" (Megjegyzés: Egyenes idézőjel kell használnia. "", illetve nem idézőjelek között "")</span><span class="sxs-lookup"><span data-stu-id="64b85-121">String constants are surrounded by quotes: For example, "Contoso" (Note: must use straight quotes "" and not smart quotes “”)</span></span>
* <span data-ttu-id="64b85-122">Numerikus értékek szerint van megadva, ajánlatok és várt toobe decimális nélkül.</span><span class="sxs-lookup"><span data-stu-id="64b85-122">Numeric values are expressed without quotes and expected toobe decimal.</span></span> <span data-ttu-id="64b85-123">Hexadecimális értékek fűzve előtagként & H.</span><span class="sxs-lookup"><span data-stu-id="64b85-123">Hexadecimal values are prefixed with &H.</span></span> <span data-ttu-id="64b85-124">Például a 98052 & HFF</span><span class="sxs-lookup"><span data-stu-id="64b85-124">For example, 98052, &HFF</span></span>
* <span data-ttu-id="64b85-125">Logikai értékek szerint van megadva, az állandókat: True, False.</span><span class="sxs-lookup"><span data-stu-id="64b85-125">Boolean values are expressed with constants: True, False.</span></span>
* <span data-ttu-id="64b85-126">Beépített állandók és literálok szerint van megadva. csak a nevükkel: NULL, CRLF, IgnoreThisFlow</span><span class="sxs-lookup"><span data-stu-id="64b85-126">Built-in constants and literals are expressed with only their name: NULL, CRLF, IgnoreThisFlow</span></span>

### <a name="functions"></a><span data-ttu-id="64b85-127">Functions</span><span class="sxs-lookup"><span data-stu-id="64b85-127">Functions</span></span>
<span data-ttu-id="64b85-128">Deklaratív kiépítés számos funkciók tooenable hello lehetőségét tootransform attribútum értékeket használja.</span><span class="sxs-lookup"><span data-stu-id="64b85-128">Declarative provisioning uses many functions tooenable hello possibility tootransform attribute values.</span></span> <span data-ttu-id="64b85-129">Ezeket a funkciókat, hello eredménye egy függvényből tooanother függvénynek átadott egymásba.</span><span class="sxs-lookup"><span data-stu-id="64b85-129">These functions can be nested so hello result from one function is passed in tooanother function.</span></span>

`Function1(Function2(Function3()))`

<span data-ttu-id="64b85-130">hello funkciók teljes listája megtalálható hello [hivatkozás működéséhez](active-directory-aadconnectsync-functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="64b85-130">hello complete list of functions can be found in hello [function reference](active-directory-aadconnectsync-functions-reference.md).</span></span>

### <a name="parameters"></a><span data-ttu-id="64b85-131">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="64b85-131">Parameters</span></span>
<span data-ttu-id="64b85-132">A paraméter PowerShell használó rendszergazda vagy egy összekötő van definiálva.</span><span class="sxs-lookup"><span data-stu-id="64b85-132">A parameter is defined either by a Connector or by an administrator using PowerShell.</span></span> <span data-ttu-id="64b85-133">Paraméterek általában tartalmaznak, amelyek eltérnek a rendszer toosystem értékek, például hello hello tartományfelhasználói hello neve található.</span><span class="sxs-lookup"><span data-stu-id="64b85-133">Parameters usually contain values that are different from system toosystem, for example hello name of hello domain hello user is located in.</span></span> <span data-ttu-id="64b85-134">Ezek a paraméterek attribútumfolyamok használható.</span><span class="sxs-lookup"><span data-stu-id="64b85-134">These parameters can be used in attribute flows.</span></span>

<span data-ttu-id="64b85-135">hello Active Directory-összekötő megadott hello paraméterek bejövő szinkronizálási szabályok a következő:</span><span class="sxs-lookup"><span data-stu-id="64b85-135">hello Active Directory Connector provided hello following parameters for inbound Synchronization Rules:</span></span>

| <span data-ttu-id="64b85-136">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="64b85-136">Parameter Name</span></span> | <span data-ttu-id="64b85-137">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="64b85-137">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="64b85-138">Domain.Netbios</span><span class="sxs-lookup"><span data-stu-id="64b85-138">Domain.Netbios</span></span> |<span data-ttu-id="64b85-139">Hello tartomány jelenleg importált, például FABRIKAMSALES NetBIOS formátuma</span><span class="sxs-lookup"><span data-stu-id="64b85-139">Netbios format of hello domain currently being imported, for example FABRIKAMSALES</span></span> |
| <span data-ttu-id="64b85-140">Domain.FQDN</span><span class="sxs-lookup"><span data-stu-id="64b85-140">Domain.FQDN</span></span> |<span data-ttu-id="64b85-141">Hello tartomány jelenleg importált, például sales.fabrikam.com FQDN-formátumban</span><span class="sxs-lookup"><span data-stu-id="64b85-141">FQDN format of hello domain currently being imported, for example sales.fabrikam.com</span></span> |
| <span data-ttu-id="64b85-142">Domain.LDAP</span><span class="sxs-lookup"><span data-stu-id="64b85-142">Domain.LDAP</span></span> |<span data-ttu-id="64b85-143">LDAP-formátum hello tartomány jelenleg importált, például a tartományvezérlő értékesítés, DC = fabrikam, DC = = com</span><span class="sxs-lookup"><span data-stu-id="64b85-143">LDAP format of hello domain currently being imported, for example DC=sales,DC=fabrikam,DC=com</span></span> |
| <span data-ttu-id="64b85-144">Forest.Netbios</span><span class="sxs-lookup"><span data-stu-id="64b85-144">Forest.Netbios</span></span> |<span data-ttu-id="64b85-145">Hello erdő neve jelenleg importált, például FABRIKAMCORP NetBIOS formátuma</span><span class="sxs-lookup"><span data-stu-id="64b85-145">Netbios format of hello forest name currently being imported, for example FABRIKAMCORP</span></span> |
| <span data-ttu-id="64b85-146">Forest.FQDN</span><span class="sxs-lookup"><span data-stu-id="64b85-146">Forest.FQDN</span></span> |<span data-ttu-id="64b85-147">Hello erdő nevének jelenleg importált, fabrikam.com például FQDN-formátumban</span><span class="sxs-lookup"><span data-stu-id="64b85-147">FQDN format of hello forest name currently being imported, for example fabrikam.com</span></span> |
| <span data-ttu-id="64b85-148">Forest.LDAP</span><span class="sxs-lookup"><span data-stu-id="64b85-148">Forest.LDAP</span></span> |<span data-ttu-id="64b85-149">LDAP-formátum hello erdő nevének jelenleg importált, például: DC = fabrikam, DC = com</span><span class="sxs-lookup"><span data-stu-id="64b85-149">LDAP format of hello forest name currently being imported, for example DC=fabrikam,DC=com</span></span> |

<span data-ttu-id="64b85-150">hello rendszer biztosít hello követően paraméter, amely használt tooget hello azonosítója hello Connector jelenleg fut:</span><span class="sxs-lookup"><span data-stu-id="64b85-150">hello system provides hello following parameter, which is used tooget hello identifier of hello Connector currently running:</span></span>  
`Connector.ID`

<span data-ttu-id="64b85-151">Íme egy példa a hello metaverzum-attribútum tartomány hello tartomány hello felhasználói hello netbios-nevét feltöltő:</span><span class="sxs-lookup"><span data-stu-id="64b85-151">Here is an example that populates hello metaverse attribute domain with hello netbios name of hello domain where hello user is located:</span></span>  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a><span data-ttu-id="64b85-152">Operátorok</span><span class="sxs-lookup"><span data-stu-id="64b85-152">Operators</span></span>
<span data-ttu-id="64b85-153">hello alábbi operátorok használhatók:</span><span class="sxs-lookup"><span data-stu-id="64b85-153">hello following operators can be used:</span></span>

* <span data-ttu-id="64b85-154">**Összehasonlítás**: <, < =, <>, =, >, > =</span><span class="sxs-lookup"><span data-stu-id="64b85-154">**Comparison**: <, <=, <>, =, >, >=</span></span>
* <span data-ttu-id="64b85-155">**Matematikai**: +, -, \*, -</span><span class="sxs-lookup"><span data-stu-id="64b85-155">**Mathematics**: +, -, \*, -</span></span>
* <span data-ttu-id="64b85-156">**Karakterlánc**: & (ÖSSZEFŰZ)</span><span class="sxs-lookup"><span data-stu-id="64b85-156">**String**: & (concatenate)</span></span>
* <span data-ttu-id="64b85-157">**Logikai**: & & (és). (vagy)</span><span class="sxs-lookup"><span data-stu-id="64b85-157">**Logical**: && (and), || (or)</span></span>
* <span data-ttu-id="64b85-158">**Kiértékelési sorrend**:)</span><span class="sxs-lookup"><span data-stu-id="64b85-158">**Evaluation order**: ( )</span></span>

<span data-ttu-id="64b85-159">Operátorok kiértékelt bal oldali tooright és hello prioritású kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="64b85-159">Operators are evaluated left tooright and have hello same evaluation priority.</span></span> <span data-ttu-id="64b85-160">Ez azt jelenti, hogy hello \* (szorzója) nem kerül kiértékelésre, mielőtt - (kivonás).</span><span class="sxs-lookup"><span data-stu-id="64b85-160">That is, hello \* (multiplier) is not evaluated before - (subtraction).</span></span> <span data-ttu-id="64b85-161">2\*(5 + 3) rendszer nem hello ugyanaz, mint 2\*5 + 3.</span><span class="sxs-lookup"><span data-stu-id="64b85-161">2\*(5+3) is not hello same as 2\*5+3.</span></span> <span data-ttu-id="64b85-162">hello zárójelek közé (-) használt toochange hello kiértékelési sorrend amikor bal oldali tooright kiértékelési sorrend nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="64b85-162">hello brackets ( ) are used toochange hello evaluation order when left tooright evaluation order isn't appropriate.</span></span>

## <a name="multi-valued-attributes"></a><span data-ttu-id="64b85-163">Többértékű attribútumok</span><span class="sxs-lookup"><span data-stu-id="64b85-163">Multi-valued attributes</span></span>
<span data-ttu-id="64b85-164">hello funkciók is egyértékű és többértékű attribútumok is működik.</span><span class="sxs-lookup"><span data-stu-id="64b85-164">hello functions can operate on both single-valued and multi-valued attributes.</span></span> <span data-ttu-id="64b85-165">Többértékű attribútumok hello függvény minden egyes értékhez keresztül működik, és érvényes hello olyan funkciókat tooevery érték.</span><span class="sxs-lookup"><span data-stu-id="64b85-165">For multi-valued attributes, hello function operates over every value and applies hello same function tooevery value.</span></span>

<span data-ttu-id="64b85-166">Példa:</span><span class="sxs-lookup"><span data-stu-id="64b85-166">For example:</span></span>  
<span data-ttu-id="64b85-167">`Trim([proxyAddresses])`Minden hello proxyAddress attribútum értékének Trim tegye.</span><span class="sxs-lookup"><span data-stu-id="64b85-167">`Trim([proxyAddresses])` Do a Trim of every value in hello proxyAddress attribute.</span></span>  
<span data-ttu-id="64b85-168">`Word([proxyAddresses],1,"@") & "@contoso.com"`Minden értékekre, egy @-sign, cserélje le a hello tartomány @contoso.com.</span><span class="sxs-lookup"><span data-stu-id="64b85-168">`Word([proxyAddresses],1,"@") & "@contoso.com"` For every value with an @-sign, replace hello domain with @contoso.com.</span></span>  
<span data-ttu-id="64b85-169">`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Keresse meg hello SIP-cím, és távolítsa el azt a hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="64b85-169">`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])` Look for hello SIP-address and remove it from hello values.</span></span>

## <a name="next-steps"></a><span data-ttu-id="64b85-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="64b85-170">Next steps</span></span>
* <span data-ttu-id="64b85-171">További információk a hello konfigurációs modell [ismertetése deklaratív kiépítés](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="64b85-171">Read more about hello configuration model in [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span></span>
* <span data-ttu-id="64b85-172">Lásd: hogyan deklaratív használt out-of-box a [ismertetése hello alapértelmezett konfiguráció](active-directory-aadconnectsync-understanding-default-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="64b85-172">See how declarative provisioning is used out-of-box in [Understanding hello default configuration](active-directory-aadconnectsync-understanding-default-configuration.md).</span></span>
* <span data-ttu-id="64b85-173">Tekintse meg, hogyan toomake egy gyakorlati módosítani, használja a deklaratív kiépítés [hogyan toomake egy módosítás toohello alapértelmezett konfiguráció](active-directory-aadconnectsync-change-the-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="64b85-173">See how toomake a practical change using declarative provisioning in [How toomake a change toohello default configuration](active-directory-aadconnectsync-change-the-configuration.md).</span></span>

<span data-ttu-id="64b85-174">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="64b85-174">**Overview topics**</span></span>

* [<span data-ttu-id="64b85-175">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="64b85-175">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="64b85-176">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="64b85-176">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

<span data-ttu-id="64b85-177">**Referencia-témakörei**</span><span class="sxs-lookup"><span data-stu-id="64b85-177">**Reference topics**</span></span>

* [<span data-ttu-id="64b85-178">Azure AD Connect szinkronizálása: funkciók referencia</span><span class="sxs-lookup"><span data-stu-id="64b85-178">Azure AD Connect sync: Functions Reference</span></span>](active-directory-aadconnectsync-functions-reference.md)

