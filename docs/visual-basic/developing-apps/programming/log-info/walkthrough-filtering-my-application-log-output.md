---
title: "Filtern der Ausgaben von „My.Application.Log“ (Visual Basic) | Microsoft-Dokumentation"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
dev_langs:
- VB
helpviewer_keywords:
- My.Log object, filtering output
- My.Application.Log object, filtering output
- application event logs, output filtering
ms.assetid: 2c0a457a-38a4-49e1-934d-a51320b7b4ca
caps.latest.revision: 22
author: dotnet-bot
ms.author: dotnetcontent
translation.priority.ht:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- ru-ru
- zh-cn
- zh-tw
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: caa4b8be16e5000d02d82a83199a25d13ad07bba
ms.lasthandoff: 03/13/2017

---
# <a name="walkthrough-filtering-myapplicationlog-output-visual-basic"></a>Exemplarische Vorgehensweise: Filterung der Ausgaben von "My.Application.Log" (Visual Basic)
In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie das standardmäßige Filtern von Protokollen für das `My.Application.Log`-Objekt ändern, um zu steuern, welche Informationen vom `Log`-Objekt an die Listener übergeben werden und welche Informationen von den Listenern geschrieben werden. Sie können das Protokollierungsverhalten auch nach dem Erstellen der Anwendung ändern, da die Konfigurationsinformationen in der Konfigurationsdatei der Anwendung gespeichert sind.  
  
## <a name="getting-started"></a>Erste Schritte  
 Alle Nachrichten, die von `My.Application.Log` geschrieben werden, haben einen zugeordneten Schweregrad, den die Filtermechanismen verwenden, um die Protokollausgabe zu steuern. Diese Beispielanwendung verwendet `My.Application.Log`-Methoden zum Schreiben mehrerer Protokollmeldungen mit unterschiedlichen Schweregraden.  
  
#### <a name="to-build-the-sample-application"></a>So installieren Sie die Beispielanwendung  
  
1.  Öffnen Sie ein neues [!INCLUDE[vbprvb](../../../../csharp/programming-guide/concepts/linq/includes/vbprvb_md.md)]-Windows-Anwendungsprojekt.  
  
2.  Fügen Sie eine Schaltfläche mit dem Namen „Button1“ zu „Form1“ hinzu.  
  
