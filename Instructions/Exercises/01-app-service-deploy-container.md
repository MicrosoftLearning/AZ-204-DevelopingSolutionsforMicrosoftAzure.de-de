---
lab:
  title: Bereitstellen einer containerisierten App in Azure App Service
  description: 'Erfahren Sie, wie Sie eine containerisierte App in Azure App Service bereitstellen.'
---

# Bereitstellen einer containerisierten App in Azure App Service

In dieser Übung stellen Sie eine einfache containerisierte App in Azure App Service bereit. 

In dieser Übung ausgeführte Aufgaben:

* Erstellen einer Azure App Service-Ressource für eine containerisierte App
* Ansicht der Ergebnisse
* Bereinigen von Ressourcen

Diese Übung dauert ca. **15** Minuten.

## Vor der Installation

Zum Abschließen der Übung benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie noch keines besitzen, können Sie sich dafür [registrieren](https://azure.microsoft.com/).

## Erstellen einer Web-App-Ressource

1. Navigieren Sie in Ihrem Browser zum Azure-Portal [https://portal.azure.com](https://portal.azure.com) und melden Sie sich mit Ihren Azure-Anmeldeinformationen an, wenn Sie dazu aufgefordert werden.
1. Wählen Sie die Option **+ Ressource erstellen** in der **Azure Services**-Überschrift oben auf der Homepage. 
1. Geben Sie in der **Im Marketplace suchen**-Suchleiste *Web-App* ein und drücken Sie die **Eingabetaste**, um die Suche zu starten.
1. Wählen Sie in der Kachel Web-App das Dropdownmenü **Erstellen** und dann die Option **Web-App** aus.

    ![Screenshot der Web-App-Kachel.](./media/01/create-web-app-tile.png)

Wenn Sie **Erstellen** auswählen, wird eine Vorlage mit einigen Registerkarten geöffnet, die Sie mit Informationen zu Ihrer Bereitstellung ausfüllen müssen. Die folgenden Schritte führen Sie durch die Änderungen, die auf den relevanten Registerkarten vorgenommen werden sollen.

1. Füllen Sie die Registerkarte **Grundlagen** mit den Informationen aus der folgenden Tabelle aus:

    | Einstellung | Aktion |
    |--|--|
    | **Abonnement** | Übernehmen Sie den Standardwert. |
    | **Ressourcengruppe** | Wählen Sie „Neu erstellen“, geben Sie `rg-WebApp` ein und wählen Sie dann „OK“. |
    | **Name** | Geben Sie einen eindeutigen Namen ein, z. B. `<your-initials>-containerwebapp`. Ersetzen Sie *\<your-initials>* durch Ihre Initialen oder einen anderen Wert. Der Name muss eindeutig sein, daher sind möglicherweise einige Änderungen erforderlich. |
    | Schieberegler unter der Einstellung **Name** | Wählen Sie den Schieberegler aus, um ihn zu deaktivieren. |
    | **Veröffentlichen** | Wählen Sie die Option **Container** aus. |
    | **Betriebssystem** | Stellen Sie sicher, dass **Linux** ausgewählt ist. |
    | **Region** | Behalten Sie die Standardauswahl bei, oder wählen Sie einen Bereich in Ihrer Nähe aus. |
    | **Linux-Plan** | Behalten Sie die Standardauswahl bei. |
    | **Tarif** | Wählen Sie die Dropdownliste und dann den kostenlosen **F1-Plan** aus. |

1. Wählen Sie die Registerkarte **Container** oder navigieren Sie dorthin und geben Sie die Informationen in der folgenden Tabelle ein:

    | Einstellung | Aktion |
    |--|--|
    | **Sidecar-Unterstützung** | Behalten Sie die Standardeinstellung „Aus“ bei. |
    | **Imagequelle** | Wählen Sie **Andere Containerregistrierungen**. |
    | **Zugriffstyp** | Behalten Sie die Standardauswahl **Öffentlich** bei. |
    | **URL des Registrierungsservers** | Geben Sie `mcr.microsoft.com/k8se` ein. |
    | **Bild und Tag** | Geben Sie `quickstart:latest` ein. |
    | **Startbefehl** | Lassen Sie dieses Feld leer. |

1. Wählen Sie die Registerkarte **Überprüfen + erstellen** aus.
1. Überprüfen Sie die von Ihnen getroffene Auswahl und wählen Sie dann die Schaltfläche **Erstellen** aus.

Es kann einige Minuten dauern, bis die Bereitstellung abgeschlossen ist. Wenn der Vorgang abgeschlossen ist, wählen Sie die Schaltfläche **Zur Ressource wechseln**.

Nachdem die Bereitstellung abgeschlossen ist, ist es an der Zeit, die Web-App aufzurufen. Wählen Sie den Link zu Ihrer Webanwendung, der sich neben dem Feld **Standarddomäne** im Abschnitt **Essentials** befindet. Der Link öffnet die Website in einer neuen Registerkarte.

>**Hinweis:** Es kann einige Minuten dauern, bis die bereitgestellte Container-App auf der neuen Registerkarte ausgeführt und angezeigt wird.

## Bereinigen von Ressourcen

Nachdem Sie die Übung beendet haben, sollten Sie die von Ihnen erstellten Cloud-Ressourcen löschen, um eine unnötige Ressourcennutzung zu vermeiden.

1. Navigieren Sie zu der Ressourcengruppe, die Sie erstellt haben, und zeigen Sie den Inhalt der in dieser Übung verwendeten Ressourcen an.
1. Wählen Sie auf der Symbolleiste die Option **Ressourcengruppe löschen** aus.
1. Geben Sie den Namen der Ressourcengruppe ein, und bestätigen Sie, dass Sie sie löschen möchten.
