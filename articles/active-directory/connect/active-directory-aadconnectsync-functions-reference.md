---
title: "Azure AD Connect szinkronizálása: funkciók referencia |} Microsoft Docs"
description: "Az Azure AD Connect szinkronizálási szolgáltatás deklaratív kiépítés kifejezéseinek hivatkozását."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 4f525ca0-be0e-4a2e-8da1-09b6b567ed5f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 926f52ef64eb79205dbfb344edc7d9bece2a6947
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-functions-reference"></a><span data-ttu-id="1d660-103">Azure AD Connect szinkronizálása: funkciók referencia</span><span class="sxs-lookup"><span data-stu-id="1d660-103">Azure AD Connect sync: Functions Reference</span></span>
<span data-ttu-id="1d660-104">Az Azure AD Connectben funkciók segítségével kezelheti egy attribútum értékét a szinkronizálás során.</span><span class="sxs-lookup"><span data-stu-id="1d660-104">In Azure AD Connect, functions are used to manipulate an attribute value during synchronization.</span></span>  
<span data-ttu-id="1d660-105">A funkciók által használt szintaxis van kifejezve a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="1d660-105">The Syntax of the functions is expressed using the following format:</span></span>  
`<output type> FunctionName(<input type> <position name>, ..)`

<span data-ttu-id="1d660-106">Egy függvény túl van terhelve, és több szintaxissal fogad el, ha az összes érvényes szintaxissal vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="1d660-106">If a function is overloaded and accepts multiple syntaxes, all valid syntaxes are listed.</span></span>  
<span data-ttu-id="1d660-107">A funkciók erős típusmegadású vannak, és azok győződjön meg arról, hogy az átadott típus: megegyezik a dokumentált típusával.</span><span class="sxs-lookup"><span data-stu-id="1d660-107">The functions are strongly typed and they verify that the type passed in matches the documented type.</span></span>  
<span data-ttu-id="1d660-108">A típus nem egyezik, ha hiba fordul elő.</span><span class="sxs-lookup"><span data-stu-id="1d660-108">If the type does not match, an error is thrown.</span></span>

<span data-ttu-id="1d660-109">A típus szerint van megadva, a következő szintaxissal:</span><span class="sxs-lookup"><span data-stu-id="1d660-109">The types are expressed with the following syntax:</span></span>

* <span data-ttu-id="1d660-110">**Bin** – bináris</span><span class="sxs-lookup"><span data-stu-id="1d660-110">**bin** – Binary</span></span>
* <span data-ttu-id="1d660-111">**logikai** – logikai</span><span class="sxs-lookup"><span data-stu-id="1d660-111">**bool** – Boolean</span></span>
* <span data-ttu-id="1d660-112">**DT** – UTC dátum és idő</span><span class="sxs-lookup"><span data-stu-id="1d660-112">**dt** – UTC Date/Time</span></span>
* <span data-ttu-id="1d660-113">**Enum** – az ismert állandók számbavétele</span><span class="sxs-lookup"><span data-stu-id="1d660-113">**enum** – Enumeration of known constants</span></span>
* <span data-ttu-id="1d660-114">**Exp** – eredménye egy logikai kifejezés</span><span class="sxs-lookup"><span data-stu-id="1d660-114">**exp** – Expression, which is expected to evaluate to a Boolean</span></span>
* <span data-ttu-id="1d660-115">**mvbin** – többértékű bináris</span><span class="sxs-lookup"><span data-stu-id="1d660-115">**mvbin** – Multi-Valued Binary</span></span>
* <span data-ttu-id="1d660-116">**mvstr** – többértékű karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d660-116">**mvstr** – Multi-Valued String</span></span>
* <span data-ttu-id="1d660-117">**mvref** – többértékű referencia</span><span class="sxs-lookup"><span data-stu-id="1d660-117">**mvref** – Multi-Valued Reference</span></span>
* <span data-ttu-id="1d660-118">**NUM** – numerikus</span><span class="sxs-lookup"><span data-stu-id="1d660-118">**num** – Numeric</span></span>
* <span data-ttu-id="1d660-119">**REF** – referencia</span><span class="sxs-lookup"><span data-stu-id="1d660-119">**ref** – Reference</span></span>
* <span data-ttu-id="1d660-120">**Str** – karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d660-120">**str** – String</span></span>
* <span data-ttu-id="1d660-121">**var** – (szinte) bármilyen más típusú variant</span><span class="sxs-lookup"><span data-stu-id="1d660-121">**var** – A variant of (almost) any other type</span></span>
* <span data-ttu-id="1d660-122">**"void"** – nem ad visszatérési értéket</span><span class="sxs-lookup"><span data-stu-id="1d660-122">**void** – doesn’t return a value</span></span>

<span data-ttu-id="1d660-123">A funkciók a típusú **mvbin**, **mvstr**, és **mvref** többértékű attribútumok csak használható.</span><span class="sxs-lookup"><span data-stu-id="1d660-123">The functions with the types **mvbin**, **mvstr**, and **mvref** can only work on multi-valued attributes.</span></span> <span data-ttu-id="1d660-124">A Functions **bin**, **str**, és **ref** munkahelyi egyaránt egyértékű és többértékű jellemzőit.</span><span class="sxs-lookup"><span data-stu-id="1d660-124">Functions with **bin**, **str**, and **ref** work on both single-valued and multi-valued attributes.</span></span>

