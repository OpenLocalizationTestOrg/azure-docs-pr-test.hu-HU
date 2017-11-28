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
ms.openlocfilehash: fbe0df856ca2efda965650fb85c7e831a0be32c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-functions-reference"></a><span data-ttu-id="8e001-103">Azure AD Connect szinkronizálása: funkciók referencia</span><span class="sxs-lookup"><span data-stu-id="8e001-103">Azure AD Connect sync: Functions Reference</span></span>
<span data-ttu-id="8e001-104">Az Azure AD Connectben funkciók használt toomanipulate egy attribútum értékét a szinkronizálás során.</span><span class="sxs-lookup"><span data-stu-id="8e001-104">In Azure AD Connect, functions are used toomanipulate an attribute value during synchronization.</span></span>  
<span data-ttu-id="8e001-105">hello hello funkciók szintaxisa a következő formátumban hello kifejezett:</span><span class="sxs-lookup"><span data-stu-id="8e001-105">hello Syntax of hello functions is expressed using hello following format:</span></span>  
`<output type> FunctionName(<input type> <position name>, ..)`

<span data-ttu-id="8e001-106">Egy függvény túl van terhelve, és több szintaxissal fogad el, ha az összes érvényes szintaxissal vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="8e001-106">If a function is overloaded and accepts multiple syntaxes, all valid syntaxes are listed.</span></span>  
<span data-ttu-id="8e001-107">hello funkciók erős típusmegadású vannak, és azok győződjön meg arról, hogy hello típust kapott találatok dokumentált hello típus.</span><span class="sxs-lookup"><span data-stu-id="8e001-107">hello functions are strongly typed and they verify that hello type passed in matches hello documented type.</span></span>  
<span data-ttu-id="8e001-108">Hello típusa nem egyezik, ha hiba fordul elő.</span><span class="sxs-lookup"><span data-stu-id="8e001-108">If hello type does not match, an error is thrown.</span></span>

<span data-ttu-id="8e001-109">hello típusok szerint van megadva, a szintaxis a következő hello:</span><span class="sxs-lookup"><span data-stu-id="8e001-109">hello types are expressed with hello following syntax:</span></span>

* <span data-ttu-id="8e001-110">**Bin** – bináris</span><span class="sxs-lookup"><span data-stu-id="8e001-110">**bin** – Binary</span></span>
* <span data-ttu-id="8e001-111">**logikai** – logikai</span><span class="sxs-lookup"><span data-stu-id="8e001-111">**bool** – Boolean</span></span>
* <span data-ttu-id="8e001-112">**DT** – UTC dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8e001-112">**dt** – UTC Date/Time</span></span>
* <span data-ttu-id="8e001-113">**Enum** – az ismert állandók számbavétele</span><span class="sxs-lookup"><span data-stu-id="8e001-113">**enum** – Enumeration of known constants</span></span>
* <span data-ttu-id="8e001-114">**Exp** – kifejezés, amely várt tooevaluate tooa logikai érték</span><span class="sxs-lookup"><span data-stu-id="8e001-114">**exp** – Expression, which is expected tooevaluate tooa Boolean</span></span>
* <span data-ttu-id="8e001-115">**mvbin** – többértékű bináris</span><span class="sxs-lookup"><span data-stu-id="8e001-115">**mvbin** – Multi-Valued Binary</span></span>
* <span data-ttu-id="8e001-116">**mvstr** – többértékű karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8e001-116">**mvstr** – Multi-Valued String</span></span>
* <span data-ttu-id="8e001-117">**mvref** – többértékű referencia</span><span class="sxs-lookup"><span data-stu-id="8e001-117">**mvref** – Multi-Valued Reference</span></span>
* <span data-ttu-id="8e001-118">**NUM** – numerikus</span><span class="sxs-lookup"><span data-stu-id="8e001-118">**num** – Numeric</span></span>
* <span data-ttu-id="8e001-119">**REF** – referencia</span><span class="sxs-lookup"><span data-stu-id="8e001-119">**ref** – Reference</span></span>
* <span data-ttu-id="8e001-120">**Str** – karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8e001-120">**str** – String</span></span>
* <span data-ttu-id="8e001-121">**var** – (szinte) bármilyen más típusú variant</span><span class="sxs-lookup"><span data-stu-id="8e001-121">**var** – A variant of (almost) any other type</span></span>
* <span data-ttu-id="8e001-122">**"void"** – nem ad visszatérési értéket</span><span class="sxs-lookup"><span data-stu-id="8e001-122">**void** – doesn’t return a value</span></span>

<span data-ttu-id="8e001-123">hello típusú funkciók hello **mvbin**, **mvstr**, és **mvref** többértékű attribútumok csak használható.</span><span class="sxs-lookup"><span data-stu-id="8e001-123">hello functions with hello types **mvbin**, **mvstr**, and **mvref** can only work on multi-valued attributes.</span></span> <span data-ttu-id="8e001-124">A Functions **bin**, **str**, és **ref** munkahelyi egyaránt egyértékű és többértékű jellemzőit.</span><span class="sxs-lookup"><span data-stu-id="8e001-124">Functions with **bin**, **str**, and **ref** work on both single-valued and multi-valued attributes.</span></span>

