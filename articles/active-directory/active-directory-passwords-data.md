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
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Jelszó alaphelyzetbe állítása, anélkül, hogy a végfelhasználói regisztrálási telepítése

Hitelesítési adatok toobe jelen önkiszolgáló jelszó alaphelyzetbe állítása (SSPR) telepítését igényli. Egyes szervezetek adja meg a hitelesítési adatok maguk a felhasználók rendelkeznek, de számos szervezet inkább az Active Directoryban már adatokat tartalmazó toosynchronize. Ha megfelelően vannak formázva adatokat a helyszíni címtárban, és konfigurálja [gyorsbeállítások használata az Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md), hogy adatok történik-e elérhető tooAzure AD és az önkiszolgáló jelszó-Változtatási szükséges felhasználói beavatkozás nélkül.

Bármely telefonszámok hello formátumban + országhívószám PhoneNumber példa kell lennie: + 1 4255551234 toowork megfelelően.

> [!NOTE]
> Jelszó alaphelyzetbe állítása nem támogatja a telefonmellék. Hello + 1 4255551234 X 12345 formátumban, még akkor is, bővítmények előtt távolítsa el a hello hívást kezdeményeznek.

## <a name="fields-populated"></a>Mezői

Ha a hello alapértelmezett beállításokat használja a következő az Azure AD Connect hello hozzárendelések történik.

| A helyszíni AD | Azure AD | Az Azure AD-alapú hitelesítés kapcsolattartási adatai |
| --- | --- | --- |
| TelephoneNumber | Irodai telefon | Telefonszámok |
| Mobileszköz | Mobiltelefon | Telefonszám |


## <a name="security-questions-and-answers"></a>Biztonsági kérdések és válaszok

Biztonsági kérdések és válaszok biztonságos tárolása az Azure AD-bérlőről, és csak elérhető toousers keresztül hello [SSPR regisztrációs portál](https://aka.ms/ssprsetup). A rendszergazdák nem tekintse meg vagy egy másik felhasználói kérdések és válaszok hello tartalmának módosítása.

### <a name="what-happens-when-a-user-registers"></a>Mi történik, ha a felhasználó regisztrálása

Amikor a felhasználó regisztrál, a hello regisztrációs lapjához beállítja a következő mezők hello:

* Hitelesítéshez megadott telefonját
* Hitelesítési E-mail
* Biztonsági kérdések és válaszok

Ha a megadott értéket **mobiltelefon** vagy **másodlagos E-mail**, felhasználók azonnal használható ezen értékek tooreset jelszavukat, még akkor is, ha azok még nem regisztrált hello szolgáltatás. Továbbá a felhasználók látni ezeket az értékeket, amikor első alkalommal hello regisztrálása, és módosíthatók, ha ki szeretnék. Miután sikeresen regisztrálják, ezek az értékek maradnak a hello **hitelesítéshez megadott telefonját** és **hitelesítési E-mail** mezők, illetve.

## <a name="set-and-read-authentication-data-using-powershell"></a>Állítsa be, és a PowerShell használatával hitelesítési adatokat olvasni.

a következő mezők hello PowerShell segítségével állítható be

* Másodlagos E-mail
* Mobiltelefon
* Irodai telefon - csak akkor állítható, ha egy helyszíni címtár nem szinkronizálása

### <a name="using-powershell-v1"></a>PowerShell V1 használatával

tooget elindult, kell túl[töltse le és telepítse az Azure AD PowerShell modult hello](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Ha már van telepítve, az alábbi tooconfigure minden mező hello lépések végrehajtásával.

#### <a name="set-authentication-data-with-powershell-v1"></a>A PowerShell V1 hitelesítési adatok beállítása

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-authentication-data-with-powershellpowershell-v1"></a>Hitelesítési adatok PowerShellPowerShell 1-es verzió

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="authentication-phone-and-authentication-email-can-only-be-read-using-powershell-v1-using-hello-commands-that-follow"></a>Hitelesítési telefon és hitelesítési E-mail csak olvasható Powershell V1 használatával hello segítségével parancsok olvashat

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="using-powershell-v2"></a>PowerShell V2 használatával

tooget elindult, kell túl[töltse le és telepítse a PowerShell-modul hello Azure AD V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md). Ha már van telepítve, az alábbi tooconfigure minden mező hello lépések végrehajtásával.

gyorsan PowerShell újabb verzióiban támogatja az Install-modul, a tooinstall futtassa az alábbi parancsokat (hello első sor egyszerűen ellenőrzi toosee Ha már telepítve van):

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-authentication-data-with-powershell-v2"></a>A PowerShell V2 hitelesítési adatok beállítása

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

### <a name="read-authentication-data-with-powershell-v2"></a>A PowerShell V2 hitelesítési adatokat olvasni.

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Következő lépések

a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk

* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál
* [**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.
* [**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? -Válaszok mindig kívánta tooask tooquestions
* [**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható
