A szervezetek használ több [(SaaS) szolgáltatott szoftver](https://azure.microsoft.com/overview/what-is-saas/) termelékenység alkalmazások mert felhőtechnológia és eszközök egyre jobban elérhető. SaaS-alkalmazások hello számának növekedésével válik az hello rendszergazdák toomanage fiókok és hozzáférési jogokat, és a hello felhasználók tooremember különböző jelszavukat. Ezeket az alkalmazásokat kezeléséhez külön-külön extra munkahelyi hoz létre, és kevésbé biztonságos.

* Alkalmazottak, akik rendelkeznek toouse általában sok jelszót nyomon tookeep kevésbé biztonságos módszerek tooremember őket, vagy a jelszavak le írása, vagy használatával hello ugyanazt a jelszót több fiókon keresztül.
* Amikor új alkalmazott megérkeznek, vagy hagyja el egy, a fiókba kell külön-külön kiépített vagy rendszer leépíti.
* Emellett az alkalmazottak kezdheti SaaS-alkalmazások használatát a munkahelyi áthaladás nélkül informatikai, ami azt jelenti, hoznak létre, amely a rendszergazdák hello rendszereken a saját felhasználói nem hagyott jóvá és nem figyelését.  

A megoldás összes ezekkel a kihívásokkal az egyszeri bejelentkezés (SSO). Rendelkezik hello legegyszerűbb módja toomanage több alkalmazást, és egységes bejelentkezés élményt biztosít a felhasználók. Azure Active Directory (Azure AD) robusztus SSO megoldást nyújt, és van elérhető számos előre integrált alkalmazás, oktatóanyagok a rendszergazdák számára a tooquickly állítson be egy új alkalmazást, és indítsa el a kiépítési felhasználók.

## <a name="how-does-azure-active-directory-integrate-apps"></a>Hogyan Azure Active Directory alkalmazások integrálása?
Azure AD lehetővé teszi, hogy toointegrate az alkalmazások és a kiépített fiókok. Ezt rábízhatja a két módszer egyikét.

* Ha hello app előre integrált hello alkalmazásban gyűjteménye, nyissa meg a portál tooset fel alkalmazásokat, és hello beállítások tooallow egyszeri bejelentkezés konfigurálása. Minden gyűjtemény alkalmazás kezdheti kövesse hello egyszerű lépésenként bemutatott hello gyűjtemény és az Azure portál tooenable hello egyszeri bejelentkezés által.
* Ha hello alkalmazás nincs hello gyűjteménye, továbbra is állíthatja be legtöbb alkalmazást az Azure AD egyéni alkalmazásként. Ehhez egy kicsit nagyobb technikai szakértelmet tooconfigure. Bármely alkalmazás, amely támogatja az SAML 2.0, mint egy összevont alkalmazás vagy a bármely alkalmazás, amely rendelkezik egy HTML-alapú bejelentkezési oldal jelszó SSO alkalmazásként is hozzáadhat.

A hello esetet, ahol felhasználók hozott létre a saját fiókját, SaaS-alkalmazásokhoz, amelyek nem felügyel informatikai hello [a Cloud App Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) eszköz megoldást kínál. Ez az eszköz hello webes forgalom tooidentify mely alkalmazások vannak használatban hello szervezet és a hozzájuk használó felhasználók száma hello figyeli. Informatikai ezen információk toolearn milyen alkalmazások hello felhasználók által előnyben részesített, és döntse el, melyik toointegrate az Azure AD-be az egyszeri bejelentkezés használhatja.  

Alkalmazások integrálása az Azure AD-be, amikor leképezheti hello felhasználók létrehozott alkalmazás identitások tootheir saját Azure AD identitását.  

