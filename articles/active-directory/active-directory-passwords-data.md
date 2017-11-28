---
title: "Az Azure AD SSPR követelményeinek |} Microsoft Docs"
description: "Az Azure AD az önkiszolgáló jelszó követelményeinek alaphelyzetbe állítása és hogyan toosatisfy őket"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: b68a1d7914dcd0bb4509d0e94914dc4309f4463a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a><span data-ttu-id="1b398-103">Jelszó alaphelyzetbe állítása, anélkül, hogy a végfelhasználói regisztrálási telepítése</span><span class="sxs-lookup"><span data-stu-id="1b398-103">Deploy password reset without requiring end-user registration</span></span>

<span data-ttu-id="1b398-104">Hitelesítési adatok toobe jelen önkiszolgáló jelszó alaphelyzetbe állítása (SSPR) telepítését igényli.</span><span class="sxs-lookup"><span data-stu-id="1b398-104">Deploying Self-Service Password Reset (SSPR) requires authentication data toobe present.</span></span> <span data-ttu-id="1b398-105">Egyes szervezetek adja meg a hitelesítési adatok maguk a felhasználók rendelkeznek, de számos szervezet inkább az Active Directoryban már adatokat tartalmazó toosynchronize.</span><span class="sxs-lookup"><span data-stu-id="1b398-105">Some organizations have their users enter their authentication data themselves, but many organizations prefer toosynchronize with existing data in Active Directory.</span></span> <span data-ttu-id="1b398-106">Ha megfelelően vannak formázva adatokat a helyszíni címtárban, és konfigurálja [gyorsbeállítások használata az Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md), hogy adatok történik-e elérhető tooAzure AD és az önkiszolgáló jelszó-Változtatási szükséges felhasználói beavatkozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="1b398-106">If you have properly formatted data in your on-premises directory, and configure [Azure AD Connect using express settings](./connect/active-directory-aadconnect-get-started-express.md), that data is made available tooAzure AD and SSPR with no user interaction required.</span></span>

<span data-ttu-id="1b398-107">Bármely telefonszámok hello formátumban + országhívószám PhoneNumber példa kell lennie: + 1 4255551234 toowork megfelelően.</span><span class="sxs-lookup"><span data-stu-id="1b398-107">Any phone numbers must be in hello format +CountryCode PhoneNumber Example: +1 4255551234 toowork properly.</span></span>

> [!NOTE]
> <span data-ttu-id="1b398-108">Jelszó alaphelyzetbe állítása nem támogatja a telefonmellék.</span><span class="sxs-lookup"><span data-stu-id="1b398-108">Password reset does not support phone extensions.</span></span> <span data-ttu-id="1b398-109">Hello + 1 4255551234 X 12345 formátumban, még akkor is, bővítmények előtt távolítsa el a hello hívást kezdeményeznek.</span><span class="sxs-lookup"><span data-stu-id="1b398-109">Even in hello +1 4255551234X12345 format, extensions are removed before hello call is placed.</span></span>

## <a name="fields-populated"></a><span data-ttu-id="1b398-110">Mezői</span><span class="sxs-lookup"><span data-stu-id="1b398-110">Fields populated</span></span>

<span data-ttu-id="1b398-111">Ha a hello alapértelmezett beállításokat használja a következő az Azure AD Connect hello hozzárendelések történik.</span><span class="sxs-lookup"><span data-stu-id="1b398-111">If you use hello default settings in Azure AD Connect hello following mappings are made.</span></span>

| <span data-ttu-id="1b398-112">A helyszíni AD</span><span class="sxs-lookup"><span data-stu-id="1b398-112">On-premises AD</span></span> | <span data-ttu-id="1b398-113">Azure AD</span><span class="sxs-lookup"><span data-stu-id="1b398-113">Azure AD</span></span> | <span data-ttu-id="1b398-114">Az Azure AD-alapú hitelesítés kapcsolattartási adatai</span><span class="sxs-lookup"><span data-stu-id="1b398-114">Azure AD Authentication contact info</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1b398-115">TelephoneNumber</span><span class="sxs-lookup"><span data-stu-id="1b398-115">telephoneNumber</span></span> | <span data-ttu-id="1b398-116">Irodai telefon</span><span class="sxs-lookup"><span data-stu-id="1b398-116">Office phone</span></span> | <span data-ttu-id="1b398-117">Telefonszámok</span><span class="sxs-lookup"><span data-stu-id="1b398-117">Alternate phone</span></span> |
| <span data-ttu-id="1b398-118">Mobileszköz</span><span class="sxs-lookup"><span data-stu-id="1b398-118">mobile</span></span> | <span data-ttu-id="1b398-119">Mobiltelefon</span><span class="sxs-lookup"><span data-stu-id="1b398-119">Mobile phone</span></span> | <span data-ttu-id="1b398-120">Telefonszám</span><span class="sxs-lookup"><span data-stu-id="1b398-120">Phone</span></span> |


