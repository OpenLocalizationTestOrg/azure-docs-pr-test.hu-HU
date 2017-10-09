### <a name="prerequisites"></a>Előfeltételek
* Egy [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol) fiók  

SFTP-fiókját a logikai alkalmazás használata előtt engedélyeznie kell a hello logic app tooconnect tooyour SFTP fiók. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon.  

Az alábbiakban hello lépéseket tooauthorize logic app tooconnect tooyour SFTP fiókja:  

1. toocreate egy kapcsolat tooSFTP hello logic app Designer kiválasztása **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, majd adja meg *SFTP* hello Keresés mezőbe. Jelölje be hello **SFTP - amikor egy fájl hozzáadása vagy módosítása** eseményindító:  
   ![Kép: SFTP online kapcsolat 1](./media/connectors-create-api-sftp/sftp-1.png)  
2. A kapcsolatok tooSFTP előtt még nem hozott létre, ha később is lesz rákérdezéses tooprovide SFTP hitelesítő adatait. Ezeket a hitelesítő adatokat kell használt tooauthorize a logic app tooconnect való, és hozzáférési SFTP fiókja adatokat:  
   ![Kép: SFTP online kapcsolat 2](./media/connectors-create-api-sftp/sftp-2.png)  
3. Figyelje meg hello kapcsolat létrejött, és most a Logic Apps alkalmazást az egyéb hello szabad tooproceed szükséges lépések:   
   ![Kép: a 3 SFTP online kapcsolat](./media/connectors-create-api-sftp/sftp-3.png) 

