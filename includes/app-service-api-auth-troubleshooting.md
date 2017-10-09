Hello idő hitelesítési hibák többségét eredménye nem megfelelő vagy nem következetes konfigurációs beállításokat. Az alábbiakban néhány dolgot toocheck adott javaslatokat.

* Győződjön meg arról, hogy teljesíti-e nem hello **mentése** gombra kattint, bármely részén. Ez gyakran könnyen toodo, és hello eredménye, hogy akkor lesz megtekintik hello helyes az értékük portál oldalon, de ténylegesen ezek még nincsenek mentve a hello Azure környezetben vagy az Azure AD-alkalmazást.
* Hello konfigurált beállítások **Alkalmazásbeállítások** panelen található hello Azure-portálon győződjön meg arról, hogy hello megfelelő API-alkalmazás vagy a web app kiválasztott hello-beállítások vannak megadva.  Emellett győződjön meg arról, hogy a hello-beállítások vannak megadva **Alkalmazásbeállítások** és nem **kapcsolati karakterláncok**, mert a hello hello két szakaszok formátuma hasonló.
* A hitelesítési tooa JavaScript előtér, letöltési hello manifest újra tooverify, amely `oauth2AllowImplicitFlow` sikeresen módosítva lett túl`true`.
* Ellenőrizze, hogy HTTPS bárhol konfigurált URL-címek:
  
  * A projekt kódját
  * A CORS
  * Az alkalmazásbeállítások Azure-alapú környezetben az egyes API-alkalmazás és a webes alkalmazás
  * Az Azure AD alkalmazás-beállításokat.
    
    Vegye figyelembe, hogy ha egy API-alkalmazás URL-cím hello portálról másolja, ez gyakran `http://` és toomanually túl módosítsa úgy, hogy`https://`.
* Ellenőrizze, hogy a kód módosításokat sikeres volt-e telepítve. Például az a többszörös-projekt megoldás is lehetséges toochange projekt tartozó kód és véletlenül válasszon egyet a hello mások Ha azt tervezi, hogy toodeploy hello módosítása.
* Győződjön meg arról, hogy a böngészőben, nem HTTP URL-címek tooHTTPS URL-címeket is. Alapértelmezés szerint a Visual Studio létrehozza a HTTP URL-címek profilok közzététele, és mi megnyitja hello böngészőben, a projekt telepítése után.
* Hitelesítési tooa JavaScript előtér győződjön meg arról, hogy a CORS megfelelően van-e beállítva a hello API-alkalmazást, amely hello JavaScript-kód hívásokat. Ha a arról, hogy hello probléma CORS kapcsolatos kétségei vannak, próbálja ki a "*" hello engedélyezett forrás URL-cím szerint. 
* A JavaScript előtér nyissa meg a böngésző fejlesztői eszközök konzol lapon tooget további hibainformációk, és vizsgálja meg a hálózati hello HTTP-kérelmekre. Előfordulhat azonban, konzol hibaüzenetek félrevezető. A CORS hibát jelző üzenet jelenik meg, ha hello valós oka valószínűleg a hitelesítést. Ha ez helyzet hello ideiglenesen átmenetileg le van tiltva hitelesítéssel hello alkalmazást futtatva ellenőrizheti.
* .NET API-alkalmazások esetén ellenőrizze, hogy kap, a rendszer mennyi információt látható a hibaüzenetekben a lehető beállításával [customErrors módot tooOff](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* A .NET API-alkalmazást, indítsa el a [távoli hibakeresési munkamenetben](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), és vizsgálja meg, amely használja az ADAL tooacquire toocode átadott hello-változók értékeit hello egy tulajdonosi jogkivonatot, vagy kódot, amely ellenőrzi, szemben a várt hello szolgáltatás egyszerű azonosítóját. Vegye figyelembe, hogy a kód ki tudja választani a konfigurációs értékeket számos különböző forrásokból, így lehetséges toofind meglepetések számát is. Például, ha írunk `ida:ClientId` , `ida:ClientID` Azure App Service-környezet beállításainak konfigurálásakor hello kód juthat, hello `ida:ClientId` érték, amely a hello Web.config fájlból, figyelmen kívül hagyja a hello Azure App Service beállítás keresi. 
* Ha egy normál Internet Explorer-ablakban probléma, akkor egy már létező napló a zavart; InPrivate próbálja, majd ismételje meg a Chrome, Firefox vagy.

