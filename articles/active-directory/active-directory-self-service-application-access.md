---
title: "aaaSelf-service alkalmazás-hozzáférés és a delegált felügyelet az Azure Active Directoryval |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan férnek hozzá az tooenable önkiszolgáló alkalmazás és a delegált felügyelet az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 448a7fe8-a162-475e-9ba2-2e3ab59302bc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: asmalser
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 90bec3bd71796f22a782929b028db0d18c3aa1c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="self-service-application-access-and-delegated-management-with-azure-active-directory"></a>Önkiszolgáló alkalmazás-hozzáférés és a delegált felügyelet az Azure Active Directoryval
Egy általános forgatókönyv a vállalati informatikai önkiszolgálói képességeit engedélyezése a végfelhasználók számára. Felhasználók, az alkalmazások és hello személy, aki best-informed toomake hozzáférés nagy mennyiségű biztosítani döntések nem lehet hello directory rendszergazdája. Gyakran hello legjobb személy toodecide ki férhet hozzá a kérelmet, de egy más meghatalmazott rendszergazda. Azonban hello felhasználó, aki hello alkalmazást használja, és hello felhasználó tudja, hogy mit toobe képes toodo munkájukhoz van szükségük.

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. 

Önkiszolgáló alkalmazás-hozzáférés csak a [Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/) P1 és P2 licencelési, amelyek lehetővé teszik a címtárban:

* Használja a "Get további alkalmazások" tooapplications csempére hello felhasználók toorequest hozzáférés engedélyezése [Azure AD hozzáférési Panel](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)
* Mely alkalmazások használói kérhetnek való hozzáférés beállítása
* Függetlenül attól, jóváhagyásának szükség a felhasználók toobe képes tooself hozzárendelése access tooan alkalmazás beállítása
* Állítsa ki kell hello kérelmek jóváhagyása és minden egyes alkalmazás-hozzáférés kezelése