## <a name="functions-reference"></a><span data-ttu-id="8e001-125">Functions – referencia</span><span class="sxs-lookup"><span data-stu-id="8e001-125">Functions Reference</span></span>
| <span data-ttu-id="8e001-126">Függvények listája</span><span class="sxs-lookup"><span data-stu-id="8e001-126">List of functions</span></span> |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="8e001-127">**Tanúsítvány**</span><span class="sxs-lookup"><span data-stu-id="8e001-127">**Certificate**</span></span> | | | | |
| [<span data-ttu-id="8e001-128">CertExtensionOids</span><span class="sxs-lookup"><span data-stu-id="8e001-128">CertExtensionOids</span></span>](#certextensionoids) |[<span data-ttu-id="8e001-129">CertFormat</span><span class="sxs-lookup"><span data-stu-id="8e001-129">CertFormat</span></span>](#certformat) |[<span data-ttu-id="8e001-130">CertFriendlyName</span><span class="sxs-lookup"><span data-stu-id="8e001-130">CertFriendlyName</span></span>](#certfriendlyname) |[<span data-ttu-id="8e001-131">CertHashString</span><span class="sxs-lookup"><span data-stu-id="8e001-131">CertHashString</span></span>](#certhashstring) | |
| [<span data-ttu-id="8e001-132">CertIssuer</span><span class="sxs-lookup"><span data-stu-id="8e001-132">CertIssuer</span></span>](#certissuer) |[<span data-ttu-id="8e001-133">CertIssuerDN</span><span class="sxs-lookup"><span data-stu-id="8e001-133">CertIssuerDN</span></span>](#certissuerdn) |[<span data-ttu-id="8e001-134">CertIssuerOid</span><span class="sxs-lookup"><span data-stu-id="8e001-134">CertIssuerOid</span></span>](#certissueroid) |[<span data-ttu-id="8e001-135">CertKeyAlgorithm</span><span class="sxs-lookup"><span data-stu-id="8e001-135">CertKeyAlgorithm</span></span>](#certkeyalgorithm) | |
| [<span data-ttu-id="8e001-136">CertKeyAlgorithmParams</span><span class="sxs-lookup"><span data-stu-id="8e001-136">CertKeyAlgorithmParams</span></span>](#certkeyalgorithmparams) |[<span data-ttu-id="8e001-137">CertNameInfo</span><span class="sxs-lookup"><span data-stu-id="8e001-137">CertNameInfo</span></span>](#certnameinfo) |[<span data-ttu-id="8e001-138">CertNotAfter</span><span class="sxs-lookup"><span data-stu-id="8e001-138">CertNotAfter</span></span>](#certnotafter) |[<span data-ttu-id="8e001-139">CertNotBefore</span><span class="sxs-lookup"><span data-stu-id="8e001-139">CertNotBefore</span></span>](#certnotbefore) | |
| [<span data-ttu-id="8e001-140">CertPublicKeyOid</span><span class="sxs-lookup"><span data-stu-id="8e001-140">CertPublicKeyOid</span></span>](#certpublickeyoid) |[<span data-ttu-id="8e001-141">CertPublicKeyParametersOid</span><span class="sxs-lookup"><span data-stu-id="8e001-141">CertPublicKeyParametersOid</span></span>](#certpublickeyparametersoid) |[<span data-ttu-id="8e001-142">CertSerialNumber</span><span class="sxs-lookup"><span data-stu-id="8e001-142">CertSerialNumber</span></span>](#certserialnumber) |[<span data-ttu-id="8e001-143">CertSignatureAlgorithmOid</span><span class="sxs-lookup"><span data-stu-id="8e001-143">CertSignatureAlgorithmOid</span></span>](#certsignaturealgorithmoid) | |
| [<span data-ttu-id="8e001-144">CertSubject</span><span class="sxs-lookup"><span data-stu-id="8e001-144">CertSubject</span></span>](#certsubject) |[<span data-ttu-id="8e001-145">CertSubjectNameDN</span><span class="sxs-lookup"><span data-stu-id="8e001-145">CertSubjectNameDN</span></span>](#certsubjectnamedn) |[<span data-ttu-id="8e001-146">CertSubjectNameOid</span><span class="sxs-lookup"><span data-stu-id="8e001-146">CertSubjectNameOid</span></span>](#certsubjectnameoid) |[<span data-ttu-id="8e001-147">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="8e001-147">CertThumbprint</span></span>](#certthumbprint) | |
[<span data-ttu-id="8e001-148">CertVersion</span><span class="sxs-lookup"><span data-stu-id="8e001-148"> CertVersion</span></span>](#certversion) |[<span data-ttu-id="8e001-149">IsCert</span><span class="sxs-lookup"><span data-stu-id="8e001-149">IsCert</span></span>](#iscert) | | | |
| <span data-ttu-id="8e001-150">**Átalakítás**</span><span class="sxs-lookup"><span data-stu-id="8e001-150">**Conversion**</span></span> | | | | |
| [<span data-ttu-id="8e001-151">CBool</span><span class="sxs-lookup"><span data-stu-id="8e001-151">CBool</span></span>](#cbool) |[<span data-ttu-id="8e001-152">CDate</span><span class="sxs-lookup"><span data-stu-id="8e001-152">CDate</span></span>](#cdate) |[<span data-ttu-id="8e001-153">CGuid</span><span class="sxs-lookup"><span data-stu-id="8e001-153">CGuid</span></span>](#cguid) |[<span data-ttu-id="8e001-154">ConvertFromBase64</span><span class="sxs-lookup"><span data-stu-id="8e001-154">ConvertFromBase64</span></span>](#convertfrombase64) | |
| [<span data-ttu-id="8e001-155">ConvertToBase64</span><span class="sxs-lookup"><span data-stu-id="8e001-155">ConvertToBase64</span></span>](#converttobase64) |[<span data-ttu-id="8e001-156">ConvertFromUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="8e001-156">ConvertFromUTF8Hex</span></span>](#convertfromutf8hex) |[<span data-ttu-id="8e001-157">ConvertToUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="8e001-157">ConvertToUTF8Hex</span></span>](#converttoutf8hex) |[<span data-ttu-id="8e001-158">CNum</span><span class="sxs-lookup"><span data-stu-id="8e001-158">CNum</span></span>](#cnum) | |
| [<span data-ttu-id="8e001-159">CRef</span><span class="sxs-lookup"><span data-stu-id="8e001-159">CRef</span></span>](#cref) |[<span data-ttu-id="8e001-160">CStr</span><span class="sxs-lookup"><span data-stu-id="8e001-160">CStr</span></span>](#cstr) |[<span data-ttu-id="8e001-161">StringFromGuid</span><span class="sxs-lookup"><span data-stu-id="8e001-161">StringFromGuid</span></span>](#StringFromGuid) |[<span data-ttu-id="8e001-162">StringFromSid</span><span class="sxs-lookup"><span data-stu-id="8e001-162">StringFromSid</span></span>](#stringfromsid) | |
| <span data-ttu-id="8e001-163">**Dátum és idő**</span><span class="sxs-lookup"><span data-stu-id="8e001-163">**Date / Time**</span></span> | | | | |
| [<span data-ttu-id="8e001-164">DateAdd</span><span class="sxs-lookup"><span data-stu-id="8e001-164">DateAdd</span></span>](#dateadd) |[<span data-ttu-id="8e001-165">DateFromNum</span><span class="sxs-lookup"><span data-stu-id="8e001-165">DateFromNum</span></span>](#datefromnum) |[<span data-ttu-id="8e001-166">FormatDateTime</span><span class="sxs-lookup"><span data-stu-id="8e001-166">FormatDateTime</span></span>](#formatdatetime) |[<span data-ttu-id="8e001-167">Most</span><span class="sxs-lookup"><span data-stu-id="8e001-167">Now</span></span>](#now) | |
| [<span data-ttu-id="8e001-168">NumFromDate</span><span class="sxs-lookup"><span data-stu-id="8e001-168">NumFromDate</span></span>](#numfromdate) | | | | |
| <span data-ttu-id="8e001-169">**Könyvtár**</span><span class="sxs-lookup"><span data-stu-id="8e001-169">**Directory**</span></span> | | | | |
| [<span data-ttu-id="8e001-170">DNComponent</span><span class="sxs-lookup"><span data-stu-id="8e001-170">DNComponent</span></span>](#dncomponent) |[<span data-ttu-id="8e001-171">DNComponentRev</span><span class="sxs-lookup"><span data-stu-id="8e001-171">DNComponentRev</span></span>](#dncomponentrev) |[<span data-ttu-id="8e001-172">EscapeDNComponent</span><span class="sxs-lookup"><span data-stu-id="8e001-172">EscapeDNComponent</span></span>](#escapedncomponent) | | |
| <span data-ttu-id="8e001-173">**Értékelés**</span><span class="sxs-lookup"><span data-stu-id="8e001-173">**Evaluation**</span></span> | | | | |
| [<span data-ttu-id="8e001-174">IsBitSet</span><span class="sxs-lookup"><span data-stu-id="8e001-174">IsBitSet</span></span>](#isbitset) |[<span data-ttu-id="8e001-175">IsDate</span><span class="sxs-lookup"><span data-stu-id="8e001-175">IsDate</span></span>](#isdate) |[<span data-ttu-id="8e001-176">Vagyis IsEmpty</span><span class="sxs-lookup"><span data-stu-id="8e001-176">IsEmpty</span></span>](#isempty) |[<span data-ttu-id="8e001-177">IsGuid</span><span class="sxs-lookup"><span data-stu-id="8e001-177">IsGuid</span></span>](#isguid) | |
| [<span data-ttu-id="8e001-178">IsNull</span><span class="sxs-lookup"><span data-stu-id="8e001-178">IsNull</span></span>](#isnull) |[<span data-ttu-id="8e001-179">IsNullOrEmpty</span><span class="sxs-lookup"><span data-stu-id="8e001-179">IsNullOrEmpty</span></span>](#isnullorempty) |[<span data-ttu-id="8e001-180">IsNumeric</span><span class="sxs-lookup"><span data-stu-id="8e001-180">IsNumeric</span></span>](#isnumeric) |[<span data-ttu-id="8e001-181">IsPresent</span><span class="sxs-lookup"><span data-stu-id="8e001-181">IsPresent</span></span>](#ispresent) | |
| [<span data-ttu-id="8e001-182">IsString</span><span class="sxs-lookup"><span data-stu-id="8e001-182">IsString</span></span>](#isstring) | | | | |
| <span data-ttu-id="8e001-183">**Matematikai**</span><span class="sxs-lookup"><span data-stu-id="8e001-183">**Math**</span></span> | | | | |
| [<span data-ttu-id="8e001-184">BitAnd</span><span class="sxs-lookup"><span data-stu-id="8e001-184">BitAnd</span></span>](#bitand) |[<span data-ttu-id="8e001-185">BitOr</span><span class="sxs-lookup"><span data-stu-id="8e001-185">BitOr</span></span>](#bitor) |[<span data-ttu-id="8e001-186">RandomNum</span><span class="sxs-lookup"><span data-stu-id="8e001-186">RandomNum</span></span>](#randomnum) | | |
| <span data-ttu-id="8e001-187">**Többértékű**</span><span class="sxs-lookup"><span data-stu-id="8e001-187">**Multi-valued**</span></span> | | | | |
| [<span data-ttu-id="8e001-188">Tartalmazza</span><span class="sxs-lookup"><span data-stu-id="8e001-188">Contains</span></span>](#contains) |[<span data-ttu-id="8e001-189">Száma</span><span class="sxs-lookup"><span data-stu-id="8e001-189">Count</span></span>](#count) |[<span data-ttu-id="8e001-190">Elem</span><span class="sxs-lookup"><span data-stu-id="8e001-190">Item</span></span>](#item) |[<span data-ttu-id="8e001-191">ItemOrNull</span><span class="sxs-lookup"><span data-stu-id="8e001-191">ItemOrNull</span></span>](#itemornull) | |
| [<span data-ttu-id="8e001-192">Csatlakozás</span><span class="sxs-lookup"><span data-stu-id="8e001-192">Join</span></span>](#join) |[<span data-ttu-id="8e001-193">RemoveDuplicates</span><span class="sxs-lookup"><span data-stu-id="8e001-193">RemoveDuplicates</span></span>](#removeduplicates) |[<span data-ttu-id="8e001-194">Vegyes</span><span class="sxs-lookup"><span data-stu-id="8e001-194">Split</span></span>](#split) | | |
| <span data-ttu-id="8e001-195">**Program folyamata**</span><span class="sxs-lookup"><span data-stu-id="8e001-195">**Program Flow**</span></span> | | | | |
| [<span data-ttu-id="8e001-196">Hiba történt</span><span class="sxs-lookup"><span data-stu-id="8e001-196">Error</span></span>](#error) |[<span data-ttu-id="8e001-197">AZ IIF</span><span class="sxs-lookup"><span data-stu-id="8e001-197">IIF</span></span>](#iif) |[<span data-ttu-id="8e001-198">Kiválasztás</span><span class="sxs-lookup"><span data-stu-id="8e001-198">Select</span></span>](#select) |[<span data-ttu-id="8e001-199">Kapcsoló</span><span class="sxs-lookup"><span data-stu-id="8e001-199">Switch</span></span>](#switch) | |
| [<span data-ttu-id="8e001-200">Ha</span><span class="sxs-lookup"><span data-stu-id="8e001-200">Where</span></span>](#where) |[<span data-ttu-id="8e001-201">A</span><span class="sxs-lookup"><span data-stu-id="8e001-201">With</span></span>](#with) | | | |
| <span data-ttu-id="8e001-202">**Szöveg**</span><span class="sxs-lookup"><span data-stu-id="8e001-202">**Text**</span></span> | | | | |
| [<span data-ttu-id="8e001-203">GUID</span><span class="sxs-lookup"><span data-stu-id="8e001-203">GUID</span></span>](#guid) |[<span data-ttu-id="8e001-204">InStr</span><span class="sxs-lookup"><span data-stu-id="8e001-204">InStr</span></span>](#instr) |[<span data-ttu-id="8e001-205">InStrRev</span><span class="sxs-lookup"><span data-stu-id="8e001-205">InStrRev</span></span>](#instrrev) |[<span data-ttu-id="8e001-206">LCase</span><span class="sxs-lookup"><span data-stu-id="8e001-206">LCase</span></span>](#lcase) | |
| [<span data-ttu-id="8e001-207">Balra</span><span class="sxs-lookup"><span data-stu-id="8e001-207">Left</span></span>](#left) |[<span data-ttu-id="8e001-208">Hossz</span><span class="sxs-lookup"><span data-stu-id="8e001-208">Len</span></span>](#len) |[<span data-ttu-id="8e001-209">LTrim</span><span class="sxs-lookup"><span data-stu-id="8e001-209">LTrim</span></span>](#ltrim) |[<span data-ttu-id="8e001-210">Mid</span><span class="sxs-lookup"><span data-stu-id="8e001-210">Mid</span></span>](#mid) | |
| [<span data-ttu-id="8e001-211">PadLeft</span><span class="sxs-lookup"><span data-stu-id="8e001-211">PadLeft</span></span>](#padleft) |[<span data-ttu-id="8e001-212">PadRight</span><span class="sxs-lookup"><span data-stu-id="8e001-212">PadRight</span></span>](#padright) |[<span data-ttu-id="8e001-213">PCase</span><span class="sxs-lookup"><span data-stu-id="8e001-213">PCase</span></span>](#pcase) |[<span data-ttu-id="8e001-214">Cserélje le</span><span class="sxs-lookup"><span data-stu-id="8e001-214">Replace</span></span>](#replace) | |
| [<span data-ttu-id="8e001-215">ReplaceChars</span><span class="sxs-lookup"><span data-stu-id="8e001-215">ReplaceChars</span></span>](#replacechars) |[<span data-ttu-id="8e001-216">Jobbra</span><span class="sxs-lookup"><span data-stu-id="8e001-216">Right</span></span>](#right) |[<span data-ttu-id="8e001-217">RTrim</span><span class="sxs-lookup"><span data-stu-id="8e001-217">RTrim</span></span>](#rtrim) |[<span data-ttu-id="8e001-218">Trim</span><span class="sxs-lookup"><span data-stu-id="8e001-218">Trim</span></span>](#trim) | |
| [<span data-ttu-id="8e001-219">UCase</span><span class="sxs-lookup"><span data-stu-id="8e001-219">UCase</span></span>](#ucase) |[<span data-ttu-id="8e001-220">Word</span><span class="sxs-lookup"><span data-stu-id="8e001-220">Word</span></span>](#word) | | | |

- - -
### <a name="bitand"></a><span data-ttu-id="8e001-221">BitAnd</span><span class="sxs-lookup"><span data-stu-id="8e001-221">BitAnd</span></span>
<span data-ttu-id="8e001-222">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-222">**Description:**</span></span>  
<span data-ttu-id="8e001-223">hello BitAnd függvény megadott bits értéket állítja be.</span><span class="sxs-lookup"><span data-stu-id="8e001-223">hello BitAnd function sets specified bits on a value.</span></span>

<span data-ttu-id="8e001-224">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-224">**Syntax:**</span></span>  
`num BitAnd(num value1, num value2)`

* <span data-ttu-id="8e001-225">Érték1, érték2: numerikus érték, amely együtt kell-e érték</span><span class="sxs-lookup"><span data-stu-id="8e001-225">value1, value2: numeric values that should be AND’ed together</span></span>

<span data-ttu-id="8e001-226">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-226">**Remarks:**</span></span>  
<span data-ttu-id="8e001-227">Ez a funkció mindkét paraméterek toohello bináris megjelenítése alakítja át, és beállítja egy kicsit:</span><span class="sxs-lookup"><span data-stu-id="8e001-227">This function converts both parameters toohello binary representation and sets a bit to:</span></span>

* <span data-ttu-id="8e001-228">0 – Ha az egyik vagy mindkét hello megfelelő bit *maszk* és *jelző* : 0</span><span class="sxs-lookup"><span data-stu-id="8e001-228">0 - if one or both of hello corresponding bits in *mask* and *flag* are 0</span></span>
* <span data-ttu-id="8e001-229">1 – Ha hello megfelelő bits mindkettő az 1.</span><span class="sxs-lookup"><span data-stu-id="8e001-229">1 - if both of hello corresponding bits are 1.</span></span>

<span data-ttu-id="8e001-230">Ez azt jelenti akkor adja vissza 0 minden esetben, kivéve ha hello megfelelő bitek száma mindkét paraméter 1.</span><span class="sxs-lookup"><span data-stu-id="8e001-230">In other words, it returns 0 in all cases except when hello corresponding bits of both parameters are 1.</span></span>

<span data-ttu-id="8e001-231">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-231">**Example:**</span></span>  
`BitAnd(&HF, &HF7)`  
<span data-ttu-id="8e001-232">7 adja vissza, mivel a hexadecimális "F" és "F7" kiértékelése toothis érték.</span><span class="sxs-lookup"><span data-stu-id="8e001-232">Returns 7 because hexadecimal "F" AND "F7" evaluate toothis value.</span></span>

- - -
### <a name="bitor"></a><span data-ttu-id="8e001-233">BitOr</span><span class="sxs-lookup"><span data-stu-id="8e001-233">BitOr</span></span>
<span data-ttu-id="8e001-234">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-234">**Description:**</span></span>  
<span data-ttu-id="8e001-235">hello BitOr függvény megadott bits értéket állítja be.</span><span class="sxs-lookup"><span data-stu-id="8e001-235">hello BitOr function sets specified bits on a value.</span></span>

<span data-ttu-id="8e001-236">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-236">**Syntax:**</span></span>  
`num BitOr(num value1, num value2)`

* <span data-ttu-id="8e001-237">Érték1, érték2: numerikus érték, amely együtt kell-e köztük</span><span class="sxs-lookup"><span data-stu-id="8e001-237">value1, value2: numeric values that should be OR’ed together</span></span>

<span data-ttu-id="8e001-238">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-238">**Remarks:**</span></span>  
<span data-ttu-id="8e001-239">Ez a funkció mindkét paraméterek toohello bináris megjelenítése konvertálja, és beállítja egy bit too1, ha az egyik vagy mindkét hello megfelelő bit maszk és jelző: 1 és too0 Ha mindkét hello megfelelő bits 0.</span><span class="sxs-lookup"><span data-stu-id="8e001-239">This function converts both parameters toohello binary representation and sets a bit too1 if one or both of hello corresponding bits in mask and flag are 1, and too0 if both of hello corresponding bits are 0.</span></span> <span data-ttu-id="8e001-240">Ez azt jelenti az 1 értékkel tér vissza minden esetben kivéve ahol hello megfelelő mindkét paraméter bitje 0.</span><span class="sxs-lookup"><span data-stu-id="8e001-240">In other words, it returns 1 in all cases except where hello corresponding bits of both parameters are 0.</span></span>

- - -
### <a name="cbool"></a><span data-ttu-id="8e001-241">CBool</span><span class="sxs-lookup"><span data-stu-id="8e001-241">CBool</span></span>
<span data-ttu-id="8e001-242">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-242">**Description:**</span></span>  
<span data-ttu-id="8e001-243">hello CBool függvényt ad vissza, olyan logikai érték alapján értékeli ki hello kifejezés</span><span class="sxs-lookup"><span data-stu-id="8e001-243">hello CBool function returns a Boolean based on hello evaluated expression</span></span>

<span data-ttu-id="8e001-244">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-244">**Syntax:**</span></span>  
`bool CBool(exp Expression)`

<span data-ttu-id="8e001-245">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-245">**Remarks:**</span></span>  
<span data-ttu-id="8e001-246">Ha hello kifejezés tooa nullától eltérő értéket, majd CBool igaz értéket ad vissza, ellenkező esetben a visszatérési hamis.</span><span class="sxs-lookup"><span data-stu-id="8e001-246">If hello expression evaluates tooa nonzero value, then CBool returns True, else it returns False.</span></span>

<span data-ttu-id="8e001-247">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-247">**Example:**</span></span>  
`CBool([attrib1] = [attrib2])`  

<span data-ttu-id="8e001-248">Értéke True, ha mindkét attribútumok beolvasása hello ugyanazt az értéket.</span><span class="sxs-lookup"><span data-stu-id="8e001-248">Returns True if both attributes have hello same value.</span></span>

- - -
### <a name="cdate"></a><span data-ttu-id="8e001-249">CDate</span><span class="sxs-lookup"><span data-stu-id="8e001-249">CDate</span></span>
<span data-ttu-id="8e001-250">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-250">**Description:**</span></span>  
<span data-ttu-id="8e001-251">hello CDate függvény UTC DateTime egy karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-251">hello CDate function returns a UTC DateTime from a string.</span></span> <span data-ttu-id="8e001-252">Dátum és idő nincs szinkronban natív attribútumtípus, de egyes funkciókat használják.</span><span class="sxs-lookup"><span data-stu-id="8e001-252">DateTime is not a native attribute type in Sync but is used by some functions.</span></span>

<span data-ttu-id="8e001-253">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-253">**Syntax:**</span></span>  
`dt CDate(str value)`

* <span data-ttu-id="8e001-254">Érték: A karakterlánc dátum, idő, és opcionálisan időzóna</span><span class="sxs-lookup"><span data-stu-id="8e001-254">Value: A string with a date, time, and optionally time zone</span></span>

<span data-ttu-id="8e001-255">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-255">**Remarks:**</span></span>  
<span data-ttu-id="8e001-256">hello visszaadott karakterlánc mindig UTC formátumban.</span><span class="sxs-lookup"><span data-stu-id="8e001-256">hello returned string is always in UTC.</span></span>

<span data-ttu-id="8e001-257">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-257">**Example:**</span></span>  
`CDate([employeeStartTime])`  
<span data-ttu-id="8e001-258">Visszaadja egy dátum és idő alapján hello alkalmazott kezdési ideje</span><span class="sxs-lookup"><span data-stu-id="8e001-258">Returns a DateTime based on hello employee’s start time</span></span>

`CDate("2013-01-10 4:00 PM -8")`  
<span data-ttu-id="8e001-259">Visszaadja egy jelölő dátum és idő "2013-01-11 12:00-kor"</span><span class="sxs-lookup"><span data-stu-id="8e001-259">Returns a DateTime representing "2013-01-11 12:00 AM"</span></span>








- - -
### <a name="certextensionoids"></a><span data-ttu-id="8e001-260">CertExtensionOids</span><span class="sxs-lookup"><span data-stu-id="8e001-260">CertExtensionOids</span></span>
<span data-ttu-id="8e001-261">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-261">**Description:**</span></span>  
<span data-ttu-id="8e001-262">Vissza az összes hello kritikus bővítmény tanúsítvány objektum Oid értékek hello.</span><span class="sxs-lookup"><span data-stu-id="8e001-262">Returns hello Oid values of all hello critical extensions of a certificate object.</span></span>

<span data-ttu-id="8e001-263">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-263">**Syntax:**</span></span>  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   <span data-ttu-id="8e001-264">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-264">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-265">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-265">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certformat"></a><span data-ttu-id="8e001-266">CertFormat</span><span class="sxs-lookup"><span data-stu-id="8e001-266">CertFormat</span></span>
<span data-ttu-id="8e001-267">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-267">**Description:**</span></span>  
<span data-ttu-id="8e001-268">Beolvasása hello hello formátum a X.509v3 tanúsítvány nevét.</span><span class="sxs-lookup"><span data-stu-id="8e001-268">Returns hello name of hello format of this X.509v3 certificate.</span></span>

<span data-ttu-id="8e001-269">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-269">**Syntax:**</span></span>  
`str CertFormat(binary certificateRawData)`  
*   <span data-ttu-id="8e001-270">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-270">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-271">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-271">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certfriendlyname"></a><span data-ttu-id="8e001-272">CertFriendlyName</span><span class="sxs-lookup"><span data-stu-id="8e001-272">CertFriendlyName</span></span>
<span data-ttu-id="8e001-273">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-273">**Description:**</span></span>  
<span data-ttu-id="8e001-274">Tanúsítvány alias társított hello adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-274">Returns hello associated alias for a certificate.</span></span>

<span data-ttu-id="8e001-275">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-275">**Syntax:**</span></span>  
`str CertFriendlyName(binary certificateRawData)`  
*   <span data-ttu-id="8e001-276">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-276">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-277">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-277">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certhashstring"></a><span data-ttu-id="8e001-278">CertHashString</span><span class="sxs-lookup"><span data-stu-id="8e001-278">CertHashString</span></span>
<span data-ttu-id="8e001-279">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-279">**Description:**</span></span>  
<span data-ttu-id="8e001-280">Beolvasása hello X.509v3 tanúsítványt SHA1-kivonatot hello hexadecimális karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-280">Returns hello SHA1 hash value for hello X.509v3 certificate as a hexadecimal string.</span></span>

<span data-ttu-id="8e001-281">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-281">**Syntax:**</span></span>  
`str CertHashString(binary certificateRawData)`  
*   <span data-ttu-id="8e001-282">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-282">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-283">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-283">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissuer"></a><span data-ttu-id="8e001-284">CertIssuer</span><span class="sxs-lookup"><span data-stu-id="8e001-284">CertIssuer</span></span>
<span data-ttu-id="8e001-285">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-285">**Description:**</span></span>  
<span data-ttu-id="8e001-286">Beolvasása hello hello X.509v3 tanúsítványt kiállító hitelesítésszolgáltató hello nevét.</span><span class="sxs-lookup"><span data-stu-id="8e001-286">Returns hello name of hello certificate authority that issued hello X.509v3 certificate.</span></span>

<span data-ttu-id="8e001-287">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-287">**Syntax:**</span></span>  
`str CertIssuer(binary certificateRawData)`  
*   <span data-ttu-id="8e001-288">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-288">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-289">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-289">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissuerdn"></a><span data-ttu-id="8e001-290">CertIssuerDN</span><span class="sxs-lookup"><span data-stu-id="8e001-290">CertIssuerDN</span></span>
<span data-ttu-id="8e001-291">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-291">**Description:**</span></span>  
<span data-ttu-id="8e001-292">Beolvasása hello hello tanúsítvány kiállítójának megkülönböztető neve.</span><span class="sxs-lookup"><span data-stu-id="8e001-292">Returns hello distinguished name of hello certificate issuer.</span></span>

<span data-ttu-id="8e001-293">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-293">**Syntax:**</span></span>  
`str CertIssuerDN(binary certificateRawData)`  
*   <span data-ttu-id="8e001-294">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-294">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-295">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-295">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissueroid"></a><span data-ttu-id="8e001-296">CertIssuerOid</span><span class="sxs-lookup"><span data-stu-id="8e001-296">CertIssuerOid</span></span>
<span data-ttu-id="8e001-297">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-297">**Description:**</span></span>  
<span data-ttu-id="8e001-298">Beolvasása hello Oid hello tanúsítványt kibocsátó.</span><span class="sxs-lookup"><span data-stu-id="8e001-298">Returns hello Oid of hello certificate issuer.</span></span>

<span data-ttu-id="8e001-299">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-299">**Syntax:**</span></span>  
`str CertIssuerOid(binary certificateRawData)`  
*   <span data-ttu-id="8e001-300">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-300">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-301">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-301">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certkeyalgorithm"></a><span data-ttu-id="8e001-302">CertKeyAlgorithm</span><span class="sxs-lookup"><span data-stu-id="8e001-302">CertKeyAlgorithm</span></span>
<span data-ttu-id="8e001-303">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-303">**Description:**</span></span>  
<span data-ttu-id="8e001-304">A string típusúként a X.509v3 tanúsítványt hello nyilvánoskulcs-képzési algoritmus információt adhatja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-304">Returns hello key algorithm information for this X.509v3 certificate as a string.</span></span>

<span data-ttu-id="8e001-305">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-305">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="8e001-306">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-306">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-307">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-307">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certkeyalgorithmparams"></a><span data-ttu-id="8e001-308">CertKeyAlgorithmParams</span><span class="sxs-lookup"><span data-stu-id="8e001-308">CertKeyAlgorithmParams</span></span>
<span data-ttu-id="8e001-309">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-309">**Description:**</span></span>  
<span data-ttu-id="8e001-310">Hello nyilvánoskulcs-képzési algoritmus paramétereinek hello X.509v3 tanúsítványt Hexadecimális karakterláncként adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-310">Returns hello key algorithm parameters for hello X.509v3 certificate as a hexadecimal string.</span></span>

<span data-ttu-id="8e001-311">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-311">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="8e001-312">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-312">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-313">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-313">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certnameinfo"></a><span data-ttu-id="8e001-314">CertNameInfo</span><span class="sxs-lookup"><span data-stu-id="8e001-314">CertNameInfo</span></span>
<span data-ttu-id="8e001-315">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-315">**Description:**</span></span>  
<span data-ttu-id="8e001-316">Ad vissza hello tulajdonos és a kiállító nevét a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="8e001-316">Returns hello subject and issuer names from a certificate.</span></span>

<span data-ttu-id="8e001-317">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-317">**Syntax:**</span></span>  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   <span data-ttu-id="8e001-318">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-318">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-319">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-319">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>
*   <span data-ttu-id="8e001-320">X509NameType: hello hello tulajdonos X509NameType értékét.</span><span class="sxs-lookup"><span data-stu-id="8e001-320">X509NameType: hello X509NameType value for hello subject.</span></span>
*   <span data-ttu-id="8e001-321">includesIssuerName: true tooinclude hello kibocsátó neve; Ellenkező esetben hamis.</span><span class="sxs-lookup"><span data-stu-id="8e001-321">includesIssuerName: true tooinclude hello issuer name; otherwise, false.</span></span>

- - -
### <a name="certnotafter"></a><span data-ttu-id="8e001-322">CertNotAfter</span><span class="sxs-lookup"><span data-stu-id="8e001-322">CertNotAfter</span></span>
<span data-ttu-id="8e001-323">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-323">**Description:**</span></span>  
<span data-ttu-id="8e001-324">Hello dátumot adja vissza, amely után a tanúsítvány már nem érvényes helyi idő.</span><span class="sxs-lookup"><span data-stu-id="8e001-324">Returns hello date in local time after which a certificate is no longer valid.</span></span>

<span data-ttu-id="8e001-325">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-325">**Syntax:**</span></span>  
`dt CertNotAfter(binary certificateRawData)`  
*   <span data-ttu-id="8e001-326">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-326">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-327">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-327">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certnotbefore"></a><span data-ttu-id="8e001-328">CertNotBefore</span><span class="sxs-lookup"><span data-stu-id="8e001-328">CertNotBefore</span></span>
<span data-ttu-id="8e001-329">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-329">**Description:**</span></span>  
<span data-ttu-id="8e001-330">A tanúsítvány hatályba lép, a helyi idő hello dátumot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-330">Returns hello date in local time on which a certificate becomes valid.</span></span>

<span data-ttu-id="8e001-331">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-331">**Syntax:**</span></span>  
`dt CertNotBefore(binary certificateRawData)`  
*   <span data-ttu-id="8e001-332">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-332">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-333">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-333">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certpublickeyoid"></a><span data-ttu-id="8e001-334">CertPublicKeyOid</span><span class="sxs-lookup"><span data-stu-id="8e001-334">CertPublicKeyOid</span></span>
<span data-ttu-id="8e001-335">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-335">**Description:**</span></span>  
<span data-ttu-id="8e001-336">Beolvasása hello Oid hello hello X.509v3 tanúsítványhoz tartozó nyilvános kulcs.</span><span class="sxs-lookup"><span data-stu-id="8e001-336">Returns hello Oid of hello public key for hello X.509v3 certificate.</span></span>

<span data-ttu-id="8e001-337">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-337">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="8e001-338">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-338">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-339">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-339">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certpublickeyparametersoid"></a><span data-ttu-id="8e001-340">CertPublicKeyParametersOid</span><span class="sxs-lookup"><span data-stu-id="8e001-340">CertPublicKeyParametersOid</span></span>
<span data-ttu-id="8e001-341">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-341">**Description:**</span></span>  
<span data-ttu-id="8e001-342">Vissza a nyilvános kulcs paraméterek hello hello X.509v3 tanúsítványt Oid hello.</span><span class="sxs-lookup"><span data-stu-id="8e001-342">Returns hello Oid of hello public key parameters for hello X.509v3 certificate.</span></span>

<span data-ttu-id="8e001-343">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-343">**Syntax:**</span></span>  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   <span data-ttu-id="8e001-344">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-344">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-345">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-345">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certserialnumber"></a><span data-ttu-id="8e001-346">CertSerialNumber</span><span class="sxs-lookup"><span data-stu-id="8e001-346">CertSerialNumber</span></span>
<span data-ttu-id="8e001-347">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-347">**Description:**</span></span>  
<span data-ttu-id="8e001-348">Hello hello X.509v3 tanúsítvány sorozatszámát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-348">Returns hello serial number of hello X.509v3 certificate.</span></span>

<span data-ttu-id="8e001-349">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-349">**Syntax:**</span></span>  
`str CertSerialNumber(binary certificateRawData)`  
*   <span data-ttu-id="8e001-350">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-350">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-351">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-351">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsignaturealgorithmoid"></a><span data-ttu-id="8e001-352">CertSignatureAlgorithmOid</span><span class="sxs-lookup"><span data-stu-id="8e001-352">CertSignatureAlgorithmOid</span></span>
<span data-ttu-id="8e001-353">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-353">**Description:**</span></span>  
<span data-ttu-id="8e001-354">Beolvasása hello hello algoritmus Oid toocreate hello aláírási tanúsítványt használja.</span><span class="sxs-lookup"><span data-stu-id="8e001-354">Returns hello Oid of hello algorithm used toocreate hello signature of a certificate.</span></span>

<span data-ttu-id="8e001-355">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-355">**Syntax:**</span></span>  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   <span data-ttu-id="8e001-356">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-356">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-357">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-357">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubject"></a><span data-ttu-id="8e001-358">CertSubject</span><span class="sxs-lookup"><span data-stu-id="8e001-358">CertSubject</span></span>
<span data-ttu-id="8e001-359">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-359">**Description:**</span></span>  
<span data-ttu-id="8e001-360">Lekérdezi hello tulajdonos tanúsítványon megjelölt megkülönböztető neve.</span><span class="sxs-lookup"><span data-stu-id="8e001-360">Gets hello subject distinguished name from a certificate.</span></span>

<span data-ttu-id="8e001-361">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-361">**Syntax:**</span></span>  
`str CertSubject(binary certificateRawData)`  
*   <span data-ttu-id="8e001-362">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-362">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-363">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-363">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubjectnamedn"></a><span data-ttu-id="8e001-364">CertSubjectNameDN</span><span class="sxs-lookup"><span data-stu-id="8e001-364">CertSubjectNameDN</span></span>
<span data-ttu-id="8e001-365">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-365">**Description:**</span></span>  
<span data-ttu-id="8e001-366">Beolvasása hello tulajdonos tanúsítványon megjelölt megkülönböztető neve.</span><span class="sxs-lookup"><span data-stu-id="8e001-366">Returns hello subject distinguished name from a certificate.</span></span>

<span data-ttu-id="8e001-367">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-367">**Syntax:**</span></span>  
`str CertSubjectNameDN(binary certificateRawData)`  
*   <span data-ttu-id="8e001-368">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-368">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-369">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-369">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubjectnameoid"></a><span data-ttu-id="8e001-370">CertSubjectNameOid</span><span class="sxs-lookup"><span data-stu-id="8e001-370">CertSubjectNameOid</span></span>
<span data-ttu-id="8e001-371">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-371">**Description:**</span></span>  
<span data-ttu-id="8e001-372">Vissza a tanúsítvány tulajdonos neve hello Oid hello.</span><span class="sxs-lookup"><span data-stu-id="8e001-372">Returns hello Oid of hello subject name from a certificate.</span></span>

<span data-ttu-id="8e001-373">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-373">**Syntax:**</span></span>  
`str CertSubjectNameOid(binary certificateRawData)`  
*   <span data-ttu-id="8e001-374">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-374">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-375">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-375">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certthumbprint"></a><span data-ttu-id="8e001-376">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="8e001-376">CertThumbprint</span></span>
<span data-ttu-id="8e001-377">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-377">**Description:**</span></span>  
<span data-ttu-id="8e001-378">Egy tanúsítvány hello ujjlenyomata értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-378">Returns hello thumbprint of a certificate.</span></span>

<span data-ttu-id="8e001-379">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-379">**Syntax:**</span></span>  
`str CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="8e001-380">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-380">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-381">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-381">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certversion"></a><span data-ttu-id="8e001-382">CertVersion</span><span class="sxs-lookup"><span data-stu-id="8e001-382">CertVersion</span></span>
<span data-ttu-id="8e001-383">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-383">**Description:**</span></span>  
<span data-ttu-id="8e001-384">Vissza a tanúsítvány X.509 formátumú verziója hello.</span><span class="sxs-lookup"><span data-stu-id="8e001-384">Returns hello X.509 format version of a certificate.</span></span>

<span data-ttu-id="8e001-385">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-385">**Syntax:**</span></span>  
`str CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="8e001-386">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-386">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-387">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-387">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="cguid"></a><span data-ttu-id="8e001-388">CGuid</span><span class="sxs-lookup"><span data-stu-id="8e001-388">CGuid</span></span>
<span data-ttu-id="8e001-389">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-389">**Description:**</span></span>  
<span data-ttu-id="8e001-390">hello CGuid függvény alakítja át a GUID tooits bináris megjelenítése a hello karakterláncos ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="8e001-390">hello CGuid function converts hello string representation of a GUID tooits binary representation.</span></span>

<span data-ttu-id="8e001-391">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-391">**Syntax:**</span></span>  
`bin CGuid(str GUID)`

* <span data-ttu-id="8e001-392">Ebben a mintában formátumú karakterlánc: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx vagy {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span><span class="sxs-lookup"><span data-stu-id="8e001-392">A String formatted in this pattern: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx or {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span></span>

- - -
### <a name="contains"></a><span data-ttu-id="8e001-393">Contains</span><span class="sxs-lookup"><span data-stu-id="8e001-393">Contains</span></span>
<span data-ttu-id="8e001-394">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-394">**Description:**</span></span>  
<span data-ttu-id="8e001-395">hello Contains keresése belüli egy többértékű karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8e001-395">hello Contains function finds a string inside a multi-valued attribute</span></span>

<span data-ttu-id="8e001-396">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-396">**Syntax:**</span></span>  
<span data-ttu-id="8e001-397">`num Contains (mvstring attribute, str search)`-nagybetűk</span><span class="sxs-lookup"><span data-stu-id="8e001-397">`num Contains (mvstring attribute, str search)` - case-sensitive</span></span>  
`num Contains (mvstring attribute, str search, enum Casetype)`  
<span data-ttu-id="8e001-398">`num Contains (mvref attribute, str search)`-nagybetűk</span><span class="sxs-lookup"><span data-stu-id="8e001-398">`num Contains (mvref attribute, str search)` - case-sensitive</span></span>

* <span data-ttu-id="8e001-399">attribútum: hello többértékű attribútum toosearch.</span><span class="sxs-lookup"><span data-stu-id="8e001-399">attribute: hello multi-valued attribute toosearch.</span></span>
* <span data-ttu-id="8e001-400">Keresés: hello attribútum toofind karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-400">search: string toofind in hello attribute.</span></span>
* <span data-ttu-id="8e001-401">Casetype: CaseInsensitive vagy CaseSensitive.</span><span class="sxs-lookup"><span data-stu-id="8e001-401">Casetype: CaseInsensitive or CaseSensitive.</span></span>

<span data-ttu-id="8e001-402">Ahol hello karakterlánc található hello többértékű attribútumban index értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-402">Returns index in hello multi-valued attribute where hello string was found.</span></span> <span data-ttu-id="8e001-403">0 eredményül, ha a hello karakterlánc nem található.</span><span class="sxs-lookup"><span data-stu-id="8e001-403">0 is returned if hello string is not found.</span></span>

<span data-ttu-id="8e001-404">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-404">**Remarks:**</span></span>  
<span data-ttu-id="8e001-405">Attribútumok a többértékű karakterlánc hello kereséséhez karakterláncrészletek hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="8e001-405">For multi-valued string attributes, hello search finds substrings in hello values.</span></span>  
<span data-ttu-id="8e001-406">Hivatkozás attribútumok hello keresett karakterlánc pontosan egyeznie kell hello érték toobe egyezés számít.</span><span class="sxs-lookup"><span data-stu-id="8e001-406">For reference attributes, hello searched string must exactly match hello value toobe considered a match.</span></span>

<span data-ttu-id="8e001-407">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-407">**Example:**</span></span>  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
<span data-ttu-id="8e001-408">Ha hello proxyAddresses attribútum egy elsődleges e-mail címet (nagybetűs által jelzett "SMTP:"), majd térjen vissza a hello proxyAddress attribútum, ellenkező esetben egy hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="8e001-408">If hello proxyAddresses attribute has a primary email address (indicated by uppercase "SMTP:"), then return hello proxyAddress attribute, else return an error.</span></span>

- - -
### <a name="convertfrombase64"></a><span data-ttu-id="8e001-409">ConvertFromBase64</span><span class="sxs-lookup"><span data-stu-id="8e001-409">ConvertFromBase64</span></span>
<span data-ttu-id="8e001-410">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-410">**Description:**</span></span>  
<span data-ttu-id="8e001-411">hello ConvertFromBase64 függvény konvertálja hello megadott base64-kódolású érték tooa rendszeres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-411">hello ConvertFromBase64 function converts hello specified base64 encoded value tooa regular string.</span></span>

<span data-ttu-id="8e001-412">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-412">**Syntax:**</span></span>  
<span data-ttu-id="8e001-413">`str ConvertFromBase64(str source)`-feltételezi, hogy a Unicode kódolási</span><span class="sxs-lookup"><span data-stu-id="8e001-413">`str ConvertFromBase64(str source)` - assumes Unicode for encoding</span></span>  
`str ConvertFromBase64(str source, enum Encoding)`

* <span data-ttu-id="8e001-414">Forrás: Base64 kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8e001-414">source: Base64 encoded string</span></span>  
* <span data-ttu-id="8e001-415">Kódolást: Unicode, ASCII, UTF8</span><span class="sxs-lookup"><span data-stu-id="8e001-415">Encoding: Unicode, ASCII, UTF8</span></span>

<span data-ttu-id="8e001-416">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-416">**Example**</span></span>  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

<span data-ttu-id="8e001-417">Mindkét példák vissza "*Hello world!*"</span><span class="sxs-lookup"><span data-stu-id="8e001-417">Both examples return "*Hello world!*"</span></span>

- - -
### <a name="convertfromutf8hex"></a><span data-ttu-id="8e001-418">ConvertFromUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="8e001-418">ConvertFromUTF8Hex</span></span>
<span data-ttu-id="8e001-419">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-419">**Description:**</span></span>  
<span data-ttu-id="8e001-420">hello ConvertFromUTF8Hex függvény konvertálja hello megadott kódolású UTF8 hexadecimális érték tooa karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-420">hello ConvertFromUTF8Hex function converts hello specified UTF8 Hex encoded value tooa string.</span></span>

<span data-ttu-id="8e001-421">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-421">**Syntax:**</span></span>  
`str ConvertFromUTF8Hex(str source)`

* <span data-ttu-id="8e001-422">Forrás: UTF8 2 bájtos kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8e001-422">source: UTF8 2-byte encoded sting</span></span>

<span data-ttu-id="8e001-423">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-423">**Remarks:**</span></span>  
<span data-ttu-id="8e001-424">hello a függvény és ConvertFromBase64([],UTF8) közötti hello eredményező különbség rövid hello megkülönböztető név attribútum.</span><span class="sxs-lookup"><span data-stu-id="8e001-424">hello difference between this function and ConvertFromBase64([],UTF8) in that hello result is friendly for hello DN attribute.</span></span>  
<span data-ttu-id="8e001-425">Ezt a formátumot használja Azure Active Directory megkülönböztető neve.</span><span class="sxs-lookup"><span data-stu-id="8e001-425">This format is used by Azure Active Directory as DN.</span></span>

<span data-ttu-id="8e001-426">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-426">**Example:**</span></span>  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
<span data-ttu-id="8e001-427">Értéket ad vissza "*Hello world!*"</span><span class="sxs-lookup"><span data-stu-id="8e001-427">Returns "*Hello world!*"</span></span>

- - -
### <a name="converttobase64"></a><span data-ttu-id="8e001-428">ConvertToBase64</span><span class="sxs-lookup"><span data-stu-id="8e001-428">ConvertToBase64</span></span>
<span data-ttu-id="8e001-429">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-429">**Description:**</span></span>  
<span data-ttu-id="8e001-430">hello ConvertToBase64 függvény egy karakterlánc tooa Unicode Base64 kódolású karakterlánc alakítja át.</span><span class="sxs-lookup"><span data-stu-id="8e001-430">hello ConvertToBase64 function converts a string tooa Unicode base64 string.</span></span>  
<span data-ttu-id="8e001-431">Egész számok tooits egyenértékű karakterlánc-ábrázolása számjegyükkel base-64 kódolású tömbje hello értékének alakítja.</span><span class="sxs-lookup"><span data-stu-id="8e001-431">Converts hello value of an array of integers tooits equivalent string representation that is encoded with base-64 digits.</span></span>

<span data-ttu-id="8e001-432">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-432">**Syntax:**</span></span>  
`str ConvertToBase64(str source)`

<span data-ttu-id="8e001-433">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-433">**Example:**</span></span>  
`ConvertToBase64("Hello world!")`  
<span data-ttu-id="8e001-434">Visszaadja a "SABlAGwAbABvACAAdwBvAHIAbABkACEA"</span><span class="sxs-lookup"><span data-stu-id="8e001-434">Returns "SABlAGwAbABvACAAdwBvAHIAbABkACEA"</span></span>

- - -
### <a name="converttoutf8hex"></a><span data-ttu-id="8e001-435">ConvertToUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="8e001-435">ConvertToUTF8Hex</span></span>
<span data-ttu-id="8e001-436">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-436">**Description:**</span></span>  
<span data-ttu-id="8e001-437">hello ConvertToUTF8Hex függvény egy karakterlánc tooa kódolású UTF8 hexadecimális érték alakítja át.</span><span class="sxs-lookup"><span data-stu-id="8e001-437">hello ConvertToUTF8Hex function converts a string tooa UTF8 Hex encoded value.</span></span>

<span data-ttu-id="8e001-438">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-438">**Syntax:**</span></span>  
`str ConvertToUTF8Hex(str source)`

<span data-ttu-id="8e001-439">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-439">**Remarks:**</span></span>  
<span data-ttu-id="8e001-440">hello kimeneti formátum függvény lesz az Azure Active Directory megkülönböztető név attribútum formátuma.</span><span class="sxs-lookup"><span data-stu-id="8e001-440">hello output format of this function is used by Azure Active Directory as DN attribute format.</span></span>

<span data-ttu-id="8e001-441">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-441">**Example:**</span></span>  
`ConvertToUTF8Hex("Hello world!")`  
<span data-ttu-id="8e001-442">48656C6C6F20776F726C6421 beolvasása</span><span class="sxs-lookup"><span data-stu-id="8e001-442">Returns 48656C6C6F20776F726C6421</span></span>

- - -
### <a name="count"></a><span data-ttu-id="8e001-443">Darabszám</span><span class="sxs-lookup"><span data-stu-id="8e001-443">Count</span></span>
<span data-ttu-id="8e001-444">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-444">**Description:**</span></span>  
<span data-ttu-id="8e001-445">hello Count függvénnyel többértékű attribútum hello számú elemet ad vissza</span><span class="sxs-lookup"><span data-stu-id="8e001-445">hello Count function returns hello number of elements in a multi-valued attribute</span></span>

<span data-ttu-id="8e001-446">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-446">**Syntax:**</span></span>  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a><span data-ttu-id="8e001-447">CNum</span><span class="sxs-lookup"><span data-stu-id="8e001-447">CNum</span></span>
<span data-ttu-id="8e001-448">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-448">**Description:**</span></span>  
<span data-ttu-id="8e001-449">hello CNum függvény lép egy karakterláncot, és adja vissza egy numerikus adattípusú.</span><span class="sxs-lookup"><span data-stu-id="8e001-449">hello CNum function takes a string and returns a numeric data type.</span></span>

<span data-ttu-id="8e001-450">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-450">**Syntax:**</span></span>  
`num CNum(str value)`

- - -
### <a name="cref"></a><span data-ttu-id="8e001-451">CRef</span><span class="sxs-lookup"><span data-stu-id="8e001-451">CRef</span></span>
<span data-ttu-id="8e001-452">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-452">**Description:**</span></span>  
<span data-ttu-id="8e001-453">Egy karakterlánc tooa hivatkozási attribútum konvertálja</span><span class="sxs-lookup"><span data-stu-id="8e001-453">Converts a string tooa reference attribute</span></span>

<span data-ttu-id="8e001-454">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-454">**Syntax:**</span></span>  
`ref CRef(str value)`

<span data-ttu-id="8e001-455">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-455">**Example:**</span></span>  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

- - -
### <a name="cstr"></a><span data-ttu-id="8e001-456">CStr</span><span class="sxs-lookup"><span data-stu-id="8e001-456">CStr</span></span>
<span data-ttu-id="8e001-457">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-457">**Description:**</span></span>  
<span data-ttu-id="8e001-458">hello CStr függvény tooa karakterlánc adattípusra alakítja át.</span><span class="sxs-lookup"><span data-stu-id="8e001-458">hello CStr function converts tooa string data type.</span></span>

<span data-ttu-id="8e001-459">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-459">**Syntax:**</span></span>  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* <span data-ttu-id="8e001-460">érték: egy numerikus értéket, a hivatkozási attribútum, vagy a logikai érték lehet.</span><span class="sxs-lookup"><span data-stu-id="8e001-460">value: Can be a numeric value, reference attribute, or Boolean.</span></span>

<span data-ttu-id="8e001-461">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-461">**Example:**</span></span>  
`CStr([dn])`  
<span data-ttu-id="8e001-462">Visszaadhatja "cn = Joe, dc = contoso, dc = com"</span><span class="sxs-lookup"><span data-stu-id="8e001-462">Could return "cn=Joe,dc=contoso,dc=com"</span></span>

- - -
### <a name="dateadd"></a><span data-ttu-id="8e001-463">DateAdd</span><span class="sxs-lookup"><span data-stu-id="8e001-463">DateAdd</span></span>
<span data-ttu-id="8e001-464">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-464">**Description:**</span></span>  
<span data-ttu-id="8e001-465">A megadott időtartam hozzáadását dátum toowhich tartalmazó egy dátumot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-465">Returns a Date containing a date toowhich a specified time interval has been added.</span></span>

<span data-ttu-id="8e001-466">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-466">**Syntax:**</span></span>  
`dt DateAdd(str interval, num value, dt date)`

* <span data-ttu-id="8e001-467">időköz: karakterlánc-kifejezés, amely azt szeretné, hogy tooadd hello időközt.</span><span class="sxs-lookup"><span data-stu-id="8e001-467">interval: String expression that is hello interval of time you want tooadd.</span></span> <span data-ttu-id="8e001-468">hello karakterlánc hello a következő értékek egyikével kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="8e001-468">hello string must have one of hello following values:</span></span>
  * <span data-ttu-id="8e001-469">ÉÉÉÉ év</span><span class="sxs-lookup"><span data-stu-id="8e001-469">yyyy Year</span></span>
  * <span data-ttu-id="8e001-470">q negyedév</span><span class="sxs-lookup"><span data-stu-id="8e001-470">q Quarter</span></span>
  * <span data-ttu-id="8e001-471">m hónap</span><span class="sxs-lookup"><span data-stu-id="8e001-471">m Month</span></span>
  * <span data-ttu-id="8e001-472">y év napja</span><span class="sxs-lookup"><span data-stu-id="8e001-472">y Day of year</span></span>
  * <span data-ttu-id="8e001-473">d nap</span><span class="sxs-lookup"><span data-stu-id="8e001-473">d Day</span></span>
  * <span data-ttu-id="8e001-474">w hét napja</span><span class="sxs-lookup"><span data-stu-id="8e001-474">w Weekday</span></span>
  * <span data-ttu-id="8e001-475">hh hét</span><span class="sxs-lookup"><span data-stu-id="8e001-475">ww Week</span></span>
  * <span data-ttu-id="8e001-476">h óra</span><span class="sxs-lookup"><span data-stu-id="8e001-476">h Hour</span></span>
  * <span data-ttu-id="8e001-477">n perc</span><span class="sxs-lookup"><span data-stu-id="8e001-477">n Minute</span></span>
  * <span data-ttu-id="8e001-478">s második</span><span class="sxs-lookup"><span data-stu-id="8e001-478">s Second</span></span>
* <span data-ttu-id="8e001-479">érték: hello szám egységek tooadd szeretné.</span><span class="sxs-lookup"><span data-stu-id="8e001-479">value: hello number of units you want tooadd.</span></span> <span data-ttu-id="8e001-480">Ez lehet a pozitív (hello jövőbeli dátumok tooget) vagy negatív (elmúlt hello tooget dátumok).</span><span class="sxs-lookup"><span data-stu-id="8e001-480">It can be positive (tooget dates in hello future) or negative (tooget dates in hello past).</span></span>
* <span data-ttu-id="8e001-481">dátum: DateTime képviselő toowhich hello dátumtartományt kerül.</span><span class="sxs-lookup"><span data-stu-id="8e001-481">date: DateTime representing date toowhich hello interval is added.</span></span>

<span data-ttu-id="8e001-482">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-482">**Example:**</span></span>  
`DateAdd("m", 3, CDate("2001-01-01"))`  
<span data-ttu-id="8e001-483">3 hónapos hozzáadja, és egy dátum és idő "2001-04-01" jelző adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-483">Adds 3 months and returns a DateTime representing "2001-04-01".</span></span>

- - -
### <a name="datefromnum"></a><span data-ttu-id="8e001-484">DateFromNum</span><span class="sxs-lookup"><span data-stu-id="8e001-484">DateFromNum</span></span>
<span data-ttu-id="8e001-485">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-485">**Description:**</span></span>  
<span data-ttu-id="8e001-486">hello DateFromNum függvény alakít egy értéket a AD meg dátum formátum tooa dátum/idő típusú.</span><span class="sxs-lookup"><span data-stu-id="8e001-486">hello DateFromNum function converts a value in AD’s date format tooa DateTime type.</span></span>

<span data-ttu-id="8e001-487">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-487">**Syntax:**</span></span>  
`dt DateFromNum(num value)`

<span data-ttu-id="8e001-488">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-488">**Example:**</span></span>  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
<span data-ttu-id="8e001-489">Visszaadja a 2012-01-01 jelölő dátum és idő 23:00:00</span><span class="sxs-lookup"><span data-stu-id="8e001-489">Returns a DateTime representing 2012-01-01 23:00:00</span></span>

- - -
### <a name="dncomponent"></a><span data-ttu-id="8e001-490">DNComponent</span><span class="sxs-lookup"><span data-stu-id="8e001-490">DNComponent</span></span>
<span data-ttu-id="8e001-491">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-491">**Description:**</span></span>  
<span data-ttu-id="8e001-492">hello DNComponent funkció egy megadott DN összetevő balról fog hello értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-492">hello DNComponent function returns hello value of a specified DN component going from left.</span></span>

<span data-ttu-id="8e001-493">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-493">**Syntax:**</span></span>  
`str DNComponent(ref dn, num ComponentNumber)`

* <span data-ttu-id="8e001-494">megkülönböztető név: hello hivatkozási attribútum toointerpret</span><span class="sxs-lookup"><span data-stu-id="8e001-494">dn: hello reference attribute toointerpret</span></span>
* <span data-ttu-id="8e001-495">ComponentNumber: a hello DN tooreturn hello összetevője</span><span class="sxs-lookup"><span data-stu-id="8e001-495">ComponentNumber: hello component in hello DN tooreturn</span></span>

<span data-ttu-id="8e001-496">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-496">**Example:**</span></span>  
`DNComponent([dn],1)`  
<span data-ttu-id="8e001-497">Megkülönböztető név esetén "cn = Joe, ou =...," Joe adja vissza</span><span class="sxs-lookup"><span data-stu-id="8e001-497">If dn is "cn=Joe,ou=…," it returns Joe</span></span>

- - -
### <a name="dncomponentrev"></a><span data-ttu-id="8e001-498">DNComponentRev</span><span class="sxs-lookup"><span data-stu-id="8e001-498">DNComponentRev</span></span>
<span data-ttu-id="8e001-499">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-499">**Description:**</span></span>  
<span data-ttu-id="8e001-500">hello DNComponentRev függvény jobb (hello célból) üzembe helyezésről meghatározott DN összetevője hello értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-500">hello DNComponentRev function returns hello value of a specified DN component going from right (hello end).</span></span>

<span data-ttu-id="8e001-501">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-501">**Syntax:**</span></span>  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* <span data-ttu-id="8e001-502">megkülönböztető név: hello hivatkozási attribútum toointerpret</span><span class="sxs-lookup"><span data-stu-id="8e001-502">dn: hello reference attribute toointerpret</span></span>
* <span data-ttu-id="8e001-503">ComponentNumber - hello DN tooreturn hello összetevő</span><span class="sxs-lookup"><span data-stu-id="8e001-503">ComponentNumber - hello component in hello DN tooreturn</span></span>
* <span data-ttu-id="8e001-504">Beállítások: DC – figyelmen kívül az összes összetevő "dc ="</span><span class="sxs-lookup"><span data-stu-id="8e001-504">Options: DC – Ignore all components with "dc="</span></span>

<span data-ttu-id="8e001-505">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-505">**Example:**</span></span>  
<span data-ttu-id="8e001-506">Megkülönböztető név esetén "cn Joe, ou = Atlanta –, ou = GA, ou = = US, dc = contoso, dc = com" majd</span><span class="sxs-lookup"><span data-stu-id="8e001-506">If dn is "cn=Joe,ou=Atlanta,ou=GA,ou=US, dc=contoso,dc=com" then</span></span>  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
<span data-ttu-id="8e001-507">Mindkét vissza VELÜNK.</span><span class="sxs-lookup"><span data-stu-id="8e001-507">Both return US.</span></span>

- - -
### <a name="error"></a><span data-ttu-id="8e001-508">Hiba</span><span class="sxs-lookup"><span data-stu-id="8e001-508">Error</span></span>
<span data-ttu-id="8e001-509">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-509">**Description:**</span></span>  
<span data-ttu-id="8e001-510">hello hiba függvény használt tooreturn egyéni hiba.</span><span class="sxs-lookup"><span data-stu-id="8e001-510">hello Error function is used tooreturn a custom error.</span></span>

<span data-ttu-id="8e001-511">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-511">**Syntax:**</span></span>  
`void Error(str ErrorMessage)`

<span data-ttu-id="8e001-512">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-512">**Example:**</span></span>  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
<span data-ttu-id="8e001-513">Hello attribútum accountName nincs jelen, ha a hiba throw hello objektumon.</span><span class="sxs-lookup"><span data-stu-id="8e001-513">If hello attribute accountName is not present, throw an error on hello object.</span></span>

- - -
### <a name="escapedncomponent"></a><span data-ttu-id="8e001-514">EscapeDNComponent</span><span class="sxs-lookup"><span data-stu-id="8e001-514">EscapeDNComponent</span></span>
<span data-ttu-id="8e001-515">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-515">**Description:**</span></span>  
<span data-ttu-id="8e001-516">hello EscapeDNComponent függvény időt vesz igénybe a DN egy összetevőt, és így ábrázolhatók az LDAP-kiszolgálón lehet kilépni.</span><span class="sxs-lookup"><span data-stu-id="8e001-516">hello EscapeDNComponent function takes one component of a DN and escapes it so it can be represented in LDAP.</span></span>

<span data-ttu-id="8e001-517">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-517">**Syntax:**</span></span>  
`str EscapeDNComponent(str value)`

<span data-ttu-id="8e001-518">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-518">**Example:**</span></span>  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
<span data-ttu-id="8e001-519">Ellenőrzi, hogy hello objektum egy LDAP-címtár is létrehozható, akkor is, ha hello displayName attribútum karakterek, amelyek az LDAP-kiszolgálón kell megjelölni.</span><span class="sxs-lookup"><span data-stu-id="8e001-519">Makes sure hello object can be created in an LDAP directory even if hello displayName attribute has characters that must be escaped in LDAP.</span></span>

- - -
### <a name="formatdatetime"></a><span data-ttu-id="8e001-520">FormatDateTime</span><span class="sxs-lookup"><span data-stu-id="8e001-520">FormatDateTime</span></span>
<span data-ttu-id="8e001-521">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-521">**Description:**</span></span>  
<span data-ttu-id="8e001-522">hello FormatDateTime függvény használt tooformat megadott formátumú dátum és idő tooa karakterláncot</span><span class="sxs-lookup"><span data-stu-id="8e001-522">hello FormatDateTime function is used tooformat a DateTime tooa string with a specified format</span></span>

<span data-ttu-id="8e001-523">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-523">**Syntax:**</span></span>  
`str FormatDateTime(dt value, str format)`

* <span data-ttu-id="8e001-524">érték: hello dátum és idő formátumú érték</span><span class="sxs-lookup"><span data-stu-id="8e001-524">value: a value in hello DateTime format</span></span>
* <span data-ttu-id="8e001-525">formátum: hello formátum tooconvert való képviselő karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="8e001-525">format: a string representing hello format tooconvert to.</span></span>

<span data-ttu-id="8e001-526">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-526">**Remarks:**</span></span>  
<span data-ttu-id="8e001-527">a lehetséges értékek hello a hello formátum itt találhatók: [felhasználói dátum és idő formátumú (Format függvény)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)</span><span class="sxs-lookup"><span data-stu-id="8e001-527">hello possible values for hello format can be found here: [User-Defined Date/Time Formats (Format Function)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)</span></span>

<span data-ttu-id="8e001-528">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-528">**Example:**</span></span>  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
<span data-ttu-id="8e001-529">"2007-12-25" eredményez.</span><span class="sxs-lookup"><span data-stu-id="8e001-529">Results in "2007-12-25".</span></span>

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
<span data-ttu-id="8e001-530">"20140905081453.0Z" eredményezhet</span><span class="sxs-lookup"><span data-stu-id="8e001-530">Can result in "20140905081453.0Z"</span></span>

- - -
### <a name="guid"></a><span data-ttu-id="8e001-531">GUID</span><span class="sxs-lookup"><span data-stu-id="8e001-531">GUID</span></span>
<span data-ttu-id="8e001-532">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-532">**Description:**</span></span>  
<span data-ttu-id="8e001-533">hello függvény GUID hoz létre egy új véletlenszerű GUID-ja</span><span class="sxs-lookup"><span data-stu-id="8e001-533">hello function GUID generates a new random GUID</span></span>

<span data-ttu-id="8e001-534">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-534">**Syntax:**</span></span>  
`str GUID()`

- - -
### <a name="iif"></a><span data-ttu-id="8e001-535">AZ IIF</span><span class="sxs-lookup"><span data-stu-id="8e001-535">IIF</span></span>
<span data-ttu-id="8e001-536">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-536">**Description:**</span></span>  
<span data-ttu-id="8e001-537">hello IIF függvény a megadott feltétel alapján lehetséges értékek egyikét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-537">hello IIF function returns one of a set of possible values based on a specified condition.</span></span>

<span data-ttu-id="8e001-538">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-538">**Syntax:**</span></span>  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* <span data-ttu-id="8e001-539">feltétel: bármely érték vagy kifejezés, amely lehet értékelni tootrue vagy HAMIS eredményt ad.</span><span class="sxs-lookup"><span data-stu-id="8e001-539">condition: any value or expression that can be evaluated tootrue or false.</span></span>
* <span data-ttu-id="8e001-540">valueIfTrue: hello feltétel tootrue, ha hello értéket adott vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-540">valueIfTrue: If hello condition evaluates tootrue, hello returned value.</span></span>
* <span data-ttu-id="8e001-541">valueIfFalse: hello feltétel toofalse, ha hello értéket adott vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-541">valueIfFalse: If hello condition evaluates toofalse, hello returned value.</span></span>

<span data-ttu-id="8e001-542">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-542">**Example:**</span></span>  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 <span data-ttu-id="8e001-543">Az internalizálási hello felhasználó esetén ad vissza, hello alias a felhasználó a "t-" hozzáadott toohello elejére, más hello felhasználói alias, adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-543">If hello user is an intern, returns hello alias of a user with "t-" added toohello beginning of it, else returns hello user’s alias as is.</span></span>

- - -
### <a name="instr"></a><span data-ttu-id="8e001-544">InStr</span><span class="sxs-lookup"><span data-stu-id="8e001-544">InStr</span></span>
<span data-ttu-id="8e001-545">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-545">**Description:**</span></span>  
<span data-ttu-id="8e001-546">hello InStr függvény hello első előfordulásának a substring talál egy karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="8e001-546">hello InStr function finds hello first occurrence of a substring in a string</span></span>

<span data-ttu-id="8e001-547">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-547">**Syntax:**</span></span>  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* <span data-ttu-id="8e001-548">stringcheck: a keresés toobe karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8e001-548">stringcheck: string toobe searched</span></span>
* <span data-ttu-id="8e001-549">stringmatch: található toobe karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8e001-549">stringmatch: string toobe found</span></span>
* <span data-ttu-id="8e001-550">Start: pozíció toofind hello substring indítása</span><span class="sxs-lookup"><span data-stu-id="8e001-550">start: starting position toofind hello substring</span></span>
* <span data-ttu-id="8e001-551">Hasonlítsa össze: vbTextCompare vagy vbBinaryCompare</span><span class="sxs-lookup"><span data-stu-id="8e001-551">compare: vbTextCompare or vbBinaryCompare</span></span>

<span data-ttu-id="8e001-552">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-552">**Remarks:**</span></span>  
<span data-ttu-id="8e001-553">Hol található a hello substring értéket ad vissza hello pozíciója vagy 0, ha nem található.</span><span class="sxs-lookup"><span data-stu-id="8e001-553">Returns hello position where hello substring was found or 0 if not found.</span></span>

<span data-ttu-id="8e001-554">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-554">**Example:**</span></span>  
`InStr("hello quick brown fox","quick")`  
<span data-ttu-id="8e001-555">Evalues too5</span><span class="sxs-lookup"><span data-stu-id="8e001-555">Evalues too5</span></span>

`InStr("repEated","e",3,vbBinaryCompare)`  
<span data-ttu-id="8e001-556">Kiértékeli too7</span><span class="sxs-lookup"><span data-stu-id="8e001-556">Evaluates too7</span></span>

- - -
### <a name="instrrev"></a><span data-ttu-id="8e001-557">InStrRev</span><span class="sxs-lookup"><span data-stu-id="8e001-557">InStrRev</span></span>
<span data-ttu-id="8e001-558">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-558">**Description:**</span></span>  
<span data-ttu-id="8e001-559">hello InStrRev függvény karakterláncrész utolsó előfordulásának hello talál egy karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="8e001-559">hello InStrRev function finds hello last occurrence of a substring in a string</span></span>

<span data-ttu-id="8e001-560">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-560">**Syntax:**</span></span>  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* <span data-ttu-id="8e001-561">stringcheck: a keresés toobe karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8e001-561">stringcheck: string toobe searched</span></span>
* <span data-ttu-id="8e001-562">stringmatch: található toobe karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8e001-562">stringmatch: string toobe found</span></span>
* <span data-ttu-id="8e001-563">Start: pozíció toofind hello substring indítása</span><span class="sxs-lookup"><span data-stu-id="8e001-563">start: starting position toofind hello substring</span></span>
* <span data-ttu-id="8e001-564">Hasonlítsa össze: vbTextCompare vagy vbBinaryCompare</span><span class="sxs-lookup"><span data-stu-id="8e001-564">compare: vbTextCompare or vbBinaryCompare</span></span>

<span data-ttu-id="8e001-565">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-565">**Remarks:**</span></span>  
<span data-ttu-id="8e001-566">Hol található a hello substring értéket ad vissza hello pozíciója vagy 0, ha nem található.</span><span class="sxs-lookup"><span data-stu-id="8e001-566">Returns hello position where hello substring was found or 0 if not found.</span></span>

<span data-ttu-id="8e001-567">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-567">**Example:**</span></span>  
`InStrRev("abbcdbbbef","bb")`  
<span data-ttu-id="8e001-568">7 beolvasása</span><span class="sxs-lookup"><span data-stu-id="8e001-568">Returns 7</span></span>

- - -
### <a name="isbitset"></a><span data-ttu-id="8e001-569">IsBitSet</span><span class="sxs-lookup"><span data-stu-id="8e001-569">IsBitSet</span></span>
<span data-ttu-id="8e001-570">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-570">**Description:**</span></span>  
<span data-ttu-id="8e001-571">hello függvény IsBitSet teszteket, ha egy kicsit van beállítva, vagy sem</span><span class="sxs-lookup"><span data-stu-id="8e001-571">hello function IsBitSet Tests if a bit is set or not</span></span>

<span data-ttu-id="8e001-572">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-572">**Syntax:**</span></span>  
`bool IsBitSet(num value, num flag)`

* <span data-ttu-id="8e001-573">érték: egy numerikus értéket, amely evaluated.flag: egy numerikus érték, amely rendelkezik hello bit toobe kiértékelése</span><span class="sxs-lookup"><span data-stu-id="8e001-573">value: a numeric value that is evaluated.flag: a numeric value that has hello bit toobe evaluated</span></span>

<span data-ttu-id="8e001-574">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-574">**Example:**</span></span>  
`IsBitSet(&HF,4)`  
<span data-ttu-id="8e001-575">Igaz értéket ad vissza, mert a bit "4" hello hexadecimális érték: "F" be van állítva</span><span class="sxs-lookup"><span data-stu-id="8e001-575">Returns True because bit "4" is set in hello hexadecimal value "F"</span></span>

- - -
### <a name="isdate"></a><span data-ttu-id="8e001-576">IsDate</span><span class="sxs-lookup"><span data-stu-id="8e001-576">IsDate</span></span>
<span data-ttu-id="8e001-577">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-577">**Description:**</span></span>  
<span data-ttu-id="8e001-578">Ha hello kifejezés lehet majd hello IsDate függvény kiértékeli tooTrue kiértékeli egy dátum/idő típusként.</span><span class="sxs-lookup"><span data-stu-id="8e001-578">If hello expression can be evaluates as a DateTime type, then hello IsDate function evaluates tooTrue.</span></span>

<span data-ttu-id="8e001-579">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-579">**Syntax:**</span></span>  
`bool IsDate(var Expression)`

<span data-ttu-id="8e001-580">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-580">**Remarks:**</span></span>  
<span data-ttu-id="8e001-581">Toodetermine használja, ha CDate() sikeres lehet.</span><span class="sxs-lookup"><span data-stu-id="8e001-581">Used toodetermine if CDate() can be successful.</span></span>

- - -
### <a name="iscert"></a><span data-ttu-id="8e001-582">IsCert</span><span class="sxs-lookup"><span data-stu-id="8e001-582">IsCert</span></span>
<span data-ttu-id="8e001-583">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-583">**Description:**</span></span>  
<span data-ttu-id="8e001-584">Igaz értéket ad eredményül, ha a nyers adatok hello .NET X509Certificate2 tanúsítványobjektum lehet szerializálni.</span><span class="sxs-lookup"><span data-stu-id="8e001-584">Returns true if hello raw data can be serialized into .NET X509Certificate2 certificate object.</span></span>

<span data-ttu-id="8e001-585">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-585">**Syntax:**</span></span>  
`bool CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="8e001-586">certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8e001-586">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="8e001-587">hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8e001-587">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>
- - -
### <a name="isempty"></a><span data-ttu-id="8e001-588">Vagyis IsEmpty</span><span class="sxs-lookup"><span data-stu-id="8e001-588">IsEmpty</span></span>
<span data-ttu-id="8e001-589">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-589">**Description:**</span></span>  
<span data-ttu-id="8e001-590">Ha hello attribútum hello CS vagy MV jelen, de üres karakterlánc tooan kiértékeli, vagyis IsEmpty függvény hello tooTrue értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="8e001-590">If hello attribute is present in hello CS or MV but evaluates tooan empty string, then hello IsEmpty function evaluates tooTrue.</span></span>

<span data-ttu-id="8e001-591">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-591">**Syntax:**</span></span>  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a><span data-ttu-id="8e001-592">IsGuid</span><span class="sxs-lookup"><span data-stu-id="8e001-592">IsGuid</span></span>
<span data-ttu-id="8e001-593">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-593">**Description:**</span></span>  
<span data-ttu-id="8e001-594">Ha hello karakterlánc lehet konvertált tooa GUID, hello IsGuid függvény tootrue értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="8e001-594">If hello string could be converted tooa GUID, then hello IsGuid function evaluated tootrue.</span></span>

<span data-ttu-id="8e001-595">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-595">**Syntax:**</span></span>  
`bool IsGuid(str GUID)`

<span data-ttu-id="8e001-596">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-596">**Remarks:**</span></span>  
<span data-ttu-id="8e001-597">GUID egy ezeket a mintákat a következő karakterláncként van definiálva: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx vagy {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span><span class="sxs-lookup"><span data-stu-id="8e001-597">A GUID is defined as a string following one of these patterns: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx or {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span></span>

<span data-ttu-id="8e001-598">Toodetermine használja, ha CGuid() sikeres lehet.</span><span class="sxs-lookup"><span data-stu-id="8e001-598">Used toodetermine if CGuid() can be successful.</span></span>

<span data-ttu-id="8e001-599">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-599">**Example:**</span></span>  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
<span data-ttu-id="8e001-600">Ha hello StrAttribute GUID formátuma, térjen vissza a bináris megjelenítése, ellenkező esetben adhat vissza Null.</span><span class="sxs-lookup"><span data-stu-id="8e001-600">If hello StrAttribute has a GUID format, return a binary representation, otherwise return a Null.</span></span>

- - -
### <a name="isnull"></a><span data-ttu-id="8e001-601">IsNull</span><span class="sxs-lookup"><span data-stu-id="8e001-601">IsNull</span></span>
<span data-ttu-id="8e001-602">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-602">**Description:**</span></span>  
<span data-ttu-id="8e001-603">Ha a kifejezés hello tooNull, majd hello IsNull függvény igaz értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-603">If hello expression evaluates tooNull, then hello IsNull function returns true.</span></span>

<span data-ttu-id="8e001-604">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-604">**Syntax:**</span></span>  
`bool IsNull(var Expression)`

<span data-ttu-id="8e001-605">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-605">**Remarks:**</span></span>  
<span data-ttu-id="8e001-606">Az attribútum egy Null hello hiányában hello attribútum által van kifejezve.</span><span class="sxs-lookup"><span data-stu-id="8e001-606">For an attribute, a Null is expressed by hello absence of hello attribute.</span></span>

<span data-ttu-id="8e001-607">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-607">**Example:**</span></span>  
`IsNull([displayName])`  
<span data-ttu-id="8e001-608">Igaz értéket ad vissza, ha nincs jelen hello CS vagy MV hello attribútum.</span><span class="sxs-lookup"><span data-stu-id="8e001-608">Returns True if hello attribute is not present in hello CS or MV.</span></span>

- - -
### <a name="isnullorempty"></a><span data-ttu-id="8e001-609">IsNullOrEmpty</span><span class="sxs-lookup"><span data-stu-id="8e001-609">IsNullOrEmpty</span></span>
<span data-ttu-id="8e001-610">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-610">**Description:**</span></span>  
<span data-ttu-id="8e001-611">Ha hello kifejezés null vagy üres karakterlánc, majd hello IsNullOrEmpty függvény igaz értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-611">If hello expression is null or an empty string, then hello IsNullOrEmpty function returns true.</span></span>

<span data-ttu-id="8e001-612">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-612">**Syntax:**</span></span>  
`bool IsNullOrEmpty(var Expression)`

<span data-ttu-id="8e001-613">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-613">**Remarks:**</span></span>  
<span data-ttu-id="8e001-614">Az attribútum a értékelné ki tooTrue, ha hello attribútum hiányzik, vagy megtalálható, de üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-614">For an attribute, this would evaluate tooTrue if hello attribute is absent or is present but is an empty string.</span></span>  
<span data-ttu-id="8e001-615">Ez a funkció hello inverzét IsPresent neve.</span><span class="sxs-lookup"><span data-stu-id="8e001-615">hello inverse of this function is named IsPresent.</span></span>

<span data-ttu-id="8e001-616">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-616">**Example:**</span></span>  
`IsNullOrEmpty([displayName])`  
<span data-ttu-id="8e001-617">Igaz értéket ad vissza, ha hello attribútum nincs jelen vagy hello CS vagy MV üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-617">Returns True if hello attribute is not present or is an empty string in hello CS or MV.</span></span>

- - -
### <a name="isnumeric"></a><span data-ttu-id="8e001-618">IsNumeric</span><span class="sxs-lookup"><span data-stu-id="8e001-618">IsNumeric</span></span>
<span data-ttu-id="8e001-619">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-619">**Description:**</span></span>  
<span data-ttu-id="8e001-620">hello IsNumeric függvény jelzi, hogy egy kifejezés kiértékelése számú típusként logikai értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-620">hello IsNumeric function returns a Boolean value indicating whether an expression can be evaluated as a number type.</span></span>

<span data-ttu-id="8e001-621">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-621">**Syntax:**</span></span>  
`bool IsNumeric(var Expression)`

<span data-ttu-id="8e001-622">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-622">**Remarks:**</span></span>  
<span data-ttu-id="8e001-623">Toodetermine használja, ha CNum() lehet sikeres tooparse hello kifejezés.</span><span class="sxs-lookup"><span data-stu-id="8e001-623">Used toodetermine if CNum() can be successful tooparse hello expression.</span></span>

- - -
### <a name="isstring"></a><span data-ttu-id="8e001-624">IsString</span><span class="sxs-lookup"><span data-stu-id="8e001-624">IsString</span></span>
<span data-ttu-id="8e001-625">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-625">**Description:**</span></span>  
<span data-ttu-id="8e001-626">Ha hello kifejezés is lehet kiértékelt tooa karakterlánc típusúnak, majd hello IsString függvény tooTrue értékelődik ki.</span><span class="sxs-lookup"><span data-stu-id="8e001-626">If hello expression can be evaluated tooa string type, then hello IsString function evaluates tooTrue.</span></span>

<span data-ttu-id="8e001-627">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-627">**Syntax:**</span></span>  
`bool IsString(var expression)`

<span data-ttu-id="8e001-628">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-628">**Remarks:**</span></span>  
<span data-ttu-id="8e001-629">Toodetermine használja, ha CStr() lehet sikeres tooparse hello kifejezés.</span><span class="sxs-lookup"><span data-stu-id="8e001-629">Used toodetermine if CStr() can be successful tooparse hello expression.</span></span>

- - -
### <a name="ispresent"></a><span data-ttu-id="8e001-630">IsPresent</span><span class="sxs-lookup"><span data-stu-id="8e001-630">IsPresent</span></span>
<span data-ttu-id="8e001-631">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-631">**Description:**</span></span>  
<span data-ttu-id="8e001-632">Ha hello kifejezés értéke nem Null, és nem üres tooa karakterláncot, majd hello IsPresent függvény igaz értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-632">If hello expression evaluates tooa string that is not Null and is not empty, then hello IsPresent function returns true.</span></span>

<span data-ttu-id="8e001-633">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-633">**Syntax:**</span></span>  
`bool IsPresent(var expression)`

<span data-ttu-id="8e001-634">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-634">**Remarks:**</span></span>  
<span data-ttu-id="8e001-635">Ez a funkció hello inverzét IsNullOrEmpty neve.</span><span class="sxs-lookup"><span data-stu-id="8e001-635">hello inverse of this function is named IsNullOrEmpty.</span></span>

<span data-ttu-id="8e001-636">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-636">**Example:**</span></span>  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a><span data-ttu-id="8e001-637">Elem</span><span class="sxs-lookup"><span data-stu-id="8e001-637">Item</span></span>
<span data-ttu-id="8e001-638">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-638">**Description:**</span></span>  
<span data-ttu-id="8e001-639">hello Item függvény egy elemet egy többértékű karakterlánc attribútum ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-639">hello Item function returns one item from a multi-valued string/attribute.</span></span>

<span data-ttu-id="8e001-640">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-640">**Syntax:**</span></span>  
`var Item(mvstr attribute, num index)`

* <span data-ttu-id="8e001-641">attribútum: többértékű attribútum</span><span class="sxs-lookup"><span data-stu-id="8e001-641">attribute: multi-valued attribute</span></span>
* <span data-ttu-id="8e001-642">index: hello többértékű karakterlánc tooan elemének indexét.</span><span class="sxs-lookup"><span data-stu-id="8e001-642">index: index tooan item in hello multi-valued string.</span></span>

<span data-ttu-id="8e001-643">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-643">**Remarks:**</span></span>  
<span data-ttu-id="8e001-644">hello Item függvény akkor hasznos, hello Contains függvény együtt, mivel ez utóbbi függvény hello hello többértékű attribútum hello index tooan elemét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-644">hello Item function is useful together with hello Contains function since hello latter function returns hello index tooan item in hello multi-valued attribute.</span></span>

<span data-ttu-id="8e001-645">Hibát jelez, ha az index az értéktartományon kívül esik.</span><span class="sxs-lookup"><span data-stu-id="8e001-645">Throws an error if index is out of bounds.</span></span>

<span data-ttu-id="8e001-646">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-646">**Example:**</span></span>  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
<span data-ttu-id="8e001-647">Beolvasása hello elsődleges e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="8e001-647">Returns hello primary email address.</span></span>

- - -
### <a name="itemornull"></a><span data-ttu-id="8e001-648">ItemOrNull</span><span class="sxs-lookup"><span data-stu-id="8e001-648">ItemOrNull</span></span>
<span data-ttu-id="8e001-649">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-649">**Description:**</span></span>  
<span data-ttu-id="8e001-650">hello ItemOrNull függvény egy elemet egy többértékű karakterlánc attribútum ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-650">hello ItemOrNull function returns one item from a multi-valued string/attribute.</span></span>

<span data-ttu-id="8e001-651">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-651">**Syntax:**</span></span>  
`var ItemOrNull(mvstr attribute, num index)`

* <span data-ttu-id="8e001-652">attribútum: többértékű attribútum</span><span class="sxs-lookup"><span data-stu-id="8e001-652">attribute: multi-valued attribute</span></span>
* <span data-ttu-id="8e001-653">index: hello többértékű karakterlánc tooan elemének indexét.</span><span class="sxs-lookup"><span data-stu-id="8e001-653">index: index tooan item in hello multi-valued string.</span></span>

<span data-ttu-id="8e001-654">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-654">**Remarks:**</span></span>  
<span data-ttu-id="8e001-655">hello ItemOrNull függvény akkor hasznos, hello Contains függvény együtt, mivel ez utóbbi függvény hello hello többértékű attribútum hello index tooan elemét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-655">hello ItemOrNull function is useful together with hello Contains function since hello latter function returns hello index tooan item in hello multi-valued attribute.</span></span>

<span data-ttu-id="8e001-656">Ha az index az értéktartományon kívül esik, majd adja vissza Null értéket.</span><span class="sxs-lookup"><span data-stu-id="8e001-656">If index is out of bounds, then returns a Null value.</span></span>

- - -
### <a name="join"></a><span data-ttu-id="8e001-657">Csatlakozás</span><span class="sxs-lookup"><span data-stu-id="8e001-657">Join</span></span>
<span data-ttu-id="8e001-658">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-658">**Description:**</span></span>  
<span data-ttu-id="8e001-659">hello illesztési függvényhez időt vesz igénybe a többértékű karakterlánc, és az egyes elemek között megadott elválasztó egyértékű karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-659">hello Join function takes a multi-valued string and returns a single-valued string with specified separator inserted between each item.</span></span>

<span data-ttu-id="8e001-660">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-660">**Syntax:**</span></span>  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* <span data-ttu-id="8e001-661">attribútum: csatlakoztatott toobe többértékű attribútumot tartalmazó karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="8e001-661">attribute: Multi-valued attribute containing strings toobe joined.</span></span>
* <span data-ttu-id="8e001-662">elválasztó karakter: A karakterlánc, a használt tooseparate hello karakterláncrészletet tartalmazza a karakterláncot adott vissza hello.</span><span class="sxs-lookup"><span data-stu-id="8e001-662">delimiter: Any string, used tooseparate hello substrings in hello returned string.</span></span> <span data-ttu-id="8e001-663">Ha nincs megadva, hello szóköz karakter ("") használatos.</span><span class="sxs-lookup"><span data-stu-id="8e001-663">If omitted, hello space character (" ") is used.</span></span> <span data-ttu-id="8e001-664">Ha elválasztó karakter egy nulla hosszúságú karakterlánc (""), illetve semmi sem, hello lista összes elemének nélkül határolójelek van kibővítve.</span><span class="sxs-lookup"><span data-stu-id="8e001-664">If Delimiter is a zero-length string ("") or Nothing, all items in hello list are concatenated with no delimiters.</span></span>

<span data-ttu-id="8e001-665">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="8e001-665">**Remarks**</span></span>  
<span data-ttu-id="8e001-666">Paritás hello illesztési és felosztott funkciók között van.</span><span class="sxs-lookup"><span data-stu-id="8e001-666">There is parity between hello Join and Split functions.</span></span> <span data-ttu-id="8e001-667">hello illesztési függvényhez karakterláncok vesz igénybe, és csatlakoztatja őket egy elválasztó karakterlánccal, tooreturn egyetlen karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-667">hello Join function takes an array of strings and joins them using a delimiter string, tooreturn a single string.</span></span> <span data-ttu-id="8e001-668">hello vegyes függvény lép egy karakterláncot, és a hello elválasztó, tooreturn karakterláncok elválasztja azt.</span><span class="sxs-lookup"><span data-stu-id="8e001-668">hello Split function takes a string and separates it at hello delimiter, tooreturn an array of strings.</span></span> <span data-ttu-id="8e001-669">A fő különbség azonban, hogy csatlakozási karakterlánc bármely határoló karakterláncot is összefűzésére, vegyes csak elkülönítheti karakterláncok egy egyetlen karaktert elválasztó használatával.</span><span class="sxs-lookup"><span data-stu-id="8e001-669">However, a key difference is that Join can concatenate strings with any delimiter string, Split can only separate strings using a single character delimiter.</span></span>

<span data-ttu-id="8e001-670">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-670">**Example:**</span></span>  
`Join([proxyAddresses],",")`  
<span data-ttu-id="8e001-671">Visszaadhatja: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"</span><span class="sxs-lookup"><span data-stu-id="8e001-671">Could return: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"</span></span>

- - -
### <a name="lcase"></a><span data-ttu-id="8e001-672">LCase</span><span class="sxs-lookup"><span data-stu-id="8e001-672">LCase</span></span>
<span data-ttu-id="8e001-673">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-673">**Description:**</span></span>  
<span data-ttu-id="8e001-674">hello LCase függvény alakítja az összes karaktert karakterlánc toolower esetben.</span><span class="sxs-lookup"><span data-stu-id="8e001-674">hello LCase function converts all characters in a string toolower case.</span></span>

<span data-ttu-id="8e001-675">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-675">**Syntax:**</span></span>  
`str LCase(str value)`

<span data-ttu-id="8e001-676">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-676">**Example:**</span></span>  
`LCase("TeSt")`  
<span data-ttu-id="8e001-677">"Test" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-677">Returns "test".</span></span>

- - -
### <a name="left"></a><span data-ttu-id="8e001-678">balra</span><span class="sxs-lookup"><span data-stu-id="8e001-678">Left</span></span>
<span data-ttu-id="8e001-679">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-679">**Description:**</span></span>  
<span data-ttu-id="8e001-680">hello bal függvény egy karakterlánc hello balról adja vissza a megadott számú karaktert.</span><span class="sxs-lookup"><span data-stu-id="8e001-680">hello Left function returns a specified number of characters from hello left of a string.</span></span>

<span data-ttu-id="8e001-681">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-681">**Syntax:**</span></span>  
`str Left(str string, num NumChars)`

* <span data-ttu-id="8e001-682">karakterlánc: hello karakterlánc tooreturn karaktert tartalmaz</span><span class="sxs-lookup"><span data-stu-id="8e001-682">string: hello string tooreturn characters from</span></span>
* <span data-ttu-id="8e001-683">NumChars: egy szám karakterek tooreturn elejétől hello (balra) karakterlánc hello száma</span><span class="sxs-lookup"><span data-stu-id="8e001-683">NumChars: a number identifying hello number of characters tooreturn from hello beginning (left) of string</span></span>

<span data-ttu-id="8e001-684">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-684">**Remarks:**</span></span>  
<span data-ttu-id="8e001-685">Hello első numChars karakterláncon belül karaktereket tartalmazó karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="8e001-685">A string containing hello first numChars characters in string:</span></span>

* <span data-ttu-id="8e001-686">Ha numChars = 0, térjen vissza az üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-686">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="8e001-687">Ha numChars < 0, térjen vissza a bemeneti karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-687">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="8e001-688">Ha karakterlánc null értékű, térjen vissza az üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-688">If string is null, return empty string.</span></span>

<span data-ttu-id="8e001-689">Ha karakterlánc numChars megadott hello számnál kevesebb karaktert tartalmaz, a karakterlánc azonos toostring (amely, 1. paraméter levő összes karakter) ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-689">If string contains fewer characters than hello number specified in numChars, a string identical toostring (that is, containing all characters in parameter 1) is returned.</span></span>

<span data-ttu-id="8e001-690">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-690">**Example:**</span></span>  
`Left("John Doe", 3)`  
<span data-ttu-id="8e001-691">"Joh" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-691">Returns "Joh".</span></span>

- - -
### <a name="len"></a><span data-ttu-id="8e001-692">Hossz</span><span class="sxs-lookup"><span data-stu-id="8e001-692">Len</span></span>
<span data-ttu-id="8e001-693">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-693">**Description:**</span></span>  
<span data-ttu-id="8e001-694">hello Len függvény egy karakterlánc karakterek számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-694">hello Len function returns number of characters in a string.</span></span>

<span data-ttu-id="8e001-695">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-695">**Syntax:**</span></span>  
`num Len(str value)`

<span data-ttu-id="8e001-696">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-696">**Example:**</span></span>  
`Len("John Doe")`  
<span data-ttu-id="8e001-697">8 beolvasása</span><span class="sxs-lookup"><span data-stu-id="8e001-697">Returns 8</span></span>

- - -
### <a name="ltrim"></a><span data-ttu-id="8e001-698">LTrim</span><span class="sxs-lookup"><span data-stu-id="8e001-698">LTrim</span></span>
<span data-ttu-id="8e001-699">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-699">**Description:**</span></span>  
<span data-ttu-id="8e001-700">LTrim függvény hello kezdő szóközöket eltávolít egy karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="8e001-700">hello LTrim function removes leading white spaces from a string.</span></span>

<span data-ttu-id="8e001-701">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-701">**Syntax:**</span></span>  
`str LTrim(str value)`

<span data-ttu-id="8e001-702">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-702">**Example:**</span></span>  
`LTrim(" Test ")`  
<span data-ttu-id="8e001-703">"Test" értéket ad vissza</span><span class="sxs-lookup"><span data-stu-id="8e001-703">Returns "Test "</span></span>

- - -
### <a name="mid"></a><span data-ttu-id="8e001-704">Mid</span><span class="sxs-lookup"><span data-stu-id="8e001-704">Mid</span></span>
<span data-ttu-id="8e001-705">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-705">**Description:**</span></span>  
<span data-ttu-id="8e001-706">hello Mid függvény egy karakterlánc megadott helyéről a megadott számú karaktert adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-706">hello Mid function returns a specified number of characters from a specified position in a string.</span></span>

<span data-ttu-id="8e001-707">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-707">**Syntax:**</span></span>  
`str Mid(str string, num start, num NumChars)`

* <span data-ttu-id="8e001-708">karakterlánc: hello karakterlánc tooreturn karaktert tartalmaz</span><span class="sxs-lookup"><span data-stu-id="8e001-708">string: hello string tooreturn characters from</span></span>
* <span data-ttu-id="8e001-709">Indítsa el: egy karakterlánc tooreturn karaktert tartalmaz a kezdőpozíció hello azonosító szám</span><span class="sxs-lookup"><span data-stu-id="8e001-709">start: a number identifying hello starting position in string tooreturn characters from</span></span>
* <span data-ttu-id="8e001-710">NumChars: egy szám karakterek tooreturn karakterlánc helyéről hello száma</span><span class="sxs-lookup"><span data-stu-id="8e001-710">NumChars: a number identifying hello number of characters tooreturn from position in string</span></span>

<span data-ttu-id="8e001-711">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-711">**Remarks:**</span></span>  
<span data-ttu-id="8e001-712">Indítsa el a pozíció visszatérési numChars karakterek karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-712">Return numChars characters starting from position start in string.</span></span>  
<span data-ttu-id="8e001-713">Pozíció indítás karakterláncban numChars karaktereket tartalmazó karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="8e001-713">A string containing numChars characters from position start in string:</span></span>

* <span data-ttu-id="8e001-714">Ha numChars = 0, térjen vissza az üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-714">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="8e001-715">Ha numChars < 0, térjen vissza a bemeneti karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-715">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="8e001-716">Ha start > hello karakterlánc hossza, térjen vissza a bemeneti karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-716">If start > hello length of string, return input string.</span></span>
* <span data-ttu-id="8e001-717">Ha start < = 0, térjen vissza a bemeneti karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-717">If start <= 0, return input string.</span></span>
* <span data-ttu-id="8e001-718">Ha karakterlánc null értékű, térjen vissza az üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-718">If string is null, return empty string.</span></span>

<span data-ttu-id="8e001-719">Ha nem numChar karakterek fennmaradó karakterlánc pozíció indítás, amit a lehető legtöbb karaktert ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-719">If there are not numChar characters remaining in string from position start, as many characters as possible are returned.</span></span>

<span data-ttu-id="8e001-720">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-720">**Example:**</span></span>  
`Mid("John Doe", 3, 5)`  
<span data-ttu-id="8e001-721">"Hn Do" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-721">Returns "hn Do".</span></span>

`Mid("John Doe", 6, 999)`  
<span data-ttu-id="8e001-722">Visszaadja a "Doe"</span><span class="sxs-lookup"><span data-stu-id="8e001-722">Returns "Doe"</span></span>

- - -
### <a name="now"></a><span data-ttu-id="8e001-723">Most</span><span class="sxs-lookup"><span data-stu-id="8e001-723">Now</span></span>
<span data-ttu-id="8e001-724">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-724">**Description:**</span></span>  
<span data-ttu-id="8e001-725">hello most függvény adja vissza egy dátum és idő megadása az aktuális dátum és idő, hello tooyour számítógép rendszer dátum és idő alapján történik.</span><span class="sxs-lookup"><span data-stu-id="8e001-725">hello Now function returns a DateTime specifying hello current date and time, according tooyour computer's system date and time.</span></span>

<span data-ttu-id="8e001-726">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-726">**Syntax:**</span></span>  
`dt Now()`

- - -
### <a name="numfromdate"></a><span data-ttu-id="8e001-727">NumFromDate</span><span class="sxs-lookup"><span data-stu-id="8e001-727">NumFromDate</span></span>
<span data-ttu-id="8e001-728">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-728">**Description:**</span></span>  
<span data-ttu-id="8e001-729">hello NumFromDate függvény dátumformátum AD meg egy dátumot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-729">hello NumFromDate function returns a date in AD’s date format.</span></span>

<span data-ttu-id="8e001-730">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-730">**Syntax:**</span></span>  
`num NumFromDate(dt value)`

<span data-ttu-id="8e001-731">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-731">**Example:**</span></span>  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
<span data-ttu-id="8e001-732">129699324000000000 beolvasása</span><span class="sxs-lookup"><span data-stu-id="8e001-732">Returns 129699324000000000</span></span>

- - -
### <a name="padleft"></a><span data-ttu-id="8e001-733">PadLeft</span><span class="sxs-lookup"><span data-stu-id="8e001-733">PadLeft</span></span>
<span data-ttu-id="8e001-734">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-734">**Description:**</span></span>  
<span data-ttu-id="8e001-735">hello PadLeft balra-kézi működik a megadott karakterlánc tooa használatával egy megadott Kitöltő karakter hosszúságú.</span><span class="sxs-lookup"><span data-stu-id="8e001-735">hello PadLeft function left-pads a string tooa specified length using a provided padding character.</span></span>

<span data-ttu-id="8e001-736">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-736">**Syntax:**</span></span>  
`str PadLeft(str string, num length, str padCharacter)`

* <span data-ttu-id="8e001-737">karakterlánc: hello karakterlánc toopad.</span><span class="sxs-lookup"><span data-stu-id="8e001-737">string: hello string toopad.</span></span>
* <span data-ttu-id="8e001-738">hossz: hello jelző egész számot szükségeskonfiguráció-karakterlánc hossza.</span><span class="sxs-lookup"><span data-stu-id="8e001-738">length: An integer representing hello desired length of string.</span></span>
* <span data-ttu-id="8e001-739">padCharacter: egy egyetlen karaktert toouse hello kitöltő karakterként karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8e001-739">padCharacter: A string consisting of a single character toouse as hello pad character</span></span>

<span data-ttu-id="8e001-740">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-740">**Remarks:**</span></span>

* <span data-ttu-id="8e001-741">Hello hosszúságú karakterlánc hossza kisebb, majd padCharacter akkor ismételten hozzáfűzött toohello kezdete (balra) karakterlánc amíg rá nem kényszerül hosszának egyenlőnek toolength.</span><span class="sxs-lookup"><span data-stu-id="8e001-741">If hello length of string is less than length, then padCharacter is repeatedly appended toohello beginning (left) of string until it has a length equal toolength.</span></span>
* <span data-ttu-id="8e001-742">padCharacter lehet szóköz karakter, de nem lehet null értékű.</span><span class="sxs-lookup"><span data-stu-id="8e001-742">PadCharacter can be a space character, but it cannot be a null value.</span></span>
* <span data-ttu-id="8e001-743">Ha hello karakterlánc hossza nagyobb, mint egyenlő tooor, karakterlánc ad változatlan vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-743">If hello length of string is equal tooor greater than length, string is returned unchanged.</span></span>
* <span data-ttu-id="8e001-744">Ha a karakterlánc hossza nagyobb, mint vagy egyenlő toolength rendelkezik, egy karakterlánc azonos toostring ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-744">If string has a length greater than or equal toolength, a string identical toostring is returned.</span></span>
* <span data-ttu-id="8e001-745">Ha hello hosszúságú karakterlánc hossza kisebb, hello új karakterlánc szükséges, feltöltve egy padCharacter karakterláncot tartalmazó hosszát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-745">If hello length of string is less than length, then a new string of hello desired length is returned containing string padded with a padCharacter.</span></span>
* <span data-ttu-id="8e001-746">Karakterlánc értéke null, ha a hello függvény egy üres karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-746">If string is null, hello function returns an empty string.</span></span>

<span data-ttu-id="8e001-747">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-747">**Example:**</span></span>  
`PadLeft("User", 10, "0")`  
<span data-ttu-id="8e001-748">"000000User" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-748">Returns "000000User".</span></span>

- - -
### <a name="padright"></a><span data-ttu-id="8e001-749">PadRight</span><span class="sxs-lookup"><span data-stu-id="8e001-749">PadRight</span></span>
<span data-ttu-id="8e001-750">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-750">**Description:**</span></span>  
<span data-ttu-id="8e001-751">hello PadRight jobb-kézi működik a megadott karakterlánc tooa használatával egy megadott Kitöltő karakter hosszúságú.</span><span class="sxs-lookup"><span data-stu-id="8e001-751">hello PadRight function right-pads a string tooa specified length using a provided padding character.</span></span>

<span data-ttu-id="8e001-752">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-752">**Syntax:**</span></span>  
`str PadRight(str string, num length, str padCharacter)`

* <span data-ttu-id="8e001-753">karakterlánc: hello karakterlánc toopad.</span><span class="sxs-lookup"><span data-stu-id="8e001-753">string: hello string toopad.</span></span>
* <span data-ttu-id="8e001-754">hossz: hello jelző egész számot szükségeskonfiguráció-karakterlánc hossza.</span><span class="sxs-lookup"><span data-stu-id="8e001-754">length: An integer representing hello desired length of string.</span></span>
* <span data-ttu-id="8e001-755">padCharacter: egy egyetlen karaktert toouse hello kitöltő karakterként karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8e001-755">padCharacter: A string consisting of a single character toouse as hello pad character</span></span>

<span data-ttu-id="8e001-756">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-756">**Remarks:**</span></span>

* <span data-ttu-id="8e001-757">Hello hosszúságú karakterlánc hossza kisebb, majd padCharacter akkor ismételten hozzáfűzött toohello végét (jobb oldali) karakterlánc amíg rá nem kényszerül hosszának egyenlőnek toolength.</span><span class="sxs-lookup"><span data-stu-id="8e001-757">If hello length of string is less than length, then padCharacter is repeatedly appended toohello end (right) of string until it has a length equal toolength.</span></span>
* <span data-ttu-id="8e001-758">padCharacter lehet szóköz karakter, de nem lehet null értékű.</span><span class="sxs-lookup"><span data-stu-id="8e001-758">padCharacter can be a space character, but it cannot be a null value.</span></span>
* <span data-ttu-id="8e001-759">Ha hello karakterlánc hossza nagyobb, mint egyenlő tooor, karakterlánc ad változatlan vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-759">If hello length of string is equal tooor greater than length, string is returned unchanged.</span></span>
* <span data-ttu-id="8e001-760">Ha a karakterlánc hossza nagyobb, mint vagy egyenlő toolength rendelkezik, egy karakterlánc azonos toostring ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-760">If string has a length greater than or equal toolength, a string identical toostring is returned.</span></span>
* <span data-ttu-id="8e001-761">Ha hello hosszúságú karakterlánc hossza kisebb, hello új karakterlánc szükséges, feltöltve egy padCharacter karakterláncot tartalmazó hosszát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-761">If hello length of string is less than length, then a new string of hello desired length is returned containing string padded with a padCharacter.</span></span>
* <span data-ttu-id="8e001-762">Karakterlánc értéke null, ha a hello függvény egy üres karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-762">If string is null, hello function returns an empty string.</span></span>

<span data-ttu-id="8e001-763">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-763">**Example:**</span></span>  
`PadRight("User", 10, "0")`  
<span data-ttu-id="8e001-764">"User000000" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-764">Returns "User000000".</span></span>

- - -
### <a name="pcase"></a><span data-ttu-id="8e001-765">PCase</span><span class="sxs-lookup"><span data-stu-id="8e001-765">PCase</span></span>
<span data-ttu-id="8e001-766">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-766">**Description:**</span></span>  
<span data-ttu-id="8e001-767">hello PCase függvény konvertálja hello karakterlánc tooupper esetben minden szóközzel elválasztott szó első karaktere, és további karakterekkel alakítja a rendszer toolower eset.</span><span class="sxs-lookup"><span data-stu-id="8e001-767">hello PCase function converts hello first character of each space delimited word in a string tooupper case, and all other characters are converted toolower case.</span></span>

<span data-ttu-id="8e001-768">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-768">**Syntax:**</span></span>  
`String PCase(string)`

<span data-ttu-id="8e001-769">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-769">**Remarks:**</span></span>

* <span data-ttu-id="8e001-770">Ez a funkció jelenleg nem biztosít megfelelő kis-és tooconvert teljesen nagybetű, például az angol szót.</span><span class="sxs-lookup"><span data-stu-id="8e001-770">This function does not currently provide proper casing tooconvert a word that is entirely uppercase, such as an acronym.</span></span>

<span data-ttu-id="8e001-771">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-771">**Example:**</span></span>  
`PCase("TEsT")`  
<span data-ttu-id="8e001-772">"Test" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-772">Returns "Test".</span></span>

`PCase(LCase("TEST"))`  
<span data-ttu-id="8e001-773">"Test" értéket ad vissza</span><span class="sxs-lookup"><span data-stu-id="8e001-773">Returns "Test"</span></span>

- - -
### <a name="randomnum"></a><span data-ttu-id="8e001-774">RandomNum</span><span class="sxs-lookup"><span data-stu-id="8e001-774">RandomNum</span></span>
<span data-ttu-id="8e001-775">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-775">**Description:**</span></span>  
<span data-ttu-id="8e001-776">hello RandomNum függvény között egy megadott időszakban egy véletlenszerű számot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-776">hello RandomNum function returns a random number between a specified interval.</span></span>

<span data-ttu-id="8e001-777">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-777">**Syntax:**</span></span>  
`num RandomNum(num start, num end)`

* <span data-ttu-id="8e001-778">Indítsa el: egy szám azonosító hello határértéke hello véletlenszerű értéket toogenerate</span><span class="sxs-lookup"><span data-stu-id="8e001-778">start: a number identifying hello lower limit of hello random value toogenerate</span></span>
* <span data-ttu-id="8e001-779">vége: egy szám azonosító hello felső korlátja hello véletlenszerű értéket toogenerate</span><span class="sxs-lookup"><span data-stu-id="8e001-779">end: a number identifying hello upper limit of hello random value toogenerate</span></span>

<span data-ttu-id="8e001-780">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-780">**Example:**</span></span>  
`Random(100,999)`  
<span data-ttu-id="8e001-781">734 adhat vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-781">Can return 734.</span></span>

- - -
### <a name="removeduplicates"></a><span data-ttu-id="8e001-782">RemoveDuplicates</span><span class="sxs-lookup"><span data-stu-id="8e001-782">RemoveDuplicates</span></span>
<span data-ttu-id="8e001-783">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-783">**Description:**</span></span>  
<span data-ttu-id="8e001-784">hello RemoveDuplicates függvény időt vesz igénybe a többértékű karakterlánc, és győződjön meg arról, hogy egyedi.</span><span class="sxs-lookup"><span data-stu-id="8e001-784">hello RemoveDuplicates function takes a multi-valued string and make sure each value is unique.</span></span>

<span data-ttu-id="8e001-785">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-785">**Syntax:**</span></span>  
`mvstr RemoveDuplicates(mvstr attribute)`

<span data-ttu-id="8e001-786">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-786">**Example:**</span></span>  
`RemoveDuplicates([proxyAddresses])`  
<span data-ttu-id="8e001-787">Adja vissza egy megtisztított proxyAddress attribútumot, ahol minden ismétlődő értékek el lettek távolítva.</span><span class="sxs-lookup"><span data-stu-id="8e001-787">Returns a sanitized proxyAddress attribute where all duplicate values have been removed.</span></span>

- - -
### <a name="replace"></a><span data-ttu-id="8e001-788">Csere</span><span class="sxs-lookup"><span data-stu-id="8e001-788">Replace</span></span>
<span data-ttu-id="8e001-789">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-789">**Description:**</span></span>  
<span data-ttu-id="8e001-790">hello Replace függvény egy karakterlánc tooanother karakterlánc összes előfordulását lecseréli.</span><span class="sxs-lookup"><span data-stu-id="8e001-790">hello Replace function replaces all occurrences of a string tooanother string.</span></span>

<span data-ttu-id="8e001-791">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-791">**Syntax:**</span></span>  
`str Replace(str string, str OldValue, str NewValue)`

* <span data-ttu-id="8e001-792">karakterlánc: egy karakterlánc tooreplace értékei.</span><span class="sxs-lookup"><span data-stu-id="8e001-792">string: A string tooreplace values in.</span></span>
* <span data-ttu-id="8e001-793">OldValue: hello karakterlánc toosearch az és tooreplace.</span><span class="sxs-lookup"><span data-stu-id="8e001-793">OldValue: hello string toosearch for and tooreplace.</span></span>
* <span data-ttu-id="8e001-794">Új érték: hello karakterlánc tooreplace számára.</span><span class="sxs-lookup"><span data-stu-id="8e001-794">NewValue: hello string tooreplace to.</span></span>

<span data-ttu-id="8e001-795">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-795">**Remarks:**</span></span>  
<span data-ttu-id="8e001-796">hello függvény felismeri a következő különleges monikerek hello:</span><span class="sxs-lookup"><span data-stu-id="8e001-796">hello function recognizes hello following special monikers:</span></span>

* <span data-ttu-id="8e001-797">\n – új sor</span><span class="sxs-lookup"><span data-stu-id="8e001-797">\n – New Line</span></span>
* <span data-ttu-id="8e001-798">\r – kocsivissza</span><span class="sxs-lookup"><span data-stu-id="8e001-798">\r – Carriage Return</span></span>
* <span data-ttu-id="8e001-799">\t – lap</span><span class="sxs-lookup"><span data-stu-id="8e001-799">\t – Tab</span></span>

<span data-ttu-id="8e001-800">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-800">**Example:**</span></span>  
`Replace([address],"\r\n",", ")`  
<span data-ttu-id="8e001-801">CRLF lecseréli egy vesszőt és területet, és túl vezethet "Egy Microsoft módja, Redmond, WA, USA"</span><span class="sxs-lookup"><span data-stu-id="8e001-801">Replaces CRLF with a comma and space, and could lead too"One Microsoft Way, Redmond, WA, USA"</span></span>

- - -
### <a name="replacechars"></a><span data-ttu-id="8e001-802">ReplaceChars</span><span class="sxs-lookup"><span data-stu-id="8e001-802">ReplaceChars</span></span>
<span data-ttu-id="8e001-803">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-803">**Description:**</span></span>  
<span data-ttu-id="8e001-804">hello ReplaceChars függvény hello ReplacePattern karakterláncban található karakterek összes előfordulását lecseréli.</span><span class="sxs-lookup"><span data-stu-id="8e001-804">hello ReplaceChars function replaces all occurrences of characters found in hello ReplacePattern string.</span></span>

<span data-ttu-id="8e001-805">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-805">**Syntax:**</span></span>  
`str ReplaceChars(str string, str ReplacePattern)`

* <span data-ttu-id="8e001-806">karakterlánc: egy karakterlánc tooreplace karakternél.</span><span class="sxs-lookup"><span data-stu-id="8e001-806">string: A string tooreplace characters in.</span></span>
* <span data-ttu-id="8e001-807">ReplacePattern: egy szótár karakterek tooreplace tartalmazó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-807">ReplacePattern: a string containing a dictionary with characters tooreplace.</span></span>

<span data-ttu-id="8e001-808">hello formátuma {source1}: {target1}, {source2}: {target2}, {sourceN}, {targetN} hello karakter toofind és a cél hello karakterlánc tooreplace a forrás esetén.</span><span class="sxs-lookup"><span data-stu-id="8e001-808">hello format is {source1}:{target1},{source2}:{target2},{sourceN},{targetN} where source is hello character toofind and target hello string tooreplace with.</span></span>

<span data-ttu-id="8e001-809">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-809">**Remarks:**</span></span>

* <span data-ttu-id="8e001-810">hello függvény minden egyes előfordulásakor meghatározott források veszi és azok hello célokat.</span><span class="sxs-lookup"><span data-stu-id="8e001-810">hello function takes each occurrence of defined sources and replaces them with hello targets.</span></span>
* <span data-ttu-id="8e001-811">hello (unicode) pontosan egy karaktert kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8e001-811">hello source must be exactly one (unicode) character.</span></span>
* <span data-ttu-id="8e001-812">hello forrás nem lehet üres vagy hosszabb, mint egy karakter (feldolgozási hiba).</span><span class="sxs-lookup"><span data-stu-id="8e001-812">hello source cannot be empty or longer than one character (parsing error).</span></span>
* <span data-ttu-id="8e001-813">hello cél több karakter, például ö:oe, β:ss lehet.</span><span class="sxs-lookup"><span data-stu-id="8e001-813">hello target can have multiple characters, for example ö:oe, β:ss.</span></span>
* <span data-ttu-id="8e001-814">lehet, hogy hello tároló üres, amely azt jelzi, hogy hello karakter el kell távolítani.</span><span class="sxs-lookup"><span data-stu-id="8e001-814">hello target can be empty indicating that hello character should be removed.</span></span>
* <span data-ttu-id="8e001-815">hello forrás kis-és nagybetűket, és pontosan egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="8e001-815">hello source is case-sensitive and must be an exact match.</span></span>
* <span data-ttu-id="8e001-816">Hello, (vessző) és: (kettőspont) fenntartott karakterek, és ez a funkció nem helyettesíthető.</span><span class="sxs-lookup"><span data-stu-id="8e001-816">hello , (comma) and : (colon) are reserved characters and cannot be replaced using this function.</span></span>
* <span data-ttu-id="8e001-817">Tárolóhelyek és egyéb fehér karakterek hello ReplacePattern karakterlánc figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="8e001-817">Spaces and other white characters in hello ReplacePattern string are ignored.</span></span>

<span data-ttu-id="8e001-818">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-818">**Example:**</span></span>  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
<span data-ttu-id="8e001-819">Raksmorgas adja vissza</span><span class="sxs-lookup"><span data-stu-id="8e001-819">Returns Raksmorgas</span></span>

`ReplaceChars("O’Neil",%ReplaceString%)`  
<span data-ttu-id="8e001-820">Vissza "ONeil" hello egyetlen osztásjelek definiált toobe eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="8e001-820">Returns "ONeil", hello single tick is defined toobe removed.</span></span>

- - -
### <a name="right"></a><span data-ttu-id="8e001-821">Jobbra</span><span class="sxs-lookup"><span data-stu-id="8e001-821">Right</span></span>
<span data-ttu-id="8e001-822">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-822">**Description:**</span></span>  
<span data-ttu-id="8e001-823">hello jobb függvény a megadott számú karaktert hello jobb (záró) karakterlánc távolságát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-823">hello Right function returns a specified number of characters from hello right (end) of a string.</span></span>

<span data-ttu-id="8e001-824">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-824">**Syntax:**</span></span>  
`str Right(str string, num NumChars)`

* <span data-ttu-id="8e001-825">karakterlánc: hello karakterlánc tooreturn karaktert tartalmaz</span><span class="sxs-lookup"><span data-stu-id="8e001-825">string: hello string tooreturn characters from</span></span>
* <span data-ttu-id="8e001-826">NumChars: egy szám karakterek tooreturn oldalától hello (jobb oldali) karakterlánc hello száma</span><span class="sxs-lookup"><span data-stu-id="8e001-826">NumChars: a number identifying hello number of characters tooreturn from hello end (right) of string</span></span>

<span data-ttu-id="8e001-827">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-827">**Remarks:**</span></span>  
<span data-ttu-id="8e001-828">NumChars karaktereket a rendszer karakterlánc hello utolsó pozícióját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-828">NumChars characters are returned from hello last position of string.</span></span>

<span data-ttu-id="8e001-829">Hello utolsó numChars karakterláncon belül karaktereket tartalmazó karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="8e001-829">A string containing hello last numChars characters in string:</span></span>

* <span data-ttu-id="8e001-830">Ha numChars = 0, térjen vissza az üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-830">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="8e001-831">Ha numChars < 0, térjen vissza a bemeneti karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-831">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="8e001-832">Ha karakterlánc null értékű, térjen vissza az üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-832">If string is null, return empty string.</span></span>

<span data-ttu-id="8e001-833">Ha a karakterlánc karakternél kevesebb, mint a megadott számú NumChars hello, egy karakterlánc azonos toostring ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-833">If string contains fewer characters than hello number specified in NumChars, a string identical toostring is returned.</span></span>

<span data-ttu-id="8e001-834">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-834">**Example:**</span></span>  
`Right("John Doe", 3)`  
<span data-ttu-id="8e001-835">"Doe" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-835">Returns "Doe".</span></span>

- - -
### <a name="rtrim"></a><span data-ttu-id="8e001-836">RTrim</span><span class="sxs-lookup"><span data-stu-id="8e001-836">RTrim</span></span>
<span data-ttu-id="8e001-837">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-837">**Description:**</span></span>  
<span data-ttu-id="8e001-838">RTrim függvény hello záró szóközt eltávolít egy karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="8e001-838">hello RTrim function removes trailing white spaces from a string.</span></span>

<span data-ttu-id="8e001-839">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-839">**Syntax:**</span></span>  
`str RTrim(str value)`

<span data-ttu-id="8e001-840">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-840">**Example:**</span></span>  
`RTrim(" Test ")`  
<span data-ttu-id="8e001-841">"Test" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-841">Returns " Test".</span></span>

- - -
### <a name="select"></a><span data-ttu-id="8e001-842">Válassza ezt:</span><span class="sxs-lookup"><span data-stu-id="8e001-842">Select</span></span>
<span data-ttu-id="8e001-843">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-843">**Description:**</span></span>  
<span data-ttu-id="8e001-844">A folyamat egy többértékű attribútum (vagy egy kifejezés eredményének) szereplő összes érték a megadott függvény alapján.</span><span class="sxs-lookup"><span data-stu-id="8e001-844">Process all values in a multi-valued attribute (or output of an expression) based on function specified.</span></span>

<span data-ttu-id="8e001-845">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-845">**Syntax:**</span></span>  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* <span data-ttu-id="8e001-846">cikk: hello többértékű attribútumban olyan elemet</span><span class="sxs-lookup"><span data-stu-id="8e001-846">item: Represents an element in hello multi-valued attribute</span></span>
* <span data-ttu-id="8e001-847">attribútum: hello többértékű attribútum</span><span class="sxs-lookup"><span data-stu-id="8e001-847">attribute: hello multi-valued attribute</span></span>
* <span data-ttu-id="8e001-848">kifejezés: értékek gyűjteményét visszaadó kifejezés</span><span class="sxs-lookup"><span data-stu-id="8e001-848">expression: an expression that returns a collection of values</span></span>
* <span data-ttu-id="8e001-849">feltétel: semmilyen feladatot, amely képes a hello attribútum elem</span><span class="sxs-lookup"><span data-stu-id="8e001-849">condition: any function that can process an item in hello attribute</span></span>

<span data-ttu-id="8e001-850">**Példák:**</span><span class="sxs-lookup"><span data-stu-id="8e001-850">**Examples:**</span></span>  
`Select($item,[otherPhone],Replace($item,“-”,“”))`  
<span data-ttu-id="8e001-851">Összes hello érték visszaadása hello többértékű attribútum otherPhone után a kötőjeleket (-) el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="8e001-851">Return all hello values in hello multi-valued attribute otherPhone after hyphens (-) have been removed.</span></span>

- - -
### <a name="split"></a><span data-ttu-id="8e001-852">Vegyes</span><span class="sxs-lookup"><span data-stu-id="8e001-852">Split</span></span>
<span data-ttu-id="8e001-853">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-853">**Description:**</span></span>  
<span data-ttu-id="8e001-854">hello vegyes függvény elválasztóval elválasztott karakterlánc vesz igénybe, és lehetővé teszi a többértékű karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-854">hello Split function takes a string separated with a delimiter and makes it a multi-valued string.</span></span>

<span data-ttu-id="8e001-855">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-855">**Syntax:**</span></span>  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* <span data-ttu-id="8e001-856">érték: karakterlánc, és egy elválasztó karakter tooseparate hello.</span><span class="sxs-lookup"><span data-stu-id="8e001-856">value: hello string with a delimiter character tooseparate.</span></span>
* <span data-ttu-id="8e001-857">elválasztó karakter: használt hello elválasztó karakter toobe egyetlen.</span><span class="sxs-lookup"><span data-stu-id="8e001-857">delimiter: single character toobe used as hello delimiter.</span></span>
* <span data-ttu-id="8e001-858">korlát: értékeket adhat vissza maximális számát.</span><span class="sxs-lookup"><span data-stu-id="8e001-858">limit: maximum number of values that can return.</span></span>

<span data-ttu-id="8e001-859">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-859">**Example:**</span></span>  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
<span data-ttu-id="8e001-860">2 elemek többértékű karakterláncot ad vissza hello proxyAddress attribútum használható.</span><span class="sxs-lookup"><span data-stu-id="8e001-860">Returns a multi-valued string with 2 elements useful for hello proxyAddress attribute.</span></span>

- - -
### <a name="stringfromguid"></a><span data-ttu-id="8e001-861">StringFromGuid</span><span class="sxs-lookup"><span data-stu-id="8e001-861">StringFromGuid</span></span>
<span data-ttu-id="8e001-862">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-862">**Description:**</span></span>  
<span data-ttu-id="8e001-863">hello StringFromGuid függvény bináris GUID tart, és konvertálja azt tooa karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8e001-863">hello StringFromGuid function takes a binary GUID and converts it tooa string</span></span>

<span data-ttu-id="8e001-864">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-864">**Syntax:**</span></span>  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a><span data-ttu-id="8e001-865">StringFromSid</span><span class="sxs-lookup"><span data-stu-id="8e001-865">StringFromSid</span></span>
<span data-ttu-id="8e001-866">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-866">**Description:**</span></span>  
<span data-ttu-id="8e001-867">hello StringFromSid függvény egy karakterlánc-egy biztonsági azonosítót tooa tartalmazó bájttömb alakítja át.</span><span class="sxs-lookup"><span data-stu-id="8e001-867">hello StringFromSid function converts a byte array containing a security identifier tooa string.</span></span>

<span data-ttu-id="8e001-868">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-868">**Syntax:**</span></span>  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a><span data-ttu-id="8e001-869">Kapcsoló</span><span class="sxs-lookup"><span data-stu-id="8e001-869">Switch</span></span>
<span data-ttu-id="8e001-870">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-870">**Description:**</span></span>  
<span data-ttu-id="8e001-871">hello kapcsoló függvény használt tooreturn értékelt feltételek alapján egyetlen értéket.</span><span class="sxs-lookup"><span data-stu-id="8e001-871">hello Switch function is used tooreturn a single value based on evaluated conditions.</span></span>

<span data-ttu-id="8e001-872">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-872">**Syntax:**</span></span>  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* <span data-ttu-id="8e001-873">kifejezés: tooevaluate kívánt Variant kifejezést.</span><span class="sxs-lookup"><span data-stu-id="8e001-873">expr: Variant expression you want tooevaluate.</span></span>
* <span data-ttu-id="8e001-874">érték: érték toobe visszaadott hello megfelelő kifejezés értéke igaz, ha.</span><span class="sxs-lookup"><span data-stu-id="8e001-874">value: Value toobe returned if hello corresponding expression is True.</span></span>

<span data-ttu-id="8e001-875">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-875">**Remarks:**</span></span>  
<span data-ttu-id="8e001-876">hello kapcsoló függvényargumentum lista kifejezések és érték párokból áll.</span><span class="sxs-lookup"><span data-stu-id="8e001-876">hello Switch function argument list consists of pairs of expressions and values.</span></span> <span data-ttu-id="8e001-877">a bal oldali tooright hello kifejezések kiértékelése és hello első kifejezés tooevaluate tooTrue társított hello értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-877">hello expressions are evaluated from left tooright, and hello value associated with hello first expression tooevaluate tooTrue is returned.</span></span> <span data-ttu-id="8e001-878">Ha nincsenek megfelelően párosítva a hello részeit, egy futásidejű hiba történik.</span><span class="sxs-lookup"><span data-stu-id="8e001-878">If hello parts aren't properly paired, a run-time error occurs.</span></span>

<span data-ttu-id="8e001-879">Például Kif1 értéke igaz, ha a kapcsoló érték1 adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-879">For example, if expr1 is True, Switch returns value1.</span></span> <span data-ttu-id="8e001-880">Ha kifejezés-1 érték hamis, de kifejezés-2 értéke igaz, kapcsoló érték-2, és így tovább adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-880">If expr-1 is False, but expr-2 is True, Switch returns value-2, and so on.</span></span>

<span data-ttu-id="8e001-881">Kapcsoló adja vissza egy Nothing ha:</span><span class="sxs-lookup"><span data-stu-id="8e001-881">Switch returns a Nothing if:</span></span>

* <span data-ttu-id="8e001-882">Hello kifejezések egyike sem igaz.</span><span class="sxs-lookup"><span data-stu-id="8e001-882">None of hello expressions are True.</span></span>
* <span data-ttu-id="8e001-883">hello első igaz kifejezés a megfelelő értékkel rendelkezik, amely null értékű.</span><span class="sxs-lookup"><span data-stu-id="8e001-883">hello first True expression has a corresponding value that is Null.</span></span>

<span data-ttu-id="8e001-884">Kapcsoló kiértékeli az összes kifejezést, annak ellenére, hogy csak az egyiket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-884">Switch evaluates all expressions, even though it returns only one of them.</span></span> <span data-ttu-id="8e001-885">Emiatt érdemes figyelemmel a nemkívánatos hatásai.</span><span class="sxs-lookup"><span data-stu-id="8e001-885">For this reason, you should watch for undesirable side effects.</span></span> <span data-ttu-id="8e001-886">Például egy nullával való osztást bármely kifejezés kiértékelése hello eredményez, ha hiba lép fel.</span><span class="sxs-lookup"><span data-stu-id="8e001-886">For example, if hello evaluation of any expression results in a division by zero error, an error occurs.</span></span>

<span data-ttu-id="8e001-887">Érték hello hiba függvénynek, amely akkor adja vissza egyéni is lehet.</span><span class="sxs-lookup"><span data-stu-id="8e001-887">Value can also be hello Error function, which would return a custom string.</span></span>

<span data-ttu-id="8e001-888">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-888">**Example:**</span></span>  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
<span data-ttu-id="8e001-889">Néhány főbb városában szóbeli hello nyelvét adja eredményül, ellenkező esetben a hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-889">Returns hello language spoken in some major cities, otherwise returns an Error.</span></span>

- - -
### <a name="trim"></a><span data-ttu-id="8e001-890">Trim</span><span class="sxs-lookup"><span data-stu-id="8e001-890">Trim</span></span>
<span data-ttu-id="8e001-891">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-891">**Description:**</span></span>  
<span data-ttu-id="8e001-892">hello Trim függvény eltávolítja a kezdő és záró szóközök karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="8e001-892">hello Trim function removes leading and trailing white spaces from a string.</span></span>

<span data-ttu-id="8e001-893">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-893">**Syntax:**</span></span>  
`str Trim(str value)`  

<span data-ttu-id="8e001-894">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-894">**Example:**</span></span>  
`Trim(" Test ")`  
<span data-ttu-id="8e001-895">"Test" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-895">Returns "Test".</span></span>

`Trim([proxyAddresses])`  
<span data-ttu-id="8e001-896">Eltávolítja a kezdő és záró szóközök hello proxyAddress attribútum szereplő összes értékhez.</span><span class="sxs-lookup"><span data-stu-id="8e001-896">Removes leading and trailing spaces for each value in hello proxyAddress attribute.</span></span>

- - -
### <a name="ucase"></a><span data-ttu-id="8e001-897">UCase</span><span class="sxs-lookup"><span data-stu-id="8e001-897">UCase</span></span>
<span data-ttu-id="8e001-898">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-898">**Description:**</span></span>  
<span data-ttu-id="8e001-899">hello UCase függvény alakítja az összes karaktert karakterlánc tooupper esetben.</span><span class="sxs-lookup"><span data-stu-id="8e001-899">hello UCase function converts all characters in a string tooupper case.</span></span>

<span data-ttu-id="8e001-900">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-900">**Syntax:**</span></span>  
`str UCase(str string)`

<span data-ttu-id="8e001-901">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-901">**Example:**</span></span>  
`UCase("TeSt")`  
<span data-ttu-id="8e001-902">"TEST" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-902">Returns "TEST".</span></span>

- - -
### <a name="where"></a><span data-ttu-id="8e001-903">Ha</span><span class="sxs-lookup"><span data-stu-id="8e001-903">Where</span></span>

<span data-ttu-id="8e001-904">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-904">**Description:**</span></span>  
<span data-ttu-id="8e001-905">A megadott feltétel alapján többértékű attribútum (vagy egy kifejezés eredményének) értékek egy alhalmazát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-905">Returns a subset of values from a multi-valued attribute (or output of an expression) based on specific condition.</span></span>

<span data-ttu-id="8e001-906">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-906">**Syntax:**</span></span>  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* <span data-ttu-id="8e001-907">cikk: hello többértékű attribútumban olyan elemet</span><span class="sxs-lookup"><span data-stu-id="8e001-907">item: Represents an element in hello multi-valued attribute</span></span>
* <span data-ttu-id="8e001-908">attribútum: hello többértékű attribútum</span><span class="sxs-lookup"><span data-stu-id="8e001-908">attribute: hello multi-valued attribute</span></span>
* <span data-ttu-id="8e001-909">feltétel: bármely, amely kiértékeli tootrue vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="8e001-909">condition: any expression that can be evaluated tootrue or false</span></span>
* <span data-ttu-id="8e001-910">kifejezés: értékek gyűjteményét visszaadó kifejezés</span><span class="sxs-lookup"><span data-stu-id="8e001-910">expression: an expression that returns a collection of values</span></span>

<span data-ttu-id="8e001-911">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-911">**Example:**</span></span>  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
<span data-ttu-id="8e001-912">Hello többértékű attribútum userCertificate nem lejárt hello tanúsítvány értékeket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-912">Return hello certificate values in hello multi-valued attribute userCertificate which aren’t expired.</span></span>

- - -
### <a name="with"></a><span data-ttu-id="8e001-913">a</span><span class="sxs-lookup"><span data-stu-id="8e001-913">With</span></span>
<span data-ttu-id="8e001-914">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-914">**Description:**</span></span>  
<span data-ttu-id="8e001-915">hello funkcióval tartalmaz egy módja toosimplify összetett kifejezést egy változó toorepresent egy alkifejezés, amely akkor jelenik meg egy vagy több alkalommal hello összetett kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="8e001-915">hello With function provides a way toosimplify a complex expression by using a variable toorepresent a subexpression which appears one or more times in hello complex expression.</span></span>

<span data-ttu-id="8e001-916">**Szintaxis:**
`With(var variable, exp subExpression, exp complexExpression)`</span><span class="sxs-lookup"><span data-stu-id="8e001-916">**Syntax:**
`With(var variable, exp subExpression, exp complexExpression)`</span></span>  
* <span data-ttu-id="8e001-917">változó: jelöli hello alkifejezés.</span><span class="sxs-lookup"><span data-stu-id="8e001-917">variable: Represents hello subexpression.</span></span>
* <span data-ttu-id="8e001-918">alkifejezés: változó által képviselt alkifejezés.</span><span class="sxs-lookup"><span data-stu-id="8e001-918">subExpression: subexpression represented by variable.</span></span>
* <span data-ttu-id="8e001-919">complexExpression: egy összetett kifejezés.</span><span class="sxs-lookup"><span data-stu-id="8e001-919">complexExpression: A complex expression.</span></span>

<span data-ttu-id="8e001-920">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-920">**Example:**</span></span>  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
<span data-ttu-id="8e001-921">Mint:</span><span class="sxs-lookup"><span data-stu-id="8e001-921">Is functionally equivalent to:</span></span>  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
<span data-ttu-id="8e001-922">Amely hello userCertificate attribútum csak leküldéshez tanúsítvány értékeket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-922">Which returns only unexpired certificate values in hello userCertificate attribute.</span></span>


- - -
### <a name="word"></a><span data-ttu-id="8e001-923">Word</span><span class="sxs-lookup"><span data-stu-id="8e001-923">Word</span></span>
<span data-ttu-id="8e001-924">**Leírás:**</span><span class="sxs-lookup"><span data-stu-id="8e001-924">**Description:**</span></span>  
<span data-ttu-id="8e001-925">hello Word függvény egy karakterlánc, hello határolójelek toouse és hello word számú tooreturn leíró paraméterek alapján belül található szót adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-925">hello Word function returns a word contained within a string, based on parameters describing hello delimiters toouse and hello word number tooreturn.</span></span>

<span data-ttu-id="8e001-926">**Szintaxis:**</span><span class="sxs-lookup"><span data-stu-id="8e001-926">**Syntax:**</span></span>  
`str Word(str string, num WordNumber, str delimiters)`

* <span data-ttu-id="8e001-927">karakterlánc: hello karakterlánc tooreturn a szót.</span><span class="sxs-lookup"><span data-stu-id="8e001-927">string: hello string tooreturn a word from.</span></span>
* <span data-ttu-id="8e001-928">WordNumber: egy szám mely word számot kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="8e001-928">WordNumber: a number identifying which word number should return.</span></span>
* <span data-ttu-id="8e001-929">elválasztó karaktert: egy karakterlánc, amely hello delimiter(s), hogy által használt tooidentify szavakat</span><span class="sxs-lookup"><span data-stu-id="8e001-929">delimiters: a string representing hello delimiter(s) that should be used tooidentify words</span></span>

<span data-ttu-id="8e001-930">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="8e001-930">**Remarks:**</span></span>  
<span data-ttu-id="8e001-931">A határoló karakter hello egyik hello elválasztott karakterlánc minden karakterlánc szavak azonosítja:</span><span class="sxs-lookup"><span data-stu-id="8e001-931">Each string of characters in string separated by hello one of hello characters in delimiters are identified as words:</span></span>

* <span data-ttu-id="8e001-932">Ha < 1-es számú, értéket ad vissza üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e001-932">If number < 1, returns empty string.</span></span>
* <span data-ttu-id="8e001-933">Ha a karakterlánc értéke null, üres karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-933">If string is null, returns empty string.</span></span>

<span data-ttu-id="8e001-934">Karakterlánc nagyobb számú karaktert tartalmaz, vagy karakterlánca nem tartalmazza az elválasztó karaktert által azonosított szavak, ha üres karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8e001-934">If string contains less than number words, or string does not contain any words identified by delimiters, an empty string is returned.</span></span>

<span data-ttu-id="8e001-935">**Példa**</span><span class="sxs-lookup"><span data-stu-id="8e001-935">**Example:**</span></span>  
`Word("hello quick brown fox",3," ")`  
<span data-ttu-id="8e001-936">Visszaadja a "nem"</span><span class="sxs-lookup"><span data-stu-id="8e001-936">Returns "brown"</span></span>

`Word("This,string!has&many separators",3,",!&#")`  
<span data-ttu-id="8e001-937">Alakítanák vissza "tartalmaz"</span><span class="sxs-lookup"><span data-stu-id="8e001-937">Would return "has"</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e001-938">További források</span><span class="sxs-lookup"><span data-stu-id="8e001-938">Additional Resources</span></span>
* [<span data-ttu-id="8e001-939">Deklaratív kiépítés kifejezéseinek ismertetése</span><span class="sxs-lookup"><span data-stu-id="8e001-939">Understanding Declarative Provisioning Expressions</span></span>](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [<span data-ttu-id="8e001-940">Az Azure AD Connect-szinkronizálás: Szinkronizálási beállítások testreszabása</span><span class="sxs-lookup"><span data-stu-id="8e001-940">Azure AD Connect Sync: Customizing Synchronization options</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="8e001-941">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8e001-941">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
