### <a name="prerequisites"></a>Előfeltételek
* A [ProjectOnline](https://products.office.com/Project/project-online-with-project-for-office-365) fiók 

A ProjectOnline fiókját a logikai alkalmazás használata előtt engedélyeznie kell a hello Logic app tooconnect tooyour ProjectOnline fiók. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon. 

Az alábbiakban hello lépéseket tooauthorize a Logic app tooconnect tooyour ProjectOnline fiók:

1. toocreate egy kapcsolat tooProjectOnline hello Logic app Designer kiválasztása **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, majd adja meg *ProjectOnline* hello Keresés mezőbe. Hello eseményindító vagy lesz, például a toouse művelet kiválasztása:  
   ![1. lépés ProjectOnline](./media/connectors-create-api-projectonline/projectonline-1.png)
2. Ha bármely kapcsolatok tooProjectOnline előtt még nem hozott létre, később is lesz felszólító tooprovide ProjectOnline hitelesítő adatait. Ezeket a hitelesítő adatokat kell használt tooauthorize a Logic app tooconnect való, és hozzáférési ProjectOnline fiókja adatokat:  
   ![2. lépés ProjectOnline](./media/connectors-create-api-projectonline/projectonline-2.png)
3. Adja meg a ProjectOnline felhasználói nevet és jelszót tooauthorize a Logic Apps alkalmazást:  
   ![3. lépés ProjectOnline](./media/connectors-create-api-projectonline/projectonline-3.png)   
4. Figyelje meg hello kapcsolat létrejött, és most a Logic Apps alkalmazást az egyéb hello szabad tooproceed szükséges lépések:  
   ![4. lépés ProjectOnline](./media/connectors-create-api-projectonline/projectonline-4.png)   

