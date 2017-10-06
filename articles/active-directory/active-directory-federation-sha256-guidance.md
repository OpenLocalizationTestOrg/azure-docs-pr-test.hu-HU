---
title: "Office 365 függő entitás megbízhatóságához aaaChange aláírás-kivonatoló algoritmus |} Microsoft Docs"
description: "Ezen a lapon útmutatást nyújt a SHA-képzési algoritmus összevonási megbízhatósági kapcsolat, és az Office 365 módosítása"
keywords: "SHA1, SHA256, O365, összevonási, aadconnect, az AD FS, az ad fs, a módosítás sha összevonási megbízhatósági kapcsolat, a megbízható függő entitás"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: samueld
editor: 
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: anandy
ms.openlocfilehash: 3333d1384aff8bdf6b3bcc894f8c633fd9ccc3a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a>Aláírás-kivonatoló algoritmus az Office 365 megbízható függő entitás módosítása
## <a name="overview"></a>Áttekintés
Active Directory összevonási szolgáltatások (AD FS) a jogkivonatok tooMicrosoft Azure Active Directory tooensure, hogy azok ne lehessen illetéktelenül jelentkezik. Az aláírás SHA1 vagy SHA-256-alapú lehet. Az Azure Active Directory mostantól támogatja az SHA-256 algoritmussal aláírt jogkivonatokat, és azt javasoljuk, hello a hello legmagasabb szintű biztonság meg jogkivonat-aláíró algoritmus a tooSHA256 beállítása. Ez a cikk szükséges tooset hello jogkivonat-aláíró algoritmus toohello biztonságosabb SHA-256 szintet hello lépéseit ismerteti.

>[!NOTE]
>A Microsoft azt javasolja, hogy SHA-256 használatát hello algoritmusként biztonságosabb, mint az SHA1, de SHA1 még mindig egy támogatott beállítás a jogkivonatok aláírásához.

## <a name="change-hello-token-signing-algorithm"></a>Hello jogkivonat-aláíró algoritmus módosítása
Hello aláírási algoritmus valamelyik az alábbi két folyamatok hello beállítása után az AD FS aláírja hello jogkivonatok az Office 365 megbízható függő entitás SHA-256. Nem kell toomake a további konfigurációs változásokat, és ez a változás nincs hatással a képes tooaccess Office 365 vagy más Azure AD-alkalmazások rendelkezik.

### <a name="ad-fs-management-console"></a>AD FS felügyeleti konzol
1. Nyissa meg a hello AD FS-kezelőkonzolon hello elsődleges AD FS-kiszolgálón.
2. Hello AD FS csomópontot, és kattintson a **függő entitás Megbízhatóságai**.
3. Kattintson a jobb gombbal az Office 365 vagy az Azure függő entitás megbízhatóságához, és válassza ki **tulajdonságok**.
4. Jelölje be hello **speciális** lapra, és jelölje be hello biztonságos kivonatoló algoritmus SHA-256.
5. Kattintson az **OK** gombra.

![SHA-256 aláírási algoritmus--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS PowerShell-parancsmagok
1. Egyetlen AD FS-kiszolgálón nyissa meg a Powershellt a rendszergazdai jogosultságokkal.
2. Set hello biztonságos kivonatoló algoritmus hello segítségével **Set-AdfsRelyingPartyTrust** parancsmag.
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Is
* [Az Azure AD Connect Office 365-megbízhatóság javítása](connect/active-directory-aadconnect-federation-management.md#repairthetrust)

