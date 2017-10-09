### <a name="prerequisites"></a>Előfeltételek
* Egy [FTP](https://wikipedia.org/wiki/File_Transfer_Protocol) fiók  

Az FTP-fiók a logikai alkalmazás használata előtt engedélyeznie kell a hello logic app tooconnect tooyour az FTP-fiók. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon.  

Az alábbiakban hello lépéseket tooauthorize logic app tooconnect tooyour FTP fiókja:  

1. toocreate egy kapcsolat tooFTP hello logic app Designer kiválasztása **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, majd adja meg *FTP* hello Keresés mezőbe. Hello eseményindító vagy lesz, például a toouse művelet kiválasztása:  
   ![FTP-kapcsolat létrehozása lépés](./media/connectors-create-api-ftp/ftp-1.png)  
2. Bármely kapcsolatok tooFTP előtt még nem hozott létre, ha később is lesz felszólító tooprovide FTP hitelesítő adatait. Ezeket a hitelesítő adatokat kell használt tooauthorize a logic app tooconnect számára, és az FTP-fiók adatok eléréséhez:  
   ![FTP-kapcsolat létrehozása lépés](./media/connectors-create-api-ftp/ftp-2.png)  
3. Figyelje meg hello kapcsolat létrejött, és most a Logic Apps alkalmazást az egyéb hello szabad tooproceed szükséges lépések:  
   ![FTP-kapcsolat létrehozása lépés](./media/connectors-create-api-ftp/ftp-3.png)  

