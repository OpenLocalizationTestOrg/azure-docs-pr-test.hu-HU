### <a name="prerequisites"></a>Előfeltételek
* A [Dropbox](https://www.Dropbox.com/) fiók 

A Dropbox-fiók a logikai alkalmazás használata előtt engedélyeznie kell a hello Logic app tooconnect tooyour Dropbox-fiók. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon. 

Az alábbiakban hello lépéseket tooauthorize a Logic app tooconnect tooyour Dropbox-fiók:

1. toocreate egy kapcsolat tooDropbox hello Logic app Designer kiválasztása **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, majd adja meg *Dropbox* hello Keresés mezőbe. Hello eseményindító vagy lesz, például a toouse művelet kiválasztása:  
   ![Dropbox 1. lépés](./media/connectors-create-api-dropbox/dropbox-1.png)
2. Bármely kapcsolatok tooDropbox előtt még nem hozott létre, ha később is lesz felszólító tooprovide a Dropbox hitelesítő adatait. Ezeket a hitelesítő adatokat kell használt tooauthorize a Logic app tooconnect számára, és a Dropbox-fiók adatok eléréséhez:  
   ![Dropbox 2. lépés](./media/connectors-create-api-dropbox/dropbox-2.png)
3. Adja meg a Dropbox felhasználói nevet és jelszót tooauthorize a Logic Apps alkalmazást:  
   ![3. lépés dropbox-bA](./media/connectors-create-api-dropbox/dropbox-3.png)   
4. Hello Logic app toouse a Dropbox-fiók engedélyezése:  
   ![4. lépés dropbox-bA](./media/connectors-create-api-dropbox/dropbox-4.png)
5. Figyelje meg hello kapcsolat létrejött, és most a Logic Apps alkalmazást az egyéb hello szabad tooproceed szükséges lépések:  
   ![5. lépés dropbox-bA](./media/connectors-create-api-dropbox/dropbox-5.png)   