## <a name="security-questions-and-answers"></a><span data-ttu-id="1b398-121">Biztonsági kérdések és válaszok</span><span class="sxs-lookup"><span data-stu-id="1b398-121">Security questions and answers</span></span>

<span data-ttu-id="1b398-122">Biztonsági kérdések és válaszok biztonságos tárolása az Azure AD-bérlőről, és csak elérhető toousers keresztül hello [SSPR regisztrációs portál](https://aka.ms/ssprsetup).</span><span class="sxs-lookup"><span data-stu-id="1b398-122">Security questions and answers are stored securely in your Azure AD tenant and are only accessible toousers via hello [SSPR registration portal](https://aka.ms/ssprsetup).</span></span> <span data-ttu-id="1b398-123">A rendszergazdák nem tekintse meg vagy egy másik felhasználói kérdések és válaszok hello tartalmának módosítása.</span><span class="sxs-lookup"><span data-stu-id="1b398-123">Administrators can't see or modify hello contents of another users questions and answers.</span></span>

### <a name="what-happens-when-a-user-registers"></a><span data-ttu-id="1b398-124">Mi történik, ha a felhasználó regisztrálása</span><span class="sxs-lookup"><span data-stu-id="1b398-124">What happens when a user registers</span></span>

<span data-ttu-id="1b398-125">Amikor a felhasználó regisztrál, a hello regisztrációs lapjához beállítja a következő mezők hello:</span><span class="sxs-lookup"><span data-stu-id="1b398-125">When a user registers, hello registration page sets hello following fields:</span></span>

* <span data-ttu-id="1b398-126">Hitelesítéshez megadott telefonját</span><span class="sxs-lookup"><span data-stu-id="1b398-126">Authentication Phone</span></span>
* <span data-ttu-id="1b398-127">Hitelesítési E-mail</span><span class="sxs-lookup"><span data-stu-id="1b398-127">Authentication Email</span></span>
* <span data-ttu-id="1b398-128">Biztonsági kérdések és válaszok</span><span class="sxs-lookup"><span data-stu-id="1b398-128">Security Questions and Answers</span></span>

<span data-ttu-id="1b398-129">Ha a megadott értéket **mobiltelefon** vagy **másodlagos E-mail**, felhasználók azonnal használható ezen értékek tooreset jelszavukat, még akkor is, ha azok még nem regisztrált hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1b398-129">If you have provided a value for **Mobile Phone** or **Alternate Email**, users can immediately use those values tooreset their passwords, even if they haven't registered for hello service.</span></span> <span data-ttu-id="1b398-130">Továbbá a felhasználók látni ezeket az értékeket, amikor első alkalommal hello regisztrálása, és módosíthatók, ha ki szeretnék.</span><span class="sxs-lookup"><span data-stu-id="1b398-130">In addition, users see those values when registering for hello first time, and modify them if they wish.</span></span> <span data-ttu-id="1b398-131">Miután sikeresen regisztrálják, ezek az értékek maradnak a hello **hitelesítéshez megadott telefonját** és **hitelesítési E-mail** mezők, illetve.</span><span class="sxs-lookup"><span data-stu-id="1b398-131">After they successfully register, these values will be persisted in hello **Authentication Phone** and **Authentication Email** fields, respectively.</span></span>

## <a name="set-and-read-authentication-data-using-powershell"></a><span data-ttu-id="1b398-132">Állítsa be, és a PowerShell használatával hitelesítési adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="1b398-132">Set and read authentication data using PowerShell</span></span>

<span data-ttu-id="1b398-133">a következő mezők hello PowerShell segítségével állítható be</span><span class="sxs-lookup"><span data-stu-id="1b398-133">hello following fields can be set using PowerShell</span></span>

* <span data-ttu-id="1b398-134">Másodlagos E-mail</span><span class="sxs-lookup"><span data-stu-id="1b398-134">Alternate Email</span></span>
* <span data-ttu-id="1b398-135">Mobiltelefon</span><span class="sxs-lookup"><span data-stu-id="1b398-135">Mobile Phone</span></span>
* <span data-ttu-id="1b398-136">Irodai telefon - csak akkor állítható, ha egy helyszíni címtár nem szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="1b398-136">Office Phone - Can only be set if not synchronizing with an on-premises directory</span></span>

### <a name="using-powershell-v1"></a><span data-ttu-id="1b398-137">PowerShell V1 használatával</span><span class="sxs-lookup"><span data-stu-id="1b398-137">Using PowerShell V1</span></span>

<span data-ttu-id="1b398-138">tooget elindult, kell túl[töltse le és telepítse az Azure AD PowerShell modult hello](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).</span><span class="sxs-lookup"><span data-stu-id="1b398-138">tooget started, you need too[download and install hello Azure AD PowerShell module](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).</span></span> <span data-ttu-id="1b398-139">Ha már van telepítve, az alábbi tooconfigure minden mező hello lépések végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="1b398-139">Once you have it installed, you can follow hello steps that follow tooconfigure each field.</span></span>

#### <a name="set-authentication-data-with-powershell-v1"></a><span data-ttu-id="1b398-140">A PowerShell V1 hitelesítési adatok beállítása</span><span class="sxs-lookup"><span data-stu-id="1b398-140">Set Authentication Data with PowerShell V1</span></span>

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-authentication-data-with-powershellpowershell-v1"></a><span data-ttu-id="1b398-141">Hitelesítési adatok PowerShellPowerShell 1-es verzió</span><span class="sxs-lookup"><span data-stu-id="1b398-141">Read Authentication Data with PowerShellPowerShell V1</span></span>

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="authentication-phone-and-authentication-email-can-only-be-read-using-powershell-v1-using-hello-commands-that-follow"></a><span data-ttu-id="1b398-142">Hitelesítési telefon és hitelesítési E-mail csak olvasható Powershell V1 használatával hello segítségével parancsok olvashat</span><span class="sxs-lookup"><span data-stu-id="1b398-142">Authentication Phone and Authentication Email can only be read using Powershell V1 using hello commands that follow</span></span>

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="using-powershell-v2"></a><span data-ttu-id="1b398-143">PowerShell V2 használatával</span><span class="sxs-lookup"><span data-stu-id="1b398-143">Using PowerShell V2</span></span>

<span data-ttu-id="1b398-144">tooget elindult, kell túl[töltse le és telepítse a PowerShell-modul hello Azure AD V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md).</span><span class="sxs-lookup"><span data-stu-id="1b398-144">tooget started, you need too[download and install hello Azure AD V2 PowerShell module](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md).</span></span> <span data-ttu-id="1b398-145">Ha már van telepítve, az alábbi tooconfigure minden mező hello lépések végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="1b398-145">Once you have it installed, you can follow hello steps that follow tooconfigure each field.</span></span>

<span data-ttu-id="1b398-146">gyorsan PowerShell újabb verzióiban támogatja az Install-modul, a tooinstall futtassa az alábbi parancsokat (hello első sor egyszerűen ellenőrzi toosee Ha már telepítve van):</span><span class="sxs-lookup"><span data-stu-id="1b398-146">tooinstall quickly from recent versions of PowerShell that support Install-Module, run these commands (hello first line simply checks toosee if it's installed already):</span></span>

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-authentication-data-with-powershell-v2"></a><span data-ttu-id="1b398-147">A PowerShell V2 hitelesítési adatok beállítása</span><span class="sxs-lookup"><span data-stu-id="1b398-147">Set Authentication Data with PowerShell V2</span></span>

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

### <a name="read-authentication-data-with-powershell-v2"></a><span data-ttu-id="1b398-148">A PowerShell V2 hitelesítési adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="1b398-148">Read Authentication Data with PowerShell V2</span></span>

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a><span data-ttu-id="1b398-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b398-149">Next steps</span></span>

<span data-ttu-id="1b398-150">a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk</span><span class="sxs-lookup"><span data-stu-id="1b398-150">hello following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="1b398-151">[**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét.</span><span class="sxs-lookup"><span data-stu-id="1b398-151">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="1b398-152">[**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1b398-152">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="1b398-153">[**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál</span><span class="sxs-lookup"><span data-stu-id="1b398-153">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR tooyour users using hello guidance found here</span></span>
* <span data-ttu-id="1b398-154">[**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.</span><span class="sxs-lookup"><span data-stu-id="1b398-154">[**Customize**](active-directory-passwords-customize.md) - Customize hello look and feel of hello SSPR experience for your company.</span></span>
* <span data-ttu-id="1b398-155">[**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.</span><span class="sxs-lookup"><span data-stu-id="1b398-155">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="1b398-156">[**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.</span><span class="sxs-lookup"><span data-stu-id="1b398-156">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="1b398-157">[**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről</span><span class="sxs-lookup"><span data-stu-id="1b398-157">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind hello curtain toounderstand how it works</span></span>
* <span data-ttu-id="1b398-158">[**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan?</span><span class="sxs-lookup"><span data-stu-id="1b398-158">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="1b398-159">Hogy miért?</span><span class="sxs-lookup"><span data-stu-id="1b398-159">Why?</span></span> <span data-ttu-id="1b398-160">Mi?</span><span class="sxs-lookup"><span data-stu-id="1b398-160">What?</span></span> <span data-ttu-id="1b398-161">Hová?</span><span class="sxs-lookup"><span data-stu-id="1b398-161">Where?</span></span> <span data-ttu-id="1b398-162">Ki?</span><span class="sxs-lookup"><span data-stu-id="1b398-162">Who?</span></span> <span data-ttu-id="1b398-163">Mikor?</span><span class="sxs-lookup"><span data-stu-id="1b398-163">When?</span></span> <span data-ttu-id="1b398-164">-Válaszok mindig kívánta tooask tooquestions</span><span class="sxs-lookup"><span data-stu-id="1b398-164">- Answers tooquestions you always wanted tooask</span></span>
* <span data-ttu-id="1b398-165">[**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható</span><span class="sxs-lookup"><span data-stu-id="1b398-165">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how tooresolve common issues that we see with SSPR</span></span>
