### <a name="prerequisites"></a>Előfeltételek
* A [Yammer](https://www.yammer.com/) fiók 

A Yammer-fiókjába a logikai alkalmazás használata előtt engedélyeznie kell a hello Logic app tooconnect tooyour Yammer-fiókjába. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon. 

Az alábbiakban hello lépéseket tooauthorize a Logic app tooconnect tooyour Yammer-fiókot:

1. toocreate egy kapcsolat tooYammer hello Logic app Designer kiválasztása **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, majd adja meg *Yammer* hello Keresés mezőbe. Hello eseményindító vagy lesz, például a toouse művelet kiválasztása:  
   ![](./media/connectors-create-api-yammer/yammer-1.png)
2. Ha bármely kapcsolatok tooYammer előtt még nem hozott létre, később is lesz felszólító tooprovide Yammer-adatait. Ezeket a hitelesítő adatokat kell használt tooauthorize a Logic app tooconnect számára, és a Yammer-fiókjába adatok eléréséhez:  
   ![](./media/connectors-create-api-yammer/yammer-2.png)
3. Adja meg a Yammer-felhasználó nevét és jelszavát tooauthorize a Logic Apps alkalmazást:  
   ![](./media/connectors-create-api-yammer/yammer-3.png)   
4. Figyelje meg hello kapcsolat létrejött, és most a Logic Apps alkalmazást az egyéb hello szabad tooproceed szükséges lépések:  
   ![](./media/connectors-create-api-yammer/yammer-4.png)   

