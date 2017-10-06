---
title: "Attribútumok szinkronizálva az Azure AD Connect |} Microsoft Docs"
description: "Listák hello attribútumokat, amelyek az Active Directory tooAzure szinkronizálva."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: c2bb36e0-5205-454c-b9b6-f4990bcedf51
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: 2fe5b944a7fc832f245631416c265fb82eedeb15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-attributes-synchronized-tooazure-active-directory"></a>Azure AD Connect szinkronizálása: attribútumok szinkronizálva tooAzure Active Directory
Ez a témakör az Azure AD Connect szinkronizálási szolgáltatás által szinkronizált attribútumok hello sorolja fel.  
hello attribútumok szerint vannak csoportosítva kapcsolódó hello Azure AD-alkalmazáshoz.

## <a name="attributes-toosynchronize"></a>Attribútumok toosynchronize
Egy gyakori kérdést *Mi az minimális attribútumok toosynchronize hello listája*. hello alapértelmezett és ajánlott módja a tookeep hello alapértelmezett attribútumok, egy teljes globális Címlista (globális címlista) lehet létrehozni a hello felhő- és tooget az Office 365 számítási feladattal minden szolgáltatás. Bizonyos esetekben egyes attribútum, hogy a szervezet nem szeretné, hogy szinkronizált toohello felhő mivel ezek az attribútumok tartalmaznak bizalmas vagy személyhez köthető adatokat (személyazonosításra alkalmas adatokat) adatok, például a ebben a példában:  
![hibás attribútumok](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

Ebben az esetben ebben a témakörben attribútumok listáját hello kezdődnie, és azonosíthatja azokat attribútumokat, amelyek bizalmas vagy személyazonosításra alkalmas adatokat tartalmaz, és nem lehet szinkronizálni. Törölje azok telepítési használata során [az Azure AD alkalmazás- és attribútumszűrés](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering).

> [!WARNING]
> Attribútumok jelölésének, amikor nagy körültekintéssel végezze el, és csak az ezen attribútumok nem feltétlenül lehetséges toosynchronize törölje. Egyéb attribútumai unselecting szolgáltatások negatív hatással lehet.
>
>

## <a name="office-365-proplus"></a>Az Office 365 ProPlus
| Attribútum neve | Felhasználó | Megjegyzés |
| --- |:---:| --- |
| AccountEnabled |X |Meghatározza, hogy a fiók engedélyezve van. |
| CN |X | |
| displayName |X | |
| objectSID |X |gépi tulajdonság. AD felhasználóazonosítót használt toomaintain szinkronizálás között Azure AD és az AD. |
| pwdLastSet |X |gépi tulajdonság. Használt tooknow tooinvalidate már kiadott jogkivonatokat. A jelszó-szinkronizálás és a összevonási használják. |
| sourceAnchor |X |gépi tulajdonság. Nem módosítható azonosítót toomaintain kapcsolat hozzáadása és az Azure AD között. |
| usageLocation |X |gépi tulajdonság. hello felhasználói ország. A licenc-hozzárendelést használatos. |
| UserPrincipalName |X |Egyszerű felhasználónév értéke hello bejelentkezési azonosító hello felhasználó számára. Leggyakrabban hello ugyanaz, mint [mail] érték. |

## <a name="exchange-online"></a>Exchange Online
| Attribútum neve | Felhasználó | Kapcsolatfelvétel | Csoport | Megjegyzés |
| --- |:---:|:---:|:---:| --- |
| AccountEnabled |X | | |Meghatározza, hogy a fiók engedélyezve van. |
| Segéd |X |X | | |
| altRecipient |X | | |Az Azure AD Connect build 1.1.552.0 igényel vagy után. |
| authOrig |X |X |X | |
| C |X |X | | |
| CN |X | |X | |
| CO |X |X | | |
| Vállalati |X |X | | |
| Országhívószám |X |X | | |
| Szervezeti egység |X |X | | |
| leírás |X |X |X | |
| displayName |X |X |X | |
| dLMemRejectPerms |X |X |X | |
| dLMemSubmitPerms |X |X |X | |
| extensionAttribute1 |X |X |X | |
| extensionAttribute10 |X |X |X | |
| extensionAttribute11 |X |X |X | |
| extensionAttribute12 |X |X |X | |
| extensionAttribute13 |X |X |X | |
| extensionAttribute14 |X |X |X | |
| extensionAttribute15 |X |X |X | |
| extensionAttribute2 |X |X |X | |
| extensionAttribute3 |X |X |X | |
| extensionAttribute4 |X |X |X | |
| extensionAttribute5 |X |X |X | |
| extensionAttribute6 |X |X |X | |
| extensionAttribute7 |X |X |X | |
| extensionAttribute8 |X |X |X | |
| extensionAttribute9 |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| HomePhone |X |X | | |
| információ |X |X |X |Ez az attribútum a csoportok jelenleg nem felhasznált. |
| monogramja |X |X | | |
| l csomag |X |X | | |
| LegacyExchangeDN |X |X |X | |
| mailNickname |X |X |X | |
| mangedBy | | |X | |
| Manager |X |X | | |
| Tag | | |X | |
| Mobileszköz |X |X | | |
| Az msDS-HABSeniorityIndex |X |X |X | |
| Az msDS-PhoneticDisplayName |X |X |X | |
| msExchArchiveGUID |X | | | |
| msExchArchiveName |X | | | |
| Arról |X |X | | |
| msExchAuditAdmin |X | | | |
| msExchAuditDelegate |X | | | |
| msExchAuditDelegateAdmin |X | | | |
| msExchAuditOwner |X | | | |
| msExchBlockedSendersHash |X |X | | |
| msExchBypassAudit |X | | | |
| msExchBypassModerationLink | | |X |Az Azure AD Connect 1.1.524.0 verzió érhető el |
| msExchCoManagedByLink | | |X | |
| msExchDelegateListLink |X | | | |
| msExchELCExpirySuspensionEnd |X | | | |
| msExchELCExpirySuspensionStart |X | | | |
| msExchELCMailboxFlags |X | | | |
| msExchEnableModeration |X | |X | |
| msExchExtensionCustomAttribute1 |X |X |X |Ez az attribútum a által Exchange Online jelenleg nem használt. |
| msExchExtensionCustomAttribute2 |X |X |X |Ez az attribútum a által Exchange Online jelenleg nem használt. |
| msExchExtensionCustomAttribute3 |X |X |X |Ez az attribútum a által Exchange Online jelenleg nem használt. |
| msExchExtensionCustomAttribute4 |X |X |X |Ez az attribútum a által Exchange Online jelenleg nem használt. |
| msExchExtensionCustomAttribute5 |X |X |X |Ez az attribútum a által Exchange Online jelenleg nem használt. |
| msExchHideFromAddressLists |X |X |X | |
| msExchImmutableID |X | | | |
| msExchLitigationHoldDate |X |X |X | |
| msExchLitigationHoldOwner |X |X |X | |
| msExchMailboxAuditEnable |X | | | |
| msExchMailboxAuditLogAgeLimit |X | | | |
| msExchMailboxGuid |X | | | |
| msExchModeratedByLink |X |X |X | |
| msExchModerationFlags |X |X |X | |
| msExchRecipientDisplayType |X |X |X | |
| msExchRecipientTypeDetails |X |X |X | |
| msExchRemoteRecipientType |X | | | |
| msExchRequireAuthToSendTo |X |X |X | |
| msExchResourceCapacity |X | | | |
| msExchResourceDisplay |X | | | |
| msExchResourceMetaData |X | | | |
| msExchResourceSearchProperties |X | | | |
| msExchRetentionComment |X |X |X | |
| msExchRetentionURL |X |X |X | |
| msExchSafeRecipientsHash |X |X | | |
| msExchSafeSendersHash |X |X | | |
| msExchSenderHintTranslations |X |X |X | |
| msExchTeamMailboxExpiration |X | | | |
| msExchTeamMailboxOwners |X | | | |
| msExchTeamMailboxSharePointUrl |X | | | |
| msExchUserHoldPolicies |X | | | |
| msOrg-IsOrganizational | | |X | |
| objectSID |X | |X |gépi tulajdonság. AD felhasználóazonosítót használt toomaintain szinkronizálás között Azure AD és az AD. |
| oOFReplyToOriginator | | |X | |
| otherFacsimileTelephone |X |X | | |
| otherHomePhone |X |X | | |
| otherTelephone |X |X | | |
| Személyhívó |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Irányítószám |X |X | | |
| proxyAddresses |X |X |X | |
| publicDelegates |X |X |X | |
| pwdLastSet |X | | |gépi tulajdonság. Használt tooknow tooinvalidate már kiadott jogkivonatokat. A jelszó-szinkronizálás és a összevonási használják. |
| reportToOriginator | | |X | |
| reportToOwner | | |X | |
| SecurityEnabled | | |X |A groupType származtatni. |
| sorozatszám |X |X | | |
| sourceAnchor |X |X |X |gépi tulajdonság. Nem módosítható azonosítót toomaintain kapcsolat hozzáadása és az Azure AD között. |
| St |X |X | | |
| StreetAddress |X |X | | |
| targetAddress |X |X | | |
| telephoneAssistant |X |X | | |
| TelephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| Cím |X |X | | |
| unauthOrig |X |X |X | |
| usageLocation |X | | |gépi tulajdonság. hello felhasználói ország. A licenc-hozzárendelést használatos. |
| userCertificate |X |X | | |
| UserPrincipalName |X | | |Egyszerű felhasználónév értéke hello bejelentkezési azonosító hello felhasználó számára. Leggyakrabban hello ugyanaz, mint [mail] érték. |
| userSMIMECertificates |X |X | | |
| wWWHomePage |X |X | | |

## <a name="sharepoint-online"></a>SharePoint Online
| Attribútum neve | Felhasználó | Kapcsolatfelvétel | Csoport | Megjegyzés |
| --- |:---:|:---:|:---:| --- |
| AccountEnabled |X | | |Meghatározza, hogy a fiók engedélyezve van. |
| authOrig |X |X |X | |
| C |X |X | | |
| CN |X | |X | |
| CO |X |X | | |
| Vállalati |X |X | | |
| Országhívószám |X |X | | |
| Szervezeti egység |X |X | | |
| leírás |X |X |X | |
| displayName |X |X |X | |
| dLMemRejectPerms |X |X |X | |
| dLMemSubmitPerms |X |X |X | |
| extensionAttribute1 |X |X |X | |
| extensionAttribute10 |X |X |X | |
| extensionAttribute11 |X |X |X | |
| extensionAttribute12 |X |X |X | |
| extensionAttribute13 |X |X |X | |
| extensionAttribute14 |X |X |X | |
| extensionAttribute15 |X |X |X | |
| extensionAttribute2 |X |X |X | |
| extensionAttribute3 |X |X |X | |
| extensionAttribute4 |X |X |X | |
| extensionAttribute5 |X |X |X | |
| extensionAttribute6 |X |X |X | |
| extensionAttribute7 |X |X |X | |
| extensionAttribute8 |X |X |X | |
| extensionAttribute9 |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| hideDLMembership | | |X | |
| homephone |X |X | | |
| információ |X |X |X | |
| monogramja |X |X | | |
| ipPhone |X |X | | |
| l csomag |X |X | | |
| mail |X |X |X | |
| mailnickname |X |X |X | |
| Felügyeli | | |X | |
| Manager |X |X | | |
| Tag | | |X | |
| middleName |X |X | | |
| Mobileszköz |X |X | | |
| msExchTeamMailboxExpiration |X | | | |
| msExchTeamMailboxOwners |X | | | |
| msExchTeamMailboxSharePointLinkedBy |X | | | |
| msExchTeamMailboxSharePointUrl |X | | | |
| objectSID |X | |X |gépi tulajdonság. AD felhasználóazonosítót használt toomaintain szinkronizálás között Azure AD és az AD. |
| oOFReplyToOriginator | | |X | |
| otherFacsimileTelephone |X |X | | |
| otherHomePhone |X |X | | |
| otherIpPhone |X |X | | |
| OtherMobile |X |X | | |
| otherPager |X |X | | |
| otherTelephone |X |X | | |
| Személyhívó |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Irányítószám |X |X | | |
| postOfficeBox |X |X | | |
| preferredLanguage |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |gépi tulajdonság. Használt tooknow tooinvalidate már kiadott jogkivonatokat. A jelszó-szinkronizálás és a összevonási használják. |
| reportToOriginator | | |X | |
| reportToOwner | | |X | |
| SecurityEnabled | | |X |A groupType származtatni. |
| sorozatszám |X |X | | |
| sourceAnchor |X |X |X |gépi tulajdonság. Nem módosítható azonosítót toomaintain kapcsolat hozzáadása és az Azure AD között. |
| St |X |X | | |
| StreetAddress |X |X | | |
| targetAddress |X |X | | |
| telephoneAssistant |X |X | | |
| TelephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| Cím |X |X | | |
| unauthOrig |X |X |X | |
| URL-címe |X |X | | |
| usageLocation |X | | |gépi tulajdonság. hello felhasználói ország. A licenc-hozzárendelést használatos. |
| UserPrincipalName |X | | |Egyszerű felhasználónév értéke hello bejelentkezési azonosító hello felhasználó számára. Leggyakrabban hello ugyanaz, mint [mail] érték. |
| wWWHomePage |X |X | | |

## <a name="lync-online"></a>Lync Online
| Attribútum neve | Felhasználó | Kapcsolatfelvétel | Csoport | Megjegyzés |
| --- |:---:|:---:|:---:| --- |
| AccountEnabled |X | | |Meghatározza, hogy a fiók engedélyezve van. |
| C |X |X | | |
| CN |X | |X | |
| CO |X |X | | |
| Vállalati |X |X | | |
| Szervezeti egység |X |X | | |
| leírás |X |X |X | |
| displayName |X |X |X | |
| facsimiletelephonenumber |X |X |X | |
| givenName |X |X | | |
| homephone |X |X | | |
| ipPhone |X |X | | |
| l csomag |X |X | | |
| mail |X |X |X | |
| mailNickname |X |X |X | |
| Felügyeli | | |X | |
| Manager |X |X | | |
| Tag | | |X | |
| Mobileszköz |X |X | | |
| msExchHideFromAddressLists |X |X |X | |
| msRTCSIP-ApplicationOptions |X | | | |
| msRTCSIP-DeploymentLocator |X |X | | |
| msRTCSIP-vonal |X |X | | |
| msRTCSIP-OptionFlags |X |X | | |
| msRTCSIP-OwnerUrn |X | | | |
| msRTCSIP-PrimaryUserAddress |X |X | | |
| msRTCSIP-UserEnabled |X |X | | |
| objectSID |X | |X |gépi tulajdonság. AD felhasználóazonosítót használt toomaintain szinkronizálás között Azure AD és az AD. |
| otherTelephone |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Irányítószám |X |X | | |
| preferredLanguage |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |gépi tulajdonság. Használt tooknow tooinvalidate már kiadott jogkivonatokat. A jelszó-szinkronizálás és a összevonási használják. |
| SecurityEnabled | | |X |A groupType származtatni. |
| sorozatszám |X |X | | |
| sourceAnchor |X |X |X |gépi tulajdonság. Nem módosítható azonosítót toomaintain kapcsolat hozzáadása és az Azure AD között. |
| St |X |X | | |
| StreetAddress |X |X | | |
| TelephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| Cím |X |X | | |
| usageLocation |X | | |gépi tulajdonság. hello felhasználói ország. A licenc-hozzárendelést használatos. |
| UserPrincipalName |X | | |Egyszerű felhasználónév értéke hello bejelentkezési azonosító hello felhasználó számára. Leggyakrabban hello ugyanaz, mint [mail] érték. |
| wWWHomePage |X |X | | |

## <a name="azure-rms"></a>Az Azure RMS
| Attribútum neve | Felhasználó | Kapcsolatfelvétel | Csoport | Megjegyzés |
| --- |:---:|:---:|:---:| --- |
| AccountEnabled |X | | |Meghatározza, hogy a fiók engedélyezve van. |
| CN |X | |X |Közös felhasználóneve vagy aliasa. Leggyakrabban hello előtag [mail] érték. |
| displayName |X |X |X |Egy karakterlánc, amely gyakran útvonalként megjelenő hello rövid (Keresztnév Vezetéknév) hello nevét jelöli. |
| mail |X |X |X |teljes e-mail címet. |
| Tag | | |X | |
| objectSID |X | |X |gépi tulajdonság. AD felhasználóazonosítót használt toomaintain szinkronizálás között Azure AD és az AD. |
| proxyAddresses |X |X |X |gépi tulajdonság. Az Azure AD által használt. Az összes másodlagos e-mail címet hello felhasználó tartalmazza. |
| pwdLastSet |X | | |gépi tulajdonság. Használt tooknow tooinvalidate már kiadott jogkivonatokat. |
| SecurityEnabled | | |X |GroupType származik. |
| sourceAnchor |X |X |X |gépi tulajdonság. Nem módosítható azonosítót toomaintain kapcsolat hozzáadása és az Azure AD között. |
| usageLocation |X | | |gépi tulajdonság. hello felhasználói ország. A licenc-hozzárendelést használatos. |
| UserPrincipalName |X | | |Az egyszerű felhasználónév értéke hello bejelentkezési azonosító hello felhasználó számára. Leggyakrabban hello ugyanaz, mint [mail] érték. |

## <a name="intune"></a>Intune-ban
| Attribútum neve | Felhasználó | Kapcsolatfelvétel | Csoport | Megjegyzés |
| --- |:---:|:---:|:---:| --- |
| AccountEnabled |X | | |Meghatározza, hogy a fiók engedélyezve van. |
| C |X |X | | |
| CN |X | |X | |
| leírás |X |X |X | |
| displayName |X |X |X | |
| mail |X |X |X | |
| mailnickname |X |X |X | |
| Tag | | |X | |
| objectSID |X | |X |gépi tulajdonság. AD felhasználóazonosítót használt toomaintain szinkronizálás között Azure AD és az AD. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |gépi tulajdonság. Használt tooknow tooinvalidate már kiadott jogkivonatokat. A jelszó-szinkronizálás és a összevonási használják. |
| SecurityEnabled | | |X |A groupType származtatni. |
| sourceAnchor |X |X |X |gépi tulajdonság. Nem módosítható azonosítót toomaintain kapcsolat hozzáadása és az Azure AD között. |
| usageLocation |X | | |gépi tulajdonság. hello felhasználói ország. A licenc-hozzárendelést használatos. |
| UserPrincipalName |X | | |Egyszerű felhasználónév értéke hello bejelentkezési azonosító hello felhasználó számára. Leggyakrabban hello ugyanaz, mint [mail] érték. |

## <a name="dynamics-crm"></a>Dynamics CRM
| Attribútum neve | Felhasználó | Kapcsolatfelvétel | Csoport | Megjegyzés |
| --- |:---:|:---:|:---:| --- |
| AccountEnabled |X | | |Meghatározza, hogy a fiók engedélyezve van. |
| C |X |X | | |
| CN |X | |X | |
| CO |X |X | | |
| Vállalati |X |X | | |
| Országhívószám |X |X | | |
| leírás |X |X |X | |
| displayName |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| l csomag |X |X | | |
| Felügyeli | | |X | |
| Manager |X |X | | |
| Tag | | |X | |
| Mobileszköz |X |X | | |
| objectSID |X | |X |gépi tulajdonság. AD felhasználóazonosítót használt toomaintain szinkronizálás között Azure AD és az AD. |
| physicalDeliveryOfficeName |X |X | | |
| Irányítószám |X |X | | |
| preferredLanguage |X | | | |
| pwdLastSet |X | | |gépi tulajdonság. Használt tooknow tooinvalidate már kiadott jogkivonatokat. A jelszó-szinkronizálás és a összevonási használják. |
| SecurityEnabled | | |X |A groupType származtatni. |
| sorozatszám |X |X | | |
| sourceAnchor |X |X |X |gépi tulajdonság. Nem módosítható azonosítót toomaintain kapcsolat hozzáadása és az Azure AD között. |
| St |X |X | | |
| StreetAddress |X |X | | |
| TelephoneNumber |X |X | | |
| Cím |X |X | | |
| usageLocation |X | | |gépi tulajdonság. hello felhasználói ország. A licenc-hozzárendelést használatos. |
| UserPrincipalName |X | | |Egyszerű felhasználónév értéke hello bejelentkezési azonosító hello felhasználó számára. Leggyakrabban hello ugyanaz, mint [mail] érték. |

## <a name="3rd-party-applications"></a>3. fél alkalmazások
Egy általános munkaterhelés vagy az alkalmazáshoz szükséges minimális attribútumok hello használt attribútumok. A munkaterhelés nem szerepel egy másik szakaszban, vagy nem Microsoft-alkalmazások használható. Hello következő explicit módon használható:

* (Csak a felhasználó felhasznált) Yammer
* [Hibrid üzleti vállalatközi (B2B) kereszt-szervezeti együttműködési forgatókönyvek erőforrásokhoz, mint a SharePoint által kínált](http://go.microsoft.com/fwlink/?LinkId=747036)

Ennek a csoportnak a, ha hello Azure AD-címtár nem használható attribútumok használt toosupport Office 365, Dynamics, vagy az Intune-ban. A core attribútumok egy kis készletét rendelkezik.

| Attribútum neve | Felhasználó | Kapcsolatfelvétel | Csoport | Megjegyzés |
| --- |:---:|:---:|:---:| --- |
| AccountEnabled |X | | |Meghatározza, hogy a fiók engedélyezve van. |
| CN |X | |X | |
| displayName |X |X |X | |
| givenName |X |X | | |
| mail |X | |X | |
| Felügyeli | | |X | |
| mailNickName |X |X |X | |
| Tag | | |X | |
| objectSID |X | | |gépi tulajdonság. AD felhasználóazonosítót használt toomaintain szinkronizálás között Azure AD és az AD. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |gépi tulajdonság. Használt tooknow tooinvalidate már kiadott jogkivonatokat. A jelszó-szinkronizálás és a összevonási használják. |
| sorozatszám |X |X | | |
| sourceAnchor |X |X |X |gépi tulajdonság. Nem módosítható azonosítót toomaintain kapcsolat hozzáadása és az Azure AD között. |
| usageLocation |X | | |gépi tulajdonság. hello felhasználói ország. A licenc-hozzárendelést használatos. |
| UserPrincipalName |X | | |Egyszerű felhasználónév értéke hello bejelentkezési azonosító hello felhasználó számára. Leggyakrabban hello ugyanaz, mint [mail] érték. |

## <a name="windows-10"></a>Windows 10
A Windows 10-tartományhoz csatlakoztatott computer(device) néhány attribútumok tooAzure AD szinkronizálja. Hello forgatókönyvek további információkért lásd: [csatlakozni a tartományhoz csatlakozó eszközök tooAzure AD, a Windows 10 észlel](../active-directory-azureadjoin-devices-group-policy.md). Ezek az attribútumok mindig szinkronizálja, és a Windows 10 nem jelenik meg az alkalmazások törölheti. A Windows 10-tartományhoz csatlakoztatott számítógép hello attribútum userCertificate feltöltve rendelkező azonosítja.

| Attribútum neve | Eszköz | Megjegyzés |
| --- |:---:| --- |
| AccountEnabled |X | |
| deviceTrustType |X |Tartományhoz csatlakozó számítógépek szoftveresen kötött érték. |
| displayName |X | |
| MS-DS-CreatorSID |X |Más néven registeredOwnerReference. |
| objectGUID |X |Más néven az eszközazonosító. |
| objectSID |X |Más néven onPremisesSecurityIdentifier. |
| operációs rendszer |X |Más néven deviceOSType. |
| operatingSystemVersion |X |Más néven deviceOSVersion. |
| userCertificate |X | |

Ezeknek az attribútumoknak **felhasználói** továbbá olyan toohello kijelölt más alkalmazások.  

| Attribútum neve | Felhasználó | Megjegyzés |
| --- |:---:| --- |
| domainFQDN |X |Más néven tartománynév. Például: contoso.com. |
| domainNetBios |X |Más néven netBiosName. Például: CONTOSO. |

## <a name="exchange-hybrid-writeback"></a>Exchange hibrid visszaírásához.
Ezek az attribútumok kerüljenek vissza az Azure AD tooon helyszíni Active Directory tooenable kiválasztásakor **Exchange hibrid**. Az Exchange verziójától függően kevesebb attribútumok szinkronizálása.

| Attribútum neve | Felhasználó | Kapcsolatfelvétel | Csoport | Megjegyzés |
| --- |:---:|:---:|:---:| --- |
| Az msDS-externaldirectoryobjectid azonosítót |X | | |Az Azure AD származó cloudAnchor. Ez az attribútum az új Exchange 2016 és a Windows Server 2016 AD. |
| msExchArchiveStatus |X | | |Online archiválás: Lehetővé teszi, hogy az ügyfelek tooarchive mail. |
| msExchBlockedSendersHash |X | | |Szűrés: Írja vissza helyszíni szűrés és online biztonságos és a blokkolt küldő adatokat az ügyfelektől. |
| msExchSafeRecipientsHash |X | | |Szűrés: Írja vissza helyszíni szűrés és online biztonságos és a blokkolt küldő adatokat az ügyfelektől. |
| msExchSafeSendersHash |X | | |Szűrés: Írja vissza helyszíni szűrés és online biztonságos és a blokkolt küldő adatokat az ügyfelektől. |
| msExchUCVoiceMailSettings |X | | |Egyesített üzenetküldési (egyesített ÜZENETKÜLDÉSI) – Online hangposta engedélyezése: Microsoft Lync Server integrációs tooindicate tooLync által használt kiszolgáló helyi hello rendelkezik hangposta online szolgáltatások. |
| msExchUserHoldPolicies |X | | |Bírósági eljárások fenntartási: Lehetővé teszi, hogy a felhőalapú szolgáltatások toodetermine, amely a felhasználók bírósági eljárások tartsa alatt. |
| proxyAddresses |X |X |X |Csak az Exchange Online-tól hello x500 cím szerepel. |
| publicDelegates |X | | |Lehetővé teszi, hogy az Exchange Online postafiók toobe SendOnBehalfTo jogok toousers a helyszíni Exchange postaládával kap. Az Azure AD Connect build 1.1.552.0 igényel vagy után. |

## <a name="exchange-mail-public-folder"></a>Exchange E-mail nyilvános mappa
Ezek az attribútumok szinkronizálva vannak a helyszíni Active Directory tooAzure tooenable kiválasztásakor AD **Exchange E-mail nyilvános mappa**.

| Attribútum neve | PublicFolder | Megjegyzés |
| --- | :---:| --- |
| displayName | X |  |
| mail | X |  |
| msExchRecipientTypeDetails | X |  |
| objectGUID | X |  |
| proxyAddresses | X |  |
| targetAddress | X |  |

## <a name="device-writeback"></a>Eszközvisszaíró
Eszköz objektum az Active Directoryban jön létre. Ezek az objektumok eszközök csatlakoztatott tooAzure AD vagy tartományhoz csatlakoztatott Windows 10-számítógépeket is lehet.

| Attribútum neve | Eszköz | Megjegyzés |
| --- |:---:| --- |
| altSecurityIdentities |X | |
| displayName |X | |
| megkülönböztető név |X | |
| Az msDS-CloudAnchor |X | |
| Az msDS-eszközazonosító |X | |
| Az msDS-DeviceObjectVersion |X | |
| Az msDS-DeviceOSType |X | |
| Az msDS-DeviceOSVersion |X | |
| Az msDS-DevicePhysicalIDs |X | |
| Az msDS-KeyCredentialLink |X |Csak a Windows Server 2016 AD séma |
| Az msDS-IsCompliant |X | |
| Az msDS-IsEnabled |X | |
| Az msDS-IsManaged |X | |
| Az msDS-RegisteredOwner |X | |

## <a name="notes"></a>Megjegyzések
* Ha a másik azonosító, hello helyszíni attribútum userPrincipalName hello Azure AD attribútum onPremisesUserPrincipalName szinkronizálva van. a másik azonosító attribútum hello, például az e-mail, hello Azure AD attribútum userPrincipalName szinkronizálva.
* A fenti hello listák, hello objektumtípus **felhasználói** is érvényes toohello objektumtípus **iNetOrgPerson**.

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.

További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
