---
lab:
  title: Erstellen einer Azure-Funktion mit Visual Studio Code
  description: 'Erfahren Sie, wie Sie eine Azure-Funktion mit einem HTTP-Trigger erstellen. Nachdem Sie den Code lokal in Visual Studio Code erstellt und getestet haben, stellen Sie die Funktion in Azure bereit.'
---

# Erstellen einer Azure-Funktion mit Visual Studio Code

In dieser Übung erfahren Sie, wie Sie eine C\#-Funktion erstellen, die auf HTTP-Anforderungen reagiert. Nachdem Sie den Code lokal in Visual Studio Code erstellt und getestet haben, stellen Sie die Funktion in Azure bereit und testen sie.

In dieser Übung ausgeführte Aufgaben:

* Erstellen einer Azure App Service-Ressource für eine containerisierte App
* Ansicht der Ergebnisse
* Bereinigen von Ressourcen

Diese Übung dauert ca. **30** Minuten.

## Vor der Installation

Stellen Sie vorab sicher, dass die folgenden Anforderungen erfüllt sind:

* Ein Azure-Abonnement. Wenn Sie noch keines besitzen, können Sie sich dafür [registrieren](https://azure.microsoft.com/).

* [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools#installing), Version 4.x.

* [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms)

* [.NET 8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) ist das Zielframework.

* [C#-Entwicklerkit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit) für Visual Studio Code

* [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) für Visual Studio Code

## Erstellen Ihres lokalen Projekts

In diesem Abschnitt wird mithilfe von Visual Studio Code ein lokales Azure Functions-Projekt in C# erstellt. Später veröffentlichen Sie in dieser Übung Ihren Funktionscode in Azure.

1. Drücken Sie in Visual Studio Code die F1-Taste, um die Befehlspalette zu öffnen. Suchen Sie dann den Befehl `Azure Functions: Create New Project...`, und führen Sie ihn aus.

1. Wählen Sie den Verzeichnisspeicherort für Ihren Projektarbeitsbereich und anschließend **Auswählen** aus. Sie sollten entweder einen neuen Ordner erstellen oder einen leeren Ordner für den Projektarbeitsbereich auswählen. Wählen Sie keinen Projektordner aus, der bereits Teil eines Arbeitsbereichs ist.

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    | Eingabeaufforderung | Aktion |
    |--|--|
    | Auswählen des Ordners, in dem Ihre Funktionsprojekte gespeichert werden sollen | Wählen Sie **Durchsuchen …**, um einen Ordner für Ihre App auszuwählen.
    | Sprache auswählen | Wählen Sie **C#** aus. |
    | Auswählen einer .NET-Laufzeit | Wählen Sie **.NET 8.0 Isoliert**. |
    | Auswählen einer Vorlage für die erste Funktion Ihres Projekts | Wählen Sie **HTTP Trigger**.<sup>1</sup> |
    | Angeben eines Funktionsnamens | Geben Sie `HttpExample` ein. |
    | Angeben eines Namespaces | Geben Sie `My.Function` ein. |
    | Autorisierungsstufe | Wählen Sie **Anonym** aus, damit jeder Ihren Funktionsendpunkt aufrufen kann. |

    <sup>1</sup> Je nach Ihren VS Code-Einstellungen müssen Sie möglicherweise die Option **Vorlagenfilter ändern** verwenden, um die vollständige Liste der Vorlagen zu sehen.

1. Visual Studio Code verwendet die bereitgestellten Informationen und generiert ein Azure Functions-Projekt mit einem HTTP-Trigger. Die lokalen Projektdateien können im Explorer angezeigt werden.

### Lokales Ausführen der Funktion

Dank der Integration zwischen Visual Studio Code und Azure Functions Core Tools können Sie dieses Projekt vor der Veröffentlichung in Azure zunächst auf Ihrem lokalen Entwicklungscomputer ausführen.

1. Stellen Sie sicher, dass das Terminal in Visual Studio Code geöffnet ist. Sie können das Terminal öffnen, indem Sie **Terminal** und dann **Neues Terminal** in der Menüleiste auswählen. 

1. Drücken Sie **F5**, um das Funktions-App-Projekt im Debugger zu starten. Die Ausgabe der Core Tools wird im Bereich **Terminal** angezeigt. Ihre App wird im **Terminal**-Bereich gestartet. Der lokal ausgeführte URL-Endpunkt Ihrer über HTTP ausgelösten Funktion wird angezeigt.

    ![Ein Screenshot des Endpunkts Ihrer HTTP-getriggerten Funktion wird im Bedienfeld „Terminal“ angezeigt.](./media/06/run-function-local.png)

1. Navigieren Sie bei ausgeführten Core Tools zum Bereich **Azure: Functions**. Erweitern Sie unter **Functions** die Option **Lokales Projekt** > **Functions**. Klicken Sie mit der rechten Maustaste auf die Funktion **HttpExample** und wählen Sie **Funktion jetzt ausführen …** aus.

    ![Screenshot, der die Position des Schritts „Funktion jetzt ausführen“ anzeigt.](./media/06/execute-function-local.png)

1. Geben Sie unter **Anforderungstext eingeben** den Wert `{ "name": "Azure" }` für den Nachrichtentext der Anfrage ein. Drücken Sie die **EINGABETASTE**, um diese Anforderungsmeldung an Ihre Funktion zu senden. Wenn die Funktion lokal ausgeführt wird und eine Antwort zurückgibt, wird in Visual Studio Code eine Benachrichtigung ausgelöst.

    Wählen Sie das Symbol der Benachrichtigungsglocke, um die Benachrichtigung anzuzeigen. Informationen zur Funktionsausführung werden im **Terminal**-Bereich angezeigt.

1. Drücken Sie **UMSCHALT+F5**, um Core Tools zu beenden und die Verbindung mit dem Debugger zu trennen.

Nachdem Sie sich vergewissert haben, dass die Funktion auf Ihrem lokalen Computer korrekt ausgeführt wird, können Sie das Projekt mithilfe von Visual Studio Code direkt in Azure veröffentlichen.

## Bereitstellen und Ausführen der Funktion in Azure

In diesem Abschnitt erstellen Sie eine Azure-Funktions-App-Ressource und stellen die Funktion für die Ressource bereit.

### Anmelden bei Azure

Bevor Sie Ihre App veröffentlichen können, müssen Sie sich bei Azure anmelden. Sollten Sie bereits angemeldet sein, fahren Sie mit dem nächsten Abschnitt fort.

1. Wenn Sie noch nicht angemeldet sind, wählen Sie auf der Aktivitätsleiste das Azure-Symbol und dann im Bereich **Azure: Funktionen** die Option **Bei Azure anmelden...** aus.

    ![Screenshot der Schaltfläche „Bei Azure anmelden“.](./media/06/functions-sign-into-azure.png)

1. Wählen Sie im Browser nach entsprechender Aufforderung Ihr Azure-Konto aus, und melden Sie sich mit Ihren Azure-Anmeldeinformationen an.

1. Wenn Sie sich erfolgreich angemeldet haben, können Sie das neue Browserfenster schließen. Die Abonnements, die zu Ihrem Azure-Konto gehören, werden auf der Seitenleiste angezeigt.

### Erstellen von Ressourcen in Azure

In diesem Abschnitt erstellen Sie die Azure-Ressourcen, die Sie benötigen, um Ihre lokale Funktions-App bereitzustellen.

1. Wählen Sie das Azure-Symbol in der Aktivitätsleiste aus, und wählen Sie dann im Bereich **Ressourcen** die Option **Ressource erstellen...** aus.

    ![Screenshot der Schaltfläche „Ressourcen erstellen“.](./media/06/create-resource.png)    

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    | Eingabeaufforderung | Aktion |
    |--|--|
    | Zu erstellende Ressource auswählen | Wählen Sie **Create Function App in Azure...** (Funktions-App in Azure erstellen...) aus. |
    | Auswählen des Abonnements | Wählen Sie das zu verwendende Abonnement aus. *Wenn Sie nur über ein Abonnement verfügen, wird diese Option nicht angezeigt.* |
    | Eingeben eines global eindeutigen Namens für die Funktions-App | Geben Sie einen Namen ein, der in einem URL-Pfad gültig ist, beispielsweise `myfunctionapp`. Der von Ihnen eingegebene Name wird überprüft, um sicherzustellen, dass er eindeutig ist. |
    | Auswählen eines Standorts für neue Ressourcen | Wählen Sie eine Region in Ihrer Nähe aus, um eine bessere Leistung zu erzielen. |
    | Auswählen eines Runtimestapels | Wählen Sie **.NET 8.0 isoliert** aus. |
    | Auswählen einer Instanzspeichergröße | Wählen Sie die Option **2048 Standard**. |
    | Wählen Sie die maximale Anzahl der Instanzen aus. | Übernehmen Sie die Standardeinstellung **100**. |

    Die Erweiterung zeigt den Status einzelner Ressourcen an, während sie im Bereich **AZURE** des Terminalfensters erstellt werden.
    
1. Nach Abschluss des Vorgangs werden in Ihrem Abonnement die folgenden Azure-Ressourcen erstellt, deren Namen auf dem Namen Ihrer Funktions-App basieren:

    * Eine Ressourcengruppe als logischen Container für verwandte Ressourcen.
    * Ein Azure Storage-Standardkonto, in dem Status- und andere Informationen zu Ihren Projekten verwaltet werden.
    * Ein Flex-Verbrauchsplan, der den zugrunde liegenden Host für Ihre serverlose Funktions-App definiert.
    * Eine Funktions-App, die als Umgebung zum Ausführen Ihres Funktionscodes dient. Mit einer Funktions-App können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen im selben Hostingplan.
    * Eine mit der Funktions-App verbundene Application Insights-Instanz, die die Nutzung Ihrer serverlosen Funktion nachverfolgt.

### Bereitstellen des Projekts in Azure

> **Wichtig:** Durch das Veröffentlichen in einer bestehenden Funktion werden alle vorherigen Bereitstellungen überschrieben.

1. Suchen Sie in der Befehlspalette den Befehl **Azure Functions: In Funktions-App bereitstellen …** und führen Sie ihn aus.

1. Wählen Sie das Abonnement aus, das Sie beim Erstellen der Ressourcen verwendet haben.

1. Wählen Sie die erstellte Funktions-App aus. Wenn Sie aufgefordert werden, vorherige Bereitstellungen zu überschreiben, wählen Sie **Bereitstellen** aus, um Ihren Funktionscode in der neuen Funktions-App-Ressource bereitzustellen.

1. Wählen Sie nach Abschluss der Bereitstellung die Option **Ausgabe anzeigen**, um die Details der Bereitstellungsergebnisse anzuzeigen. Sollten Sie die Benachrichtigung übersehen haben, klicken Sie bitte auf das Benachrichtigungssymbol in der unteren rechten Ecke, um sie erneut anzuzeigen.

    ![Screenshot der Schaltfläche „Ausgabe anzeigen“.](./media/06/function-view-output.png)

### Ausführen der Funktion in Azure

1. Erweitern Sie im Bereich **Ressourcen** auf der Seitenleiste Ihr Abonnement, die neue Funktions-App und **Funktionen**. **Klicken Sie mit der rechten Maustaste** auf die Funktion **HttpExample** und wählen Sie **Funktion jetzt ausführen …** aus.

    ![Screenshot der Option „Funktion jetzt ausführen“.](./media/06/execute-function-remote.png)

1. Unter **Enter request body** (Anforderungstext eingeben) wird `{ "name": "Azure" }` als Wert für den Text der Anforderungsnachricht angezeigt. Drücken Sie die EINGABETASTE, um diese Anforderungsnachricht an Ihre Funktion zu senden.

1. Wenn die Funktion in Azure ausgeführt wird und eine Antwort zurückgibt, wird in Visual Studio Code eine Benachrichtigung ausgelöst. Wählen Sie das Symbol der Benachrichtigungsglocke, um die Benachrichtigung anzuzeigen.

## Bereinigen von Ressourcen

Nachdem Sie die Übung beendet haben, sollten Sie die von Ihnen erstellten Cloud-Ressourcen löschen, um eine unnötige Ressourcennutzung zu vermeiden.

1. Navigieren Sie in Ihrem Browser zum Azure-Portal [https://portal.azure.com](https://portal.azure.com) und melden Sie sich mit Ihren Azure-Anmeldeinformationen an, wenn Sie dazu aufgefordert werden.
1. Navigieren Sie zu der Ressourcengruppe, die Sie erstellt haben, und zeigen Sie den Inhalt der in dieser Übung verwendeten Ressourcen an.
1. Wählen Sie auf der Symbolleiste die Option **Ressourcengruppe löschen** aus.
1. Geben Sie den Namen der Ressourcengruppe ein, und bestätigen Sie, dass Sie sie löschen möchten.
