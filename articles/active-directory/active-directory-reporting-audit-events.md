---
title: "Az Azure Active Directory naplójelentési eseményei |} Microsoft Docs"
description: "Elérhető megtekintésére és letöltésére az Azure Active Directory események naplózása"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 307eedf7-05bc-448d-a84d-bead5a4c5770
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 1620d917acf5a2c36767b5b03750c405f3631ee2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-audit-report-events"></a>Azure Active Directory naplózási jelentésének eseményei
*Ez a dokumentáció az [Azure Active Directory Reporting-útmutató](active-directory-reporting-guide.md) része.*

Az Azure Active Directory naplózási jelentés segítségével az ügyfelek azonosítása az Azure Active Directoryban történt privilegizált műveleteket. A privilegizált műveletekhez tartalmazza az illetéktelen módosítások (például a szerepkör létrehozása vagy a jelszó alaphelyzetbe állítása), változó Csoportházirend konfigurálásának (például jelszóházirendek) és (például tartomány-összevonási beállításai módosításának) directory konfigurációjának módosításai. A jelentések az esemény nevét, a szereplő ki hajtotta végre a műveletet, a cél erőforráson hatással a módosítás dátuma és időpontja (UTC) adja meg a naplórekordot. Az ügyfelek képesek naplózási események listájának beolvasása az Azure Active Directory keresztül a [Azure Portal](https://portal.azure.com/)leírtak szerint [megtekintése a vizsgálati naplókban](active-directory-reporting-azure-portal.md).

## <a name="list-of-audit-report-events"></a>Naplózási jelentés események listája
<!--- audit event descriptions should be in the past tense --->

| Események | Esemény leírása |
| --- | --- |
| **Felhasználói események** | |
| Felhasználó hozzáadása |A felhasználó felvéve a címtárba. |
| Felhasználó törlése |A felhasználó törölte a könyvtárból. |
| Licenc tulajdonságainak beállítása |A licenc tulajdonságainak megadása egy felhasználót a címtárban. |
| Felhasználói jelszó alaphelyzetbe állítása |A felhasználó jelszavának visszaállítása. |
| Felhasználói jelszó módosítása |A címtárban a felhasználó jelszava megváltozott. |
| Változás felhasználói licenc |Megváltozott egy felhasználót a címtárban a licenccel. Hogy milyen licencek volt frissítve, tekintse meg a [frissítés felhasználói](#update-user-attributes) az alábbi tulajdonságok |
| Felhasználó módosítása |Frissítve a felhasználót a címtárban. [Lásd az alábbi](#update-user-attributes) attribútumok frissíthető. |
| Set kényszerített felhasználói jelszó módosítása |A tulajdonság, amely arra kényszeríti a felhasználót, hogy módosítania kell a jelszavát, a bejelentkezési állítható be. |
| Felhasználói hitelesítő adatok frissítése |Felhasználó módosította a jelszót |
| **Események csoportosítása** | |
| Csoport hozzáadása |Olyan csoportot hozott létre a címtárban. |
| Frissítési csoporthoz |A címtárban csoport frissítése. Hogy milyen tulajdonságai volt frissítve, tekintse meg [csoport tulajdonságainak naplózása](#update-group-attributes) az alábbi részben |
| Csoport törlése |A csoport törlése a könyvtárból. |
| CreateGroupSettings |Létrehozott csoport beállításai |
| UpdateGroupSettings |Csoport beállításainak frissítése. Milyen csoportjára vonatkozó beállításait frissíteni volt megtekintéséhez tekintse meg [csoport tulajdonságainak naplózása](#update-group-attributes) az alábbi részben |
| DeleteGroupSettings |Törölt beállításai |
| SetGroupLicense |Állítsa be a csoport licenc. |
| SetGroupManagedBy |Felhasználó által kezelt csoport beállítása |
| AddGroupMember |Csoportba felvett tag |
| RemoveGroupMember |Tag eltávolítása a csoportból |
| AddGroupOwner |Csoporthoz hozzáadott tulajdonosa |
| RemoveGroupOwner |A csoportból eltávolított tulajdonosa |
| **Alkalmazás-események** | |
| Egyszerű szolgáltatásnév hozzáadása |A szolgáltatás egyszerű felvéve a címtárba. |
| Távolítsa el az egyszerű szolgáltatásnév |A szolgáltatás egyszerű távolítani a címtárból. |
| Adja hozzá a szolgáltatás egyszerű hitelesítő adatait |A szolgáltatás egyszerű hozzáadott hitelesítő adatokat. |
| Távolítsa el a szolgáltatás egyszerű hitelesítő adatait |Egy egyszerű szolgáltatásnév eltávolított hitelesítő adatokat. |
| Delegálás bejegyzés hozzáadása |Létrehozott egy [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) a címtárban. |
| Készlet delegálása bejegyzés |Frissítve egy [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) a címtárban. |
| Delegálás bejegyzés eltávolítása |Törölt egy [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) a címtárban. |
| **Szerepkör-események** | |
| Szerepkör tagjának szerepkör hozzáadása |A felhasználó hozzá a címtár szerepkörrel. |
| Szerepkör szerepkör tagjának eltávolítása |A felhasználó eltávolítja a címtár szerepkörrel. |
| AddRoleDefinition |A hozzáadott szerepkör-definíció. |
| UpdateRoleDefinition |Szerepkör-definíció frissítése. Milyen szerepkör beállításainak frissítése megtekintéséhez tekintse meg [szerepkör definíció tulajdonságainak naplózása](#update-role-definition-attributes) az alábbi részben |
| DeleteRoleDefinition |Szerepkör-definíció törlése. |
| AddRoleAssignmentToRoleDefinition |A szerepkör-definíció hozzáadott szerepkör-hozzárendelés. |
| RemoveRoleAssignmentFromRoleDefinition |A szerepkör-definíció eltávolított szerepkör-hozzárendelés. |
| AddRoleFromTemplate |A hozzáadott szerepkör sablonból. |
| UpdateRole |Frissített szerepkört. |
| AddRoleScopeMemberToRole |A hozzáadott hatókörön belüli tag szerepkörhöz. |
| RemoveRoleScopedMemberFromRole |Szerepkör hatókörrel rendelkező tagot eltávolítani. |
| **Események (az összes új esemény)** | |
| AddDevice |A hozzáadott eszköz. |
| UpdateDevice |Frissített eszköz. Hogy milyen eszköztulajdonságok volt frissítve, tekintse meg [eszköztulajdonságok Audited](#update-device-attributes) az alábbi részben |
| DeleteDevice |Törölt eszköz. |
| AddDeviceConfiguration |A hozzáadott eszközök konfigurációját. |
| UpdateDeviceConfiguration |Frissített eszközök konfigurációját. Hogy mely eszköz konfigurációs tulajdonságok volt frissítve, tekintse meg [eszköz konfigurációs tulajdonságok Audited](#update-device-configuration-attributes) az alábbi részben |
| DeleteDeviceConfiguration |Törölt eszközök konfigurációját. |
| AddRegisteredOwner |A hozzáadott regisztrált felhasználó eszközre. |
| AddRegisteredUsers |A regisztrált felhasználók hozzáadandó eszköz. |
| RemoveRegisteredOwner |Regisztrált felhasználó távolíthatja el. |
| RemoveRegisteredUsers |A regisztrált felhasználók távolíthatja el. |
| RemoveDeviceCredentials |Távolítsa el a hitelesítő adatai. |
| **B2B események** | |
| Kötegelt meghívja feltöltve. |A rendszergazda fel van töltve a partner felhasználóknak kérését tartalmazó fájlt. |
| Kötegelt küldték el feldolgozásra. |Egy partner felhasználók meghívást tartalmazó fájl feldolgozása megtörtént. |
| Külső felhasználói meghívásához. |A külső felhasználó kérték a könyvtárba. |
| Külső felhasználói a meghívás beváltani. |A külső felhasználó rendelkezik sikerült beváltani a meghívó a könyvtárba. |
| Külső felhasználó felvétele csoportba. |A külső felhasználó rendelt tagsági egy csoportot a címtárban. |
| Külső felhasználó hozzárendelése alkalmazás. |A külső felhasználó rendelték közvetlen hozzáférést egy alkalmazáshoz. |
| Ugrásszerű bérlő létrehozásakor. |Egy új bérlőt a meghívó érvényesítési hozta az Azure ad-ben. |
| A felhasználó ugrásszerű létrehozása. |A felhasználó hozta található meglévő bérlő az Azure ad-ben a meghívó érvényesítési. |
| **Adminisztratív egységei (az összes új esemény)** | |
| AddAdministrativeUnit |Adja hozzá a felügyeleti egység. |
| UpdateAdministrativeUnit |Frissítés felügyeleti egység. Hogy milyen felügyeleti egység tulajdonságai volt frissítve, tekintse meg [naplózva felügyeleti egység tulajdonságai](#update-administrative-unit-attributes) az alábbi részben |
| DeleteAdministrativeUnit |Törölje a felügyeleti egységet. |
| AddMemberToAdministrativeUnit |Tag hozzáadása a felügyeleti egység. |
| RemoveMemberFromAdministrativeUnit |Tag eltávolítása a felügyeleti egység. |
| **Directory eseményei** | |
| Partner hozzáadása vállalati |Egy partner felvéve a címtárba. |
| Távolítsa el a Partner a vállalati |Egy partner távolítani a címtárból. |
| DemotePartner |Partner fokozni. |
| Vállalati tartomány hozzáadása |A tartomány felvéve a címtárba. |
| Távolítsa el a tartomány a vállalati |A tartomány távolítani a címtárból. |
| Tartomány frissítése |A tartomány a könyvtár frissítése. Hogy milyen tartomány tulajdonságai volt frissítve, tekintse meg [ellenőrzött tartomány tulajdonságai](#update-domain-attributes) az alábbi részben |
| Set tartományi hitelesítéshez |Módosította a vállalat alapértelmezett beállítása. |
| Állítsa be a vállalat kapcsolattartási adatai |Vállalati szintű kapcsolattartási beállítások megadása. Ez magában foglalja a Microsoft Online Services marketing, műszaki értesítések tartozó e-mail-címeket. |
| A tartomány összevonási beállításai |A tartomány összevonási beállításainak frissítése. |
| Ellenőrizze a tartomány |A címtár tartomány ellenőrzése. |
| E-mailek ellenőrzött tartomány ellenőrzése |E-mail ellenőrzése alkalmazásával a tartomány ellenőrzése. |
| A vállalat DirSyncEnabled jelző beállítása |A tulajdonság, amely lehetővé teszi egy könyvtárat az Azure AD-szinkronizálás beállítása. |
| Jelszószabályzat beállítása |Felhasználói jelszavak hosszát vagy a karakter megkötések beállítása. |
| Cégadatok beállítása |Módosította a vállalati szintű adatait. Tekintse meg a [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) további részleteket a PowerShell-parancsmagot. |
| SetCompanyAllowedDataLocation |Set vállalati adatok helye engedélyezett. |
| SetCompanyDirSyncEnabled |Állítsa a DirSyncEnabled jelzőjét. |
| SetCompanyDirSyncFeature |Állítsa be a DirSync szolgáltatást. |
| SetCompanyInformation |Set vállalati adatokat. |
| SetCompanyMultiNationalEnabled |Állítsa be a vállalati nemzetközi szolgáltatás engedélyezve van. |
| SetDirectoryFeatureOnTenant |Állítsa be a címtár szolgáltatást tenant. |
| SetTenantLicenseProperties |Bérlői licenc tulajdonságainak beállítása. |
| CreateCompanySettings |Hozzon létre a vállalati beállítások |
| UpdateCompanySettings |Vállalati beállítások frissítése. Hogy mely vállalati tulajdonságok volt frissítve, tekintse meg [naplózva vállalati tulajdonságok](#update-company-attributes) az alábbi részben |
| DeleteCompanySettings |Törölje a vállalati beállítások |
| SetAccidentalDeletionThreshold |Megadhatja a véletlen törlés küszöbértéket. |
| SetRightsManagementProperties |Rights management beállításainak megadása. |
| PurgeRightsManagementProperties |Rights management tulajdonságok kiürítése. |
| UpdateExternalSecrets |Külső kulcsok frissítése |
| **Házirend-események (az összes új esemény)** | |
| AddPolicy |Szabályzat hozzáadása. |
| UpdatePolicy |Házirend frissítése. |
| Szabályzat törlése |Házirend törlése. |
| AddDefaultPolicyApplication |Alkalmazás-házirend hozzáadása. |
| AddDefaultPolicyServicePrincipal |Szolgáltatásnév-házirend hozzáadása. |
| RemoveDefaultPolicyApplication |Távolítsa el a házirend-alkalmazás. |
| RemoveDefaultPolicyServicePrincipal |Távolítsa el a házirendet a szolgáltatásnevet. |
| RemovePolicyCredentials |Távolítsa el a házirend hitelesítő adatokat. |

## <a name="audit-report-retention"></a>Naplózási jelentés adatmegőrzési

A megőrzési legfrissebb tudnivalókat lásd: [Azure Active Directory-jelentés adatmegőrzési](active-directory-reporting-retention.md).


Az ügyfelek a naplóesemények tárolásához hosszabb megőrzési ideig iránt érdeklődik a Reporting API segítségével rendszeresen lekéréses külön adattárba események naplózása. Lásd: [Bevezetés a Reporting API használatába](active-directory-reporting-api-getting-started.md) részleteiről.

## <a name="properties-included-with-each-audit-event"></a>Összes naplózási eseményt részét képező tulajdonságok
| Tulajdonság | Leírás |
| --- | --- |
| Dátum és idő |A dátum és időpont, amikor a naplózási esemény történt |
| Aktor |A felhasználó vagy az egyszerű szolgáltatásnév a műveletet végző |
| Műveletek |Végre lett hajtva a kívánt műveletet |
| cél |A felhasználó vagy szolgáltatás egyszerű, amely a műveletet hajtott végre |

## <a name="update-user-attributes"></a>"A felhasználó frissítése" attribútumok
A "Frissítés user" naplózási eseményt tartalmaz milyen felhasználói attribútumok frissültek további információkat. Minden attribútum a korábbi és új értéke is megtalálható.

| Attribútum | Leírás |
| --- | --- |
| AccountEnabled |A felhasználói bejelentkezés. |
| AssignedLicense |A felhasználóhoz rendelt összes licencet. |
| AssignedPlan |A licencek a felhasználóhoz rendelt eredő szolgáltatás tervek. |
| LicenseAssignmentDetail |A licencek a felhasználóhoz rendelt adatokat. Például ha Csoportalapú licencelési volt szó, ebbe beletartozik a csoportot, amelyhez meg a licenc. |
| Mobiltelefon |A felhasználó mobiltelefon. |
| OtherMail |A felhasználó másodlagos e-mail címét. |
| OtherMobile |A felhasználó alternatív mobiltelefon. |
| StrongAuthenticationMethod |A felhasználó által beállított többtényezős hitelesítést, például a hang hívja, SMS vagy ellenőrző kódot egy mobilalkalmazás az ellenőrzési módszereket listáját. |
| StrongAuthenticationRequirement |Többtényezős hitelesítés van érvényben, ha engedélyezve van, vagy a felhasználó le van tiltva. |
| StrongAuthenticationUserDetails |A felhasználó telefonszáma, másodlagos és az e-mail cím, a többtényezős hitelesítés és a jelszó alaphelyzetbe állítása ellenőrzéshez használt telefonszám. |
| StrongAuthenticationPhoneAppDetail |Részletes forof phone-alkalmazásokat regisztrálni 2FA bejelentkezési végrehajtásához |
| TelephoneNumber |A felhasználó telefonszámát. |
| AlternativeSecurityId |Az objektum alternatív biztonsági azonosítója. |
| CreationType |Létrehozási módjának a felhasználó (vagy meghívó ugrásszerű). |
| InviteTicket |A felhasználó felkérést jegyek listája. |
| InviteReplyUrl |Meghívót elfogadták válasz URL-címek listáját. |
| InviteResources |Amelyhez a felhasználó kérték erőforrások listáját. |
| LastDirSyncTime |Az objektum frissült a a mérvadó szinkronizálása miatt legutóbbi (ügyfél helyi) directory. |
| MSExchRemoteRecipientType |A Maps MSO címzett típusú lehet. Tekintse meg a címzett típusok [MSO címzett típusok] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx |
| PreferredDataLocation |A felhasználó, a csoport, az ügyfél, a nyilvános mappa elsődleges helyét, vagy az eszköz adatok. |
| ProxyAddresses |A címet, amely az Exchange Server címzett objektum egy idegen levelezőrendszer elismert. |
| StsRefreshTokensValidFrom |Végzi a token visszavonási információk frissítése, mert egyetlen most minősülnek kiadott STS frissítési jogkivonatokat lejárt. |
| UserPrincipalName |A felhasználónév, amely egy felhasználó egy Internet-stílusú bejelentkezési nevét. |
| Userstate objektumhoz |Felhasználói jóváhagyásra vár/PendingAcceptance/elfogadva/PendingVerification állapota. |
| UserStateChangedOn |Utolsó módosítás userstate objektumhoz időbélyegzője. Elindítása életciklus munkafolyamatok használatával. |
| UserType |Felhasználó típusa. A tag (0), (1), Vendég Viral(2). |

## <a name="update-group-attributes"></a>"A frissítési csoport" attribútumok
| Attribútum | Leírás |
| --- | --- |
| Besorolás |Egy egyesített csoportot (HBI, MBI stb.) a besorolást. |
| Leírás |Emberek számára olvasható leíró kifejezések arról az objektumról. |
| displayName |Az objektum megjelenített neve |
| DirSyncEnabled |Meghatározza, hogy szinkronizálás az egy mérvadó (ügyfél helyi) directory. |
| GroupLicenseAssignment |Licenc-hozzárendelést egy csoport |
| GroupType |Csoportja, egységes (0) |
| IsMembershipRuleLocked |Azt jelzi, hogy a MembershipRule tulajdonság van beállítva a önkiszolgáló csoportkezelési szolgáltatás, és a felhasználók által nem módosítható. Csak ha GroupType tartalmaz GroupType.DynamicMembership csoportok használható |
| IsPublic |Ez a jelző azt jelzi, ha a csoport nyilvános/titkos. |
| LastDirSyncTime |Az objektum frissült a mérvadó a szinkronizálás eredményeképpen legutóbbi (a helyszínen ügyfél) directory. |
| Levelezés |Elsődleges e-mail címét |
| MailEnabled |Azt jelzi, hogy ez a csoport e-mail funkció. |
| mailNickname |Cím könyv objektum kézjegy, általában az előző e-mail név része a "@" szimbólummal. |
| MembershipRule |Egy karakterlánc kifejezése a használt feltételeket a önkiszolgáló csoportkezelési szolgáltatás mely tagokat a csoporthoz kell tartoznia. További információ a IsMembershipRuleLocked. Csak ha GroupType tartalmaz GroupType.DynamicMembership csoportok használható. |
| MembershipRuleProcessingState |A feldolgozása a csoport tagságát állapotának meghatározása önkiszolgáló csoportkezelési szolgáltatás definiált enum érték. Csak ha GroupType tartalmaz GroupType.DynamicMembership csoportok használható. |
| ProxyAddresses |A címet, amely az Exchange Server címzett objektum egy idegen levelezőrendszer elismert. |
| RenewedDateTime |Ha egy csoport legutóbb megújított Timestamp típusú rekord. |
| SecurityEnabled |Azt jelzi, hogy a csoportban lévő tagság befolyásolhatják felhasználását engedélyezési döntésekhez. |
| WellKnownObject |A Deleted object, előre meghatározott egyik a kijelölő létrehozott. |

## <a name="update-device-attributes"></a>"Az eszköz frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| AccountEnabled |Azt jelzi, hogy a rendszerbiztonsági tag hitelesítheti. |
| CloudAccountEnabled |Azt jelzi, hogy a rendszerbiztonsági tag hitelesítheti. Eszközállapotokat az InTune, amikor az eszköz a helyszínen értékűre. |
| CloudDeviceOSType |Az eszköz típusa alapján az operációs rendszer például Windows RT, iOS. Ha a beállítás egy felhőalapú szolgáltatás (például az Intune), ez az attribútum a lesz a mérvadó a címtárban DeviceOSType. |
| CloudDeviceOSVersion |Az operációs rendszer verzióját. Ha a beállítás egy felhőalapú szolgáltatás (például az Intune), ez az attribútum a lesz a mérvadó a címtárban DeviceOSVersion. |
| CloudDisplayName |A displayName LDAP attribútum értéke. Ha a beállítás egy felhőalapú szolgáltatás (például az Intune), ez az attribútum lesz mérvadó displayName könyvtárában. |
| CloudCreated |Azt jelzi, hogy az objektum felhőszolgáltatások hozta-e. |
| CompliantUntil |Milyen időpontig eszköz megfelelőnek minősül. |
| DeviceMetadata |Az eszköz egyéni metaadatok |
| DeviceObjectVersion |Ez az attribútum a séma verziója, az eszköz azonosítására szolgál. |
| DeviceOSType |Az eszköz típusa alapján az operációs rendszer például Windows RT, iOS. A regisztrációs szolgáltatástól, frissítenie kell a mobileszköz-kezelési készült felügyeleti szolgáltatás vagy az STS világos felügyeleti szolgáltatást. |
| DeviceOSVersion |Az operációs rendszer verzióját. A regisztrációs szolgáltatástól, frissítenie kell a mobileszköz-kezelési készült felügyeleti szolgáltatás vagy az STS világos felügyeleti szolgáltatást. |
| DevicePhysicalIds |Többértékű attribútum célja, hogy a fizikai eszköz azonosítói tárolja. Ebbe beletartozik a BIOS-azonosítókat, TPM-ujjlenyomatok, a hardver egyedi azonosítók stb. |
| DirSyncEnabled |Meghatározza, hogy szinkronizálás egy mérvadó (ügyfél, a helyszínen) directory. |
| displayName |Az objektum megjelenített neve |
| IsCompliant |Ez az attribútum a mobileszköz-kezelési állapota az eszköz kezelésére szolgál. |
| IsManaged |Ezt az attribútumot használja annak jelzésére, hogy az eszköz felügyelete a felhőalapú mobileszköz-kezelést. |
| LastDirSyncTime |Az objektum frissült a a mérvadó szinkronizálása miatt legutóbbi (a helyszínen ügyfél) directory. |

## <a name="update-device-configuration-attributes"></a>"Az eszköz konfigurációs frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| MaximumRegistrationInactivityPeriod |A maximális időtartam napban megadva, az eszköz lehet inaktív, mielőtt azt eltávolítása tekinthető meg. |
| RegistrationQuota |A házirend használható korlátozására megengedett egy felhasználó-eszköz regisztrációinak száma. |

## <a name="update-service-principal-configuration-attributes"></a>"A szolgáltatás egyszerű konfigurációjának frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| AccountEnabled |Azt jelzi, hogy a rendszerbiztonsági tag hitelesítheti. |
| AppPrincipalId |Külső, az alkalmazás által meghatározott identitás rendszerbiztonsági tag. |
| displayName |Az objektum megjelenített neve |
| ServicePrincipalName |Egy egyszerű szolgáltatásnevét, amely tartalmazza a "név/authority" ahol nevét megadja egy alkalmazás osztály értékét, és hatóság tartalmaz legalább állomásnév [: port] vagy a "name" azonosítót, amely meghatározza a szolgáltatás egyszerű. |

## <a name="update-app-attributes"></a>"Az alkalmazás frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| AppAddress |Az egyszerű szolgáltatás rendelt (átirányítási URL-címek) címek készletét. |
| Alkalmazásazonosító |Az alkalmazás azonosítója |
| AppIdentifierUri |Alkalmazás URI, amely azonosítja az alkalmazást.  Akkor általában az alkalmazás URL-CÍMEN. |
| AppLogoUrl |Az alkalmazás embléma-lemezképet tárolja a CDN URL-címe |
| AvailableToOtherTenants |Az alkalmazás lehet TRUE több-bérlős alkalmazás (azaz használhatja más bérlők). |
| displayName |A megjelenítési nevet az alkalmazás nevét |
| Jogosultság |Alkalmazás jogosultságok listáját. |
| ExternalUserAccountDelegationsAllowed |Jelzi, hogy erőforrás alkalmazás egy megbízható, és delegálás bejegyzéseket külső felhasználói fiókokat hozhat létre. |
| GroupMembershipClaims |A csoporttagság jogcímek házirend. |
| PublicClient |Értéke TRUE, ha az ügyfél nem képes lépést tartani titkos (azaz nem bizalmas ügyfél OAuth2.0) |
| RecordConsentConditions |Hozzájárulás állapotától függően a szerződési feltételek által meghatározott típusú: None (0), SilentConsentForPartnerManagedApp(1). Ez az érték a Graph API sémában elérhetővé tehető, és lehet, hogy csak a bérlői rendszergazda által beállított vagy módosult. |
| RequiredResourceAccess |XML-tartalom a RequiredResourceAccess tulajdonság értéke. |
| Webalkalmazás |Amennyiben az értéke igaz, azt jelzi, hogy az alkalmazás egy webalkalmazást. |
| WwwHomepage |Az elsődleges weblap. |

## <a name="update-role-attributes"></a>"Szerepkör módosítása" attribútumok
| Attribútum | Leírás |
| --- | --- |
| AppAddress |Az egyszerű szolgáltatás rendelt (átirányítási URL-címek) címek készletét. |
| BelongsToFirstLoginObjectSet |Amennyiben az értéke igaz, azt jelzi, hogy ez az objektum tartozik-e a szükséges ahhoz, hogy egy új bérlő első rendszergazda bejelentkezési objektumok csoportja. |
| A beépített |Azt jelzi, hogy az objektum élettartama tulajdonában van-e a rendszer. |
| Leírás |Emberek számára olvasható leíró kifejezések arról az objektumról. |
| displayName |Az objektum megjelenített neve |
| mailNickname |Cím könyv objektum kézjegy, általában az előző e-mail név része a "@" szimbólummal. |
| RoleDisabled |Azt jelzi, hogy a szerepkör figyelmen kívül hagyja a hozzáférés-ellenőrzés céljából. |
| RoleTemplateId |A szerepkör sablonok identitása. |
| ServiceInfo |Szolgáltatás-specifikus jogosultságkiosztási információkat, amelyeket esetleg használni MOAC és/vagy más szolgáltatáspéldány (azonos vagy eltérő szolgáltatási típusú). |
| TaskSetScopeReference |Azonosítja a TaskSet és egy szerepkör vagy RoleTemplate társított hatókörök csoportja. |
| ValidationError |A leíró nem átmeneti, szolgáltatással kapcsolatos hiba történt a Tulajdonságok vagy a hivatkozás a megoldásához objektum rendszergazda művelet összevont szolgáltatás által közzétett adatokat. |
| WellKnownObject |A Deleted object, előre meghatározott egyik a kijelölő létrehozott. |

## <a name="update-role-definition-attributes"></a>"Szerepkör-definíció frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| AssignableScopes |A RoleDefinition hozzárendelése egy rendszerbiztonsági tag hivatkozható engedélyezési hatókörök gyűjteménye. |
| displayName |Az objektum megjelenített neve |
| GrantedPermissions |A RoleDefinition engedélyekre. |

## <a name="update-administrative-unit-attributes"></a>"Az adminisztratív egység frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| Leírás |Ez a tulajdonság akkor frissül, amikor módosítja egy felügyeleti egység leírását. |
| displayName |Ez a tulajdonság akkor frissül, amikor módosítja a felügyeleti egység nevét. |

## <a name="update-company-attributes"></a>"Frissítés vállalati" attribútumok
| Attribútum | Leírás |
| --- | --- |
| AllowedDataLocation |Egy helyet, ahol a vállalati felhasználók számára engedélyezett üzembe. |
| AuthorizedServiceInstance |A terv telepítésére szolgáltatáspéldány nevét. |
| DirSyncEnabled |Meghatározza, hogy szinkronizálás egy mérvadó (ügyfél, a helyszínen) directory. |
| DirSyncStatus |Meghatározza, hogy szinkronizálás cím könyv objektumok a bérlő környezetben egy mérvadó (ügyfél, a helyszínen) directory; a vállalati objektumok DirSyncEnabled tulajdonság bővítése. |
| DirSyncFeatures |Bit jelzőt a bérlő számára engedélyezett és letiltott dirsync szolgáltatások készlete nyomon követéséhez. |
| DirectoryFeatures |Engedélyezett vagy letiltott címtárfunkciókat. |
| DirSyncConfiguration |Az aktuális bérlőjéhez tartozik a DirSync konfigurációs tartalmazza. |
| displayName |Az objektum megjelenített neve |
| IsMnc |Egy logikai jelző beállítása "true" iff a nemzetközi vállalati szolgáltatás engedélyezve van a vállalat számára. |
| ObjectSettings |Az objektum hatókörének beállítások gyűjteménye. |
| PartnerCommerceUrl |A Partner kereskedelmi webhely URL-címe. |
| PartnerHelpUrl |A Partner súgó webhely URL-címe. |
| PartnerSupportEmail |A partner által létrehozott támogatási e-mail cím URL-címe. |
| PartnerSupportTelephone |A Partner támogatás telefonon URL-címe. |
| PartnerSupportUrl |A partner által létrehozott támogatási webhely URL-címe. |
| StrongAuthenticationDetails |StrongAuthentication kapcsolatos részleteket. |
| StrongAuthenticationPolicy |A vállalat erős hitelesítési házirendet. |
| TechnicalNotificationMail |Egy vállalat vonatkozó technikai problémák értesítendő e-Mail címet. |
| TelephoneNumber |Az telefonszámokat, amelyek megfelelnek a ITU ajánlás E.123. |
| TenantType |A bérlő típusú. Ha ez az érték nincs megadva, a bérlő az adott vállalat. Ellenkező esetben a lehetséges értékek:: (0) MicrosoftSupport, SyndicatePartner (1), (2) BreadthPartner BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5). |
| VerifiedDomain |DNS-nevek készlete egy vállalat kötve. |

## <a name="update-domain-attributes"></a>"A tartomány frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| Funkciók |A képességek a tartomány leíró jelzőket bit, ha van ilyen. |
| Alapértelmezett |Azt jelzi, hogy a tartomány az alapértelmezett érték; például az alapértelmezett UserPrincipalName utótag amikor a rendszergazda létrehoz egy új felhasználót a MOAC. |
| Kezdeti |Azt jelzi, hogy a tartomány a vállalat számára a kezdeti tartomány OCP által. A kezdeti tartomány pedig a Microsoft Online tartomány; egyedi altartomány e.g.contoso3.microsoftonline.com. |
| LiveType |A megfelelő Windows Live névtér, ha van ilyen típusú. |
| Név |A végpont azonosítója. |
| PasswordNotificationWindowDays |A hány nap elteltével jelszó lejár, a felhasználó értesítést kap. |
| PasswordValidityPeriodDays |Meg kell változtatni a jelszót kiválóan alkalmas előtt eltelt napok száma. |

Auditálási rekordok a szükséges vezérlőt a sok megfelelőségi szabályzat. Felhasználói számára az Azure Active Directory naplózási jelentés a megfelelőségi szabályzat teljesítéséhez javasoljuk, hogy az ügyfél nyújtaniuk ebben a témakörben az ügyfél exportált naplózási jelentés a részletekről magyarázatának elősegítésére példányát. Az auditor szeretné tudni, hogy a megfelelőségi szabályzat Azure jelenleg megfelelő, ha közvetlenül az auditor az [megfelelőségi oldalának](https://azure.microsoft.com/support/trust-center/compliance/) a Microsoft Azure Trust Center.