## <a name="functions-reference"></a><span data-ttu-id="1d660-125">Functions – referencia</span><span class="sxs-lookup"><span data-stu-id="1d660-125">Functions Reference</span></span>
| <span data-ttu-id="1d660-126">Függvények listája</span><span class="sxs-lookup"><span data-stu-id="1d660-126">List of functions</span></span> |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="1d660-127">**Tanúsítvány**</span><span class="sxs-lookup"><span data-stu-id="1d660-127">**Certificate**</span></span> | | | | |
| [<span data-ttu-id="1d660-128">CertExtensionOids</span><span class="sxs-lookup"><span data-stu-id="1d660-128">CertExtensionOids</span></span>](#certextensionoids) |[<span data-ttu-id="1d660-129">CertFormat</span><span class="sxs-lookup"><span data-stu-id="1d660-129">CertFormat</span></span>](#certformat) |[<span data-ttu-id="1d660-130">CertFriendlyName</span><span class="sxs-lookup"><span data-stu-id="1d660-130">CertFriendlyName</span></span>](#certfriendlyname) |[<span data-ttu-id="1d660-131">CertHashString</span><span class="sxs-lookup"><span data-stu-id="1d660-131">CertHashString</span></span>](#certhashstring) | |
| [<span data-ttu-id="1d660-132">CertIssuer</span><span class="sxs-lookup"><span data-stu-id="1d660-132">CertIssuer</span></span>](#certissuer) |[<span data-ttu-id="1d660-133">CertIssuerDN</span><span class="sxs-lookup"><span data-stu-id="1d660-133">CertIssuerDN</span></span>](#certissuerdn) |[<span data-ttu-id="1d660-134">CertIssuerOid</span><span class="sxs-lookup"><span data-stu-id="1d660-134">CertIssuerOid</span></span>](#certissueroid) |[<span data-ttu-id="1d660-135">CertKeyAlgorithm</span><span class="sxs-lookup"><span data-stu-id="1d660-135">CertKeyAlgorithm</span></span>](#certkeyalgorithm) | |
| [<span data-ttu-id="1d660-136">CertKeyAlgorithmParams</span><span class="sxs-lookup"><span data-stu-id="1d660-136">CertKeyAlgorithmParams</span></span>](#certkeyalgorithmparams) |[<span data-ttu-id="1d660-137">CertNameInfo</span><span class="sxs-lookup"><span data-stu-id="1d660-137">CertNameInfo</span></span>](#certnameinfo) |[<span data-ttu-id="1d660-138">CertNotAfter</span><span class="sxs-lookup"><span data-stu-id="1d660-138">CertNotAfter</span></span>](#certnotafter) |[<span data-ttu-id="1d660-139">CertNotBefore</span><span class="sxs-lookup"><span data-stu-id="1d660-139">CertNotBefore</span></span>](#certnotbefore) | |
| [<span data-ttu-id="1d660-140">CertPublicKeyOid</span><span class="sxs-lookup"><span data-stu-id="1d660-140">CertPublicKeyOid</span></span>](#certpublickeyoid) |[<span data-ttu-id="1d660-141">CertPublicKeyParametersOid</span><span class="sxs-lookup"><span data-stu-id="1d660-141">CertPublicKeyParametersOid</span></span>](#certpublickeyparametersoid) |[<span data-ttu-id="1d660-142">CertSerialNumber</span><span class="sxs-lookup"><span data-stu-id="1d660-142">CertSerialNumber</span></span>](#certserialnumber) |[<span data-ttu-id="1d660-143">CertSignatureAlgorithmOid</span><span class="sxs-lookup"><span data-stu-id="1d660-143">CertSignatureAlgorithmOid</span></span>](#certsignaturealgorithmoid) | |
| [<span data-ttu-id="1d660-144">CertSubject</span><span class="sxs-lookup"><span data-stu-id="1d660-144">CertSubject</span></span>](#certsubject) |[<span data-ttu-id="1d660-145">CertSubjectNameDN</span><span class="sxs-lookup"><span data-stu-id="1d660-145">CertSubjectNameDN</span></span>](#certsubjectnamedn) |[<span data-ttu-id="1d660-146">CertSubjectNameOid</span><span class="sxs-lookup"><span data-stu-id="1d660-146">CertSubjectNameOid</span></span>](#certsubjectnameoid) |[<span data-ttu-id="1d660-147">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="1d660-147">CertThumbprint</span></span>](#certthumbprint) | |
[<span data-ttu-id="1d660-148">CertVersion</span><span class="sxs-lookup"><span data-stu-id="1d660-148"> CertVersion</span></span>](#certversion) |[<span data-ttu-id="1d660-149">IsCert</span><span class="sxs-lookup"><span data-stu-id="1d660-149">IsCert</span></span>](#iscert) | | | |
| <span data-ttu-id="1d660-150">**Átalakítás**</span><span class="sxs-lookup"><span data-stu-id="1d660-150">**Conversion**</span></span> | | | | |
| [<span data-ttu-id="1d660-151">CBool</span><span class="sxs-lookup"><span data-stu-id="1d660-151">CBool</span></span>](#cbool) |[<span data-ttu-id="1d660-152">CDate</span><span class="sxs-lookup"><span data-stu-id="1d660-152">CDate</span></span>](#cdate) |[<span data-ttu-id="1d660-153">CGuid</span><span class="sxs-lookup"><span data-stu-id="1d660-153">CGuid</span></span>](#cguid) |[<span data-ttu-id="1d660-154">ConvertFromBase64</span><span class="sxs-lookup"><span data-stu-id="1d660-154">ConvertFromBase64</span></span>](#convertfrombase64) | |
| [<span data-ttu-id="1d660-155">ConvertToBase64</span><span class="sxs-lookup"><span data-stu-id="1d660-155">ConvertToBase64</span></span>](#converttobase64) |[<span data-ttu-id="1d660-156">ConvertFromUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="1d660-156">ConvertFromUTF8Hex</span></span>](#convertfromutf8hex) |[<span data-ttu-id="1d660-157">ConvertToUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="1d660-157">ConvertToUTF8Hex</span></span>](#converttoutf8hex) |[<span data-ttu-id="1d660-158">CNum</span><span class="sxs-lookup"><span data-stu-id="1d660-158">CNum</span></span>](#cnum) | |
| [<span data-ttu-id="1d660-159">CRef</span><span class="sxs-lookup"><span data-stu-id="1d660-159">CRef</span></span>](#cref) |[<span data-ttu-id="1d660-160">CStr</span><span class="sxs-lookup"><span data-stu-id="1d660-160">CStr</span></span>](#cstr) |[<span data-ttu-id="1d660-161">StringFromGuid</span><span class="sxs-lookup"><span data-stu-id="1d660-161">StringFromGuid</span></span>](#StringFromGuid) |[<span data-ttu-id="1d660-162">StringFromSid</span><span class="sxs-lookup"><span data-stu-id="1d660-162">StringFromSid</span></span>](#stringfromsid) | |
| <span data-ttu-id="1d660-163">**Dátum és idő**</span><span class="sxs-lookup"><span data-stu-id="1d660-163">**Date / Time**</span></span> | | | | |
| [<span data-ttu-id="1d660-164">DateAdd</span><span class="sxs-lookup"><span data-stu-id="1d660-164">DateAdd</span></span>](#dateadd) |[<span data-ttu-id="1d660-165">DateFromNum</span><span class="sxs-lookup"><span data-stu-id="1d660-165">DateFromNum</span></span>](#datefromnum) |[<span data-ttu-id="1d660-166">FormatDateTime</span><span class="sxs-lookup"><span data-stu-id="1d660-166">FormatDateTime</span></span>](#formatdatetime) |[<span data-ttu-id="1d660-167">Most</span><span class="sxs-lookup"><span data-stu-id="1d660-167">Now</span></span>](#now) | |
| [<span data-ttu-id="1d660-168">NumFromDate</span><span class="sxs-lookup"><span data-stu-id="1d660-168">NumFromDate</span></span>](#numfromdate) | | | | |
| <span data-ttu-id="1d660-169">**Könyvtár**</span><span class="sxs-lookup"><span data-stu-id="1d660-169">**Directory**</span></span> | | | | |
| [<span data-ttu-id="1d660-170">DNComponent</span><span class="sxs-lookup"><span data-stu-id="1d660-170">DNComponent</span></span>](#dncomponent) |[<span data-ttu-id="1d660-171">DNComponentRev</span><span class="sxs-lookup"><span data-stu-id="1d660-171">DNComponentRev</span></span>](#dncomponentrev) |[<span data-ttu-id="1d660-172">EscapeDNComponent</span><span class="sxs-lookup"><span data-stu-id="1d660-172">EscapeDNComponent</span></span>](#escapedncomponent) | | |
| <span data-ttu-id="1d660-173">**Értékelés**</span><span class="sxs-lookup"><span data-stu-id="1d660-173">**Evaluation**</span></span> | | | | |
| [<span data-ttu-id="1d660-174">IsBitSet</span><span class="sxs-lookup"><span data-stu-id="1d660-174">IsBitSet</span></span>](#isbitset) |[<span data-ttu-id="1d660-175">IsDate</span><span class="sxs-lookup"><span data-stu-id="1d660-175">IsDate</span></span>](#isdate) |[<span data-ttu-id="1d660-176">Vagyis IsEmpty</span><span class="sxs-lookup"><span data-stu-id="1d660-176">IsEmpty</span></span>](#isempty) |[<span data-ttu-id="1d660-177">IsGuid</span><span class="sxs-lookup"><span data-stu-id="1d660-177">IsGuid</span></span>](#isguid) | |
| [<span data-ttu-id="1d660-178">IsNull</span><span class="sxs-lookup"><span data-stu-id="1d660-178">IsNull</span></span>](#isnull) |[<span data-ttu-id="1d660-179">IsNullOrEmpty</span><span class="sxs-lookup"><span data-stu-id="1d660-179">IsNullOrEmpty</span></span>](#isnullorempty) |[<span data-ttu-id="1d660-180">IsNumeric</span><span class="sxs-lookup"><span data-stu-id="1d660-180">IsNumeric</span></span>](#isnumeric) |[<span data-ttu-id="1d660-181">IsPresent</span><span class="sxs-lookup"><span data-stu-id="1d660-181">IsPresent</span></span>](#ispresent) | |
| [<span data-ttu-id="1d660-182">IsString</span><span class="sxs-lookup"><span data-stu-id="1d660-182">IsString</span></span>](#isstring) | | | | |
| <span data-ttu-id="1d660-183">**Matematikai**</span><span class="sxs-lookup"><span data-stu-id="1d660-183">**Math**</span></span> | | | | |
| [<span data-ttu-id="1d660-184">BitAnd</span><span class="sxs-lookup"><span data-stu-id="1d660-184">BitAnd</span></span>](#bitand) |[<span data-ttu-id="1d660-185">BitOr</span><span class="sxs-lookup"><span data-stu-id="1d660-185">BitOr</span></span>](#bitor) |[<span data-ttu-id="1d660-186">RandomNum</span><span class="sxs-lookup"><span data-stu-id="1d660-186">RandomNum</span></span>](#randomnum) | | |
| <span data-ttu-id="1d660-187">**Többértékű**</span><span class="sxs-lookup"><span data-stu-id="1d660-187">**Multi-valued**</span></span> | | | | |
| [<span data-ttu-id="1d660-188">Tartalmazza</span><span class="sxs-lookup"><span data-stu-id="1d660-188">Contains</span></span>](#contains) |[<span data-ttu-id="1d660-189">Száma</span><span class="sxs-lookup"><span data-stu-id="1d660-189">Count</span></span>](#count) |[<span data-ttu-id="1d660-190">Elem</span><span class="sxs-lookup"><span data-stu-id="1d660-190">Item</span></span>](#item) |[<span data-ttu-id="1d660-191">ItemOrNull</span><span class="sxs-lookup"><span data-stu-id="1d660-191">ItemOrNull</span></span>](#itemornull) | |
| [<span data-ttu-id="1d660-192">Csatlakozás</span><span class="sxs-lookup"><span data-stu-id="1d660-192">Join</span></span>](#join) |[<span data-ttu-id="1d660-193">RemoveDuplicates</span><span class="sxs-lookup"><span data-stu-id="1d660-193">RemoveDuplicates</span></span>](#removeduplicates) |[<span data-ttu-id="1d660-194">Vegyes</span><span class="sxs-lookup"><span data-stu-id="1d660-194">Split</span></span>](#split) | | |
| <span data-ttu-id="1d660-195">**Program folyamata**</span><span class="sxs-lookup"><span data-stu-id="1d660-195">**Program Flow**</span></span> | | | | |
| [<span data-ttu-id="1d660-196">Hiba történt</span><span class="sxs-lookup"><span data-stu-id="1d660-196">Error</span></span>](#error) |[<span data-ttu-id="1d660-197">AZ IIF</span><span class="sxs-lookup"><span data-stu-id="1d660-197">IIF</span></span>](#iif) |[<span data-ttu-id="1d660-198">Kiválasztás</span><span class="sxs-lookup"><span data-stu-id="1d660-198">Select</span></span>](#select) |[<span data-ttu-id="1d660-199">Kapcsoló</span><span class="sxs-lookup"><span data-stu-id="1d660-199">Switch</span></span>](#switch) | |
| [<span data-ttu-id="1d660-200">Ha</span><span class="sxs-lookup"><span data-stu-id="1d660-200">Where</span></span>](#where) |[<span data-ttu-id="1d660-201">A</span><span class="sxs-lookup"><span data-stu-id="1d660-201">With</span></span>](#with) | | | |
| <span data-ttu-id="1d660-202">**Szöveg**</span><span class="sxs-lookup"><span data-stu-id="1d660-202">**Text**</span></span> | | | | |
| [<span data-ttu-id="1d660-203">GUID</span><span class="sxs-lookup"><span data-stu-id="1d660-203">GUID</span></span>](#guid) |[<span data-ttu-id="1d660-204">InStr</span><span class="sxs-lookup"><span data-stu-id="1d660-204">InStr</span></span>](#instr) |[<span data-ttu-id="1d660-205">InStrRev</span><span class="sxs-lookup"><span data-stu-id="1d660-205">InStrRev</span></span>](#instrrev) |[<span data-ttu-id="1d660-206">LCase</span><span class="sxs-lookup"><span data-stu-id="1d660-206">LCase</span></span>](#lcase) | |
| [<span data-ttu-id="1d660-207">Balra</span><span class="sxs-lookup"><span data-stu-id="1d660-207">Left</span></span>](#left) |[<span data-ttu-id="1d660-208">Hossz</span><span class="sxs-lookup"><span data-stu-id="1d660-208">Len</span></span>](#len) |[<span data-ttu-id="1d660-209">LTrim</span><span class="sxs-lookup"><span data-stu-id="1d660-209">LTrim</span></span>](#ltrim) |[<span data-ttu-id="1d660-210">Mid</span><span class="sxs-lookup"><span data-stu-id="1d660-210">Mid</span></span>](#mid) | |
| [<span data-ttu-id="1d660-211">PadLeft</span><span class="sxs-lookup"><span data-stu-id="1d660-211">PadLeft</span></span>](#padleft) |[<span data-ttu-id="1d660-212">PadRight</span><span class="sxs-lookup"><span data-stu-id="1d660-212">PadRight</span></span>](#padright) |[<span data-ttu-id="1d660-213">PCase</span><span class="sxs-lookup"><span data-stu-id="1d660-213">PCase</span></span>](#pcase) |[<span data-ttu-id="1d660-214">Cserélje le</span><span class="sxs-lookup"><span data-stu-id="1d660-214">Replace</span></span>](#replace) | |
| [<span data-ttu-id="1d660-215">ReplaceChars</span><span class="sxs-lookup"><span data-stu-id="1d660-215">ReplaceChars</span></span>](#replacechars) |[<span data-ttu-id="1d660-216">Jobbra</span><span class="sxs-lookup"><span data-stu-id="1d660-216">Right</span></span>](#right) |[<span data-ttu-id="1d660-217">RTrim</span><span class="sxs-lookup"><span data-stu-id="1d660-217">RTrim</span></span>](#rtrim) |[<span data-ttu-id="1d660-218">Trim</span><span class="sxs-lookup"><span data-stu-id="1d660-218">Trim</span></span>](#trim) | |
| [<span data-ttu-id="1d660-219">UCase</span><span class="sxs-lookup"><span data-stu-id="1d660-219">UCase</span></span>](#ucase) |[<span data-ttu-id="1d660-220">Word</span><span class="sxs-lookup"><span data-stu-id="1d660-220">Word</span></span>](#word) | | | |

- - -
### <a name="bitand"></a><span data-ttu-id="1d660-221">BitAnd</span><span class="sxs-lookup"><span data-stu-id="1d660-221">BitAnd</span></span>
<span data-ttu-id="1d660-222">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-222">**Description:**</span></span>  
<span data-ttu-id="1d660-223">A BitAnd függvény megadott bits értéket állítja be.</span><span class="sxs-lookup"><span data-stu-id="1d660-223">The BitAnd function sets specified bits on a value.</span></span>

<span data-ttu-id="1d660-224">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-224">**Syntax:**</span></span>  
`num BitAnd(num value1, num value2)`

* <span data-ttu-id="1d660-225">Érték1, érték2: numerikus érték, amely együtt kell-e érték</span><span class="sxs-lookup"><span data-stu-id="1d660-225">value1, value2: numeric values that should be AND’ed together</span></span>

<span data-ttu-id="1d660-226">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-226">**Remarks:**</span></span>  
<span data-ttu-id="1d660-227">Ez a funkció mindkét paraméter átalakítja a bináris megjelenítése, és beállítja egy kicsit:</span><span class="sxs-lookup"><span data-stu-id="1d660-227">This function converts both parameters to the binary representation and sets a bit to:</span></span>

* <span data-ttu-id="1d660-228">0 – Ha egy vagy mindkét a megfelelő bit *maszk* és *jelző* : 0</span><span class="sxs-lookup"><span data-stu-id="1d660-228">0 - if one or both of the corresponding bits in *mask* and *flag* are 0</span></span>
* <span data-ttu-id="1d660-229">1 – Ha a megfelelő bits mindkettő az 1.</span><span class="sxs-lookup"><span data-stu-id="1d660-229">1 - if both of the corresponding bits are 1.</span></span>

<span data-ttu-id="1d660-230">Ez azt jelenti akkor adja vissza 0 minden esetben, kivéve ha a megfelelő bitek száma mindkét paraméter 1.</span><span class="sxs-lookup"><span data-stu-id="1d660-230">In other words, it returns 0 in all cases except when the corresponding bits of both parameters are 1.</span></span>

<span data-ttu-id="1d660-231">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-231">**Example:**</span></span>  
`BitAnd(&HF, &HF7)`  
<span data-ttu-id="1d660-232">7 adja vissza, mert hexadecimális "F" AND "F7" határozni, hogy erre az értékre.</span><span class="sxs-lookup"><span data-stu-id="1d660-232">Returns 7 because hexadecimal "F" AND "F7" evaluate to this value.</span></span>

- - -
### <a name="bitor"></a><span data-ttu-id="1d660-233">BitOr</span><span class="sxs-lookup"><span data-stu-id="1d660-233">BitOr</span></span>
<span data-ttu-id="1d660-234">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-234">**Description:**</span></span>  
<span data-ttu-id="1d660-235">A BitOr függvény megadott bits értéket állítja be.</span><span class="sxs-lookup"><span data-stu-id="1d660-235">The BitOr function sets specified bits on a value.</span></span>

<span data-ttu-id="1d660-236">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-236">**Syntax:**</span></span>  
`num BitOr(num value1, num value2)`

* <span data-ttu-id="1d660-237">Érték1, érték2: numerikus érték, amely együtt kell-e köztük</span><span class="sxs-lookup"><span data-stu-id="1d660-237">value1, value2: numeric values that should be OR’ed together</span></span>

<span data-ttu-id="1d660-238">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-238">**Remarks:**</span></span>  
<span data-ttu-id="1d660-239">Ez a funkció mindkét paraméter a bináris megjelenítése alakítja át, és beállítja egy kicsit Ha egyik vagy mindkét a megfelelő bit maszk és jelző 1 1 és 0 Ha mindkét megfelelő bit 0.</span><span class="sxs-lookup"><span data-stu-id="1d660-239">This function converts both parameters to the binary representation and sets a bit to 1 if one or both of the corresponding bits in mask and flag are 1, and to 0 if both of the corresponding bits are 0.</span></span> <span data-ttu-id="1d660-240">Ez azt jelenti az 1 értékkel tér vissza minden esetben, ahol a megfelelő mindkét paraméter bitje 0 kivételével.</span><span class="sxs-lookup"><span data-stu-id="1d660-240">In other words, it returns 1 in all cases except where the corresponding bits of both parameters are 0.</span></span>

- - -
### <a name="cbool"></a><span data-ttu-id="1d660-241">CBool</span><span class="sxs-lookup"><span data-stu-id="1d660-241">CBool</span></span>
<span data-ttu-id="1d660-242">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-242">**Description:**</span></span>  
<span data-ttu-id="1d660-243">A CBool függvény a kiértékelt kifejezés alapuló logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="1d660-243">The CBool function returns a Boolean based on the evaluated expression</span></span>

<span data-ttu-id="1d660-244">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-244">**Syntax:**</span></span>  
`bool CBool(exp Expression)`

<span data-ttu-id="1d660-245">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-245">**Remarks:**</span></span>  
<span data-ttu-id="1d660-246">Ha a kifejezés értéke nulla, majd CBool igaz értéket ad vissza, ellenkező esetben a visszatérési hamis.</span><span class="sxs-lookup"><span data-stu-id="1d660-246">If the expression evaluates to a nonzero value, then CBool returns True, else it returns False.</span></span>

<span data-ttu-id="1d660-247">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-247">**Example:**</span></span>  
`CBool([attrib1] = [attrib2])`  

<span data-ttu-id="1d660-248">Értéket ad vissza IGAZ, ha mindkét attribútumok ugyanazon értékekkel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="1d660-248">Returns True if both attributes have the same value.</span></span>

- - -
### <a name="cdate"></a><span data-ttu-id="1d660-249">CDate</span><span class="sxs-lookup"><span data-stu-id="1d660-249">CDate</span></span>
<span data-ttu-id="1d660-250">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-250">**Description:**</span></span>  
<span data-ttu-id="1d660-251">A CDate függvény UTC DateTime egy karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-251">The CDate function returns a UTC DateTime from a string.</span></span> <span data-ttu-id="1d660-252">Dátum és idő nincs szinkronban natív attribútumtípus, de egyes funkciókat használják.</span><span class="sxs-lookup"><span data-stu-id="1d660-252">DateTime is not a native attribute type in Sync but is used by some functions.</span></span>

<span data-ttu-id="1d660-253">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-253">**Syntax:**</span></span>  
`dt CDate(str value)`

* <span data-ttu-id="1d660-254">Érték: A karakterlánc dátum, idő, és opcionálisan időzóna</span><span class="sxs-lookup"><span data-stu-id="1d660-254">Value: A string with a date, time, and optionally time zone</span></span>

<span data-ttu-id="1d660-255">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-255">**Remarks:**</span></span>  
<span data-ttu-id="1d660-256">A visszaadott karakterlánc mindig UTC formátumban van.</span><span class="sxs-lookup"><span data-stu-id="1d660-256">The returned string is always in UTC.</span></span>

<span data-ttu-id="1d660-257">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-257">**Example:**</span></span>  
`CDate([employeeStartTime])`  
<span data-ttu-id="1d660-258">Egy dátum és idő alapján az alkalmazott beolvasása kezdési időpontja</span><span class="sxs-lookup"><span data-stu-id="1d660-258">Returns a DateTime based on the employee’s start time</span></span>

`CDate("2013-01-10 4:00 PM -8")`  
<span data-ttu-id="1d660-259">Visszaadja egy jelölő dátum és idő "2013-01-11 12:00-kor"</span><span class="sxs-lookup"><span data-stu-id="1d660-259">Returns a DateTime representing "2013-01-11 12:00 AM"</span></span>








- - -
### <a name="certextensionoids"></a><span data-ttu-id="1d660-260">CertExtensionOids</span><span class="sxs-lookup"><span data-stu-id="1d660-260">CertExtensionOids</span></span>
<span data-ttu-id="1d660-261">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-261">**Description:**</span></span>  
<span data-ttu-id="1d660-262">A kritikus bővítmények tanúsítvány objektum Oid értékeit adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-262">Returns the Oid values of all the critical extensions of a certificate object.</span></span>

<span data-ttu-id="1d660-263">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-263">**Syntax:**</span></span>  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   <span data-ttu-id="1d660-264">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-264">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-265">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-265">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certformat"></a><span data-ttu-id="1d660-266">CertFormat</span><span class="sxs-lookup"><span data-stu-id="1d660-266">CertFormat</span></span>
<span data-ttu-id="1d660-267">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-267">**Description:**</span></span>  
<span data-ttu-id="1d660-268">A X.509v3 tanúsítványt formátumú nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-268">Returns the name of the format of this X.509v3 certificate.</span></span>

<span data-ttu-id="1d660-269">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-269">**Syntax:**</span></span>  
`str CertFormat(binary certificateRawData)`  
*   <span data-ttu-id="1d660-270">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-270">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-271">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-271">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certfriendlyname"></a><span data-ttu-id="1d660-272">CertFriendlyName</span><span class="sxs-lookup"><span data-stu-id="1d660-272">CertFriendlyName</span></span>
<span data-ttu-id="1d660-273">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-273">**Description:**</span></span>  
<span data-ttu-id="1d660-274">A tanúsítványhoz társított alias adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-274">Returns the associated alias for a certificate.</span></span>

<span data-ttu-id="1d660-275">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-275">**Syntax:**</span></span>  
`str CertFriendlyName(binary certificateRawData)`  
*   <span data-ttu-id="1d660-276">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-276">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-277">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-277">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certhashstring"></a><span data-ttu-id="1d660-278">CertHashString</span><span class="sxs-lookup"><span data-stu-id="1d660-278">CertHashString</span></span>
<span data-ttu-id="1d660-279">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-279">**Description:**</span></span>  
<span data-ttu-id="1d660-280">Egy hexadecimális karakterláncban a X.509v3 tanúsítványt SHA1 ujjlenyomat értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-280">Returns the SHA1 hash value for the X.509v3 certificate as a hexadecimal string.</span></span>

<span data-ttu-id="1d660-281">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-281">**Syntax:**</span></span>  
`str CertHashString(binary certificateRawData)`  
*   <span data-ttu-id="1d660-282">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-282">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-283">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-283">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissuer"></a><span data-ttu-id="1d660-284">CertIssuer</span><span class="sxs-lookup"><span data-stu-id="1d660-284">CertIssuer</span></span>
<span data-ttu-id="1d660-285">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-285">**Description:**</span></span>  
<span data-ttu-id="1d660-286">A X.509v3 tanúsítványt kiállító hitelesítésszolgáltató nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-286">Returns the name of the certificate authority that issued the X.509v3 certificate.</span></span>

<span data-ttu-id="1d660-287">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-287">**Syntax:**</span></span>  
`str CertIssuer(binary certificateRawData)`  
*   <span data-ttu-id="1d660-288">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-288">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-289">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-289">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissuerdn"></a><span data-ttu-id="1d660-290">CertIssuerDN</span><span class="sxs-lookup"><span data-stu-id="1d660-290">CertIssuerDN</span></span>
<span data-ttu-id="1d660-291">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-291">**Description:**</span></span>  
<span data-ttu-id="1d660-292">A tanúsítvány kiállítójának megkülönböztető nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-292">Returns the distinguished name of the certificate issuer.</span></span>

<span data-ttu-id="1d660-293">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-293">**Syntax:**</span></span>  
`str CertIssuerDN(binary certificateRawData)`  
*   <span data-ttu-id="1d660-294">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-294">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-295">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-295">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissueroid"></a><span data-ttu-id="1d660-296">CertIssuerOid</span><span class="sxs-lookup"><span data-stu-id="1d660-296">CertIssuerOid</span></span>
<span data-ttu-id="1d660-297">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-297">**Description:**</span></span>  
<span data-ttu-id="1d660-298">Az objektumazonosító, a tanúsítvány kiállítójának adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-298">Returns the Oid of the certificate issuer.</span></span>

<span data-ttu-id="1d660-299">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-299">**Syntax:**</span></span>  
`str CertIssuerOid(binary certificateRawData)`  
*   <span data-ttu-id="1d660-300">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-300">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-301">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-301">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certkeyalgorithm"></a><span data-ttu-id="1d660-302">CertKeyAlgorithm</span><span class="sxs-lookup"><span data-stu-id="1d660-302">CertKeyAlgorithm</span></span>
<span data-ttu-id="1d660-303">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-303">**Description:**</span></span>  
<span data-ttu-id="1d660-304">Ezt a X.509v3 tanúsítványt karakterláncként nyilvánoskulcs-képzési algoritmus információkat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-304">Returns the key algorithm information for this X.509v3 certificate as a string.</span></span>

<span data-ttu-id="1d660-305">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-305">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="1d660-306">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-306">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-307">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-307">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certkeyalgorithmparams"></a><span data-ttu-id="1d660-308">CertKeyAlgorithmParams</span><span class="sxs-lookup"><span data-stu-id="1d660-308">CertKeyAlgorithmParams</span></span>
<span data-ttu-id="1d660-309">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-309">**Description:**</span></span>  
<span data-ttu-id="1d660-310">A nyilvánoskulcs-képzési algoritmus a X.509v3 tanúsítványt paramétereinek Hexadecimális karakterláncként adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-310">Returns the key algorithm parameters for the X.509v3 certificate as a hexadecimal string.</span></span>

<span data-ttu-id="1d660-311">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-311">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="1d660-312">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-312">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-313">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-313">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certnameinfo"></a><span data-ttu-id="1d660-314">CertNameInfo</span><span class="sxs-lookup"><span data-stu-id="1d660-314">CertNameInfo</span></span>
<span data-ttu-id="1d660-315">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-315">**Description:**</span></span>  
<span data-ttu-id="1d660-316">Ad vissza a tulajdonos és a kiállító nevét a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="1d660-316">Returns the subject and issuer names from a certificate.</span></span>

<span data-ttu-id="1d660-317">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-317">**Syntax:**</span></span>  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   <span data-ttu-id="1d660-318">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-318">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-319">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-319">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>
*   <span data-ttu-id="1d660-320">X509NameType: A tulajdonos X509NameType értéke.</span><span class="sxs-lookup"><span data-stu-id="1d660-320">X509NameType: The X509NameType value for the subject.</span></span>
*   <span data-ttu-id="1d660-321">includesIssuerName: igaz értékre közé tartozik a kibocsátó neve; Ellenkező esetben hamis.</span><span class="sxs-lookup"><span data-stu-id="1d660-321">includesIssuerName: true to include the issuer name; otherwise, false.</span></span>

- - -
### <a name="certnotafter"></a><span data-ttu-id="1d660-322">CertNotAfter</span><span class="sxs-lookup"><span data-stu-id="1d660-322">CertNotAfter</span></span>
<span data-ttu-id="1d660-323">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-323">**Description:**</span></span>  
<span data-ttu-id="1d660-324">A dátumot adja vissza, amely után a tanúsítvány már nem érvényes helyi idő.</span><span class="sxs-lookup"><span data-stu-id="1d660-324">Returns the date in local time after which a certificate is no longer valid.</span></span>

<span data-ttu-id="1d660-325">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-325">**Syntax:**</span></span>  
`dt CertNotAfter(binary certificateRawData)`  
*   <span data-ttu-id="1d660-326">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-326">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-327">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-327">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certnotbefore"></a><span data-ttu-id="1d660-328">CertNotBefore</span><span class="sxs-lookup"><span data-stu-id="1d660-328">CertNotBefore</span></span>
<span data-ttu-id="1d660-329">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-329">**Description:**</span></span>  
<span data-ttu-id="1d660-330">A tanúsítvány hatályba lép, a helyi idő a dátumot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-330">Returns the date in local time on which a certificate becomes valid.</span></span>

<span data-ttu-id="1d660-331">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-331">**Syntax:**</span></span>  
`dt CertNotBefore(binary certificateRawData)`  
*   <span data-ttu-id="1d660-332">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-332">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-333">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-333">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certpublickeyoid"></a><span data-ttu-id="1d660-334">CertPublicKeyOid</span><span class="sxs-lookup"><span data-stu-id="1d660-334">CertPublicKeyOid</span></span>
<span data-ttu-id="1d660-335">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-335">**Description:**</span></span>  
<span data-ttu-id="1d660-336">Az objektumazonosító a X.509v3 tanúsítványhoz tartozó nyilvános kulcs adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-336">Returns the Oid of the public key for the X.509v3 certificate.</span></span>

<span data-ttu-id="1d660-337">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-337">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="1d660-338">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-338">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-339">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-339">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certpublickeyparametersoid"></a><span data-ttu-id="1d660-340">CertPublicKeyParametersOid</span><span class="sxs-lookup"><span data-stu-id="1d660-340">CertPublicKeyParametersOid</span></span>
<span data-ttu-id="1d660-341">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-341">**Description:**</span></span>  
<span data-ttu-id="1d660-342">Az objektumazonosító, a nyilvános kulcs paramétereit a X.509v3 tanúsítványt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-342">Returns the Oid of the public key parameters for the X.509v3 certificate.</span></span>

<span data-ttu-id="1d660-343">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-343">**Syntax:**</span></span>  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   <span data-ttu-id="1d660-344">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-344">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-345">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-345">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certserialnumber"></a><span data-ttu-id="1d660-346">CertSerialNumber</span><span class="sxs-lookup"><span data-stu-id="1d660-346">CertSerialNumber</span></span>
<span data-ttu-id="1d660-347">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-347">**Description:**</span></span>  
<span data-ttu-id="1d660-348">A X.509v3 tanúsítvány sorozatszámát adja eredményül.</span><span class="sxs-lookup"><span data-stu-id="1d660-348">Returns the serial number of the X.509v3 certificate.</span></span>

<span data-ttu-id="1d660-349">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-349">**Syntax:**</span></span>  
`str CertSerialNumber(binary certificateRawData)`  
*   <span data-ttu-id="1d660-350">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-350">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-351">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-351">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsignaturealgorithmoid"></a><span data-ttu-id="1d660-352">CertSignatureAlgorithmOid</span><span class="sxs-lookup"><span data-stu-id="1d660-352">CertSignatureAlgorithmOid</span></span>
<span data-ttu-id="1d660-353">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-353">**Description:**</span></span>  
<span data-ttu-id="1d660-354">Az objektumazonosító, a tanúsítvány aláírásának létrehozására használt algoritmus adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-354">Returns the Oid of the algorithm used to create the signature of a certificate.</span></span>

<span data-ttu-id="1d660-355">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-355">**Syntax:**</span></span>  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   <span data-ttu-id="1d660-356">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-356">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-357">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-357">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubject"></a><span data-ttu-id="1d660-358">CertSubject</span><span class="sxs-lookup"><span data-stu-id="1d660-358">CertSubject</span></span>
<span data-ttu-id="1d660-359">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-359">**Description:**</span></span>  
<span data-ttu-id="1d660-360">A tanúsítvány tulajdonosának megkülönböztető nevét lekérése.</span><span class="sxs-lookup"><span data-stu-id="1d660-360">Gets the subject distinguished name from a certificate.</span></span>

<span data-ttu-id="1d660-361">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-361">**Syntax:**</span></span>  
`str CertSubject(binary certificateRawData)`  
*   <span data-ttu-id="1d660-362">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-362">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-363">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-363">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubjectnamedn"></a><span data-ttu-id="1d660-364">CertSubjectNameDN</span><span class="sxs-lookup"><span data-stu-id="1d660-364">CertSubjectNameDN</span></span>
<span data-ttu-id="1d660-365">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-365">**Description:**</span></span>  
<span data-ttu-id="1d660-366">A tanúsítvány tulajdonosának megkülönböztető nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-366">Returns the subject distinguished name from a certificate.</span></span>

<span data-ttu-id="1d660-367">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-367">**Syntax:**</span></span>  
`str CertSubjectNameDN(binary certificateRawData)`  
*   <span data-ttu-id="1d660-368">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-368">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-369">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-369">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubjectnameoid"></a><span data-ttu-id="1d660-370">CertSubjectNameOid</span><span class="sxs-lookup"><span data-stu-id="1d660-370">CertSubjectNameOid</span></span>
<span data-ttu-id="1d660-371">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-371">**Description:**</span></span>  
<span data-ttu-id="1d660-372">Egy tanúsítványt az objektumazonosító, a tulajdonos nevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-372">Returns the Oid of the subject name from a certificate.</span></span>

<span data-ttu-id="1d660-373">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-373">**Syntax:**</span></span>  
`str CertSubjectNameOid(binary certificateRawData)`  
*   <span data-ttu-id="1d660-374">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-374">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-375">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-375">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certthumbprint"></a><span data-ttu-id="1d660-376">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="1d660-376">CertThumbprint</span></span>
<span data-ttu-id="1d660-377">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-377">**Description:**</span></span>  
<span data-ttu-id="1d660-378">A tanúsítvány ujjlenyomatát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-378">Returns the thumbprint of a certificate.</span></span>

<span data-ttu-id="1d660-379">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-379">**Syntax:**</span></span>  
`str CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="1d660-380">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-380">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-381">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-381">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certversion"></a><span data-ttu-id="1d660-382">CertVersion</span><span class="sxs-lookup"><span data-stu-id="1d660-382">CertVersion</span></span>
<span data-ttu-id="1d660-383">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-383">**Description:**</span></span>  
<span data-ttu-id="1d660-384">A tanúsítvány X.509 formátumú verziója adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-384">Returns the X.509 format version of a certificate.</span></span>

<span data-ttu-id="1d660-385">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-385">**Syntax:**</span></span>  
`str CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="1d660-386">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-386">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-387">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-387">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="cguid"></a><span data-ttu-id="1d660-388">CGuid</span><span class="sxs-lookup"><span data-stu-id="1d660-388">CGuid</span></span>
<span data-ttu-id="1d660-389">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-389">**Description:**</span></span>  
<span data-ttu-id="1d660-390">A CGuid függvény GUID karakterláncos ábrázolása konvertálja a bináris megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="1d660-390">The CGuid function converts the string representation of a GUID to its binary representation.</span></span>

<span data-ttu-id="1d660-391">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-391">**Syntax:**</span></span>  
`bin CGuid(str GUID)`

* <span data-ttu-id="1d660-392">Ebben a mintában formátumú karakterlánc: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx vagy {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span><span class="sxs-lookup"><span data-stu-id="1d660-392">A String formatted in this pattern: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx or {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span></span>

- - -
### <a name="contains"></a><span data-ttu-id="1d660-393">Contains</span><span class="sxs-lookup"><span data-stu-id="1d660-393">Contains</span></span>
<span data-ttu-id="1d660-394">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-394">**Description:**</span></span>  
<span data-ttu-id="1d660-395">A Contains belüli egy többértékű karakterlánc keresése</span><span class="sxs-lookup"><span data-stu-id="1d660-395">The Contains function finds a string inside a multi-valued attribute</span></span>

<span data-ttu-id="1d660-396">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-396">**Syntax:**</span></span>  
<span data-ttu-id="1d660-397">`num Contains (mvstring attribute, str search)`-nagybetűk</span><span class="sxs-lookup"><span data-stu-id="1d660-397">`num Contains (mvstring attribute, str search)` - case-sensitive</span></span>  
`num Contains (mvstring attribute, str search, enum Casetype)`  
<span data-ttu-id="1d660-398">`num Contains (mvref attribute, str search)`-nagybetűk</span><span class="sxs-lookup"><span data-stu-id="1d660-398">`num Contains (mvref attribute, str search)` - case-sensitive</span></span>

* <span data-ttu-id="1d660-399">attribútum: a többértékű attribútum kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="1d660-399">attribute: the multi-valued attribute to search.</span></span>
* <span data-ttu-id="1d660-400">Keresés: karakterlánc az attribútumban található.</span><span class="sxs-lookup"><span data-stu-id="1d660-400">search: string to find in the attribute.</span></span>
* <span data-ttu-id="1d660-401">Casetype: CaseInsensitive vagy CaseSensitive.</span><span class="sxs-lookup"><span data-stu-id="1d660-401">Casetype: CaseInsensitive or CaseSensitive.</span></span>

<span data-ttu-id="1d660-402">A többértékű attribútum, ahol a karakterlánc található index értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-402">Returns index in the multi-valued attribute where the string was found.</span></span> <span data-ttu-id="1d660-403">0 eredményül, ha a karakterlánc nem található.</span><span class="sxs-lookup"><span data-stu-id="1d660-403">0 is returned if the string is not found.</span></span>

<span data-ttu-id="1d660-404">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-404">**Remarks:**</span></span>  
<span data-ttu-id="1d660-405">Attribútumok a többértékű karakterlánc a keresés karakterláncrészletek megkeresi az értékek.</span><span class="sxs-lookup"><span data-stu-id="1d660-405">For multi-valued string attributes, the search finds substrings in the values.</span></span>  
<span data-ttu-id="1d660-406">A hivatkozási attribútum a keresett karakterlánc pontosan meg kell egyeznie az érték akkor, ha egyezés.</span><span class="sxs-lookup"><span data-stu-id="1d660-406">For reference attributes, the searched string must exactly match the value to be considered a match.</span></span>

<span data-ttu-id="1d660-407">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-407">**Example:**</span></span>  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
<span data-ttu-id="1d660-408">Ha a proxyAddresses attribútum egy elsődleges e-mail címet (nagybetűs által jelzett "SMTP:"), majd térjen vissza a proxyAddress attribútum, ellenkező esetben egy hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="1d660-408">If the proxyAddresses attribute has a primary email address (indicated by uppercase "SMTP:"), then return the proxyAddress attribute, else return an error.</span></span>

- - -
### <a name="convertfrombase64"></a><span data-ttu-id="1d660-409">ConvertFromBase64</span><span class="sxs-lookup"><span data-stu-id="1d660-409">ConvertFromBase64</span></span>
<span data-ttu-id="1d660-410">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-410">**Description:**</span></span>  
<span data-ttu-id="1d660-411">A ConvertFromBase64 függvény a megadott base64-kódolású érték konvertál egy rendszeres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-411">The ConvertFromBase64 function converts the specified base64 encoded value to a regular string.</span></span>

<span data-ttu-id="1d660-412">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-412">**Syntax:**</span></span>  
<span data-ttu-id="1d660-413">`str ConvertFromBase64(str source)`-feltételezi, hogy a Unicode kódolási</span><span class="sxs-lookup"><span data-stu-id="1d660-413">`str ConvertFromBase64(str source)` - assumes Unicode for encoding</span></span>  
`str ConvertFromBase64(str source, enum Encoding)`

* <span data-ttu-id="1d660-414">Forrás: Base64 kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d660-414">source: Base64 encoded string</span></span>  
* <span data-ttu-id="1d660-415">Kódolást: Unicode, ASCII, UTF8</span><span class="sxs-lookup"><span data-stu-id="1d660-415">Encoding: Unicode, ASCII, UTF8</span></span>

<span data-ttu-id="1d660-416">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-416">**Example**</span></span>  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

<span data-ttu-id="1d660-417">Mindkét példák vissza "*Hello world!*"</span><span class="sxs-lookup"><span data-stu-id="1d660-417">Both examples return "*Hello world!*"</span></span>

- - -
### <a name="convertfromutf8hex"></a><span data-ttu-id="1d660-418">ConvertFromUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="1d660-418">ConvertFromUTF8Hex</span></span>
<span data-ttu-id="1d660-419">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-419">**Description:**</span></span>  
<span data-ttu-id="1d660-420">A ConvertFromUTF8Hex függvény a megadott kódolású UTF8 hexadecimális érték karakterlánccá alakítja át.</span><span class="sxs-lookup"><span data-stu-id="1d660-420">The ConvertFromUTF8Hex function converts the specified UTF8 Hex encoded value to a string.</span></span>

<span data-ttu-id="1d660-421">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-421">**Syntax:**</span></span>  
`str ConvertFromUTF8Hex(str source)`

* <span data-ttu-id="1d660-422">Forrás: UTF8 2 bájtos kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d660-422">source: UTF8 2-byte encoded sting</span></span>

<span data-ttu-id="1d660-423">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-423">**Remarks:**</span></span>  
<span data-ttu-id="1d660-424">A függvény és, hogy a megkülönböztető név attribútum rövid-e az eredmény a ConvertFromBase64([],UTF8) közötti különbség.</span><span class="sxs-lookup"><span data-stu-id="1d660-424">The difference between this function and ConvertFromBase64([],UTF8) in that the result is friendly for the DN attribute.</span></span>  
<span data-ttu-id="1d660-425">Ezt a formátumot használja Azure Active Directory megkülönböztető neve.</span><span class="sxs-lookup"><span data-stu-id="1d660-425">This format is used by Azure Active Directory as DN.</span></span>

<span data-ttu-id="1d660-426">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-426">**Example:**</span></span>  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
<span data-ttu-id="1d660-427">Értéket ad vissza "*Hello world!*"</span><span class="sxs-lookup"><span data-stu-id="1d660-427">Returns "*Hello world!*"</span></span>

- - -
### <a name="converttobase64"></a><span data-ttu-id="1d660-428">ConvertToBase64</span><span class="sxs-lookup"><span data-stu-id="1d660-428">ConvertToBase64</span></span>
<span data-ttu-id="1d660-429">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-429">**Description:**</span></span>  
<span data-ttu-id="1d660-430">A ConvertToBase64 függvény egy karakterlánc konvertál egy Unicode Base64 kódolású karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-430">The ConvertToBase64 function converts a string to a Unicode base64 string.</span></span>  
<span data-ttu-id="1d660-431">Konvertálja az értéket egy tömb egész számok a megfelelő karakterlánc-ábrázolása számjegyükkel base-64 kódolású.</span><span class="sxs-lookup"><span data-stu-id="1d660-431">Converts the value of an array of integers to its equivalent string representation that is encoded with base-64 digits.</span></span>

<span data-ttu-id="1d660-432">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-432">**Syntax:**</span></span>  
`str ConvertToBase64(str source)`

<span data-ttu-id="1d660-433">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-433">**Example:**</span></span>  
`ConvertToBase64("Hello world!")`  
<span data-ttu-id="1d660-434">Visszaadja a "SABlAGwAbABvACAAdwBvAHIAbABkACEA"</span><span class="sxs-lookup"><span data-stu-id="1d660-434">Returns "SABlAGwAbABvACAAdwBvAHIAbABkACEA"</span></span>

- - -
### <a name="converttoutf8hex"></a><span data-ttu-id="1d660-435">ConvertToUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="1d660-435">ConvertToUTF8Hex</span></span>
<span data-ttu-id="1d660-436">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-436">**Description:**</span></span>  
<span data-ttu-id="1d660-437">A ConvertToUTF8Hex függvény egy karakterlánc UTF8 hexadecimális encoded értékre konvertálja.</span><span class="sxs-lookup"><span data-stu-id="1d660-437">The ConvertToUTF8Hex function converts a string to a UTF8 Hex encoded value.</span></span>

<span data-ttu-id="1d660-438">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-438">**Syntax:**</span></span>  
`str ConvertToUTF8Hex(str source)`

<span data-ttu-id="1d660-439">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-439">**Remarks:**</span></span>  
<span data-ttu-id="1d660-440">A kimeneti formátum függvény lesz az Azure Active Directory megkülönböztető név attribútum formátuma.</span><span class="sxs-lookup"><span data-stu-id="1d660-440">The output format of this function is used by Azure Active Directory as DN attribute format.</span></span>

<span data-ttu-id="1d660-441">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-441">**Example:**</span></span>  
`ConvertToUTF8Hex("Hello world!")`  
<span data-ttu-id="1d660-442">48656C6C6F20776F726C6421 beolvasása</span><span class="sxs-lookup"><span data-stu-id="1d660-442">Returns 48656C6C6F20776F726C6421</span></span>

- - -
### <a name="count"></a><span data-ttu-id="1d660-443">Darabszám</span><span class="sxs-lookup"><span data-stu-id="1d660-443">Count</span></span>
<span data-ttu-id="1d660-444">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-444">**Description:**</span></span>  
<span data-ttu-id="1d660-445">A Count függvénnyel elemek számát adja vissza egy többértékű attribútum</span><span class="sxs-lookup"><span data-stu-id="1d660-445">The Count function returns the number of elements in a multi-valued attribute</span></span>

<span data-ttu-id="1d660-446">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-446">**Syntax:**</span></span>  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a><span data-ttu-id="1d660-447">CNum</span><span class="sxs-lookup"><span data-stu-id="1d660-447">CNum</span></span>
<span data-ttu-id="1d660-448">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-448">**Description:**</span></span>  
<span data-ttu-id="1d660-449">A CNum függvény lép egy karakterláncot, és adja vissza egy numerikus adattípusú.</span><span class="sxs-lookup"><span data-stu-id="1d660-449">The CNum function takes a string and returns a numeric data type.</span></span>

<span data-ttu-id="1d660-450">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-450">**Syntax:**</span></span>  
`num CNum(str value)`

- - -
### <a name="cref"></a><span data-ttu-id="1d660-451">CRef</span><span class="sxs-lookup"><span data-stu-id="1d660-451">CRef</span></span>
<span data-ttu-id="1d660-452">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-452">**Description:**</span></span>  
<span data-ttu-id="1d660-453">Alakít át egy hivatkozási attribútum</span><span class="sxs-lookup"><span data-stu-id="1d660-453">Converts a string to a reference attribute</span></span>

<span data-ttu-id="1d660-454">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-454">**Syntax:**</span></span>  
`ref CRef(str value)`

<span data-ttu-id="1d660-455">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-455">**Example:**</span></span>  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

- - -
### <a name="cstr"></a><span data-ttu-id="1d660-456">CStr</span><span class="sxs-lookup"><span data-stu-id="1d660-456">CStr</span></span>
<span data-ttu-id="1d660-457">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-457">**Description:**</span></span>  
<span data-ttu-id="1d660-458">A CStr függvény egy karakterlánc adattípusra konvertálja.</span><span class="sxs-lookup"><span data-stu-id="1d660-458">The CStr function converts to a string data type.</span></span>

<span data-ttu-id="1d660-459">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-459">**Syntax:**</span></span>  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* <span data-ttu-id="1d660-460">érték: egy numerikus értéket, a hivatkozási attribútum, vagy a logikai érték lehet.</span><span class="sxs-lookup"><span data-stu-id="1d660-460">value: Can be a numeric value, reference attribute, or Boolean.</span></span>

<span data-ttu-id="1d660-461">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-461">**Example:**</span></span>  
`CStr([dn])`  
<span data-ttu-id="1d660-462">Visszaadhatja "cn = Joe, dc = contoso, dc = com"</span><span class="sxs-lookup"><span data-stu-id="1d660-462">Could return "cn=Joe,dc=contoso,dc=com"</span></span>

- - -
### <a name="dateadd"></a><span data-ttu-id="1d660-463">DateAdd</span><span class="sxs-lookup"><span data-stu-id="1d660-463">DateAdd</span></span>
<span data-ttu-id="1d660-464">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-464">**Description:**</span></span>  
<span data-ttu-id="1d660-465">Egy dátumot, amelyhez a megadott időtartam hozzáadását tartalmazó dátumot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-465">Returns a Date containing a date to which a specified time interval has been added.</span></span>

<span data-ttu-id="1d660-466">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-466">**Syntax:**</span></span>  
`dt DateAdd(str interval, num value, dt date)`

* <span data-ttu-id="1d660-467">időköz: karakterlánc-kifejezés, amely hozzá szeretné adni az időköz.</span><span class="sxs-lookup"><span data-stu-id="1d660-467">interval: String expression that is the interval of time you want to add.</span></span> <span data-ttu-id="1d660-468">A karakterlánc a következő értékek egyikével kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="1d660-468">The string must have one of the following values:</span></span>
  * <span data-ttu-id="1d660-469">ÉÉÉÉ év</span><span class="sxs-lookup"><span data-stu-id="1d660-469">yyyy Year</span></span>
  * <span data-ttu-id="1d660-470">q negyedév</span><span class="sxs-lookup"><span data-stu-id="1d660-470">q Quarter</span></span>
  * <span data-ttu-id="1d660-471">m hónap</span><span class="sxs-lookup"><span data-stu-id="1d660-471">m Month</span></span>
  * <span data-ttu-id="1d660-472">y év napja</span><span class="sxs-lookup"><span data-stu-id="1d660-472">y Day of year</span></span>
  * <span data-ttu-id="1d660-473">d nap</span><span class="sxs-lookup"><span data-stu-id="1d660-473">d Day</span></span>
  * <span data-ttu-id="1d660-474">w hét napja</span><span class="sxs-lookup"><span data-stu-id="1d660-474">w Weekday</span></span>
  * <span data-ttu-id="1d660-475">hh hét</span><span class="sxs-lookup"><span data-stu-id="1d660-475">ww Week</span></span>
  * <span data-ttu-id="1d660-476">h óra</span><span class="sxs-lookup"><span data-stu-id="1d660-476">h Hour</span></span>
  * <span data-ttu-id="1d660-477">n perc</span><span class="sxs-lookup"><span data-stu-id="1d660-477">n Minute</span></span>
  * <span data-ttu-id="1d660-478">s második</span><span class="sxs-lookup"><span data-stu-id="1d660-478">s Second</span></span>
* <span data-ttu-id="1d660-479">érték: A hozzáadni kívánt egységek számát.</span><span class="sxs-lookup"><span data-stu-id="1d660-479">value: The number of units you want to add.</span></span> <span data-ttu-id="1d660-480">(A jövőbeli dátumok eléréséhez) pozitív vagy negatív (dátuma a múltban eléréséhez) lehet.</span><span class="sxs-lookup"><span data-stu-id="1d660-480">It can be positive (to get dates in the future) or negative (to get dates in the past).</span></span>
* <span data-ttu-id="1d660-481">dátum: DateTime jelölő dátum, amelyhez az időköz kerül.</span><span class="sxs-lookup"><span data-stu-id="1d660-481">date: DateTime representing date to which the interval is added.</span></span>

<span data-ttu-id="1d660-482">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-482">**Example:**</span></span>  
`DateAdd("m", 3, CDate("2001-01-01"))`  
<span data-ttu-id="1d660-483">3 hónapos hozzáadja, és egy dátum és idő "2001-04-01" jelző adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-483">Adds 3 months and returns a DateTime representing "2001-04-01".</span></span>

- - -
### <a name="datefromnum"></a><span data-ttu-id="1d660-484">DateFromNum</span><span class="sxs-lookup"><span data-stu-id="1d660-484">DateFromNum</span></span>
<span data-ttu-id="1d660-485">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-485">**Description:**</span></span>  
<span data-ttu-id="1d660-486">A DateFromNum függvény konvertál egy értéket AD meg dátum formátumú dátum és idő típusra.</span><span class="sxs-lookup"><span data-stu-id="1d660-486">The DateFromNum function converts a value in AD’s date format to a DateTime type.</span></span>

<span data-ttu-id="1d660-487">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-487">**Syntax:**</span></span>  
`dt DateFromNum(num value)`

<span data-ttu-id="1d660-488">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-488">**Example:**</span></span>  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
<span data-ttu-id="1d660-489">Visszaadja a 2012-01-01 jelölő dátum és idő 23:00:00</span><span class="sxs-lookup"><span data-stu-id="1d660-489">Returns a DateTime representing 2012-01-01 23:00:00</span></span>

- - -
### <a name="dncomponent"></a><span data-ttu-id="1d660-490">DNComponent</span><span class="sxs-lookup"><span data-stu-id="1d660-490">DNComponent</span></span>
<span data-ttu-id="1d660-491">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-491">**Description:**</span></span>  
<span data-ttu-id="1d660-492">A DNComponent függvény bal üzembe helyezésről meghatározott DN összetevője értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-492">The DNComponent function returns the value of a specified DN component going from left.</span></span>

<span data-ttu-id="1d660-493">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-493">**Syntax:**</span></span>  
`str DNComponent(ref dn, num ComponentNumber)`

* <span data-ttu-id="1d660-494">megkülönböztető név: a hivatkozási attribútum értelmezése</span><span class="sxs-lookup"><span data-stu-id="1d660-494">dn: the reference attribute to interpret</span></span>
* <span data-ttu-id="1d660-495">ComponentNumber: A DN vissza az összetevő</span><span class="sxs-lookup"><span data-stu-id="1d660-495">ComponentNumber: The component in the DN to return</span></span>

<span data-ttu-id="1d660-496">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-496">**Example:**</span></span>  
`DNComponent([dn],1)`  
<span data-ttu-id="1d660-497">Megkülönböztető név esetén "cn = Joe, ou =...," Joe adja vissza</span><span class="sxs-lookup"><span data-stu-id="1d660-497">If dn is "cn=Joe,ou=…," it returns Joe</span></span>

- - -
### <a name="dncomponentrev"></a><span data-ttu-id="1d660-498">DNComponentRev</span><span class="sxs-lookup"><span data-stu-id="1d660-498">DNComponentRev</span></span>
<span data-ttu-id="1d660-499">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-499">**Description:**</span></span>  
<span data-ttu-id="1d660-500">A DNComponentRev függvény jobb (záró) üzembe helyezésről meghatározott DN összetevője értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-500">The DNComponentRev function returns the value of a specified DN component going from right (the end).</span></span>

<span data-ttu-id="1d660-501">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-501">**Syntax:**</span></span>  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* <span data-ttu-id="1d660-502">megkülönböztető név: a hivatkozási attribútum értelmezése</span><span class="sxs-lookup"><span data-stu-id="1d660-502">dn: the reference attribute to interpret</span></span>
* <span data-ttu-id="1d660-503">ComponentNumber - való visszatéréshez DN az összetevőt</span><span class="sxs-lookup"><span data-stu-id="1d660-503">ComponentNumber - The component in the DN to return</span></span>
* <span data-ttu-id="1d660-504">Beállítások: DC – figyelmen kívül az összes összetevő "dc ="</span><span class="sxs-lookup"><span data-stu-id="1d660-504">Options: DC – Ignore all components with "dc="</span></span>

<span data-ttu-id="1d660-505">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-505">**Example:**</span></span>  
<span data-ttu-id="1d660-506">Megkülönböztető név esetén "cn Joe, ou = Atlanta –, ou = GA, ou = = US, dc = contoso, dc = com" majd</span><span class="sxs-lookup"><span data-stu-id="1d660-506">If dn is "cn=Joe,ou=Atlanta,ou=GA,ou=US, dc=contoso,dc=com" then</span></span>  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
<span data-ttu-id="1d660-507">Mindkét vissza VELÜNK.</span><span class="sxs-lookup"><span data-stu-id="1d660-507">Both return US.</span></span>

- - -
### <a name="error"></a><span data-ttu-id="1d660-508">Hiba</span><span class="sxs-lookup"><span data-stu-id="1d660-508">Error</span></span>
<span data-ttu-id="1d660-509">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-509">**Description:**</span></span>  
<span data-ttu-id="1d660-510">A hiba függvényt ad vissza egy egyéni hiba okozta.</span><span class="sxs-lookup"><span data-stu-id="1d660-510">The Error function is used to return a custom error.</span></span>

<span data-ttu-id="1d660-511">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-511">**Syntax:**</span></span>  
`void Error(str ErrorMessage)`

<span data-ttu-id="1d660-512">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-512">**Example:**</span></span>  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
<span data-ttu-id="1d660-513">Ha az attribútum accountName nincs jelen, throw hiba az objektum.</span><span class="sxs-lookup"><span data-stu-id="1d660-513">If the attribute accountName is not present, throw an error on the object.</span></span>

- - -
### <a name="escapedncomponent"></a><span data-ttu-id="1d660-514">EscapeDNComponent</span><span class="sxs-lookup"><span data-stu-id="1d660-514">EscapeDNComponent</span></span>
<span data-ttu-id="1d660-515">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-515">**Description:**</span></span>  
<span data-ttu-id="1d660-516">Az EscapeDNComponent függvény időt vesz igénybe a DN egy összetevőt, és így ábrázolhatók az LDAP-kiszolgálón lehet kilépni.</span><span class="sxs-lookup"><span data-stu-id="1d660-516">The EscapeDNComponent function takes one component of a DN and escapes it so it can be represented in LDAP.</span></span>

<span data-ttu-id="1d660-517">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-517">**Syntax:**</span></span>  
`str EscapeDNComponent(str value)`

<span data-ttu-id="1d660-518">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-518">**Example:**</span></span>  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
<span data-ttu-id="1d660-519">Ellenőrzi, hogy az objektum egy LDAP-címtár is létrehozható, akkor is, ha a displayName attribútum karakterek, amelyek az LDAP-kiszolgálón kell megjelölni.</span><span class="sxs-lookup"><span data-stu-id="1d660-519">Makes sure the object can be created in an LDAP directory even if the displayName attribute has characters that must be escaped in LDAP.</span></span>

- - -
### <a name="formatdatetime"></a><span data-ttu-id="1d660-520">FormatDateTime</span><span class="sxs-lookup"><span data-stu-id="1d660-520">FormatDateTime</span></span>
<span data-ttu-id="1d660-521">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-521">**Description:**</span></span>  
<span data-ttu-id="1d660-522">A FormatDateTime függvény segítségével egy dátum/idő karakterláncot formázni a megadott formátumban</span><span class="sxs-lookup"><span data-stu-id="1d660-522">The FormatDateTime function is used to format a DateTime to a string with a specified format</span></span>

<span data-ttu-id="1d660-523">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-523">**Syntax:**</span></span>  
`str FormatDateTime(dt value, str format)`

* <span data-ttu-id="1d660-524">érték: a dátum és idő formátumú érték</span><span class="sxs-lookup"><span data-stu-id="1d660-524">value: a value in the DateTime format</span></span>
* <span data-ttu-id="1d660-525">formátum: átalakítása formátum képviselő karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="1d660-525">format: a string representing the format to convert to.</span></span>

<span data-ttu-id="1d660-526">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-526">**Remarks:**</span></span>  
<span data-ttu-id="1d660-527">A lehetséges értékek a formátum itt található: [felhasználói dátum és idő formátumú (Format függvény)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)</span><span class="sxs-lookup"><span data-stu-id="1d660-527">The possible values for the format can be found here: [User-Defined Date/Time Formats (Format Function)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)</span></span>

<span data-ttu-id="1d660-528">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-528">**Example:**</span></span>  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
<span data-ttu-id="1d660-529">"2007-12-25" eredményez.</span><span class="sxs-lookup"><span data-stu-id="1d660-529">Results in "2007-12-25".</span></span>

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
<span data-ttu-id="1d660-530">"20140905081453.0Z" eredményezhet</span><span class="sxs-lookup"><span data-stu-id="1d660-530">Can result in "20140905081453.0Z"</span></span>

- - -
### <a name="guid"></a><span data-ttu-id="1d660-531">GUID</span><span class="sxs-lookup"><span data-stu-id="1d660-531">GUID</span></span>
<span data-ttu-id="1d660-532">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-532">**Description:**</span></span>  
<span data-ttu-id="1d660-533">A függvény GUID hoz létre egy új véletlenszerű GUID-ja</span><span class="sxs-lookup"><span data-stu-id="1d660-533">The function GUID generates a new random GUID</span></span>

<span data-ttu-id="1d660-534">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-534">**Syntax:**</span></span>  
`str GUID()`

- - -
### <a name="iif"></a><span data-ttu-id="1d660-535">AZ IIF</span><span class="sxs-lookup"><span data-stu-id="1d660-535">IIF</span></span>
<span data-ttu-id="1d660-536">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-536">**Description:**</span></span>  
<span data-ttu-id="1d660-537">Az IIF függvény a megadott feltétel alapján lehetséges értékek egyikét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-537">The IIF function returns one of a set of possible values based on a specified condition.</span></span>

<span data-ttu-id="1d660-538">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-538">**Syntax:**</span></span>  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* <span data-ttu-id="1d660-539">feltétel: bármely érték vagy kifejezés, amelynek kiértékelése igaz vagy hamis.</span><span class="sxs-lookup"><span data-stu-id="1d660-539">condition: any value or expression that can be evaluated to true or false.</span></span>
* <span data-ttu-id="1d660-540">valueIfTrue: Ha a feltétel igaz, a visszaadott érték.</span><span class="sxs-lookup"><span data-stu-id="1d660-540">valueIfTrue: If the condition evaluates to true, the returned value.</span></span>
* <span data-ttu-id="1d660-541">valueIfFalse: Ha a feltétel hamis, a visszaadott érték.</span><span class="sxs-lookup"><span data-stu-id="1d660-541">valueIfFalse: If the condition evaluates to false, the returned value.</span></span>

<span data-ttu-id="1d660-542">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-542">**Example:**</span></span>  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 <span data-ttu-id="1d660-543">Ha a felhasználó egy internalizálási, adja vissza egy felhasználó a "t-", más elejéhez adja hozzá az aliast, a felhasználói alias adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-543">If the user is an intern, returns the alias of a user with "t-" added to the beginning of it, else returns the user’s alias as is.</span></span>

- - -
### <a name="instr"></a><span data-ttu-id="1d660-544">InStr</span><span class="sxs-lookup"><span data-stu-id="1d660-544">InStr</span></span>
<span data-ttu-id="1d660-545">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-545">**Description:**</span></span>  
<span data-ttu-id="1d660-546">InStr keresése a első előfordulása karakterláncrész egy karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="1d660-546">The InStr function finds the first occurrence of a substring in a string</span></span>

<span data-ttu-id="1d660-547">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-547">**Syntax:**</span></span>  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* <span data-ttu-id="1d660-548">stringcheck: karakterláncának keresése</span><span class="sxs-lookup"><span data-stu-id="1d660-548">stringcheck: string to be searched</span></span>
* <span data-ttu-id="1d660-549">stringmatch: keresett karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d660-549">stringmatch: string to be found</span></span>
* <span data-ttu-id="1d660-550">Start: kiindulási helyzet a karakterláncrész keresése</span><span class="sxs-lookup"><span data-stu-id="1d660-550">start: starting position to find the substring</span></span>
* <span data-ttu-id="1d660-551">Hasonlítsa össze: vbTextCompare vagy vbBinaryCompare</span><span class="sxs-lookup"><span data-stu-id="1d660-551">compare: vbTextCompare or vbBinaryCompare</span></span>

<span data-ttu-id="1d660-552">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-552">**Remarks:**</span></span>  
<span data-ttu-id="1d660-553">A helyét, ahol a substring található, vagy a 0, ha nincs találat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-553">Returns the position where the substring was found or 0 if not found.</span></span>

<span data-ttu-id="1d660-554">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-554">**Example:**</span></span>  
`InStr("The quick brown fox","quick")`  
<span data-ttu-id="1d660-555">5 Evalues</span><span class="sxs-lookup"><span data-stu-id="1d660-555">Evalues to 5</span></span>

`InStr("repEated","e",3,vbBinaryCompare)`  
<span data-ttu-id="1d660-556">7 kiértékelésének eredménye</span><span class="sxs-lookup"><span data-stu-id="1d660-556">Evaluates to 7</span></span>

- - -
### <a name="instrrev"></a><span data-ttu-id="1d660-557">InStrRev</span><span class="sxs-lookup"><span data-stu-id="1d660-557">InStrRev</span></span>
<span data-ttu-id="1d660-558">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-558">**Description:**</span></span>  
<span data-ttu-id="1d660-559">InStrRev keresése a utolsó előfordulása karakterláncrész egy karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="1d660-559">The InStrRev function finds the last occurrence of a substring in a string</span></span>

<span data-ttu-id="1d660-560">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-560">**Syntax:**</span></span>  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* <span data-ttu-id="1d660-561">stringcheck: karakterláncának keresése</span><span class="sxs-lookup"><span data-stu-id="1d660-561">stringcheck: string to be searched</span></span>
* <span data-ttu-id="1d660-562">stringmatch: keresett karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d660-562">stringmatch: string to be found</span></span>
* <span data-ttu-id="1d660-563">Start: kiindulási helyzet a karakterláncrész keresése</span><span class="sxs-lookup"><span data-stu-id="1d660-563">start: starting position to find the substring</span></span>
* <span data-ttu-id="1d660-564">Hasonlítsa össze: vbTextCompare vagy vbBinaryCompare</span><span class="sxs-lookup"><span data-stu-id="1d660-564">compare: vbTextCompare or vbBinaryCompare</span></span>

<span data-ttu-id="1d660-565">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-565">**Remarks:**</span></span>  
<span data-ttu-id="1d660-566">A helyét, ahol a substring található, vagy a 0, ha nincs találat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-566">Returns the position where the substring was found or 0 if not found.</span></span>

<span data-ttu-id="1d660-567">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-567">**Example:**</span></span>  
`InStrRev("abbcdbbbef","bb")`  
<span data-ttu-id="1d660-568">7 beolvasása</span><span class="sxs-lookup"><span data-stu-id="1d660-568">Returns 7</span></span>

- - -
### <a name="isbitset"></a><span data-ttu-id="1d660-569">IsBitSet</span><span class="sxs-lookup"><span data-stu-id="1d660-569">IsBitSet</span></span>
<span data-ttu-id="1d660-570">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-570">**Description:**</span></span>  
<span data-ttu-id="1d660-571">A függvény IsBitSet teszteket, ha egy bit van beállítva, vagy sem</span><span class="sxs-lookup"><span data-stu-id="1d660-571">The function IsBitSet Tests if a bit is set or not</span></span>

<span data-ttu-id="1d660-572">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-572">**Syntax:**</span></span>  
`bool IsBitSet(num value, num flag)`

* <span data-ttu-id="1d660-573">érték: egy numerikus értéket, amely evaluated.flag: egy numerikus érték, amely rendelkezik a kiértékelendő bit</span><span class="sxs-lookup"><span data-stu-id="1d660-573">value: a numeric value that is evaluated.flag: a numeric value that has the bit to be evaluated</span></span>

<span data-ttu-id="1d660-574">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-574">**Example:**</span></span>  
`IsBitSet(&HF,4)`  
<span data-ttu-id="1d660-575">Igaz értéket ad vissza, mert a bit "4" be van állítva a "F" hexadecimális érték</span><span class="sxs-lookup"><span data-stu-id="1d660-575">Returns True because bit "4" is set in the hexadecimal value "F"</span></span>

- - -
### <a name="isdate"></a><span data-ttu-id="1d660-576">IsDate</span><span class="sxs-lookup"><span data-stu-id="1d660-576">IsDate</span></span>
<span data-ttu-id="1d660-577">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-577">**Description:**</span></span>  
<span data-ttu-id="1d660-578">Ha a kifejezés lehet kiértékeli dátum/idő típusként, majd a IsDate függvény eredménye igaz.</span><span class="sxs-lookup"><span data-stu-id="1d660-578">If the expression can be evaluates as a DateTime type, then the IsDate function evaluates to True.</span></span>

<span data-ttu-id="1d660-579">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-579">**Syntax:**</span></span>  
`bool IsDate(var Expression)`

<span data-ttu-id="1d660-580">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-580">**Remarks:**</span></span>  
<span data-ttu-id="1d660-581">Azt határozza meg, ha CDate() sikeres lehet.</span><span class="sxs-lookup"><span data-stu-id="1d660-581">Used to determine if CDate() can be successful.</span></span>

- - -
### <a name="iscert"></a><span data-ttu-id="1d660-582">IsCert</span><span class="sxs-lookup"><span data-stu-id="1d660-582">IsCert</span></span>
<span data-ttu-id="1d660-583">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-583">**Description:**</span></span>  
<span data-ttu-id="1d660-584">Igaz értéket ad eredményül, ha a nyers adatok .NET X509Certificate2 tanúsítványobjektum lehet szerializálni.</span><span class="sxs-lookup"><span data-stu-id="1d660-584">Returns true if the raw data can be serialized into .NET X509Certificate2 certificate object.</span></span>

<span data-ttu-id="1d660-585">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-585">**Syntax:**</span></span>  
`bool CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="1d660-586">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d660-586">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="1d660-587">A bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="1d660-587">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>
- - -
### <a name="isempty"></a><span data-ttu-id="1d660-588">Vagyis IsEmpty</span><span class="sxs-lookup"><span data-stu-id="1d660-588">IsEmpty</span></span>
<span data-ttu-id="1d660-589">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-589">**Description:**</span></span>  
<span data-ttu-id="1d660-590">Ha az attribútum a CS vagy MV szerepel, de üres karakterláncra értékelődik ki, majd a vagyis IsEmpty függvény eredménye igaz.</span><span class="sxs-lookup"><span data-stu-id="1d660-590">If the attribute is present in the CS or MV but evaluates to an empty string, then the IsEmpty function evaluates to True.</span></span>

<span data-ttu-id="1d660-591">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-591">**Syntax:**</span></span>  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a><span data-ttu-id="1d660-592">IsGuid</span><span class="sxs-lookup"><span data-stu-id="1d660-592">IsGuid</span></span>
<span data-ttu-id="1d660-593">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-593">**Description:**</span></span>  
<span data-ttu-id="1d660-594">Ha a karakterlánc egyedi azonosítóvá konvertálhatók, majd a IsGuid függvény értékeli igaz értékre.</span><span class="sxs-lookup"><span data-stu-id="1d660-594">If the string could be converted to a GUID, then the IsGuid function evaluated to true.</span></span>

<span data-ttu-id="1d660-595">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-595">**Syntax:**</span></span>  
`bool IsGuid(str GUID)`

<span data-ttu-id="1d660-596">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-596">**Remarks:**</span></span>  
<span data-ttu-id="1d660-597">GUID egy ezeket a mintákat a következő karakterláncként van definiálva: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx vagy {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span><span class="sxs-lookup"><span data-stu-id="1d660-597">A GUID is defined as a string following one of these patterns: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx or {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span></span>

<span data-ttu-id="1d660-598">Azt határozza meg, ha CGuid() sikeres lehet.</span><span class="sxs-lookup"><span data-stu-id="1d660-598">Used to determine if CGuid() can be successful.</span></span>

<span data-ttu-id="1d660-599">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-599">**Example:**</span></span>  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
<span data-ttu-id="1d660-600">Ha a StrAttribute GUID formátuma, térjen vissza a bináris megjelenítése, ellenkező esetben adhat vissza Null.</span><span class="sxs-lookup"><span data-stu-id="1d660-600">If the StrAttribute has a GUID format, return a binary representation, otherwise return a Null.</span></span>

- - -
### <a name="isnull"></a><span data-ttu-id="1d660-601">IsNull</span><span class="sxs-lookup"><span data-stu-id="1d660-601">IsNull</span></span>
<span data-ttu-id="1d660-602">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-602">**Description:**</span></span>  
<span data-ttu-id="1d660-603">Ha a kifejezés értéke Null, majd a IsNull függvény igaz értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-603">If the expression evaluates to Null, then the IsNull function returns true.</span></span>

<span data-ttu-id="1d660-604">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-604">**Syntax:**</span></span>  
`bool IsNull(var Expression)`

<span data-ttu-id="1d660-605">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-605">**Remarks:**</span></span>  
<span data-ttu-id="1d660-606">Egy attribútum a rendszer Null által a attribútum hiányában van kifejezve.</span><span class="sxs-lookup"><span data-stu-id="1d660-606">For an attribute, a Null is expressed by the absence of the attribute.</span></span>

<span data-ttu-id="1d660-607">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-607">**Example:**</span></span>  
`IsNull([displayName])`  
<span data-ttu-id="1d660-608">Igaz értéket ad vissza, ha az attribútum nem szerepel a CS vagy MV.</span><span class="sxs-lookup"><span data-stu-id="1d660-608">Returns True if the attribute is not present in the CS or MV.</span></span>

- - -
### <a name="isnullorempty"></a><span data-ttu-id="1d660-609">IsNullOrEmpty</span><span class="sxs-lookup"><span data-stu-id="1d660-609">IsNullOrEmpty</span></span>
<span data-ttu-id="1d660-610">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-610">**Description:**</span></span>  
<span data-ttu-id="1d660-611">Ha a kifejezés null vagy üres karakterlánc, majd a IsNullOrEmpty függvény igaz értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-611">If the expression is null or an empty string, then the IsNullOrEmpty function returns true.</span></span>

<span data-ttu-id="1d660-612">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-612">**Syntax:**</span></span>  
`bool IsNullOrEmpty(var Expression)`

<span data-ttu-id="1d660-613">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-613">**Remarks:**</span></span>  
<span data-ttu-id="1d660-614">Egy attribútum Ez akkor igaz értéket, ha az attribútum hiányzik, vagy megtalálható, de üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-614">For an attribute, this would evaluate to True if the attribute is absent or is present but is an empty string.</span></span>  
<span data-ttu-id="1d660-615">Ez a függvény inverzét IsPresent neve.</span><span class="sxs-lookup"><span data-stu-id="1d660-615">The inverse of this function is named IsPresent.</span></span>

<span data-ttu-id="1d660-616">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-616">**Example:**</span></span>  
`IsNullOrEmpty([displayName])`  
<span data-ttu-id="1d660-617">Igaz értéket ad vissza, ha az attribútum nincs jelen, vagy a Tanúsítványszolgáltatások vagy MV üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-617">Returns True if the attribute is not present or is an empty string in the CS or MV.</span></span>

- - -
### <a name="isnumeric"></a><span data-ttu-id="1d660-618">IsNumeric</span><span class="sxs-lookup"><span data-stu-id="1d660-618">IsNumeric</span></span>
<span data-ttu-id="1d660-619">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-619">**Description:**</span></span>  
<span data-ttu-id="1d660-620">A IsNumeric függvény jelzi, hogy egy kifejezés kiértékelése számú típusként logikai értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-620">The IsNumeric function returns a Boolean value indicating whether an expression can be evaluated as a number type.</span></span>

<span data-ttu-id="1d660-621">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-621">**Syntax:**</span></span>  
`bool IsNumeric(var Expression)`

<span data-ttu-id="1d660-622">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-622">**Remarks:**</span></span>  
<span data-ttu-id="1d660-623">Azt határozza meg, ha CNum() elemzése a kifejezés sikeres lehet.</span><span class="sxs-lookup"><span data-stu-id="1d660-623">Used to determine if CNum() can be successful to parse the expression.</span></span>

- - -
### <a name="isstring"></a><span data-ttu-id="1d660-624">IsString</span><span class="sxs-lookup"><span data-stu-id="1d660-624">IsString</span></span>
<span data-ttu-id="1d660-625">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-625">**Description:**</span></span>  
<span data-ttu-id="1d660-626">Ha a kifejezés kiértékelése karakterlánc típusú, majd a IsString függvény eredménye igaz.</span><span class="sxs-lookup"><span data-stu-id="1d660-626">If the expression can be evaluated to a string type, then the IsString function evaluates to True.</span></span>

<span data-ttu-id="1d660-627">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-627">**Syntax:**</span></span>  
`bool IsString(var expression)`

<span data-ttu-id="1d660-628">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-628">**Remarks:**</span></span>  
<span data-ttu-id="1d660-629">Azt határozza meg, ha CStr() elemzése a kifejezés sikeres lehet.</span><span class="sxs-lookup"><span data-stu-id="1d660-629">Used to determine if CStr() can be successful to parse the expression.</span></span>

- - -
### <a name="ispresent"></a><span data-ttu-id="1d660-630">IsPresent</span><span class="sxs-lookup"><span data-stu-id="1d660-630">IsPresent</span></span>
<span data-ttu-id="1d660-631">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-631">**Description:**</span></span>  
<span data-ttu-id="1d660-632">Ha a kifejezés értéke nem Null, és nem üres karakterláncot, majd a IsPresent függvény igaz értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-632">If the expression evaluates to a string that is not Null and is not empty, then the IsPresent function returns true.</span></span>

<span data-ttu-id="1d660-633">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-633">**Syntax:**</span></span>  
`bool IsPresent(var expression)`

<span data-ttu-id="1d660-634">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-634">**Remarks:**</span></span>  
<span data-ttu-id="1d660-635">Ez a függvény inverzét IsNullOrEmpty neve.</span><span class="sxs-lookup"><span data-stu-id="1d660-635">The inverse of this function is named IsNullOrEmpty.</span></span>

<span data-ttu-id="1d660-636">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-636">**Example:**</span></span>  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a><span data-ttu-id="1d660-637">Elem</span><span class="sxs-lookup"><span data-stu-id="1d660-637">Item</span></span>
<span data-ttu-id="1d660-638">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-638">**Description:**</span></span>  
<span data-ttu-id="1d660-639">Az Item függvény egy elemet egy többértékű karakterlánc attribútum ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-639">The Item function returns one item from a multi-valued string/attribute.</span></span>

<span data-ttu-id="1d660-640">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-640">**Syntax:**</span></span>  
`var Item(mvstr attribute, num index)`

* <span data-ttu-id="1d660-641">attribútum: többértékű attribútum</span><span class="sxs-lookup"><span data-stu-id="1d660-641">attribute: multi-valued attribute</span></span>
* <span data-ttu-id="1d660-642">index: index a többértékű karakterlánc elemen.</span><span class="sxs-lookup"><span data-stu-id="1d660-642">index: index to an item in the multi-valued string.</span></span>

<span data-ttu-id="1d660-643">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-643">**Remarks:**</span></span>  
<span data-ttu-id="1d660-644">Az Item függvény akkor hasznos, a Contains függvény együtt, mert az utóbbi függvény az index a többértékű attribútum elemen.</span><span class="sxs-lookup"><span data-stu-id="1d660-644">The Item function is useful together with the Contains function since the latter function returns the index to an item in the multi-valued attribute.</span></span>

<span data-ttu-id="1d660-645">Hibát jelez, ha az index az értéktartományon kívül esik.</span><span class="sxs-lookup"><span data-stu-id="1d660-645">Throws an error if index is out of bounds.</span></span>

<span data-ttu-id="1d660-646">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-646">**Example:**</span></span>  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
<span data-ttu-id="1d660-647">Az elsődleges e-mail címét adja meg.</span><span class="sxs-lookup"><span data-stu-id="1d660-647">Returns the primary email address.</span></span>

- - -
### <a name="itemornull"></a><span data-ttu-id="1d660-648">ItemOrNull</span><span class="sxs-lookup"><span data-stu-id="1d660-648">ItemOrNull</span></span>
<span data-ttu-id="1d660-649">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-649">**Description:**</span></span>  
<span data-ttu-id="1d660-650">A ItemOrNull függvény egy elemet egy többértékű karakterlánc attribútum ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-650">The ItemOrNull function returns one item from a multi-valued string/attribute.</span></span>

<span data-ttu-id="1d660-651">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-651">**Syntax:**</span></span>  
`var ItemOrNull(mvstr attribute, num index)`

* <span data-ttu-id="1d660-652">attribútum: többértékű attribútum</span><span class="sxs-lookup"><span data-stu-id="1d660-652">attribute: multi-valued attribute</span></span>
* <span data-ttu-id="1d660-653">index: index a többértékű karakterlánc elemen.</span><span class="sxs-lookup"><span data-stu-id="1d660-653">index: index to an item in the multi-valued string.</span></span>

<span data-ttu-id="1d660-654">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-654">**Remarks:**</span></span>  
<span data-ttu-id="1d660-655">A ItemOrNull függvény akkor hasznos, együtt a Contains függvény, mert az utóbbi függvény az index a többértékű attribútum elemen.</span><span class="sxs-lookup"><span data-stu-id="1d660-655">The ItemOrNull function is useful together with the Contains function since the latter function returns the index to an item in the multi-valued attribute.</span></span>

<span data-ttu-id="1d660-656">Ha az index az értéktartományon kívül esik, majd adja vissza Null értéket.</span><span class="sxs-lookup"><span data-stu-id="1d660-656">If index is out of bounds, then returns a Null value.</span></span>

- - -
### <a name="join"></a><span data-ttu-id="1d660-657">Csatlakozás</span><span class="sxs-lookup"><span data-stu-id="1d660-657">Join</span></span>
<span data-ttu-id="1d660-658">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-658">**Description:**</span></span>  
<span data-ttu-id="1d660-659">Illesztési függvényhez időt vesz igénybe a többértékű karakterlánc, és az egyes elemek között megadott elválasztó egyértékű karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-659">The Join function takes a multi-valued string and returns a single-valued string with specified separator inserted between each item.</span></span>

<span data-ttu-id="1d660-660">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-660">**Syntax:**</span></span>  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* <span data-ttu-id="1d660-661">attribútum: többértékű attribútum, amely tartalmazza a karakterláncot, amelyet egyesíteni.</span><span class="sxs-lookup"><span data-stu-id="1d660-661">attribute: Multi-valued attribute containing strings to be joined.</span></span>
* <span data-ttu-id="1d660-662">elválasztó karakter: bármilyen karakterlánc, a visszaadott karakterlánc karakterláncrészletek elválasztó.</span><span class="sxs-lookup"><span data-stu-id="1d660-662">delimiter: Any string, used to separate the substrings in the returned string.</span></span> <span data-ttu-id="1d660-663">Ha nincs megadva, a szóköz karakter ("") használatos.</span><span class="sxs-lookup"><span data-stu-id="1d660-663">If omitted, the space character (" ") is used.</span></span> <span data-ttu-id="1d660-664">Ha elválasztó karakter egy nulla hosszúságú karakterlánc (""), illetve semmi sem, a lista összes elemének nélkül határolójelek van kibővítve.</span><span class="sxs-lookup"><span data-stu-id="1d660-664">If Delimiter is a zero-length string ("") or Nothing, all items in the list are concatenated with no delimiters.</span></span>

<span data-ttu-id="1d660-665">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="1d660-665">**Remarks**</span></span>  
<span data-ttu-id="1d660-666">A csatlakozási és felosztott funkciók között paritásos van.</span><span class="sxs-lookup"><span data-stu-id="1d660-666">There is parity between the Join and Split functions.</span></span> <span data-ttu-id="1d660-667">Az illesztési függvényhez karakterláncok veszi, és csatlakoztatja őket egy elválasztó karakterlánc használatával egyetlen karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="1d660-667">The Join function takes an array of strings and joins them using a delimiter string, to return a single string.</span></span> <span data-ttu-id="1d660-668">A felosztott függvény lép egy karakterláncot, és elválasztja azt a elválasztó, térjen vissza a karakterláncok:.</span><span class="sxs-lookup"><span data-stu-id="1d660-668">The Split function takes a string and separates it at the delimiter, to return an array of strings.</span></span> <span data-ttu-id="1d660-669">A fő különbség azonban, hogy csatlakozási karakterlánc bármely határoló karakterláncot is összefűzésére, vegyes csak elkülönítheti karakterláncok egy egyetlen karaktert elválasztó használatával.</span><span class="sxs-lookup"><span data-stu-id="1d660-669">However, a key difference is that Join can concatenate strings with any delimiter string, Split can only separate strings using a single character delimiter.</span></span>

<span data-ttu-id="1d660-670">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-670">**Example:**</span></span>  
`Join([proxyAddresses],",")`  
<span data-ttu-id="1d660-671">Visszaadhatja: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"</span><span class="sxs-lookup"><span data-stu-id="1d660-671">Could return: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"</span></span>

- - -
### <a name="lcase"></a><span data-ttu-id="1d660-672">LCase</span><span class="sxs-lookup"><span data-stu-id="1d660-672">LCase</span></span>
<span data-ttu-id="1d660-673">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-673">**Description:**</span></span>  
<span data-ttu-id="1d660-674">A LCase függvény egy karakterlánc karaktereinek összes kisbetűsre konvertálja.</span><span class="sxs-lookup"><span data-stu-id="1d660-674">The LCase function converts all characters in a string to lower case.</span></span>

<span data-ttu-id="1d660-675">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-675">**Syntax:**</span></span>  
`str LCase(str value)`

<span data-ttu-id="1d660-676">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-676">**Example:**</span></span>  
`LCase("TeSt")`  
<span data-ttu-id="1d660-677">"Test" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-677">Returns "test".</span></span>

- - -
### <a name="left"></a><span data-ttu-id="1d660-678">balra</span><span class="sxs-lookup"><span data-stu-id="1d660-678">Left</span></span>
<span data-ttu-id="1d660-679">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-679">**Description:**</span></span>  
<span data-ttu-id="1d660-680">A bal oldali függvény egy karakterlánc bal oldali egy megadott számú karaktert adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-680">The Left function returns a specified number of characters from the left of a string.</span></span>

<span data-ttu-id="1d660-681">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-681">**Syntax:**</span></span>  
`str Left(str string, num NumChars)`

* <span data-ttu-id="1d660-682">karakterlánc: a karaktert adja vissza a karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="1d660-682">string: the string to return characters from</span></span>
* <span data-ttu-id="1d660-683">NumChars: egy szám karakterek kezdetétől (balra) karakterlánc vissza</span><span class="sxs-lookup"><span data-stu-id="1d660-683">NumChars: a number identifying the number of characters to return from the beginning (left) of string</span></span>

<span data-ttu-id="1d660-684">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-684">**Remarks:**</span></span>  
<span data-ttu-id="1d660-685">A karakterlánc első numChars karaktereket tartalmazó karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="1d660-685">A string containing the first numChars characters in string:</span></span>

* <span data-ttu-id="1d660-686">Ha numChars = 0, térjen vissza az üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-686">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="1d660-687">Ha numChars < 0, térjen vissza a bemeneti karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-687">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="1d660-688">Ha karakterlánc null értékű, térjen vissza az üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-688">If string is null, return empty string.</span></span>

<span data-ttu-id="1d660-689">Ha a karakterlánc a száma a megadott numChars-nál kevesebb karaktert tartalmaz, egy karakterlánc, karakterlánc (amely, 1. paraméter levő összes karakter) azonos ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-689">If string contains fewer characters than the number specified in numChars, a string identical to string (that is, containing all characters in parameter 1) is returned.</span></span>

<span data-ttu-id="1d660-690">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-690">**Example:**</span></span>  
`Left("John Doe", 3)`  
<span data-ttu-id="1d660-691">"Joh" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-691">Returns "Joh".</span></span>

- - -
### <a name="len"></a><span data-ttu-id="1d660-692">Hossz</span><span class="sxs-lookup"><span data-stu-id="1d660-692">Len</span></span>
<span data-ttu-id="1d660-693">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-693">**Description:**</span></span>  
<span data-ttu-id="1d660-694">A Len függvény egy karakterlánc számú karaktert adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-694">The Len function returns number of characters in a string.</span></span>

<span data-ttu-id="1d660-695">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-695">**Syntax:**</span></span>  
`num Len(str value)`

<span data-ttu-id="1d660-696">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-696">**Example:**</span></span>  
`Len("John Doe")`  
<span data-ttu-id="1d660-697">8 beolvasása</span><span class="sxs-lookup"><span data-stu-id="1d660-697">Returns 8</span></span>

- - -
### <a name="ltrim"></a><span data-ttu-id="1d660-698">LTrim</span><span class="sxs-lookup"><span data-stu-id="1d660-698">LTrim</span></span>
<span data-ttu-id="1d660-699">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-699">**Description:**</span></span>  
<span data-ttu-id="1d660-700">LTrim függvény kezdő szóközöket eltávolít egy karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="1d660-700">The LTrim function removes leading white spaces from a string.</span></span>

<span data-ttu-id="1d660-701">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-701">**Syntax:**</span></span>  
`str LTrim(str value)`

<span data-ttu-id="1d660-702">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-702">**Example:**</span></span>  
`LTrim(" Test ")`  
<span data-ttu-id="1d660-703">"Test" értéket ad vissza</span><span class="sxs-lookup"><span data-stu-id="1d660-703">Returns "Test "</span></span>

- - -
### <a name="mid"></a><span data-ttu-id="1d660-704">Mid</span><span class="sxs-lookup"><span data-stu-id="1d660-704">Mid</span></span>
<span data-ttu-id="1d660-705">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-705">**Description:**</span></span>  
<span data-ttu-id="1d660-706">A közép függvény egy karakterlánc megadott helyéről adja vissza a megadott számú karaktert.</span><span class="sxs-lookup"><span data-stu-id="1d660-706">The Mid function returns a specified number of characters from a specified position in a string.</span></span>

<span data-ttu-id="1d660-707">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-707">**Syntax:**</span></span>  
`str Mid(str string, num start, num NumChars)`

* <span data-ttu-id="1d660-708">karakterlánc: a karaktert adja vissza a karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="1d660-708">string: the string to return characters from</span></span>
* <span data-ttu-id="1d660-709">Indítsa el: egy szám kezdő pozíciója a visszaadandó karakterek a karakterláncban</span><span class="sxs-lookup"><span data-stu-id="1d660-709">start: a number identifying the starting position in string to return characters from</span></span>
* <span data-ttu-id="1d660-710">NumChars: egy szám karakterek karakterláncon visszaadására</span><span class="sxs-lookup"><span data-stu-id="1d660-710">NumChars: a number identifying the number of characters to return from position in string</span></span>

<span data-ttu-id="1d660-711">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-711">**Remarks:**</span></span>  
<span data-ttu-id="1d660-712">Indítsa el a pozíció visszatérési numChars karakterek karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-712">Return numChars characters starting from position start in string.</span></span>  
<span data-ttu-id="1d660-713">Pozíció indítás karakterláncban numChars karaktereket tartalmazó karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="1d660-713">A string containing numChars characters from position start in string:</span></span>

* <span data-ttu-id="1d660-714">Ha numChars = 0, térjen vissza az üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-714">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="1d660-715">Ha numChars < 0, térjen vissza a bemeneti karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-715">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="1d660-716">Ha start > karakterlánc hosszát adja vissza a bemeneti karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-716">If start > the length of string, return input string.</span></span>
* <span data-ttu-id="1d660-717">Ha start < = 0, térjen vissza a bemeneti karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-717">If start <= 0, return input string.</span></span>
* <span data-ttu-id="1d660-718">Ha karakterlánc null értékű, térjen vissza az üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-718">If string is null, return empty string.</span></span>

<span data-ttu-id="1d660-719">Ha nem numChar karakterek fennmaradó karakterlánc pozíció indítás, amit a lehető legtöbb karaktert ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-719">If there are not numChar characters remaining in string from position start, as many characters as possible are returned.</span></span>

<span data-ttu-id="1d660-720">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-720">**Example:**</span></span>  
`Mid("John Doe", 3, 5)`  
<span data-ttu-id="1d660-721">"Hn Do" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-721">Returns "hn Do".</span></span>

`Mid("John Doe", 6, 999)`  
<span data-ttu-id="1d660-722">Visszaadja a "Doe"</span><span class="sxs-lookup"><span data-stu-id="1d660-722">Returns "Doe"</span></span>

- - -
### <a name="now"></a><span data-ttu-id="1d660-723">Most</span><span class="sxs-lookup"><span data-stu-id="1d660-723">Now</span></span>
<span data-ttu-id="1d660-724">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-724">**Description:**</span></span>  
<span data-ttu-id="1d660-725">A funkcióval egy dátum/idő az aktuális dátum és idő alapján a számítógép rendszer dátum és idő megadása adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-725">The Now function returns a DateTime specifying the current date and time, according to your computer's system date and time.</span></span>

<span data-ttu-id="1d660-726">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-726">**Syntax:**</span></span>  
`dt Now()`

- - -
### <a name="numfromdate"></a><span data-ttu-id="1d660-727">NumFromDate</span><span class="sxs-lookup"><span data-stu-id="1d660-727">NumFromDate</span></span>
<span data-ttu-id="1d660-728">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-728">**Description:**</span></span>  
<span data-ttu-id="1d660-729">A NumFromDate függvény dátumformátum AD meg egy dátumot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-729">The NumFromDate function returns a date in AD’s date format.</span></span>

<span data-ttu-id="1d660-730">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-730">**Syntax:**</span></span>  
`num NumFromDate(dt value)`

<span data-ttu-id="1d660-731">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-731">**Example:**</span></span>  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
<span data-ttu-id="1d660-732">129699324000000000 beolvasása</span><span class="sxs-lookup"><span data-stu-id="1d660-732">Returns 129699324000000000</span></span>

- - -
### <a name="padleft"></a><span data-ttu-id="1d660-733">PadLeft</span><span class="sxs-lookup"><span data-stu-id="1d660-733">PadLeft</span></span>
<span data-ttu-id="1d660-734">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-734">**Description:**</span></span>  
<span data-ttu-id="1d660-735">A PadLeft függvény bal-kézi a megadott kitöltési karaktereket használ egy meghatározott hosszúságú karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="1d660-735">The PadLeft function left-pads a string to a specified length using a provided padding character.</span></span>

<span data-ttu-id="1d660-736">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-736">**Syntax:**</span></span>  
`str PadLeft(str string, num length, str padCharacter)`

* <span data-ttu-id="1d660-737">karakterlánc: a karakterlánc szegélynél.</span><span class="sxs-lookup"><span data-stu-id="1d660-737">string: the string to pad.</span></span>
* <span data-ttu-id="1d660-738">hossz: a kívánt karakterlánc hossza jelző egész számot.</span><span class="sxs-lookup"><span data-stu-id="1d660-738">length: An integer representing the desired length of string.</span></span>
* <span data-ttu-id="1d660-739">padCharacter: kitöltő karakterként használandó egyetlen karaktert tartalmazó karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d660-739">padCharacter: A string consisting of a single character to use as the pad character</span></span>

<span data-ttu-id="1d660-740">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-740">**Remarks:**</span></span>

* <span data-ttu-id="1d660-741">Ha a karakterlánc hossza nagyobb, mint, majd padCharacter ismételten fűz hozzá a (múlva) karakterlánc hossza azt hosszának egyenlőnek elején.</span><span class="sxs-lookup"><span data-stu-id="1d660-741">If the length of string is less than length, then padCharacter is repeatedly appended to the beginning (left) of string until it has a length equal to length.</span></span>
* <span data-ttu-id="1d660-742">padCharacter lehet szóköz karakter, de nem lehet null értékű.</span><span class="sxs-lookup"><span data-stu-id="1d660-742">PadCharacter can be a space character, but it cannot be a null value.</span></span>
* <span data-ttu-id="1d660-743">Ha a karakterlánc hossza hosszának nagyobbnak vagy azzal egyenlőnek, karakterlánc ad változatlan vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-743">If the length of string is equal to or greater than length, string is returned unchanged.</span></span>
* <span data-ttu-id="1d660-744">Ha karakterlánc hossza nagyobb vagy egyenlő, hosszra, karakterlánc megegyezik egy karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-744">If string has a length greater than or equal to length, a string identical to string is returned.</span></span>
* <span data-ttu-id="1d660-745">Ha a karakterlánc hossza nagyobb, mint, majd a kívánt hosszúságú új karakterlánc ad vissza egy padCharacter feltöltve tartalmazó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-745">If the length of string is less than length, then a new string of the desired length is returned containing string padded with a padCharacter.</span></span>
* <span data-ttu-id="1d660-746">Karakterlánc értéke null, ha a függvény egy üres karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-746">If string is null, the function returns an empty string.</span></span>

<span data-ttu-id="1d660-747">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-747">**Example:**</span></span>  
`PadLeft("User", 10, "0")`  
<span data-ttu-id="1d660-748">"000000User" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-748">Returns "000000User".</span></span>

- - -
### <a name="padright"></a><span data-ttu-id="1d660-749">PadRight</span><span class="sxs-lookup"><span data-stu-id="1d660-749">PadRight</span></span>
<span data-ttu-id="1d660-750">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-750">**Description:**</span></span>  
<span data-ttu-id="1d660-751">A PadRight függvény jobb-kézi a megadott kitöltési karaktereket használ egy meghatározott hosszúságú karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="1d660-751">The PadRight function right-pads a string to a specified length using a provided padding character.</span></span>

<span data-ttu-id="1d660-752">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-752">**Syntax:**</span></span>  
`str PadRight(str string, num length, str padCharacter)`

* <span data-ttu-id="1d660-753">karakterlánc: a karakterlánc szegélynél.</span><span class="sxs-lookup"><span data-stu-id="1d660-753">string: the string to pad.</span></span>
* <span data-ttu-id="1d660-754">hossz: a kívánt karakterlánc hossza jelző egész számot.</span><span class="sxs-lookup"><span data-stu-id="1d660-754">length: An integer representing the desired length of string.</span></span>
* <span data-ttu-id="1d660-755">padCharacter: kitöltő karakterként használandó egyetlen karaktert tartalmazó karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d660-755">padCharacter: A string consisting of a single character to use as the pad character</span></span>

<span data-ttu-id="1d660-756">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-756">**Remarks:**</span></span>

* <span data-ttu-id="1d660-757">Ha a karakterlánc hossza nagyobb, mint, majd padCharacter ismételten fűz hozzá a karakterlánc (jobb oldali) vége amíg rá nem kényszerül hosszának egyenlőnek hosszát.</span><span class="sxs-lookup"><span data-stu-id="1d660-757">If the length of string is less than length, then padCharacter is repeatedly appended to the end (right) of string until it has a length equal to length.</span></span>
* <span data-ttu-id="1d660-758">padCharacter lehet szóköz karakter, de nem lehet null értékű.</span><span class="sxs-lookup"><span data-stu-id="1d660-758">padCharacter can be a space character, but it cannot be a null value.</span></span>
* <span data-ttu-id="1d660-759">Ha a karakterlánc hossza hosszának nagyobbnak vagy azzal egyenlőnek, karakterlánc ad változatlan vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-759">If the length of string is equal to or greater than length, string is returned unchanged.</span></span>
* <span data-ttu-id="1d660-760">Ha karakterlánc hossza nagyobb vagy egyenlő, hosszra, karakterlánc megegyezik egy karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-760">If string has a length greater than or equal to length, a string identical to string is returned.</span></span>
* <span data-ttu-id="1d660-761">Ha a karakterlánc hossza nagyobb, mint, majd a kívánt hosszúságú új karakterlánc ad vissza egy padCharacter feltöltve tartalmazó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-761">If the length of string is less than length, then a new string of the desired length is returned containing string padded with a padCharacter.</span></span>
* <span data-ttu-id="1d660-762">Karakterlánc értéke null, ha a függvény egy üres karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-762">If string is null, the function returns an empty string.</span></span>

<span data-ttu-id="1d660-763">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-763">**Example:**</span></span>  
`PadRight("User", 10, "0")`  
<span data-ttu-id="1d660-764">"User000000" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-764">Returns "User000000".</span></span>

- - -
### <a name="pcase"></a><span data-ttu-id="1d660-765">PCase</span><span class="sxs-lookup"><span data-stu-id="1d660-765">PCase</span></span>
<span data-ttu-id="1d660-766">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-766">**Description:**</span></span>  
<span data-ttu-id="1d660-767">PCase függvény összes szóközzel elválasztott karakterlánc szó első karaktere átalakítja nagybetű, és további karakterekkel kisbetű alakulnak.</span><span class="sxs-lookup"><span data-stu-id="1d660-767">The PCase function converts the first character of each space delimited word in a string to upper case, and all other characters are converted to lower case.</span></span>

<span data-ttu-id="1d660-768">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-768">**Syntax:**</span></span>  
`String PCase(string)`

<span data-ttu-id="1d660-769">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-769">**Remarks:**</span></span>

* <span data-ttu-id="1d660-770">Ez a funkció jelenleg nem biztosít a megfelelő kis-és teljes mértékben nagybetű, például az angol szót konvertálni.</span><span class="sxs-lookup"><span data-stu-id="1d660-770">This function does not currently provide proper casing to convert a word that is entirely uppercase, such as an acronym.</span></span>

<span data-ttu-id="1d660-771">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-771">**Example:**</span></span>  
`PCase("TEsT")`  
<span data-ttu-id="1d660-772">"Test" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-772">Returns "Test".</span></span>

`PCase(LCase("TEST"))`  
<span data-ttu-id="1d660-773">"Test" értéket ad vissza</span><span class="sxs-lookup"><span data-stu-id="1d660-773">Returns "Test"</span></span>

- - -
### <a name="randomnum"></a><span data-ttu-id="1d660-774">RandomNum</span><span class="sxs-lookup"><span data-stu-id="1d660-774">RandomNum</span></span>
<span data-ttu-id="1d660-775">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-775">**Description:**</span></span>  
<span data-ttu-id="1d660-776">A RandomNum függvény között egy megadott időszakban egy véletlenszerű számot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-776">The RandomNum function returns a random number between a specified interval.</span></span>

<span data-ttu-id="1d660-777">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-777">**Syntax:**</span></span>  
`num RandomNum(num start, num end)`

* <span data-ttu-id="1d660-778">Indítsa el: egy szám alacsonyabb korlátot a véletlenszerű érték létrehozásához</span><span class="sxs-lookup"><span data-stu-id="1d660-778">start: a number identifying the lower limit of the random value to generate</span></span>
* <span data-ttu-id="1d660-779">vége: készítése a felső határ véletlenszerű érték azonosító szám</span><span class="sxs-lookup"><span data-stu-id="1d660-779">end: a number identifying the upper limit of the random value to generate</span></span>

<span data-ttu-id="1d660-780">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-780">**Example:**</span></span>  
`Random(100,999)`  
<span data-ttu-id="1d660-781">734 adhat vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-781">Can return 734.</span></span>

- - -
### <a name="removeduplicates"></a><span data-ttu-id="1d660-782">RemoveDuplicates</span><span class="sxs-lookup"><span data-stu-id="1d660-782">RemoveDuplicates</span></span>
<span data-ttu-id="1d660-783">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-783">**Description:**</span></span>  
<span data-ttu-id="1d660-784">A RemoveDuplicates függvény időt vesz igénybe a többértékű karakterlánc, és győződjön meg arról, hogy egyedi.</span><span class="sxs-lookup"><span data-stu-id="1d660-784">The RemoveDuplicates function takes a multi-valued string and make sure each value is unique.</span></span>

<span data-ttu-id="1d660-785">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-785">**Syntax:**</span></span>  
`mvstr RemoveDuplicates(mvstr attribute)`

<span data-ttu-id="1d660-786">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-786">**Example:**</span></span>  
`RemoveDuplicates([proxyAddresses])`  
<span data-ttu-id="1d660-787">Adja vissza egy megtisztított proxyAddress attribútumot, ahol minden ismétlődő értékek el lettek távolítva.</span><span class="sxs-lookup"><span data-stu-id="1d660-787">Returns a sanitized proxyAddress attribute where all duplicate values have been removed.</span></span>

- - -
### <a name="replace"></a><span data-ttu-id="1d660-788">Csere</span><span class="sxs-lookup"><span data-stu-id="1d660-788">Replace</span></span>
<span data-ttu-id="1d660-789">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-789">**Description:**</span></span>  
<span data-ttu-id="1d660-790">A csere függvény egy karakterlánc másik karakterláncra összes előfordulását lecseréli.</span><span class="sxs-lookup"><span data-stu-id="1d660-790">The Replace function replaces all occurrences of a string to another string.</span></span>

<span data-ttu-id="1d660-791">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-791">**Syntax:**</span></span>  
`str Replace(str string, str OldValue, str NewValue)`

* <span data-ttu-id="1d660-792">karakterlánc: egy karakterláncot cserélje le az értékeket.</span><span class="sxs-lookup"><span data-stu-id="1d660-792">string: A string to replace values in.</span></span>
* <span data-ttu-id="1d660-793">OldValue: A karakterlánc kereséséhez, és cserélje le.</span><span class="sxs-lookup"><span data-stu-id="1d660-793">OldValue: The string to search for and to replace.</span></span>
* <span data-ttu-id="1d660-794">Új érték: A cserélendő karakterláncot számára.</span><span class="sxs-lookup"><span data-stu-id="1d660-794">NewValue: The string to replace to.</span></span>

<span data-ttu-id="1d660-795">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-795">**Remarks:**</span></span>  
<span data-ttu-id="1d660-796">A függvény felismeri a következő különleges monikerek:</span><span class="sxs-lookup"><span data-stu-id="1d660-796">The function recognizes the following special monikers:</span></span>

* <span data-ttu-id="1d660-797">\n – új sor</span><span class="sxs-lookup"><span data-stu-id="1d660-797">\n – New Line</span></span>
* <span data-ttu-id="1d660-798">\r – kocsivissza</span><span class="sxs-lookup"><span data-stu-id="1d660-798">\r – Carriage Return</span></span>
* <span data-ttu-id="1d660-799">\t – lap</span><span class="sxs-lookup"><span data-stu-id="1d660-799">\t – Tab</span></span>

<span data-ttu-id="1d660-800">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-800">**Example:**</span></span>  
`Replace([address],"\r\n",", ")`  
<span data-ttu-id="1d660-801">CRLF lecseréli egy vesszőt és területet, és előfordulhat, hogy "Egy Microsoft módja, Redmond, WA, USA"</span><span class="sxs-lookup"><span data-stu-id="1d660-801">Replaces CRLF with a comma and space, and could lead to "One Microsoft Way, Redmond, WA, USA"</span></span>

- - -
### <a name="replacechars"></a><span data-ttu-id="1d660-802">ReplaceChars</span><span class="sxs-lookup"><span data-stu-id="1d660-802">ReplaceChars</span></span>
<span data-ttu-id="1d660-803">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-803">**Description:**</span></span>  
<span data-ttu-id="1d660-804">A ReplaceChars függvény a ReplacePattern karakterláncban található karakterek összes előfordulását lecseréli.</span><span class="sxs-lookup"><span data-stu-id="1d660-804">The ReplaceChars function replaces all occurrences of characters found in the ReplacePattern string.</span></span>

<span data-ttu-id="1d660-805">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-805">**Syntax:**</span></span>  
`str ReplaceChars(str string, str ReplacePattern)`

* <span data-ttu-id="1d660-806">karakterlánc: egy karakterlánc karaktereinek lecseréli.</span><span class="sxs-lookup"><span data-stu-id="1d660-806">string: A string to replace characters in.</span></span>
* <span data-ttu-id="1d660-807">ReplacePattern: a lecserélni kívánt karakterek szótár tartalmazó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-807">ReplacePattern: a string containing a dictionary with characters to replace.</span></span>

<span data-ttu-id="1d660-808">A formátum: {source1}: {target1}, {source2}: {target2}, {sourceN}, {targetN} ahol a forrás az a karakter, keresése és cseréje karakterlánc cél.</span><span class="sxs-lookup"><span data-stu-id="1d660-808">The format is {source1}:{target1},{source2}:{target2},{sourceN},{targetN} where source is the character to find and target the string to replace with.</span></span>

<span data-ttu-id="1d660-809">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-809">**Remarks:**</span></span>

* <span data-ttu-id="1d660-810">A függvény minden egyes előfordulásakor meghatározott források veszi és azok a célokat.</span><span class="sxs-lookup"><span data-stu-id="1d660-810">The function takes each occurrence of defined sources and replaces them with the targets.</span></span>
* <span data-ttu-id="1d660-811">A forrás (unicode) pontosan egy karaktert kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1d660-811">The source must be exactly one (unicode) character.</span></span>
* <span data-ttu-id="1d660-812">A forrás nem lehet üres vagy hosszabb, mint egy karakter (feldolgozási hiba).</span><span class="sxs-lookup"><span data-stu-id="1d660-812">The source cannot be empty or longer than one character (parsing error).</span></span>
* <span data-ttu-id="1d660-813">A cél több karakter, például ö:oe, β:ss lehet.</span><span class="sxs-lookup"><span data-stu-id="1d660-813">The target can have multiple characters, for example ö:oe, β:ss.</span></span>
* <span data-ttu-id="1d660-814">Lehet, hogy a tároló üres, amely azt jelzi, hogy az a karakter el kell távolítani.</span><span class="sxs-lookup"><span data-stu-id="1d660-814">The target can be empty indicating that the character should be removed.</span></span>
* <span data-ttu-id="1d660-815">A forrás kis-és nagybetűket, és pontosan egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="1d660-815">The source is case-sensitive and must be an exact match.</span></span>
* <span data-ttu-id="1d660-816">A, (vessző) és: (kettőspont) fenntartott karakterek, és ez a funkció nem helyettesíthető.</span><span class="sxs-lookup"><span data-stu-id="1d660-816">The , (comma) and : (colon) are reserved characters and cannot be replaced using this function.</span></span>
* <span data-ttu-id="1d660-817">Szóközt és más fehér karaktereket a ReplacePattern karakterláncban a rendszer figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="1d660-817">Spaces and other white characters in the ReplacePattern string are ignored.</span></span>

<span data-ttu-id="1d660-818">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-818">**Example:**</span></span>  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
<span data-ttu-id="1d660-819">Raksmorgas adja vissza</span><span class="sxs-lookup"><span data-stu-id="1d660-819">Returns Raksmorgas</span></span>

`ReplaceChars("O’Neil",%ReplaceString%)`  
<span data-ttu-id="1d660-820">Beolvasása "ONeil", az egyetlen osztásjelek van definiálva, el kell távolítani.</span><span class="sxs-lookup"><span data-stu-id="1d660-820">Returns "ONeil", the single tick is defined to be removed.</span></span>

- - -
### <a name="right"></a><span data-ttu-id="1d660-821">Jobbra</span><span class="sxs-lookup"><span data-stu-id="1d660-821">Right</span></span>
<span data-ttu-id="1d660-822">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-822">**Description:**</span></span>  
<span data-ttu-id="1d660-823">A jobb oldali függvény a megadott számú karaktert ad vissza egy karakterlánc jobb (záró).</span><span class="sxs-lookup"><span data-stu-id="1d660-823">The Right function returns a specified number of characters from the right (end) of a string.</span></span>

<span data-ttu-id="1d660-824">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-824">**Syntax:**</span></span>  
`str Right(str string, num NumChars)`

* <span data-ttu-id="1d660-825">karakterlánc: a karaktert adja vissza a karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="1d660-825">string: the string to return characters from</span></span>
* <span data-ttu-id="1d660-826">NumChars: egy szám karakterek végén (jobb oldali) karakterlánc vissza</span><span class="sxs-lookup"><span data-stu-id="1d660-826">NumChars: a number identifying the number of characters to return from the end (right) of string</span></span>

<span data-ttu-id="1d660-827">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-827">**Remarks:**</span></span>  
<span data-ttu-id="1d660-828">NumChars karaktereket a rendszer karakterlánc utolsó pozícióját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-828">NumChars characters are returned from the last position of string.</span></span>

<span data-ttu-id="1d660-829">A karakterláncon belül az utolsó numChars karaktereket tartalmazó karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="1d660-829">A string containing the last numChars characters in string:</span></span>

* <span data-ttu-id="1d660-830">Ha numChars = 0, térjen vissza az üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-830">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="1d660-831">Ha numChars < 0, térjen vissza a bemeneti karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-831">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="1d660-832">Ha karakterlánc null értékű, térjen vissza az üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-832">If string is null, return empty string.</span></span>

<span data-ttu-id="1d660-833">Ha karakterlánc a száma a megadott NumChars-nál kevesebb karaktert tartalmaz, karakterlánc megegyezik egy karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-833">If string contains fewer characters than the number specified in NumChars, a string identical to string is returned.</span></span>

<span data-ttu-id="1d660-834">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-834">**Example:**</span></span>  
`Right("John Doe", 3)`  
<span data-ttu-id="1d660-835">"Doe" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-835">Returns "Doe".</span></span>

- - -
### <a name="rtrim"></a><span data-ttu-id="1d660-836">RTrim</span><span class="sxs-lookup"><span data-stu-id="1d660-836">RTrim</span></span>
<span data-ttu-id="1d660-837">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-837">**Description:**</span></span>  
<span data-ttu-id="1d660-838">RTrim függvény záró szóközt eltávolít egy karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="1d660-838">The RTrim function removes trailing white spaces from a string.</span></span>

<span data-ttu-id="1d660-839">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-839">**Syntax:**</span></span>  
`str RTrim(str value)`

<span data-ttu-id="1d660-840">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-840">**Example:**</span></span>  
`RTrim(" Test ")`  
<span data-ttu-id="1d660-841">"Test" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-841">Returns " Test".</span></span>

- - -
### <a name="select"></a><span data-ttu-id="1d660-842">Válassza ezt:</span><span class="sxs-lookup"><span data-stu-id="1d660-842">Select</span></span>
<span data-ttu-id="1d660-843">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-843">**Description:**</span></span>  
<span data-ttu-id="1d660-844">A folyamat egy többértékű attribútum (vagy egy kifejezés eredményének) szereplő összes érték a megadott függvény alapján.</span><span class="sxs-lookup"><span data-stu-id="1d660-844">Process all values in a multi-valued attribute (or output of an expression) based on function specified.</span></span>

<span data-ttu-id="1d660-845">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-845">**Syntax:**</span></span>  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* <span data-ttu-id="1d660-846">cikk: a többértékű attribútumban olyan elemet</span><span class="sxs-lookup"><span data-stu-id="1d660-846">item: Represents an element in the multi-valued attribute</span></span>
* <span data-ttu-id="1d660-847">attribútum: a többértékű attribútum</span><span class="sxs-lookup"><span data-stu-id="1d660-847">attribute: the multi-valued attribute</span></span>
* <span data-ttu-id="1d660-848">kifejezés: értékek gyűjteményét visszaadó kifejezés</span><span class="sxs-lookup"><span data-stu-id="1d660-848">expression: an expression that returns a collection of values</span></span>
* <span data-ttu-id="1d660-849">feltétel: semmilyen feladatot, amely képes a attribútumban elem</span><span class="sxs-lookup"><span data-stu-id="1d660-849">condition: any function that can process an item in the attribute</span></span>

<span data-ttu-id="1d660-850">**Példák:**</span><span class="sxs-lookup"><span data-stu-id="1d660-850">**Examples:**</span></span>  
`Select($item,[otherPhone],Replace($item,“-”,“”))`  
<span data-ttu-id="1d660-851">Összes érték visszaadása a többértékű attribútum otherPhone után a kötőjeleket (-) el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="1d660-851">Return all the values in the multi-valued attribute otherPhone after hyphens (-) have been removed.</span></span>

- - -
### <a name="split"></a><span data-ttu-id="1d660-852">Vegyes</span><span class="sxs-lookup"><span data-stu-id="1d660-852">Split</span></span>
<span data-ttu-id="1d660-853">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-853">**Description:**</span></span>  
<span data-ttu-id="1d660-854">A felosztott függvény elválasztóval elválasztott karakterlánc vesz igénybe, és lehetővé teszi egy többértékű karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-854">The Split function takes a string separated with a delimiter and makes it a multi-valued string.</span></span>

<span data-ttu-id="1d660-855">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-855">**Syntax:**</span></span>  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* <span data-ttu-id="1d660-856">érték: a karakterlánc elválasztásához szövegelválasztó karaktert.</span><span class="sxs-lookup"><span data-stu-id="1d660-856">value: the string with a delimiter character to separate.</span></span>
* <span data-ttu-id="1d660-857">elválasztó karakter: egyetlen, az elválasztó karakter használható.</span><span class="sxs-lookup"><span data-stu-id="1d660-857">delimiter: single character to be used as the delimiter.</span></span>
* <span data-ttu-id="1d660-858">korlát: értékeket adhat vissza maximális számát.</span><span class="sxs-lookup"><span data-stu-id="1d660-858">limit: maximum number of values that can return.</span></span>

<span data-ttu-id="1d660-859">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-859">**Example:**</span></span>  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
<span data-ttu-id="1d660-860">2 elemek többértékű karakterláncot ad vissza a proxyAddress attribútum használható.</span><span class="sxs-lookup"><span data-stu-id="1d660-860">Returns a multi-valued string with 2 elements useful for the proxyAddress attribute.</span></span>

- - -
### <a name="stringfromguid"></a><span data-ttu-id="1d660-861">StringFromGuid</span><span class="sxs-lookup"><span data-stu-id="1d660-861">StringFromGuid</span></span>
<span data-ttu-id="1d660-862">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-862">**Description:**</span></span>  
<span data-ttu-id="1d660-863">Az StringFromGuid függvény bináris GUID vesz igénybe, és konvertálja azt a karakterláncot</span><span class="sxs-lookup"><span data-stu-id="1d660-863">The StringFromGuid function takes a binary GUID and converts it to a string</span></span>

<span data-ttu-id="1d660-864">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-864">**Syntax:**</span></span>  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a><span data-ttu-id="1d660-865">StringFromSid</span><span class="sxs-lookup"><span data-stu-id="1d660-865">StringFromSid</span></span>
<span data-ttu-id="1d660-866">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-866">**Description:**</span></span>  
<span data-ttu-id="1d660-867">A StringFromSid függvény egy karakterlánc biztonsági azonosítót tartalmazó bájttömb konvertálja.</span><span class="sxs-lookup"><span data-stu-id="1d660-867">The StringFromSid function converts a byte array containing a security identifier to a string.</span></span>

<span data-ttu-id="1d660-868">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-868">**Syntax:**</span></span>  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a><span data-ttu-id="1d660-869">Kapcsoló</span><span class="sxs-lookup"><span data-stu-id="1d660-869">Switch</span></span>
<span data-ttu-id="1d660-870">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-870">**Description:**</span></span>  
<span data-ttu-id="1d660-871">A kapcsoló függvényt ad vissza az értékelt feltételek alapján egyetlen értéket.</span><span class="sxs-lookup"><span data-stu-id="1d660-871">The Switch function is used to return a single value based on evaluated conditions.</span></span>

<span data-ttu-id="1d660-872">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-872">**Syntax:**</span></span>  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* <span data-ttu-id="1d660-873">kifejezés: Variant kifejezés ki kell számítani.</span><span class="sxs-lookup"><span data-stu-id="1d660-873">expr: Variant expression you want to evaluate.</span></span>
* <span data-ttu-id="1d660-874">érték: vissza kell adni, ha a megfelelő kifejezés igaz értéket.</span><span class="sxs-lookup"><span data-stu-id="1d660-874">value: Value to be returned if the corresponding expression is True.</span></span>

<span data-ttu-id="1d660-875">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-875">**Remarks:**</span></span>  
<span data-ttu-id="1d660-876">A kapcsoló függvény argumentumlista kifejezések és érték párokból áll.</span><span class="sxs-lookup"><span data-stu-id="1d660-876">The Switch function argument list consists of pairs of expressions and values.</span></span> <span data-ttu-id="1d660-877">A kifejezések kiértékelése balról jobbra, és a rendelt érték, amely az első kifejezés igaz értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-877">The expressions are evaluated from left to right, and the value associated with the first expression to evaluate to True is returned.</span></span> <span data-ttu-id="1d660-878">Ha nincsenek megfelelően párosítva a kijelzők, a futásidejű hiba történik.</span><span class="sxs-lookup"><span data-stu-id="1d660-878">If the parts aren't properly paired, a run-time error occurs.</span></span>

<span data-ttu-id="1d660-879">Például Kif1 értéke igaz, ha a kapcsoló érték1 adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-879">For example, if expr1 is True, Switch returns value1.</span></span> <span data-ttu-id="1d660-880">Ha kifejezés-1 érték hamis, de kifejezés-2 értéke igaz, kapcsoló érték-2, és így tovább adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-880">If expr-1 is False, but expr-2 is True, Switch returns value-2, and so on.</span></span>

<span data-ttu-id="1d660-881">Kapcsoló adja vissza egy Nothing ha:</span><span class="sxs-lookup"><span data-stu-id="1d660-881">Switch returns a Nothing if:</span></span>

* <span data-ttu-id="1d660-882">A kifejezések egyike sem igaz.</span><span class="sxs-lookup"><span data-stu-id="1d660-882">None of the expressions are True.</span></span>
* <span data-ttu-id="1d660-883">Az első igaz kifejezés a megfelelő értékkel rendelkezik, amely null értékű.</span><span class="sxs-lookup"><span data-stu-id="1d660-883">The first True expression has a corresponding value that is Null.</span></span>

<span data-ttu-id="1d660-884">Kapcsoló kiértékeli az összes kifejezést, annak ellenére, hogy csak az egyiket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-884">Switch evaluates all expressions, even though it returns only one of them.</span></span> <span data-ttu-id="1d660-885">Emiatt érdemes figyelemmel a nemkívánatos hatásai.</span><span class="sxs-lookup"><span data-stu-id="1d660-885">For this reason, you should watch for undesirable side effects.</span></span> <span data-ttu-id="1d660-886">Például a nullával való osztást bármely kifejezés kiértékelése eredményez, ha hiba lép fel.</span><span class="sxs-lookup"><span data-stu-id="1d660-886">For example, if the evaluation of any expression results in a division by zero error, an error occurs.</span></span>

<span data-ttu-id="1d660-887">A hiba függvénynek, amely akkor adja vissza egyéni érték is lehet.</span><span class="sxs-lookup"><span data-stu-id="1d660-887">Value can also be the Error function, which would return a custom string.</span></span>

<span data-ttu-id="1d660-888">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-888">**Example:**</span></span>  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
<span data-ttu-id="1d660-889">Néhány főbb városában szóbeli nyelvét adja eredményül, ellenkező esetben a hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-889">Returns the language spoken in some major cities, otherwise returns an Error.</span></span>

- - -
### <a name="trim"></a><span data-ttu-id="1d660-890">Trim</span><span class="sxs-lookup"><span data-stu-id="1d660-890">Trim</span></span>
<span data-ttu-id="1d660-891">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-891">**Description:**</span></span>  
<span data-ttu-id="1d660-892">A vágás függvény eltávolítja a kezdő és záró szóközök karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="1d660-892">The Trim function removes leading and trailing white spaces from a string.</span></span>

<span data-ttu-id="1d660-893">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-893">**Syntax:**</span></span>  
`str Trim(str value)`  

<span data-ttu-id="1d660-894">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-894">**Example:**</span></span>  
`Trim(" Test ")`  
<span data-ttu-id="1d660-895">"Test" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-895">Returns "Test".</span></span>

`Trim([proxyAddresses])`  
<span data-ttu-id="1d660-896">Eltávolítja a kezdő és záró szóközök proxyAddress attribútum szereplő összes értékhez.</span><span class="sxs-lookup"><span data-stu-id="1d660-896">Removes leading and trailing spaces for each value in the proxyAddress attribute.</span></span>

- - -
### <a name="ucase"></a><span data-ttu-id="1d660-897">UCase</span><span class="sxs-lookup"><span data-stu-id="1d660-897">UCase</span></span>
<span data-ttu-id="1d660-898">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-898">**Description:**</span></span>  
<span data-ttu-id="1d660-899">A UCase függvény egy karakterlánc karaktereinek összes nagybetűvel konvertálja.</span><span class="sxs-lookup"><span data-stu-id="1d660-899">The UCase function converts all characters in a string to upper case.</span></span>

<span data-ttu-id="1d660-900">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-900">**Syntax:**</span></span>  
`str UCase(str string)`

<span data-ttu-id="1d660-901">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-901">**Example:**</span></span>  
`UCase("TeSt")`  
<span data-ttu-id="1d660-902">"TEST" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-902">Returns "TEST".</span></span>

- - -
### <a name="where"></a><span data-ttu-id="1d660-903">Ha</span><span class="sxs-lookup"><span data-stu-id="1d660-903">Where</span></span>

<span data-ttu-id="1d660-904">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-904">**Description:**</span></span>  
<span data-ttu-id="1d660-905">A megadott feltétel alapján többértékű attribútum (vagy egy kifejezés eredményének) értékek egy alhalmazát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-905">Returns a subset of values from a multi-valued attribute (or output of an expression) based on specific condition.</span></span>

<span data-ttu-id="1d660-906">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-906">**Syntax:**</span></span>  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* <span data-ttu-id="1d660-907">cikk: a többértékű attribútumban olyan elemet</span><span class="sxs-lookup"><span data-stu-id="1d660-907">item: Represents an element in the multi-valued attribute</span></span>
* <span data-ttu-id="1d660-908">attribútum: a többértékű attribútum</span><span class="sxs-lookup"><span data-stu-id="1d660-908">attribute: the multi-valued attribute</span></span>
* <span data-ttu-id="1d660-909">feltétel: bármely kifejezés igaz vagy hamis kiértékelhető</span><span class="sxs-lookup"><span data-stu-id="1d660-909">condition: any expression that can be evaluated to true or false</span></span>
* <span data-ttu-id="1d660-910">kifejezés: értékek gyűjteményét visszaadó kifejezés</span><span class="sxs-lookup"><span data-stu-id="1d660-910">expression: an expression that returns a collection of values</span></span>

<span data-ttu-id="1d660-911">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-911">**Example:**</span></span>  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
<span data-ttu-id="1d660-912">A többértékű attribútum userCertificate, amely nem található a tanúsítvány értékek visszaadása.</span><span class="sxs-lookup"><span data-stu-id="1d660-912">Return the certificate values in the multi-valued attribute userCertificate which aren’t expired.</span></span>

- - -
### <a name="with"></a><span data-ttu-id="1d660-913">a</span><span class="sxs-lookup"><span data-stu-id="1d660-913">With</span></span>
<span data-ttu-id="1d660-914">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-914">**Description:**</span></span>  
<span data-ttu-id="1d660-915">A With függvény segítségével egyszerűbbé teheti a összetett kifejezést egy változó segítségével egy alkifejezés, egy megjelenő képviselő vagy többször az összetett kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="1d660-915">The With function provides a way to simplify a complex expression by using a variable to represent a subexpression which appears one or more times in the complex expression.</span></span>

<span data-ttu-id="1d660-916">**Szintaxis:**
`With(var variable, exp subExpression, exp complexExpression)`</span><span class="sxs-lookup"><span data-stu-id="1d660-916">**Syntax:**
`With(var variable, exp subExpression, exp complexExpression)`</span></span>  
* <span data-ttu-id="1d660-917">változó: a alkifejezés jelöli.</span><span class="sxs-lookup"><span data-stu-id="1d660-917">variable: Represents the subexpression.</span></span>
* <span data-ttu-id="1d660-918">alkifejezés: változó által képviselt alkifejezés.</span><span class="sxs-lookup"><span data-stu-id="1d660-918">subExpression: subexpression represented by variable.</span></span>
* <span data-ttu-id="1d660-919">complexExpression: egy összetett kifejezés.</span><span class="sxs-lookup"><span data-stu-id="1d660-919">complexExpression: A complex expression.</span></span>

<span data-ttu-id="1d660-920">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-920">**Example:**</span></span>  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
<span data-ttu-id="1d660-921">Mint:</span><span class="sxs-lookup"><span data-stu-id="1d660-921">Is functionally equivalent to:</span></span>  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
<span data-ttu-id="1d660-922">Amely a userCertificate attribútum csak leküldéshez tanúsítvány értékeket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-922">Which returns only unexpired certificate values in the userCertificate attribute.</span></span>


- - -
### <a name="word"></a><span data-ttu-id="1d660-923">Word</span><span class="sxs-lookup"><span data-stu-id="1d660-923">Word</span></span>
<span data-ttu-id="1d660-924">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="1d660-924">**Description:**</span></span>  
<span data-ttu-id="1d660-925">A Word függvény egy karakterlánc, és a word számát adja vissza az elválasztó karaktert leíró paraméterek alapján belül található szót adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-925">The Word function returns a word contained within a string, based on parameters describing the delimiters to use and the word number to return.</span></span>

<span data-ttu-id="1d660-926">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="1d660-926">**Syntax:**</span></span>  
`str Word(str string, num WordNumber, str delimiters)`

* <span data-ttu-id="1d660-927">karakterlánc: a karakterláncot. térjen vissza a szót.</span><span class="sxs-lookup"><span data-stu-id="1d660-927">string: the string to return a word from.</span></span>
* <span data-ttu-id="1d660-928">WordNumber: egy szám mely word számot kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="1d660-928">WordNumber: a number identifying which word number should return.</span></span>
* <span data-ttu-id="1d660-929">elválasztó karaktert: egy karakterlánc, amely a szavakat azonosítására használt delimiter(s)</span><span class="sxs-lookup"><span data-stu-id="1d660-929">delimiters: a string representing the delimiter(s) that should be used to identify words</span></span>

<span data-ttu-id="1d660-930">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="1d660-930">**Remarks:**</span></span>  
<span data-ttu-id="1d660-931">A határoló karaktereit elválasztott karakterlánc minden karakterlánchoz szavak azonosítja:</span><span class="sxs-lookup"><span data-stu-id="1d660-931">Each string of characters in string separated by the one of the characters in delimiters are identified as words:</span></span>

* <span data-ttu-id="1d660-932">Ha < 1-es számú, értéket ad vissza üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d660-932">If number < 1, returns empty string.</span></span>
* <span data-ttu-id="1d660-933">Ha a karakterlánc értéke null, üres karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-933">If string is null, returns empty string.</span></span>

<span data-ttu-id="1d660-934">Karakterlánc nagyobb számú karaktert tartalmaz, vagy karakterlánca nem tartalmazza az elválasztó karaktert által azonosított szavak, ha üres karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1d660-934">If string contains less than number words, or string does not contain any words identified by delimiters, an empty string is returned.</span></span>

<span data-ttu-id="1d660-935">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1d660-935">**Example:**</span></span>  
`Word("The quick brown fox",3," ")`  
<span data-ttu-id="1d660-936">Visszaadja a "nem"</span><span class="sxs-lookup"><span data-stu-id="1d660-936">Returns "brown"</span></span>

`Word("This,string!has&many separators",3,",!&#")`  
<span data-ttu-id="1d660-937">Alakítanák vissza "tartalmaz"</span><span class="sxs-lookup"><span data-stu-id="1d660-937">Would return "has"</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1d660-938">További források</span><span class="sxs-lookup"><span data-stu-id="1d660-938">Additional Resources</span></span>
* [<span data-ttu-id="1d660-939">Deklaratív kiépítés kifejezéseinek ismertetése</span><span class="sxs-lookup"><span data-stu-id="1d660-939">Understanding Declarative Provisioning Expressions</span></span>](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [<span data-ttu-id="1d660-940">Az Azure AD Connect-szinkronizálás: Szinkronizálási beállítások testreszabása</span><span class="sxs-lookup"><span data-stu-id="1d660-940">Azure AD Connect Sync: Customizing Synchronization options</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="1d660-941">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="1d660-941">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
