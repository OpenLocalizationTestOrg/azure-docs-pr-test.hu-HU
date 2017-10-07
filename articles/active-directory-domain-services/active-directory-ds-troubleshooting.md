---
title: "Az Azure Active Directory tartományi szolgáltatások: Hibaelhárítási útmutatója |} Microsoft Docs"
description: "Az Azure AD tartományi szolgáltatásokra vonatkozó hibaelhárítási útmutató"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 86eb3513b7bc921c59287600b1b76eeda20c1356
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD tartományi szolgáltatások - hibaelhárítási útmutatója
Ez a cikk hibaelhárítási tippek biztosít a problémák jelentkezhetnek, ha beállítása és felügyelete az Azure Active Directory (AD) tartományi szolgáltatásokban.

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Nem lehet engedélyezni az Azure AD-címtár az Azure AD tartományi szolgáltatások
Ez a szakasz segítséget nyújt hibák elhárítása az Azure AD tartományi szolgáltatásokat a címtáron tooenable és meghibásodik, vagy lekérdezi váltható hátsó too'Disabled ".

Válassza ki a megfelelő tapasztal toohello hibaüzenet hibaelhárítási hello.

| **Hibaüzenet** | **Megoldás** |
| --- |:--- |
| *hello neve contoso100.com már használatban a hálózaton. Adjon meg olyan nevet, amely még nincs használatban.* |[Tartomány névütközés hello virtuális hálózatban](active-directory-ds-troubleshooting.md#domain-name-conflict) |
| *Tartományi szolgáltatások nem sikerült engedélyezni ezt az Azure AD-bérlőben. hello szolgáltatás nem rendelkezik megfelelő engedélyekkel toohello alkalmazás "Azure AD tartományi szolgáltatások Sync" nevezik. "Az Azure AD tartományi szolgáltatások Sync" nevű hello alkalmazás törlése, és ismételje meg az tooenable tartományi szolgáltatások az Azure AD-bérlő.* |[Tartományi szolgáltatások nem rendelkezik megfelelő engedélyekkel toohello Azure AD tartományi szolgáltatások Szinkronizáló alkalmazás](active-directory-ds-troubleshooting.md#inadequate-permissions) |
| *Tartományi szolgáltatások nem sikerült engedélyezni ezt az Azure AD-bérlőben. Tartományi szolgáltatások az Azure AD-bérlő az alkalmazás nem rendelkezik hello hello szükséges engedélyek tooenable tartományi szolgáltatásokban. Hello alkalmazás azonosítója d87dcbc6-a371-462e-88e3-28ad15ec4e64 hello alkalmazás törlése, és ismételje meg az Azure AD-bérlő tartományszolgáltatásokban tooenable.* |[hello tartományi szolgáltatások alkalmazás nincs megfelelően konfigurálva az Ön bérlőjében](active-directory-ds-troubleshooting.md#invalid-configuration) |
| *Tartományi szolgáltatások nem sikerült engedélyezni ezt az Azure AD-bérlőben. Microsoft Azure AD alkalmazás hello le van tiltva, az Azure AD-bérlőben. Hello alkalmazás azonosítója 00000002-0000-0000-c000-000000000000 hello alkalmazás engedélyezése, és ismételje meg az Azure AD-bérlő tartományszolgáltatásokban tooenable.* |[Microsoft Graph alkalmazás hello le van tiltva, az Azure AD-bérlőben](active-directory-ds-troubleshooting.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Tartomány névütközés
**Hibaüzenet:**

*hello neve contoso100.com már használatban a hálózaton. Adjon meg olyan nevet, amely még nincs használatban.*

**Szervizelés:**

Győződjön meg arról, hogy nem rendelkezik egy meglévő tartomány hello ugyanazon a néven érhető el a virtuális hálózat. Például során feltételezzük, hogy a "contoso.com" már elérhető nevű hello kijelölt virtuális hálózati tartományhoz. Később, próbálja meg az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz való hello tooenable ugyanazon tartomány nevét (például "contoso.com") a virtuális hálózat. Tooenable az Azure AD tartományi szolgáltatások közben hibát tapasztal.

Ez a hiba van a virtuális hálózat hello tartománynév tooname ütközések miatt. Ebben az esetben egy másik nevet tooset mentése az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz kell használnia. Alternatív megoldásként leépíti hello meglévő tartományhoz, és folytathatja a tooenable az Azure AD tartományi szolgáltatások.

### <a name="inadequate-permissions"></a>Nincsenek megfelelő engedélyei
**Hibaüzenet:**

*Tartományi szolgáltatások nem sikerült engedélyezni ezt az Azure AD-bérlőben. hello szolgáltatás nem rendelkezik megfelelő engedélyekkel toohello alkalmazás "Azure AD tartományi szolgáltatások Sync" nevezik. "Az Azure AD tartományi szolgáltatások Sync" nevű hello alkalmazás törlése, és ismételje meg az tooenable tartományi szolgáltatások az Azure AD-bérlő.*

**Szervizelés:**

Ellenőrizze a toosee, ha "Azure AD tartományi szolgáltatások Sync" hello nevű alkalmazás az Azure AD-címtárát. Ha ezt az alkalmazást, törölje azt, majd engedélyezze újra Azure AD tartományi szolgáltatásokat.

Hajtsa végre hello következő lépések toocheck hello alkalmazás és toodelete hello meglétét, ha hello alkalmazás létezik:

1. Keresse meg a toohello **a klasszikus Azure portálon** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. Jelölje be hello **Active Directory** csomópontot a bal oldali panelen hello.
3. Jelölje be hello Azure AD bérlőt (címtárat), amelynek szeretné tooenable Azure AD tartományi szolgáltatásokat.
4. Keresse meg a toohello **alkalmazások** fülre.
5. Jelölje be hello **a vállalat tulajdonában lévő alkalmazások** hello legördülő lehetőséget.
6. Ellenőrizze az alkalmazás **Azure AD tartományi szolgáltatások szinkronizáló**. Hello alkalmazás létezik, folytassa a műveletet toodelete azt.
7. Miután hello alkalmazás törölt, próbálkozzon újra a tooenable az Azure AD tartományi szolgáltatásokban.

### <a name="invalid-configuration"></a>Érvénytelen konfiguráció
**Hibaüzenet:**

*Tartományi szolgáltatások nem sikerült engedélyezni ezt az Azure AD-bérlőben. Tartományi szolgáltatások az Azure AD-bérlő az alkalmazás nem rendelkezik hello hello szükséges engedélyek tooenable tartományi szolgáltatásokban. Hello alkalmazás azonosítója d87dcbc6-a371-462e-88e3-28ad15ec4e64 hello alkalmazás törlése, és ismételje meg az Azure AD-bérlő tartományszolgáltatásokban tooenable.*

**Szervizelés:**

Ellenőrizze a toosee, ha nem található a "AzureActiveDirectoryDomainControllerServices" (az alkalmazás azonosítója, amelyet d87dcbc6-a371-462e-88e3-28ad15ec4e64) hello nevű alkalmazás az Azure AD-címtárát. Ha az alkalmazás már létezik, toodelete szüksége, és ezután engedélyezze újra az Azure AD tartományi szolgáltatások.

A következő PowerShell parancsfájl toofind hello alkalmazás hello használja, és törölje.

> [!NOTE]
> A parancsfájl a **Azure AD PowerShell 2-es** parancsmagok. Az összes elérhető parancsmagok és toodownload hello modul teljes listáját, olvassa el a hello [AzureAD PowerShell referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt757189.aspx).
>
>

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted hello Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted hello Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted hello Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>A Microsoft Graph le van tiltva
**Hibaüzenet:**

Tartományi szolgáltatások nem sikerült engedélyezni ezt az Azure AD-bérlőben. Microsoft Azure AD alkalmazás hello le van tiltva, az Azure AD-bérlőben. Hello alkalmazás azonosítója 00000002-0000-0000-c000-000000000000 hello alkalmazás engedélyezése, és ismételje meg az Azure AD-bérlő tartományszolgáltatásokban tooenable.

**Szervizelés:**

Ellenőrizze a toosee, ha le van tiltva az alkalmazás hello azonosítója 00000002-0000-0000-c000-000000000000. Az alkalmazás hello Microsoft Azure AD-alkalmazáshoz, és a Graph API access tooyour az Azure AD bérlő biztosít. Azure AD tartományi szolgáltatások kell ezt az Azure AD-bérlő tooyour felügyelt alkalmazás engedélyezve toobe toosynchronize tartomány.

tooresolve ezt a hibát alkalmazás engedélyezése, és próbálkozzon tooenable tartományi szolgáltatások az Azure AD-bérlő.

## <a name="users-are-unable-toosign-in-toohello-azure-ad-domain-services-managed-domain"></a>Felhasználók az Azure AD tartományi szolgáltatások által kezelt tartomány toohello nem toosign
Ha egy vagy több felhasználó az Azure AD-bérlőben található, újonnan létrehozott toohello felügyelt tartomány nem toosign, hajtsa végre az alábbi hibaelhárítási lépésekkel hello:

* **Jelentkezzen be UPN formátum használatával:** próbálja toosign hello UPN formátumban (például "joeuser@contoso.com") helyett hello SAMAccountName formátumban ("CONTOSO\joeuser"). hello SAMAccountName előfordulhat, hogy hozható létre automatikusan, a felhasználók számára, amelynek UPN-előtag túlságosan hosszú, vagy van hello megegyezik egy másik felhasználóként hello felügyelt tartományon. hello UPN formátum az Azure AD-bérlő belül egyedi garantáltan toobe.

> [!NOTE]
> Az Azure AD tartományi szolgáltatások által kezelt tartomány toohello hello UPN formátum toosign használatát javasoljuk.
>
>

* Győződjön meg arról, hogy [engedélyezve a jelszó-szinkronizálás](active-directory-ds-getting-started-password-sync.md) hello a kezdeti lépések útmutatóban található lépések hello megfelelően.
* **Külső fiókok:** győződjön meg arról, hogy hatással hello felhasználói fiók nem külső fiók hello Azure AD-bérlőben. Külső fiókok például Microsoft-fiókok (például "joe@live.com") vagy külső felhasználói fiókok Azure AD-címtárban. Azure AD tartományi szolgáltatások nem tartozik ilyen felhasználói fiókhoz tartozó hitelesítő adatok, ezek a felhasználók által kezelt tartomány toohello nem tud bejelentkezni.
* **Fiókok szinkronizálva:** hatással hello felhasználói fiókok szinkronizálása a helyszíni könyvtárból, ellenőrizze, hogy:

  * Telepítése vagy frissítése toohello [legújabb ajánlott az Azure AD Connect kiadás](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
  * Az Azure AD Connect túl konfigurált[teljes szinkronizálást végezzen](active-directory-ds-getting-started-password-sync.md).
  * Attól függően, hogy a címtárban hello méretét akkor előfordulhat, hogy igénybe a felhasználói fiókok, és hitelesítőadat-kivonatok toobe érhető el az Azure AD tartományi szolgáltatásokban. Gondoskodjon arról, hogy elegendő ideig, mielőtt újra próbálkozna a (attól függően, a címtár - néhány óra tooa a napot vagy a kettő nagyméretű címtárak esetében hello mérete) várja.
  * Ha az előző lépésekben hello ellenőrzése után hello a probléma továbbra is fennáll, indítsa újra a Microsoft Azure AD Sync szolgáltatás hello. A szinkronizálás gépről elindítani a parancssort, és hajtsa végre a következő parancsok hello:

    1. net stop "Microsoft Azure AD Sync"
    2. a net start "Microsoft Azure AD Sync"
* **Csak felhőalapú fiókok**: Ha hatással hello felhasználói fiók egy kizárólag felhőalapú felhasználói fiókot, győződjön meg arról, hello felhasználó módosította a jelszavát, miután engedélyezte az Azure AD tartományi szolgáltatásokat. Ez a lépés azt eredményezi, hello hitelesítő kivonatok Azure AD tartományi szolgáltatások toobe jön létre a szükséges.

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Eltávolítja az Azure AD-bérlő felhasználók a felügyelt tartományok nem törlődnek
Az Azure AD megvédi Önt a felhasználói objektumok véletlen törlésétől. Az Azure AD-bérlő töröl egy felhasználói fiókot, hello megfelelő felhasználói objektum esetén áthelyezett toohello Lomtár. Ha ezt a műveletet szinkronizált tooyour által felügyelt tartományokhoz, okoz hello megfelelő felhasználói fiók toobe megjelölve le van tiltva. Ez a szolgáltatás segít a helyre, vagy később törlés visszavonása hello felhasználói fiókot.

hello hello marad le van tiltva a felügyelt tartományok lévő felhasználói fiók, akkor is, ha a felhasználói fiók újbóli létrehozása hello azonos egyszerű felhasználónév az Azure AD-címtárát. tooforce tooremove hello felhasználói fiók a felügyelt tartomány kell törölni az Azure AD-bérlőn.

tooremove hello felhasználói fiókot a teljes mértékben a felügyelt tartományok hello felhasználó végleges törlése az Azure AD-bérlő. Hello Remove-MsolUser PowerShell-parancsmag használata hello "-RemoveFromRecycleBin" lehetőség, mert leírt [MSDN-cikk](https://msdn.microsoft.com/library/azure/dn194132.aspx).

## <a name="contact-us"></a>Kapcsolat
Lépjen kapcsolatba a hello Azure Active Directory tartományi szolgáltatások termékért felelős csoport túl[visszajelzés megosztása vagy támogatásához](active-directory-ds-contact-us.md).
