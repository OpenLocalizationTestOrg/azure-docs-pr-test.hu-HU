

<span data-ttu-id="30c19-101">Használata esetén csatlakozhassanak az **SharePoint Online**, meg kell adnia az identitás (felhasználónév és jelszó, intelligens kártyához tartozó hitelesítő adatok, stb.) a SharePoint online.</span><span class="sxs-lookup"><span data-stu-id="30c19-101">In order to connect to **SharePoint Online**, you need to provide your identity (username and password, smart card credentials, etc.) to SharePoint Online.</span></span> <span data-ttu-id="30c19-102">Amennyiben Ön már hitelesítve, a SharePoint Online-összekötő használatához a logic App lépne.</span><span class="sxs-lookup"><span data-stu-id="30c19-102">Once you've been authenticated, you can proceed to use the SharePoint Online connector  in your logic app.</span></span> 

<span data-ttu-id="30c19-103">A Logic Apps alkalmazást designer, a lépések végrehajtásával jelentkezzen be a SharePoint létrehozásához a **kapcsolat** használható a Logic Apps alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="30c19-103">While on the designer of your logic app, follow these steps to sign into SharePoint to create the **connection** for use in your logic app:</span></span>

1. <span data-ttu-id="30c19-104">A keresőmezőbe írja be a SharePoint, és várja meg, a keresés vissza eseményindítók és a SharePoint online-hoz kapcsolódó műveletek:</span><span class="sxs-lookup"><span data-stu-id="30c19-104">Enter SharePoint in the search box and wait for the search to return all triggers and actions related to SharePoint Online:</span></span>   
   ![A SharePoint konfigurálása][1]  
2. <span data-ttu-id="30c19-106">Válassza ki a **SharePoint online-hoz - fájl jön létre** eseményindító</span><span class="sxs-lookup"><span data-stu-id="30c19-106">Select the **SharePoint Online - When a file is created** trigger</span></span>  
3. <span data-ttu-id="30c19-107">Válassza ki **jelentkezzen be a SharePoint Online**:</span><span class="sxs-lookup"><span data-stu-id="30c19-107">Select **Sign in to SharePoint Online**:</span></span>   
   <span data-ttu-id="30c19-108">![A SharePoint konfigurálása][2]</span><span class="sxs-lookup"><span data-stu-id="30c19-108">![Configure SharePoint][2]</span></span>    
4. <span data-ttu-id="30c19-109">Jelentkezzen be a SharePoint való hitelesítéshez szükséges a SharePoint hitelesítő adatok megadása</span><span class="sxs-lookup"><span data-stu-id="30c19-109">Provide your SharePoint credentials to sign in to authenticate with SharePoint</span></span>   
   ![A SharePoint konfigurálása][3]     
5. <span data-ttu-id="30c19-111">A hitelesítés befejezése után a lesz átirányítva, a logikai alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="30c19-111">After the authentication completes you'll be redirected to your logic app.</span></span> <span data-ttu-id="30c19-112">Ennyi az egész, a kapcsolat létrejött.</span><span class="sxs-lookup"><span data-stu-id="30c19-112">That's it, the connection has been created.</span></span> <span data-ttu-id="30c19-113">Figyelje meg, az üzenet azt jelzi, hogy most már csatlakozott SharePoint alján.</span><span class="sxs-lookup"><span data-stu-id="30c19-113">Notice the message at the bottom that indicates that you are now connected to SharePoint.</span></span>  
   ![A SharePoint konfigurálása][4]  
6. <span data-ttu-id="30c19-115">Más eseményindítók és műveletek végre kell hajtania a Logic Apps alkalmazást, majd adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="30c19-115">You can then add other triggers and actions that you need to complete your logic app.</span></span>   

[1]: ./media/connectors-create-api-sharepointonline/connectionconfig1.png
[2]: ./media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ./media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ./media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ./media/connectors-create-api-sharepointonline/connectionconfig5.png
