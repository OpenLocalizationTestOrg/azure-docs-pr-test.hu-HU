## <a name="connect-toooutlookcom"></a>Csatlakozás tooOutlook.com
### <a name="prerequisites"></a>Előfeltételek
* Egy Outlook.com-fiók

Outlook.com-os fiókját a logikai alkalmazás használata előtt engedélyeznie kell a hello Logic app tooconnect tooyour Outlook.com-os fiók. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon. 

Az alábbiakban hello lépéseket tooauthorize a Logic app tooconnect tooyour Outlook.com-fiók:

1. Minden a Logic apps toobe indította eseményindítót, így miután hoz létre a logikai alkalmazás hello Tervező megnyílik, és megjeleníti, hogy elindítja toostart a Logic Apps alkalmazást kell:
   
   ![](./media/connectors-create-api-outlook/office365-outlook-0.png)
2. Adja meg a "outlook" hello Keresés mezőbe. Értesítés hello lista szűrt toolist összes hello hello neve "Outlook", eseményindító:![](./media/connectors-create-api-outlook/office365-outlook-0-5.png)
3. Válassza ki **Office 365 Outlook - az új e-mailek**.   
   Ha bármely kapcsolatok tooOutlook előtt még nem hozott létre, később is lesz felszólító tooprovide a Outlook.com-os hitelesítő adataival. Ezeket a hitelesítő adatokat kell használt tooauthorize a Logic app tooconnect számára, és elérni az Outlook.com-os fiók adatait:![](./media/connectors-create-api-outlook/office365-outlook-1.png)
4. Adja meg a hitelesítő adatait az Outlook, és jelentkezzen be:![](./media/connectors-create-api-outlook/office365-outlook-2.png)  
   Ennyi az egész. Most létrehozott egy kapcsolatot tooOutlook. Ez a kapcsolat bármely más logikai alkalmazás az Ön által létrehozott használható lesz.

