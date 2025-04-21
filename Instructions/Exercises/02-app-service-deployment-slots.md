---
lab:
  title: Austauschen von Bereitstellungsslots in Azure App Service
  description: 'Erfahren Sie, wie Sie Bereitstellungsslots in Azure App Service austauschen können. In dieser Übung werden Sie eine einfache App in App Service bereitstellen, eine kleine Änderung an der App vornehmen und diese in einem Stagingslot bereitstellen und schließlich die Slots austauschen, sodass die aktualisierte App in der Produktion ist.'
---

# Austauschen von Bereitstellungsslots in Azure App Service

In dieser Übung stellen Sie mithilfe des Azure CLI-Befehls **az webapp up** eine einfache HTML+CSS-Website in Azure App Service bereit. Als Nächstes aktualisieren Sie den Code und stellen die Änderung in einem Stagingslot bereit. Schließlich tauschen Sie die Slots aus.

In dieser Übung ausgeführte Aufgaben:

* Laden Sie die Beispielanwendung herunter und stellen Sie sie in Azure App Service bereit.
* Erstellen Sie einen Staging-Bereitstellungsslot.
* Nehmen Sie eine Änderung an der Beispiel-App vor, und stellen Sie sie im Stagingslot bereit.
* Tauschen Sie die Staging- und Standardproduktionsslots aus, um die Änderungen an den Produktionsslots zu verschieben.

Diese Übung dauert ca. **20** Minuten.

## Vor der Installation

Zum Abschließen der Übung benötigen Sie Folgendes:

