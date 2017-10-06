---
title: "tanúsítványok az Azure AD jelentéskészítési API aaaGet használatával végzett hello |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello tanúsítvány hitelesítő adatok tooget származó adatokkal könyvtárak felhasználói beavatkozás nélkül az Azure AD jelentéskészítési API."
services: active-directory
documentationcenter: 
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/24/2017
ms.author: ramical
ms.openlocfilehash: 00ddfaefe32ea6ae48f276c974a17ddcf84f7894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-data-using-hello-azure-ad-reporting-api-with-certificates"></a>Adatok és tanúsítványok hello Azure AD jelentéskészítési API segítségével.
A cikk ismerteti, hogyan toouse hello tanúsítvány hitelesítő adatok tooget származó adatokkal könyvtárak felhasználói beavatkozás nélkül az Azure AD jelentéskészítési API. 

## <a name="use-hello-azure-ad-reporting-api"></a>Használja az Azure AD jelentéskészítési API hello 
Azure AD jelentéskészítési API szükséges, végezze el a lépéseket követve hello:
 *  Az előfeltételek telepítése
 *  Az alkalmazás hello tanúsítvány beállítása
 *  Hozzáférési jogkivonat lekérése
 *  Hello access token toocall hello Graph API használata

További információ a forráskódról: [A Report API modul használata](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils). 

### <a name="install-prerequisites"></a>Az előfeltételek telepítése
Azure AD PowerShell V2 toohave és AzureADUtils modul telepítve lesz szüksége.

1. Töltse le és telepítse az Azure AD Powershell V2 hello található utasítások segítségével: a következő [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).
2. Töltse le a hello Azure AD Utils modulnak a [AzureAD/azure-Active Directoryban-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1). 
  Ez a modul számos segédprogramként használható parancsmagot biztosít, többek között:
   * az adal TÁRAT használó Nuget hello legújabb verziója
   * a felhasználó, alkalmazáskulcsok és tanúsítványok jogkivonatainak elérését az ADAL használatával,
   * a lapokra bontott eredményeket kezelő Graph API-t.

**tooinstall hello Azure AD Utils modul:**

1. Hozzon létre egy könyvtárat toosave hello segédprogramok modult (például c:\azureAD), és töltse le a hello modul a Githubról.
2. Nyisson meg egy PowerShell-munkamenetet, és válassza az imént létrehozott toohello könyvtár. 
3. Hello modul importálásához, majd telepítse hello PowerShell modul elérési útján hello Install-AzureADUtilsModule parancsmag használatával. 

hello munkamenet toothis hasonló képernyő kell kinéznie:

  ![Windows PowerShell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

### <a name="set-hello-certificate-in-your-app"></a>Az alkalmazás hello tanúsítvány beállítása
1. Ha már telepítette az alkalmazást, az objektum Azonosítóját beszerzése hello Azure portálon. 

  ![Azure Portal](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. Nyisson meg egy PowerShell-munkamenetet, és csatlakozzon a tooAzure AD Connect-AzureAD hello parancsmag használatával.

  ![Azure Portal](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. A AzureADUtils tooadd egy tanúsítvány credential tooit hello New-AzureADApplicationCertificateCredential parancsmag használható. 

>[!Note]
>Meg kell tooprovide hello alkalmazás Objektumazonosító, korábban rögzített, valamint tanúsítványobjektum hello (beolvasása a használatával hello Cert: meghajtón).
>


  ![Azure Portal](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
### <a name="get-an-access-token"></a>Hozzáférési jogkivonat lekérése

tooget hozzáférési tokent, AzureADUtils hello Get-AzureADGraphAPIAccessTokenFromCert parancsmag használható. 

>[!NOTE]
>Toouse hello Alkalmazásazonosító helyett hello hello utolsó szakaszban használt Objektumazonosító van szüksége.
>

 ![Azure Portal](./media/active-directory-report-api-with-certificates/application-id.png)

### <a name="use-hello-access-token-toocall-hello-graph-api"></a>Hello access token toocall hello Graph API használata

Most hello parancsfájlt is létrehozhat. Alább látható egy példa, hello AzureADUtils hello Invoke-AzureADGraphAPIQuery parancsmag használatával. Ez a parancsmag több lapozható eredmények kezeli, és ezután elküldi az adott eredmények toohello PowerShell kimenetátirányításának. 

 ![Azure Portal](./media/active-directory-report-api-with-certificates/script-completed.png)

Most már készen áll a tooexport tooa CSV áll, és mentse a tooa SIEM-rendszerben. Akkor is futtathatja a parancsfájl egy ütemezett feladat tooget az Azure AD-adatok a a bérlőhöz rendszeres időközönként hello forráskód toostore alkalmazás kulcsok nélkül. 

## <a name="next-steps"></a>Következő lépések
[Azure-identitás felügyeleti hello alapjai](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity)<br>



