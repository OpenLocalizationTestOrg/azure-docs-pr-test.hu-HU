Az Azure-ügyfelek havonta 25 000 ingyenes e-mailt oldhatnak fel. Ezek 25 000 ingyenes havi e-maileket kap hozzáférést tooadvanced jelentéskészítés és elemzés és [minden API] [ all APIs] (Web, SMTP, esemény, elemzési és több). Kiegészítő szolgáltatásokat nyújtja SendGrid kapcsolatos információkért látogasson el a hello [SendGrid megoldások] [ SendGrid Solutions] lap.

### <a name="toosign-up-for-a-sendgrid-account"></a>a SendGrid-fiókot toosign
1. Jelentkezzen be toohello [Azure felügyeleti portálon][Azure Management Portal].
2. Hello hello bal oldali menüben kattintson a **új**.

    ![command-bar-new][command-bar-new]
3. Kattintson a **Bővítmények**, majd pedig a **SendGrid Email Delivery** lehetőségre.

    ![sendgrid-store][sendgrid-store]
4. Hello bejelentkezési űrlap kitöltése és kiválasztása **létrehozása**.

    ![sendgrid-create][sendgrid-create]
5. Adjon meg egy **neve** tooidentify a SendGrid szolgáltatás az Azure beállításaiban. A neveknek 1–100 karakter hosszúságúnak kell lenniük, és kizárólag alfanumerikus karaktereket, kötőjeleket, pontokat és aláhúzásjeleket tartalmazhatnak. hello nevének elemre az Azure-tároló előfizetett elemek egyedinek kell lennie.
6. Adja meg és erősítse meg a **Jelszót**.
7. Válassza ki az **Előfizetést**.
8. Hozzon létre egy új **Erőforráscsoportot** vagy használjon egy meglévőt.
9. A hello **tarifacsomag** csoportban jelölje be a toosign kívánt hello SendGrid terv.

    ![sendgrid-pricing][sendgrid-pricing]
10. Adjon meg egy **Promóciókódot**, ha rendelkezik ilyennel.
11. Adja meg a **Kapcsolattartási adatait**.
12. Tekintse át és fogadja el a hello **jogi feltételeket**.
13. A vásárlás megerősítése után megjelenik egy **sikeres telepítés** előugró ablak, és megjelenik a fiókját a hello **összes erőforrás** szakasz.

    ![all-resources][all-resources]

    Miután befejezte a vásárlást, és kattint hello **kezelése** gomb tooinitiate hello e-mail ellenőrzési folyamata, kapni fog egy e-mailt a SendGrid tooverify kéri fel a fiókot. Ha nem kapja meg ezt az e-mailt, vagy problémába ütközik a fiókja megerősítésével kapcsolatban, tekintse meg a Gyakori kérdéseket.

    ![kezelés][manage]

    **Csak küldhet e-mailek/nap/too100 mentése amíg nem ellenőrizte a fiókját.**

    toomodify az előfizetési csomagot vagy lásd: hello SendGrid kapcsolattartási beállításai, kattintson a SendGrid szolgáltatás tooopen hello SendGrid piactér irányítópult hello nevére.

    ![beállítások][settings]

    az e-mailek SendGrid toosend, meg kell adnia az API-kulcs.

### <a name="toofind-your-sendgrid-api-key"></a>toofind a SendGrid API-kulcs
1. Kattintson a **Kezelés** gombra.

    ![kezelés][manage]
2. A SendGrid-irányítópulton, válassza ki a **beállítások** , majd **API-kulcsokat** hello bal oldali hello menüben.

    ![api-keys][api-keys]

3. Hello kattintson **API-kulcs létrehozása** legördülő menüből válassza ki **általános API-kulcs**.

    ![general-api-key][general-api-key]
4. Minimális, adja meg a hello **nevet ennek a kulcsnak** , és adja meg a teljes körű hozzáférési túl**E-mail küldése** válassza **mentése**.

    ![hozzáférés][access]
5. Ekkor az API egyetlen alkalommal megjelenik. Felhívjuk meg arról, hogy toostore azt biztonságosan.

### <a name="toofind-your-sendgrid-credentials"></a>toofind a Sendgridbeli hitelesítő adataival
1. Kattintson a kulcs ikonra toofind hello a **felhasználónév**.

    ![kulcs][key]
2. hello jelszava hello egyet a telepítéskor választott. Kiválaszthatja **jelszó módosítása** vagy **jelszó-átállítási** toomake módosításokat.

toomanage az e-mailek deliverability beállításokat, kattintson a hello **Manage gomb**. A program átirányítja tooyour SendGrid irányítópult.

    ![manage][manage]

    For more information on sending email through SendGrid, visit hello [Email API Overview][Email API Overview].

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/new-addon.png
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid-store.png
[sendgrid-create]: ./media/sendgrid-sign-up/sendgrid-create.png
[sendgrid-pricing]: ./media/sendgrid-sign-up/sendgrid-pricing.png
[all-resources]: ./media/sendgrid-sign-up/all-resources.png
[manage]: ./media/sendgrid-sign-up/manage.png
[settings]: ./media/sendgrid-sign-up/settings.png
[api-keys]: ./media/sendgrid-sign-up/api-keys.png
[general-api-key]: ./media/sendgrid-sign-up/general-api-key.png
[access]: ./media/sendgrid-sign-up/access.png
[key]: ./media/sendgrid-sign-up/key.png

<!--Links-->

[SendGrid Solutions]: https://sendgrid.com/solutions
[Azure Management Portal]: https://manage.windowsazure.com
[SendGrid Getting Started]: http://sendgrid.com/docs
[SendGrid Provisioning Process]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[all APIs]: https://sendgrid.com/docs/API_Reference/index.html
[Email API Overview]: https://sendgrid.com/docs/API_Reference/Web_API_v3/Mail/index.html
