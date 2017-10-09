### <a name="grant-access-tooyour-push-certificate-toomobile-engagement"></a>Adjon hozzáférést tooyour leküldéses tanúsítványhoz tooMobile Engagement
tooallow a Mobile Engagement toosend leküldéses értesítések küldése az Ön nevében, meg kell azt elérni tooyour tanúsítvány toogrant. Ehhez írja be a tanúsítvány hello a Mobile Engagement portált és konfigurálása. Győződjön meg róla, hogy az [Apple dokumentációjában](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6) leírtak alapján szerezte be .p12-tanúsítványát.

1. Keresse meg a tooyour Mobile Engagement portálon. Győződjön meg arról is a megfelelő hello, és kattintson a hello **Engage** hello alsó gombra:
   
    ![](./media/mobile-engagement-ios-send-push/engage-button.png)
2. Kattintson a hello **beállítások** lapra az Engagement portálon. Kattintson a hello **natív leküldés** szakasz tooupload a p12-tanúsítvány:
   
    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)
3. Válassza ki a p12-tanúsítványát, töltse fel, majd írja be a jelszavát:
   
    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

## <a id="send"></a>Egy értesítési tooyour app küldése
Most létrehozunk egy egyszerű leküldéses értesítési kampányt, amely elküld egy leküldéses tooour alkalmazást:

1. Keresse meg a toohello **elérni** a Mobile Engagement portálra a lap.
2. Kattintson a **új hirdetmény** toocreate a leküldéses kampány
   
    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)
3. A telepítő a kampány első mezőinek hello:
   
    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)
   
   * A **Name** (Név) mezőben adja meg a kampány nevét. 
   * Jelölje be hello **kézbesítési időhöz** , **csak az alkalmazáson kívül**: Ez az hello egyszerű Apple leküldéses értesítési típus, amely egy kevés szöveget.
   * Hello értesítés szövegéhez, írja be az első hello **cím** amely lesz hello hello leküldéses első sorában.
   * Írja be a **üzenet** amely lesz hello második sor
4. Görgessen lefelé, és a hello tartalomszakasz válassza **csak értesítés**
   
    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)
5. Elkészült a beállítással hello lehető legegyszerűbb kampányt. Görgessen lefelé, és kattintson a **létrehozása** toosave gombra a leküldéses értesítési kampány. 
6. Végezetül kattintson a **aktiválás** toosend leküldéses értesítést. 
   
    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)
7. Akkor lehet értesítés hello iOS-eszközön a hello következő hello értesítési központjában:
   
    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)
8. Ha egy az iOS-eszközzel párosított Apple Watch majd jelenik meg hello értesítési meg az Apple Watch:
   
    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)

