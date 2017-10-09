### <a name="prerequisites"></a>Előfeltételek
Rendelkeznie kell egy [Service Bus](https://azure.microsoft.com/services/service-bus/) fiók.  

Azure Service Bus-fiókját a logikai alkalmazás használata előtt engedélyeznie kell a hello logic app tooconnect tooyour service bus fiók. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure-portálon.  

Az alábbiakban hello lépéseket tooauthorize a logic app tooconnect tooyour Service Bus-fiókot:  

1. toocreate hello logic app tervezőben kapcsolat tooService Bus, válasszon **megjelenítése Microsoft felügyelt API-k** hello legördülő listában. Írja be **service bus** hello Keresés mezőbe. Válassza ki a hello eseményindító vagy toouse kívánt műveletet.  
    ![A Service Bus kapcsolati kép 1](./media/connectors-create-api-servicebus/servicebus-1.png)  
2. Ha még nem hozott létre bármely kapcsolatok tooService Bus előtt, a Service Bus-hitelesítő adatokat fogja felszólító tooprovide. Ezek a hitelesítő adatok használt tooauthorize a logic app tooconnect tooand hozzáférni a Service Bus fiók adatait. hello Service Bus összekötő hello Service Bus-névtér hello kapcsolódási karakterlánc szükséges. Is szükséges **kezelése** engedélyek. Egy jó módszer tooknow, ha a kapcsolati karakterlánc hello névtérhez, vagy egy adott entitás tartalmaz-e a hello `EntityPath` paraméter. Ha igen, nincs logikai alkalmazás hello megfelelő kapcsolati karakterláncot.  
    ![Service Bus kapcsolati karakterlánca](./media/connectors-create-api-servicebus/connectionstring.png)
3. Miután hello névtér hello kapcsolati karakterláncot kapott, használhatja a Logic Apps az API-kapcsolat hello.  
    ![Kép: a Service Bus kapcsolat 2](./media/connectors-create-api-servicebus/servicebus-2.png)  
4. Értesítés hello kapcsolat létrejött, és most az egyéb hello szabad tooproceed lépései a Logic Apps alkalmazást.  
    ![A Service Bus kapcsolati kép 3](./media/connectors-create-api-servicebus/servicebus-3.png)   

