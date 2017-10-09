#### <a name="prerequisites"></a>Előfeltételek
* Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)
* A [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) fiók 

A OneDrive-fiókja a logikai alkalmazás használata előtt engedélyezze a hello logic app tooconnect tooyour OneDrive-fiókja.  Ehhez egyszerűen a logikai alkalmazásban a hello Azure-portálon. 

Engedélyezze a logic app tooconnect tooyour OneDrive-fiókja lépések hello használata:

1. Logikai alkalmazás létrehozása. Hello Logic Apps-tervezőben, válassza ki a **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, és írja be a "onedrive" hello keresőmezőbe. Hello eseményindítók és műveletek közül választhat:  
   ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Ha korábban még nem létrehozott bármely kapcsolatok tooOneDrive, a onedrive vállalati verzió hitelesítő adataival felszólító toosign áll:  
   ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Válassza ki **bejelentkezés**, és írja be a felhasználónevet és jelszót. Válassza ki **bejelentkezés**:  
   ![](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Ezeket a hitelesítő adatokat használt tooauthorize a logic app tooconnect számára, és a onedrive vállalati verzió fiókjában hello adatok eléréséhez. 
4. Válassza ki **Igen** tooauthorize hello logic app toouse a OneDrive-fiókja:  
   ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Értesítés hello kapcsolat létrejött. Most, folytassa a Logic Apps alkalmazást hello más szükséges lépések:  
   ![](./media/connectors-create-api-onedrive/onedrive-5.png)

