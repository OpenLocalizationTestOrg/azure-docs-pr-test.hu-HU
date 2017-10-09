#### <a name="prerequisites"></a>Előfeltételek
* Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)
* Egy [Office 365](https://office365.com) fiók  

A logikai alkalmazást az Office 365-fiók használatához engedélyezni, hello logic app tooconnect tooyour Office 365-fiókra. Ehhez egyszerűen a logikai alkalmazásban a hello Azure-portálon.  

Engedélyezze a logic app tooconnect tooyour Office 365 fiókját hello a következő lépéseket:

1. Logikai alkalmazás létrehozása. Hello Logic Apps-tervezőben, válassza ki a **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, és írja be a "office 365" hello keresési mezőbe. Hello eseményindítók és műveletek közül választhat:  
    ![Az Office 365 kapcsolat létrehozását lépést](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  
2. Ha korábban még nem létrehozott bármely kapcsolatok tooOffice 365, az Office 365 hitelesítő adataival felszólító toosign áll:  
    ![Az Office 365 kapcsolat létrehozását lépést](./media/connectors-create-api-office365-outlook/office365-signin.png)  
3. Válassza ki **bejelentkezés**, és írja be a felhasználónevet és jelszót. Válassza ki **bejelentkezés**:  
    ![Az Office 365 kapcsolat létrehozását lépést](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)
   
    Ezek a hitelesítő adatok használt tooauthorize a logic app tooconnect számára, és Office 365-fiókját. 
4. Értesítés hello kapcsolat létrejött. Most, folytassa a Logic Apps alkalmazást hello más szükséges lépések:   
    ![Az Office 365 kapcsolat létrehozását lépést](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  