* Ein Azure-Abonnement. Falls Sie noch kein Abonnement haben, können Sie sich hier anmelden [https://azure.microsoft.com/](https://azure.microsoft.com/).

## Herunterladen und Bereitstellen der Beispiel-App

In diesem Abschnitt laden Sie die Beispiel-App herunter, legen Variablen fest, um die Eingabe der Befehle zu vereinfachen, erstellen eine Azure App Service-Ressource und stellen eine statische HTML-Website mithilfe von Azure CLI-Befehlen bereit.

1. Navigieren Sie in Ihrem Browser zum Azure-Portal [https://portal.azure.com](https://portal.azure.com) und melden Sie sich mit Ihren Azure-Anmeldeinformationen an, wenn Sie dazu aufgefordert werden.

1. Verwenden Sie die Schaltfläche **[\>_]** rechts neben der Suchleiste oben auf der Seite, um eine neue Cloud Shell im Azure-Portal zu erstellen, und wählen Sie eine ***Bash***-Umgebung aus. Die Cloud Shell bietet eine Befehlszeilenschnittstelle in einem Bereich am unteren Rand des Azure-Portals.

    > **Hinweis**: Wenn Sie zuvor eine Cloud Shell erstellt haben, die eine *PowerShell*-Umgebung verwendet, wechseln Sie diese zu ***Bash***.

1. Wählen Sie in der Cloud Shell-Symbolleiste im Menü **Einstellungen** das Menüelement **Zur klassischen Version wechseln** aus (dies ist für die Verwendung des Code-Editors erforderlich).

1. Führen Sie den folgenden **git**-Befehl aus, um das Beispiel-App-Repository zu klonen.

    ```bash
    git clone https://github.com/Azure-Samples/html-docs-hello-world.git
    ```

1. Legen Sie Variablen zur Aufnahme der Ressourcengruppen- und App-Namen fest, indem Sie die folgenden Befehle ausführen. Notieren Sie sich den Wert von **appName**, der nach Ausführung der Befehle angezeigt wird. Sie benötigen ihn später in dieser Übung.

    ```bash
    resourceGroup=rg-mywebapp

    appName=mywebapp$RANDOM
    echo $appName
    ```

1. Navigieren Sie zu dem Verzeichnis, das den Beispielcode enthält, und führen Sie den Befehl **az webapp up** aus. **Hinweis:** Die Ausführung dieses Befehls kann einige Minuten dauern.

    ```bash
    cd html-docs-hello-world

    az webapp up -g $resourceGroup -n $appName --sku P0V3 --html
    ```

    Nachdem die Bereitstellung abgeschlossen ist, ist es an der Zeit, die Web-App aufzurufen.

1. Navigieren Sie im Azure-Portal zu der von Ihnen bereitgestellten Web-App. Sie können den zuvor notierten Namen in die Suchleiste **Ressourcen, Dienste und Dokumente suchen (G + /)** eingeben und die Ressource aus der Liste auswählen.

1. Wählen Sie den Link zu Ihrer Web-App aus, der sich im Feld **Standarddomäne** im Abschnitt **Grundlagen** befindet. Der Link öffnet die Website in einer neuen Registerkarte.

## Bereitstellen des aktualisierten Codes in einem Bereitstellungsslot

In diesem Abschnitt erstellen Sie einen Bereitstellungsslot, ändern den HTML-Code in der App und stellen den aktualisierten Code im neuen Bereitstellungsslot bereit.

### Erstellen eines Bereitstellungsslots 

1. Kehren Sie zur Registerkarte mit dem Azure-Portal und der Cloud-Shell zurück.

1. Geben Sie den folgenden Befehl in der Cloud-Shell ein, um einen Bereitstellungsslot mit dem Namen *staging* zu erstellen.

    ```bash
    az webapp deployment slot create -n $appName -g $resourceGroup --slot staging
    ```

1. Warten Sie, bis der Befehl abgeschlossen ist, und wählen Sie dann im linken Menü **Bereitstellung > Bereitstellungsslots**, um die Bereitstellungsslots für Ihre Web-App anzuzeigen. Beachten Sie, dass der Name des neuen Slots den Zusatz *-staging* hinter dem Namen Ihrer Web-App enthält.

### Aktualisieren des Codes und Bereitstellung im Stagingslot

1. Geben Sie in Cloud Shell `code index.html` ein, um den Editor zu öffnen. Ändern Sie im `<h1>`-Überschriften-Tag *Azure App Service – Beispiel für eine statische HTML-Website* in *Azure App Service-Stagingslot* oder in einen anderen Namen Ihrer Wahl.

1. Drücken Sie zum Speichern **STRG + S** und zum Beenden **STRG + Q**.

1. Führen Sie in der Cloud Shell den folgenden Befehl aus, um eine ZIP-Datei des aktualisierten Projekts zu erstellen. Für den nächsten Schritt wird eine ZIP-Datei oder eine Webanwendungsressource (WAR) benötigt.

    ```bash
    zip -r stagingcode.zip .
    ```

1. Führen Sie den folgenden Befehl in der Cloud Shell aus, um Ihre Updates für den Stagingslot bereitzustellen.

    ```bash
    az webapp deploy -g $resourceGroup -n $appName --src-path ./stagingcode.zip --slot staging
    ```

1. Wählen Sie im linken Menü Ihrer Web-App **Bereitstellung > Bereitstellungsslots** und anschließend den zuvor erstellten Stagingslot aus.

1. Wählen Sie den Link im Feld **Standarddomäne** im Abschnitt **Essentials**. Der Link öffnet die Website für den Stagingslot in einer neuen Registerkarte.

## Austauschen von Staging- und Produktionsslots

Sie können einen Austausch im Azure-Portal über die Option **Austauschen** in der Symbolleiste durchführen. Die Option **Austauschen** wird in der Symbolleiste angezeigt, wenn Sie im linken Menü **Übersicht** oder **Bereitstellung > Bereitstellungsslots** auswählen.

1. Wählen Sie im Azure-Portal in der Symbolleiste die Option **Austauschen**, um das Bedienfeld **Austauschen** zu öffnen.

1. Überprüfen Sie die Einstellungen im Austauschbedienfeld. Die **Quelle** sollte den **-staging**-Slot anzeigen, und das **Ziel** sollte den Standard-Produktionsslot anzeigen.

    ![Screenshot des Austauschbedienfelds.](./media/02/app-service-swap-panel.png)

1. Klicken Sie auf **Austausch starten**, und warten Sie, bis der Vorgang abgeschlossen ist. Sie können den Fortschritt im Bedienfeld **Benachrichtigungen** verfolgen, das Sie durch Auswahl des Glockensymbols oben im Portal öffnen können.

1. Um den Austausch zu überprüfen, navigieren Sie zu der Web-App, die Sie bereitgestellt haben. Geben Sie den zuvor erstellten Namen der Webanwendung (z. B. *mywebapp12360*) in die Suchleiste **Ressourcen, Dienste und Dokumente suchen (G + /)** ein und wählen Sie die Ressource aus der Liste aus.

1. Wählen Sie den Link zu Ihrer Web-App aus, der sich im Feld **Standarddomäne** im Abschnitt **Grundlagen** befindet. Der Link öffnet die Website (Produktionsslot) in einer neuen Registerkarte.

1. Überprüfen Sie Ihre Änderungen, möglicherweise müssen Sie die Seite aktualisieren, damit sie angezeigt wird.

## Bereinigen von Ressourcen

Nachdem Sie die Übung beendet haben, sollten Sie die von Ihnen erstellten Cloud-Ressourcen löschen, um eine unnötige Ressourcennutzung zu vermeiden.

1. Navigieren Sie zu der Ressourcengruppe, die Sie erstellt haben, und zeigen Sie den Inhalt der in dieser Übung verwendeten Ressourcen an.
1. Wählen Sie auf der Symbolleiste die Option **Ressourcengruppe löschen** aus.
1. Geben Sie den Namen der Ressourcengruppe ein, und bestätigen Sie, dass Sie sie löschen möchten.