3.  Fügen Sie im <xref:System.Windows.Forms.Control.Click>-Ereignishandler für Button1 folgenden Code hinzu:  
  
     [!code-vb[VbVbcnMyApplicationLogFiltering#1](../../../../visual-basic/developing-apps/programming/log-info/codesnippet/VisualBasic/walkthrough-filtering-my-application-log-output_1.vb)]  
  
4.  Führen Sie die Anwendung im Debugger aus.  
  
5.  Drücken Sie **Button1**.  
  
     Die Anwendung schreibt die folgenden Informationen in die Debugausgabe und die Protokolldatei der Anwendung.  
  
     `DefaultSource Information: 0 : In Button1_Click`  
  
     `DefaultSource Error: 2 : Error in the application.`  
  
6.  Schließen Sie die Anwendung.  
  
     Informationen zum Anzeigen des Ausgabefensters der Anwendung finden Sie unter [Ausgabefenster](https://docs.microsoft.com/visualstudio/ide/reference/output-window). Informationen zum Speicherort der Protokolldatei der Anwendung finden Sie unter [Walkthrough: Determining Where My.Application.Log Writes Information (Exemplarische Vorgehensweise: Bestimmen, wohin „My.Application.Log“ Informationen schreibt)](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-determining-where-my-application-log-writes-information.md).  
  
    > [!NOTE]
    >  Standardmäßig leert die Anwendung die Ausgabe der Protokolldatei, wenn die Anwendung geschlossen wird.  
  
     Im obigen Beispiel erzeugen der zweite Aufruf der Methode <xref:Microsoft.VisualBasic.Logging.Log.WriteEntry%2A> und der Aufruf der Methode <xref:Microsoft.VisualBasic.Logging.Log.WriteException%2A> Protokollausgaben. Der erste und letzte Aufruf der Methode `WriteEntry` erzeugt keine Ausgabe. Dies liegt daran, dass die Schweregrade von `WriteEntry` und `WriteException` „Information“ und „Error“ sind, die beide durch das standardmäßige Filtern von Protokollen des Objekts `My.Application.Log` zugelassen sind. Bei Ereignissen mit den Schweregraden „Start“ und „Stop“ wird das Erzeugen von Protokollausgaben verhindert.  
  
## <a name="filtering-for-all-myapplicationlog-listeners"></a>Filterung für alle „My.Application.Log“-Listener  
 Das Objekt `My.Application.Log` verwendet einen <xref:System.Diagnostics.SourceSwitch> mit dem Namen `DefaultSwitch`, um zu steuern, welche Meldungen von den Methoden `WriteEntry` und `WriteException` an die Protokolllistener übergeben werden. Sie können `DefaultSwitch` in der Konfigurationsdatei der Anwendung konfigurieren, indem Sie dessen Wert auf einen der <xref:System.Diagnostics.SourceLevels>-Enumerationswerte festlegen. Standardmäßig ist der Wert „Information“.  
  
 In dieser Tabelle werden die Schweregrade gezeigt, die von Protokollen für das Schreiben einer Nachricht an die Listener in einer bestimmten `DefaultSwitch`-Einstellung benötigt werden.  
  
|DefaultSwitch-Wert|Für die Ausgabe benötigter Schweregrad der Nachrichten|  
|---|---| 
|`Critical`|`Critical`|  
|`Error`|`Critical` oder `Error`|  
|`Warning`|`Critical`, `Error` oder `Warning`|  
|`Information`|`Critical`, `Error`, `Warning` oder `Information`|  
|`Verbose`|`Critical`, `Error`, `Warning`, `Information` oder `Verbose`|  
|`ActivityTracing`|`Start`, `Stop`, `Suspend`, `Resume` oder `Transfer`|  
|`All`|Alle Nachrichten sind zulässig.|  
|`Off`|Alle Nachrichten sind blockiert.|  
  
> [!NOTE]
>  Die Methoden `WriteEntry` und `WriteException` verfügen beide über eine Überladung, die keinen Schweregrad angibt. Der implizite Schweregrad für die Überladung `WriteEntry` ist „Information“, und der implizite Schweregrad für die Überladung `WriteException` ist „Error“.  
  
 In dieser Tabelle wird die im vorherigen Beispiel gezeigte Protokollausgabe erklärt: Mit der Standardeinstellung `DefaultSwitch` von „Information“ erzeugt nur der zweite Aufruf der Methode `WriteEntry` und der Aufruf der Methode `WriteException` eine Protokollausgabe.  
  
#### <a name="to-log-only-activity-tracing-events"></a>So protokollieren Sie nur Aktivitätsablauf-Verfolgungsereignisse  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf „app.config“, und wählen Sie **Öffnen** aus.  
  
     - oder -   
  
     Wenn keine app.config-Datei vorhanden ist:  
  
    1.  Klicken Sie im Menü **Projekt** auf **Neues Element hinzufügen**.  
  
    2.  Wählen Sie im Dialogfeld **Neues Element hinzufügen** den Eintrag **Anwendungskonfigurationsdatei**aus.  
  
    3.  Klicken Sie auf **Hinzufügen**.  
  
2.  Suchen Sie den Abschnitt `<switches>`, der sich im Abschnitt `<system.diagnostics>` im Abschnitt `<configuration>` der obersten Ebene befindet.  
  
3.  Suchen Sie das Element, das `DefaultSwitch` zur Auflistung von Schaltern hinzufügt. Es sollte in etwa wie dieses Element aussehen:  
  
     `<add name="DefaultSwitch" value="Information" />`  
  
4.  Ändern Sie den Wert des `value`-Attributs zu „ActivityTracing“.  
  
5.  Der Inhalt der app.config-Datei sollte ähnlich dem folgenden XML-Code sein:  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.diagnostics>  
        <sources>  
          <!-- This section configures My.Application.Log -->  
          <source name="DefaultSource" switchName="DefaultSwitch">  
            <listeners>  
              <add name="FileLog"/>  
            </listeners>  
          </source>  
        </sources>  
        <switches>  
          <add name="DefaultSwitch" value="ActivityTracing" />  
        </switches>  
        <sharedListeners>  
          <add name="FileLog"  
               type="Microsoft.VisualBasic.Logging.FileLogTraceListener,   
                     Microsoft.VisualBasic, Version=8.0.0.0,   
                     Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a,   
                     processorArchitecture=MSIL"   
               initializeData="FileLogWriter"/>  
        </sharedListeners>  
      </system.diagnostics>  
    </configuration>  
    ```  
  
6.  Führen Sie die Anwendung im Debugger aus.  
  
7.  Drücken Sie **Button1**.  
  
     Die Anwendung schreibt die folgenden Informationen in die Debugausgabe und die Protokolldatei der Anwendung:  
  
     `DefaultSource Start: 4 : Entering Button1_Click`  
  
     `DefaultSource Stop: 5 : Leaving Button1_Click`  
  
8.  Schließen Sie die Anwendung.  
  
9. Ändern Sie den Wert des `value`-Attributs zurück zu „Information“.  
  
    > [!NOTE]
    >  Die Einstellung des Schalters `DefaultSwitch` steuert nur `My.Application.Log`. Das Verhalten der [!INCLUDE[dnprdnshort](../../../../csharp/getting-started/includes/dnprdnshort_md.md)]-Klassen <xref:System.Diagnostics.Trace?displayProperty=fullName> und <xref:System.Diagnostics.Debug?displayProperty=fullName> wird nicht geändert.  
  
## <a name="individual-filtering-for-myapplicationlog-listeners"></a>Einzelne Filterung für alle „My.Application.Log“-Listener  
 Im vorherige Beispiel wurde gezeigt, wie Sie die Filterung für alle `My.Application.Log`-Ausgaben ändern können. In diesem Beispiel wird veranschaulicht, wie Sie einen einzelnen Protokolllistener filtern. Standardmäßig verwendet eine Anwendung zwei Listener, die in die Debugausgabe und die Protokolldatei der Anwendung schreiben.  
  
 Die Konfigurationsdatei steuert das Verhalten der Protokolllistener durch Zulassen eines Filters für jeden Listener, vergleichbar mit einem Schalter für `My.Application.Log`. Ein Protokolllistener gibt nur eine Meldung aus, wenn der Schweregrad der Meldung durch den `DefaultSwitch` des Protokolls und den Filter des Protokolllisteners zugelassen ist.  
  
 In diesem Beispiel wird veranschaulicht, wie Sie eine Filterung für einen neuen Debuglistener konfigurieren und zum `Log`-Objekt hinzufügen. Der standardmäßige Debuglistener sollte aus dem `Log`-Objekt entfernt werden. Hierdurch wird deutlich gemacht, dass die Debugmeldungen direkt vom neuen Debuglistener stammen.  
  
#### <a name="to-log-only-activity-tracing-events"></a>So protokollieren Sie nur Aktivitätsablaufverfolgungs-Ereignisse  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf „app.config“, und wählen Sie **Öffnen** aus.  
  
     - oder -   
  
     Wenn keine app.config-Datei vorhanden ist:  
  
    1.  Klicken Sie im Menü **Projekt** auf **Neues Element hinzufügen**.  
  
    2.  Wählen Sie im Dialogfeld **Neues Element hinzufügen** den Eintrag **Anwendungskonfigurationsdatei**aus.  
  
    3.  Klicken Sie auf **Hinzufügen**.  
  
2.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf „app.config“. Wählen Sie **Öffnen** aus.  
  
3.  Suchen Sie den Abschnitt `<listeners>` im Abschnitt `<source>` mit dem `name`-Attribut „DefaultSource“, das sich im Abschnitt `<sources>` befindet. Der Abschnitt `<sources>` befindet sich im Abschnitt `<system.diagnostics>` im Abschnitt `<configuration>` der obersten Ebene.  
  
4.  Fügen Sie dem Abschnitt `<listeners>` dieses Element hinzu:  
  
    ```xml  
    <!-- Remove the default debug listener. -->  
    <remove name="Default"/>  
    <!-- Add a filterable debug listener. -->  
    <add name="NewDefault"/>  
    ```  
  
5.  Suchen Sie den Abschnitt `<sharedListeners>` im `<system.diagnostics>` -Abschnitt im Abschnitt `<configuration>` der obersten Ebene.  
  
6.  Fügen Sie dem `<sharedListeners>` -Abschnitt dieses Element hinzu:  
  
    ```xml  
    <add name="NewDefault"   
         type="System.Diagnostics.DefaultTraceListener,   
               System, Version=2.0.0.0, Culture=neutral,   
               PublicKeyToken=b77a5c561934e089,   
               processorArchitecture=MSIL">  
        <filter type="System.Diagnostics.EventTypeFilter"   
                initializeData="Error" />  
    </add>  
    ```  
  
     Der Filter <xref:System.Diagnostics.EventTypeFilter> verwendet einen der <xref:System.Diagnostics.SourceLevels>-Enumerationswerte als `initializeData`-Attribut.  
  
7.  Der Inhalt der app.config-Datei sollte ähnlich dem folgenden XML-Code sein:  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.diagnostics>  
        <sources>  
          <!-- This section configures My.Application.Log -->  
          <source name="DefaultSource" switchName="DefaultSwitch">  
            <listeners>  
              <add name="FileLog"/>  
              <!-- Remove the default debug listener. -->  
              <remove name="Default"/>  
              <!-- Add a filterable debug listener. -->  
              <add name="NewDefault"/>  
            </listeners>  
          </source>  
        </sources>  
        <switches>  
          <add name="DefaultSwitch" value="Information" />  
        </switches>  
        <sharedListeners>  
          <add name="FileLog"  
               type="Microsoft.VisualBasic.Logging.FileLogTraceListener,   
                     Microsoft.VisualBasic, Version=8.0.0.0,   
                     Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a,   
                     processorArchitecture=MSIL"   
               initializeData="FileLogWriter"/>  
          <add name="NewDefault"   
               type="System.Diagnostics.DefaultTraceListener,   
                     System, Version=2.0.0.0, Culture=neutral,   
                     PublicKeyToken=b77a5c561934e089,   
                     processorArchitecture=MSIL">  
            <filter type="System.Diagnostics.EventTypeFilter"   
                    initializeData="Error" />  
          </add>  
        </sharedListeners>  
      </system.diagnostics>  
    </configuration>  
    ```  
  
8.  Führen Sie die Anwendung im Debugger aus.  
  
9. Drücken Sie **Button1**.  
  
     Die Anwendung schreibt die folgenden Informationen in die Protokolldatei der Anwendung:  
  
     `Default Information: 0 : In Button1_Click`  
  
     `Default Error: 2 : Error in the application.`  
  
     Die Anwendung schreibt aufgrund der restriktiveren Filterung weniger Informationen in die Debugausgabe.  
  
     `Default Error   2   Error`  
  
10. Schließen Sie die Anwendung.  
  
 Weitere Informationen zum Ändern der Protokolleinstellungen nach der Bereitstellung finden Sie unter [Working with Application Logs (Arbeiten mit Anwendungsprotokollen)](../../../../visual-basic/developing-apps/programming/log-info/working-with-application-logs.md).  
  
## <a name="see-also"></a>Siehe auch  
 [Walkthrough: Determining Where My.Application.Log Writes Information (Exemplarische Vorgehensweise: Bestimmen, wohin „My.Application.Log“ Informationen schreibt)](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-determining-where-my-application-log-writes-information.md)   
 [Walkthrough: Changing Where My.Application.Log Writes Information (Exemplarische Vorgehensweise: Ändern des Orts, in den „My.Application.Log“ Informationen schreibt)](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-changing-where-my-application-log-writes-information.md)   
 [Walkthrough: Creating Custom Log Listeners (Exemplarische Vorgehensweise: Erstellen von benutzerdefinierten Protokolllistenern)](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-creating-custom-log-listeners.md)   
 [How to: Write Log Messages (Vorgehensweise: Schreiben von Protokollmeldungen)](../../../../visual-basic/developing-apps/programming/log-info/how-to-write-log-messages.md)   
 [Trace Switches (Ablaufverfolgungsschalter)](http://msdn.microsoft.com/library/8ab913aa-f400-4406-9436-f45bc6e54fbe)   
 [Protokollieren von Informationen aus der Anwendung](../../../../visual-basic/developing-apps/programming/log-info/logging-information-from-the-application.md)
