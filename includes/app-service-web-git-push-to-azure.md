## <a name="push-to-azure-from-git"></a><span data-ttu-id="cdfa2-101">Leküldéses üzenet küldése a Gitből az Azure-ra</span><span class="sxs-lookup"><span data-stu-id="cdfa2-101">Push to Azure from Git</span></span>

<span data-ttu-id="cdfa2-102">Adjon hozzá egy távoli Azure-mappát a helyi Git-tárházhoz.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-102">Add an Azure remote to your local Git repository.</span></span>

```bash
git remote add azure <URI from previous step>
```

<span data-ttu-id="cdfa2-103">A távoli Azure-mappához történő küldéssel helyezze üzembe az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-103">Push to the Azure remote to deploy your app.</span></span> <span data-ttu-id="cdfa2-104">A rendszer rákérdez az előzőleg, az üzembe helyező felhasználó létrehozásakor létrehozott jelszóra.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-104">You are prompted for the password you created earlier when you created the deployment user.</span></span> <span data-ttu-id="cdfa2-105">Ügyeljen arra, hogy az [Üzembe helyező felhasználó konfigurálása](#configure-a-deployment-user) szakaszban létrehozott jelszót adja meg, ne azt, amelyet az Azure Portalra való bejelentkezéshez használ.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-105">Make sure that you enter the password you created in [Configure a deployment user](#configure-a-deployment-user), not the password you use to log in to the Azure portal.</span></span>

```bash
git push azure master
```

<span data-ttu-id="cdfa2-106">A fenti parancs a következő példához hasonló információkat jelenít meg:</span><span class="sxs-lookup"><span data-stu-id="cdfa2-106">The preceding command displays information similar to the following example:</span></span>