Ez a funkció minden előzetesen beépített és egyéni alkalmazás, amely támogatja az összevont vagy a jelszó-alapú egyszeri bejelentkezést hello támogatott Ma [Azure Active Directory alkalmazáskatalógusában](https://azure.microsoft.com/marketplace/active-directory/all/), beleértve az alkalmazások, például a Salesforce, Dropbox, Google Apps, és több.
Ez a cikk ismerteti, hogyan:

* A végfelhasználók számára, beleértve a nem kötelező jóváhagyási munkafolyamat konfigurálása az önkiszolgáló alkalmazás-hozzáférés konfigurálása 
* Bizonyos alkalmazások toohello legmegfelelőbb a szervezet dolgozói, hozzáférés-kezelés delegálása és toouse hello Azure AD hozzáférési panel tooapprove hozzáférési kérelmek lehetővé teszi, közvetlenül tooselected felhasználók az hozzáférés hozzárendelése vagy (opcionális) beállítása Ha jelszóalapú egyszeri bejelentkezésre van beállítva alkalmazás-hozzáférési hitelesítő adatok

## <a name="configuring-self-service-application-access"></a>Önkiszolgáló alkalmazás-hozzáférés konfigurálása
tooenable önkiszolgáló alkalmazás-hozzáférés és mely alkalmazások lehet hozzáadni, vagy a végfelhasználók által kért kövesse ezeket az utasításokat.

1. Jelentkezzen be a hello [a klasszikus Azure portálon](https://manage.windowsazure.com/).

2.   A hello **Active Directory** területen válassza ki azt a címtárat, majd válassza ki a hello **alkalmazások** fülre. 

3. Jelölje be hello **Hozzáadás** gombra és hello gyűjtemény beállítás tooselect használja, és hozzáadhat egy alkalmazást.

4. Az alkalmazás hozzáadása után fog kapni olyan hello alkalmazás gyors kezdés lapon. Kattintson a **konfigurálása egyszeri bejelentkezéshez**hello kívánt egyszeri bejelentkezés mód kiválasztása és hello konfigurációjának mentéséhez. 

5. Ezután válassza ki a hello **konfigurálása** külön-külön tooenable felhasználók toorequest hozzáférés toothis alkalmazásából hello Azure AD hozzáférési panel, állítsa be **önkiszolgáló alkalmazás-hozzáférés engedélyezése** túl**Igen**.
  
  ![][1]

6. toooptionally konfigurálhatja a hozzáférési kérelem jóváhagyási munkafolyamat beállítása **hozzáférés biztosítása előtt jóvá kell hagyni** túl**Igen**. Ezután egy vagy több jóváhagyó hello kiválasztható **jóváhagyóknak** gombra.

  Jóváhagyó lehet bármely felhasználó hello szervezet az Azure AD-fiókkal, és lehet felelős fiók kiépítése, licencelési, vagy bármely más üzleti folyamat a szervezet megköveteli tooan app hozzáférés megadása előtt. hello jóváhagyó hello csoporttulajdonos egy vagy több megosztott csoportok, és rendelhet hozzá hello felhasználók tooone ezen csoportok toogive őket érhetnek el egy megosztott fiókkal, még akkor is lehet. 

  Ha nincsenek jóváhagyásra szükség, majd azonnal felhasználónál hello alkalmazáshoz hozzáadott tootheir az Azure AD hozzáférési panel. Ha hello alkalmazás beállítva fel az [automatikus felhasználólétesítés](active-directory-saas-app-provisioning.md), vagy be van állítva ["felhasználó által kezelt" jelszó SSO mód](active-directory-appssoaccess-whatis.md#password-based-single-sign-on), hello felhasználó már rendelkezik egy felhasználói fiókot, és hello jelszó ismerete.

7. Ha az alkalmazás hello konfigurált toouse jelszó-alapú egyszeri bejelentkezést, majd a beállítás a hello jóváhagyó engedélyezése minden felhasználó nevében tooset hello egyszeri bejelentkezési hitelesítő adatok érhető el. További információkért lásd: hello szakasz a [kezelési meghatalmazott](#delegated-application-access-management).

8. Végezetül hello **a Self-Assigned felhasználók** mutat be hello hello csoport, amely használt toostore hello felhasználók nyújtott vagy hozzárendelt hozzáférés toohello alkalmazás nevét. hello hozzáférés jóváhagyó csoport hello tulajdonosa lesz. Ha látható hello csoportnév nem létezik, akkor automatikusan létrejön. Opcionálisan hello csoportnév állíthat be egy meglévő csoport toohello neve.

9. toosave hello konfigurációs, kattintson a **mentése** üdvözlő képernyőt hello alján. A felhasználók most már toorequest hozzáférés toothis alkalmazásából hello hozzáférési panel el.

10. tootry hello végfelhasználói élményt, jelentkezzen be a szervezet az Azure AD hozzáférési panelre a https://myapps.microsoft.com, lehetőség szerint másik fiókkal, amely nem az alkalmazás jóváhagyó. 

11. A hello **alkalmazások** lapra, majd hello **további alkalmazások első** csempére. Ez a csempe hello alkalmazások, amelyeken engedélyezve van hello a könyvtárban, hello képességét toosearch és szűrő a app kategória hello bal oldali önkiszolgáló alkalmazások elérésére vonatkozó összes gyűjteményt jeleníti meg. 

12. Hello lekérő folyamat elindít egy kattint. Ha nincs jóváhagyási folyamat szükség, akkor hello alkalmazás azonnal megjelenik a hello **alkalmazások** után egy rövid megerősítő lapon. Ha jóváhagyásra szükség, majd megjelenik egy párbeszédpanel, ezzel jelezve, és egy e-mailt küld toohello jóváhagyóknak. Be kell jelentkeznie a hello hozzáférési panel egy nem jóváhagyó toosee, a folyamat.

13. hello e-mail hello jóváhagyó toosign vezeti be hello Azure AD hozzáférési panel és hello kérés jóváhagyása. Miután hello kérelem jóváhagyása (és adhat meg semmilyen különleges folyamatot hello jóváhagyó által végrehajtott), hello felhasználónál hello alkalmazás csoportban jelennek meg azok **alkalmazások** lap, ahol bejelentkeznének bele.

## <a name="delegated-application-access-management"></a>Delegált alkalmazáshozzáférés-kezeléshez
Az alkalmazás hozzáférési jóváhagyó hello leginkább megfelelő személy tooapprove, aki a szervezet bármely felhasználója vagy megtagadja a hozzáférést toohello alkalmazást a szóban forgó. A felhasználói fiók kiépítése, felelős licencelési, vagy más üzleti folyamat a szervezet megköveteli tooan app hozzáférés megadása előtt.

A fent leírt önkiszolgáló alkalmazás-hozzáférés konfigurálásakor bármely alkalmazás hozzárendelése jóváhagyóknak lásd további **alkalmazások kezelése** csempe az hello Azure AD hozzáférési panel, amely azt is, hogy mely alkalmazások hello szolgáltatás rendszergazdája számára. Az alkalmazásra kattintva megnyílik számos lehetőség közül választhat a képernyőn látható.

![][2]

### <a name="approve-requests"></a>Kérelmek jóváhagyása
Hello **kérelmek jóváhagyása** csempe lehetővé teszi, hogy a jóváhagyóknak toosee minden függőben lévő jóváhagyások adott toothat alkalmazást, és átirányítja a toohello jóváhagyások lapon ahol hello kérelmek erősíthető vagy tagadható meg. hello jóváhagyó automatikus e-mailek is kap, amikor kérést hoz létre, amely milyen toodo utasítja őket.

### <a name="add-users"></a>Felhasználók hozzáadása
Hello **felhasználó hozzáadása** csempe lehetővé teszi, hogy jóváhagyóknak toodirectly grant kijelölt felhasználók access toohello alkalmazást. Ez a csempe kattint, hello jóváhagyó látja, egy párbeszédpanelen lehetővé teszi, hogy azok tooview, majd keresse meg a felhasználók a könyvtárban. Egy felhasználó eredmények hozzáadása az adott felhasználó az Azure AD hozzáférési panelek vagy az Office 365 látható hello alkalmazásban. Ha bármely manuális felhasználói létesítésének folyamatát kell használnia kell, mielőtt hello felhasználó hello alkalmazás képes toosign a, akkor hello jóváhagyó végre kell hajtania ezt a folyamatot hozzáférés engedélyezése előtt.  

### <a name="manage-users"></a>Felhasználók kezelése
Hello **felhasználók kezelése** csempe lehetővé teszi a jóváhagyóknak toodirectly frissítés, vagy távolítsa el, mely felhasználók férhessenek toohello alkalmazás. 

### <a name="configure-password-sso-credentials-if-applicable"></a>Jelszó egyszeri bejelentkezési hitelesítő adatok beállítása (ha van ilyen)
Hello **konfigurálása** csempe csak akkor jelenik meg, ha hello alkalmazás informatikai rendszergazda toouse jelszó-alapú egyszeri bejelentkezést hello lett konfigurálva, és hogy a hello rendszergazda adott hello jóváhagyó hello képességét tooset jelszó egyszeri bejelentkezési hitelesítő adatok korábban leírt. Kiválasztásakor hello jóváhagyó találkozik számos lehetőség közül választhat a hogyan hello hitelesítő adatok propagált tooassigned felhasználók:

![][3]

* **Felhasználók jelentkezzen be a saját jelszavukat** – ebben a módban hello hozzárendelt felhasználók tudják, mit a felhasználónevek és jelszavak hello alkalmazáshoz, és rákérdezéses tooenter első bejelentkezés toohello jogosította őket. hello forgatókönyv megfelel-e toohello jelszó SSO esetben ha hello [felhasználók kezelhetik a hitelesítő adatok](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Felhasználók automatikusan bejelentkeztetjük külön fiókokkal, amelyek kezelhető** – ebben a módban hello hozzárendelt felhasználók nem szükséges tooenter és alkalmazás-specifikus hitelesítő adataikkal tudja hello alkalmazásba aláírásakor. Ehelyett hello jóváhagyó hello hitelesítő adatok beállítása az egyes felhasználók hello hozzáférés hozzárendelése után **felhasználó hozzáadása** csempére. A hozzáférési panel vagy az Office 365 hello alkalmazás hello felhasználó kattint, automatikusan aláírt hello jóváhagyó által beállított hello hitelesítő adataival. hello forgatókönyv megfelel-e toohello jelszó SSO esetben ha hello [rendszergazdák hitelesítő adatok kezelése](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Felhasználók egyetlen fiókkal, amely a kezelhető automatikusan bejelentkeztetjük** -egy különleges esetben, ebben az esetben megfelelő toouse Ha az összes hozzárendelt felhasználók toobe hozzáférést egyetlen megosztott fiókkal. Ez a szolgáltatás hello leggyakoribb használati eset közösségi alkalmazásokkal, ahol a szervezet egyetlen "Vállalati" fiókkal rendelkezik, és több felhasználó toomake frissítések toothat fiókra van szükség van. hello forgatókönyv szintén megfelelő toohello jelszó SSO esetben ha hello [rendszergazdák hitelesítő adatok kezelése](active-directory-appssoaccess-whatis.md#password-based-single-sign-on). Ez a beállítás kiválasztása után azonban hello jóváhagyó lesz felszólító tooenter hello felhasználónév és jelszó hello egyetlen megosztott fiók. Ezt követően az összes hozzárendelt felhasználó jelentkezik be ezt a fiókot használja, az Office 365 vagy az Azure AD hozzáférési panel hello alkalmazásra kattintva.

## <a name="additional-resources"></a>További források
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)

<!--Image references-->
[1]: ./media/active-directory-self-service-application-access/ssaa_admin.PNG
[2]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app.PNG
[3]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app_config.PNG
