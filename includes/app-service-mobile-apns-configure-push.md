

1. Indítsa el a Mac gépen **Keychain Access**. Hello a bal oldali navigációs sávon a **kategória**, nyissa meg **saját tanúsítványok**. Hello előző szakaszban letöltött hello SSL-tanúsítvány található, és fedjük fel annak tartalmát. Válassza ki a tanúsítvány csak hello (ne válassza hello titkos kulcs), és [exportálni kell](https://support.apple.com/kb/PH20122?locale=en_US).
2. A hello [Azure-portálon](https://portal.azure.com/), kattintson a **összes tallózása** > **alkalmazásszolgáltatások**, és kattintson a Mobile Apps háttér. A **beállítások**, kattintson a **App Service leküldéses**, majd kattintson az értesítési központ nevére. Nyissa meg túl**Apple leküldéses értesítéseket kezelő szolgáltatása** > **tanúsítvány feltöltése**. Töltse fel hello .p12 fájlt, megfelelő hello kiválasztása **mód** (attól függően, hogy az SSL-ügyféltanúsítvány a korábban az éles vagy védőfal). A módosítások mentéséhez.

A szolgáltatás már IOS leküldéses értesítések konfigurált toowork.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
