---
title: "Sicherheit mit benutzerdefinierten Bindungen | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: a6383dff-4308-46d2-bc6d-acd4e18b4b8d
caps.latest.revision: 30
author: "BrucePerlerMS"
ms.author: "bruceper"
manager: "mbaldwin"
caps.handback.revision: 30
---
# Sicherheit mit benutzerdefinierten Bindungen
In diesem Beispiel wird veranschaulicht, wie die Sicherheitsfunktion mit einer benutzerdefinierten Bindung konfiguriert wird.Es zeigt, wie Sicherheit auf Nachrichtenebene und ein sicherer Transport mithilfe einer benutzerdefinierten Bindung aktiviert wird.Dies ist hilfreich, wenn ein sicherer Transport zum Übertragen von Nachrichten zwischen Client und Dienst erforderlich ist und daher die Nachrichten auf Nachrichtenebene gesichert werden müssen.Diese Konfiguration wird nicht von Bindungen unterstützt, die vom System bereitgestellt werden.  
  
 Dieses Beispiel besteht aus einem Clientkonsolenprogramm \(EXE\) und einem Dienstkonsolenprogramm \(EXE\).Der Dienst implementiert einen Duplexvertrag.Der Vertrag wird von der `ICalculatorDuplex`\-Schnittstelle definiert, die mathematische Operationen \(Addieren, Subtrahieren, Multiplizieren und Dividieren\) verfügbar macht.Durch die `ICalculatorDuplex`\-Schnittstelle kann der Client mathematische Operationen ausführen \(Berechnen eines laufenden Ergebnisses über eine Sitzung\).Unabhängig davon kann der Dienst Ergebnisse auf der `ICalculatorDuplexCallback`\-Schnittstelle zurückgeben.Ein Duplexvertrag erfordert eine Sitzung, da ein Kontext eingerichtet werden muss, um die zwischen dem Client und dem Dienst gesendeten Nachrichten in Beziehung zu setzen.Es wird eine benutzerdefinierte Bindung definiert, die Duplexkommunikation unterstützt und sicher ist.  
  
> [!NOTE]
>  Die Setupprozedur und die Buildanweisungen für dieses Beispiel befinden sich am Ende dieses Themas.  
  
 Die Dienstkonfiguration definiert eine benutzerdefinierte Bindung, die Folgendes unterstützt:  
  
-   TCP\-Kommunikation mit Schutz durch Verwendung des TLS\/SSL\-Protokolls.  
  
-   Windows\-Nachrichtensicherheit.  
  
 Durch die benutzerdefinierte Bindungskonfiguration wird der sichere Transport ermöglicht, da gleichzeitig die Sicherheit auf Nachrichtenebene aktiviert wird.Beim Definieren einer benutzerdefinierten Bindung ist die Reihenfolge der Bindungselemente von Bedeutung, da jedes eine Schicht im Kanalstapel darstellt \(siehe [Benutzerdefinierte Bindungen](../../../../docs/framework/wcf/extending/custom-bindings.md)\).Die benutzerdefinierte Bindung wird in den Dienst\- und Clientkonfigurationsdateien definiert, wie in der folgenden Beispielkonfiguration dargestellt.  
  
```  
<bindings>  
  <!-- Configure a custom binding. -->  
  <customBinding>  
    <binding name="Binding1">  
      <security authenticationMode="SecureConversation"  
                 requireSecurityContextCancellation="true">  
      </security>  
      <textMessageEncoding messageVersion="Soap12WSAddressing10" writeEncoding="utf-8"/>  
      <sslStreamSecurity requireClientCertificate="false"/>  
      <tcpTransport/>  
    </binding>  
  </customBinding>  
</bindings>  
```  
  
 Die benutzerdefinierte Bindung verwendet ein Dienstzertifikat zum Authentifizieren des Diensts auf Transportebene und zum Sichern der Nachrichten während der Übertragung zwischen Client und Dienst.Dies wird durch das `sslStreamSecurity`\-Bindungselement umgesetzt.Das Zertifikat des Diensts wird mit einem Dienstverhalten konfiguriert, wie in der folgenden Beispielkonfiguration dargestellt.  
  
```  
<behaviors>  
      <serviceBehaviors>  
        <behavior name="CalculatorServiceBehavior">  
          <serviceMetadata />  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <serviceCredentials>  
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName"/>  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  
```  
  
 Außerdem verwendet die benutzerdefinierte Bindung Nachrichtensicherheit mit dem Windows\-Anmeldeinformationstyp. Dies ist der Standard\-Anmeldeinformationstyp.Dies wird durch das `security`\-Bindungselement umgesetzt.Sowohl der Client als auch der Dienst werden mithilfe von Sicherheitsfunktionen auf Nachrichtenebene authentifiziert, wenn der Kerberos\-Authentifizierungsmechanismus verfügbar ist.Dies geschieht, wenn das Beispiel in der Active Directory\-Umgebung ausgeführt wird.Ist der Kerberos\-Authentifizierungsmechanismus nicht verfügbar, wird die NTLM\-Authentifizierung verwendet.NTLM authentifiziert den Client für den Dienst, authentifiziert aber nicht den Dienst für den Client.Das `security`\-Bindungselement ist für die Verwendung von `SecureConversation``authenticationType` konfiguriert. Daher wird eine Sicherheitssitzung auf dem Client und für den Dienst erstellt.Dies ist erforderlich, damit der Duplexvertrag des Diensts funktioniert.  
  
 Wenn Sie das Beispiel ausführen, werden die Anforderungen und Antworten für den Vorgang im Konsolenfenster des Clients angezeigt.Drücken Sie im Clientfenster die EINGABETASTE, um den Client zu schließen.  
  
```  
Press <ENTER> to terminate client.  
Result(100)  
Result(50)  
Result(882.5)  
Result(441.25)  
Equation(0 + 100 - 50 * 17.65 / 2 = 441.25)  
  
```  
  
 Wenn Sie das Beispiel ausführen, werden die vom Client zurückgegebenen Nachrichten in der vom Dienst gesendeten Rückrufschnittstelle angezeigt.Alle Zwischenergebnisse werden angezeigt, gefolgt von der ganzen Formel nach Abschluss aller Vorgänge.Drücken Sie die EINGABETASTE, um den Client zu schließen.  
  
 Mit der in diesem Beispiel enthaltenen Datei Setup.bat können Sie Client und Server mit relevanten Dienstzertifikaten zum Ausführen einer gehosteten Anwendung konfigurieren, die serverzertifikatbasierte Sicherheit erfordert.Diese Batchdatei muss angepasst werden, wenn sie computerübergreifend oder in einem nicht gehosteten Szenario verwendet werden soll.  
  
 Nachfolgend erhalten Sie einen kurzen Überblick über die verschiedenen Abschnitte der Batchdateien für dieses Beispiel, damit Sie sie so ändern können, dass sie in der entsprechenden Konfiguration ausgeführt werden:  
  
-   Erstellen des Serverzertifikats.  
  
     Mit den folgenden Zeilen aus der Datei "Setup.bat" wird das zu verwendende Serverzertifikat erstellt.Die Variable `%SERVER_NAME%` gibt den Servernamen an.Ändern Sie diese Variable, und geben Sie Ihren eigenen Servernamen an.In dieser Batchdatei ist der Servername standardmäßig auf localhost festgelegt.  
  
     Das Zertifikat wird im Speicher CurrentUser für im Internet gehostete Dienste gespeichert.  
  
    ```  
    echo ************  
    echo Server cert setup starting  
    echo %SERVER_NAME%  
    echo ************  
    echo making server cert  
    echo ************  
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe  
  
    ```  
  
-   Installieren Sie das Serverzertifikat im Speicher für vertrauenswürdige Zertifikate des Clients.  
  
     Mit den folgenden Zeilen in der Datei "Setup.bat" wird das Serverzertifikat in den Clientspeicher für vertrauenswürdige Personen kopiert.Dieser Schritt ist erforderlich, da von Makecert.exe generierten Zertifikaten nicht implizit vom Clientsystem vertraut wird.Wenn Sie bereits über ein Zertifikat verfügen, das von einem vertrauenswürdigen Clientstammzertifikat stammt \(z. B. ein von Microsoft ausgegebenes Zertifikat\), ist dieser Schritt zum Füllen des Clientzertifikatspeichers mit dem Serverzertifikat nicht erforderlich.  
  
    ```  
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
    > [!NOTE]
    >  Die Batchdatei Setup.bat ist darauf ausgelegt, an einer Visual Studio\-Eingabeaufforderung \(2010\) ausgeführt zu werden.Die MSSDK\-Umgebungsvariable muss auf das Verzeichnis zeigen, in dem das SDK installiert ist.Diese Umgebungsvariable ist innerhalb einer Visual Studio\-Eingabeaufforderung \(2010\) automatisch festgelegt.  
  
### So richten Sie das Beispiel ein, erstellen es und führen es aus  
  
1.  Stellen Sie sicher, dass Sie die [Einmaliges Setupverfahren für Windows Communication Foundation\-Beispiele](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md) ausgeführt haben.  
  
2.  Folgen Sie zum Erstellen der C\#\- bzw. Visual Basic .NET\-Version der Projektmappe den Anweisungen unter [Erstellen der Windows Communication Foundation\-Beispiele](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3.  Um das Beispiel in einer Konfiguration mit einem Computer oder computerübergreifend auszuführen, folgen Sie den Anweisungen unter [Durchführen der Windows Communication Foundation\-Beispiele](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
### So führen Sie das Beispiel auf demselben Computer aus  
  
1.  Öffnen Sie ein Visual Studio\-Eingabeaufforderungsfenster mit Administratorrechten, und führen Sie die Datei Setup.bat aus dem Beispielinstallationsordner aus.Hiermit werden alle Zertifikate installiert, die zum Ausführen des Beispiels erforderlich sind.  
  
    > [!NOTE]
    >  Die Batchdatei Setup.bat ist darauf ausgelegt, an einer [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)]\-Eingabeaufforderung ausgeführt zu werden.Die innerhalb der [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)]\-Eingabeaufforderung festgelegte PATH\-Umgebungsvariable zeigt auf das Verzeichnis mit den ausführbaren Dateien, die für das Skript Setup.bat erforderlich sind.  
  
2.  Starten Sie Service.exe aus dem Ordner \\service\\bin.  
  
3.  Starten Sie Client.exe aus dem Ordner \\client\\bin.In der Clientkonsolenanwendung wird Clientaktivität angezeigt.  
  
4.  Wenn der Client und der Dienst nicht miteinander kommunizieren können, finden Sie weitere Informationen unter [Troubleshooting Tips](http://msdn.microsoft.com/de-de/8787c877-5e96-42da-8214-fa737a38f10b).  
  
### So führen Sie das Beispiel computerübergreifend aus  
  
1.  Auf dem Dienstcomputer:  
  
    1.  Erstellen Sie auf dem Dienstcomputer ein virtuelles Verzeichnis mit dem Namen servicemodelsamples.  
  
    2.  Kopieren Sie die Dienstprogrammdateien aus \\inetpub\\wwwroot\\servicemodelsamples in das virtuelle Verzeichnis auf dem Dienstcomputer.Stellen Sie sicher, dass Sie die Dateien in das Unterverzeichnis \\bin kopieren.  
  
    3.  Kopieren Sie die Dateien Setup.bat und Cleanup.bat auf den Dienstcomputer.  
  
    4.  Öffnen Sie eine Visual Studio\-Eingabeaufforderung mit Administratorrechten, und führen Sie den folgenden Befehl aus: `Setup.bat service`.Hiermit wird das Dienstzertifikat erstellt, dessen Antragstellername mit dem Namen des Computers übereinstimmt, auf dem die Batchdatei ausgeführt wurde.  
  
        > [!NOTE]
        >  Die Batchdatei Setup.bat ist darauf ausgelegt, an einer Visual Studio\-Eingabeaufforderung \(2010\) ausgeführt zu werden.Die PATH\-Umgebungsvariable muss auf das Verzeichnis zeigen, in dem das SDK installiert ist.Diese Umgebungsvariable ist innerhalb einer Visual Studio\-Eingabeaufforderung \(2010\) automatisch festgelegt.  
  
    5.  Ändern Sie [\<serviceCertificate\>](../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) in der Datei Service.exe.config so, dass der Antragstellername des im vorherigen Schritt erstellten Zertifikats angegeben wird.  
  
    6.  Führen Sie Service.exe an einer Eingabeaufforderung aus.  
  
2.  Auf dem Clientcomputer:  
  
    1.  Kopieren Sie die Clientprogrammdateien aus dem Ordner \\client\\bin\\ auf den Clientcomputer.Kopieren Sie auch die Datei Cleanup.bat.  
  
    2.  Führen Sie Cleanup.bat aus, um alte Zertifikate aus vorherigen Beispielen zu entfernen.  
  
    3.  Exportieren Sie das Dienstzertifikat, indem Sie eine Visual Studio\-Eingabeaufforderung mit Administratorrechten öffnen und den folgenden Befehl auf dem Dienstcomputer ausführen \(ersetzen Sie `%SERVER_NAME%` durch den vollqualifizierten Namen des Computers, auf dem der Dienst ausgeführt wird\):  
  
        ```  
        certmgr -put -r LocalMachine -s My -c -n %SERVER_NAME% %SERVER_NAME%.cer  
        ```  
  
    4.  Kopieren Sie %SERVER\_NAME%.cer auf den Clientcomputer \(ersetzen Sie %SERVER\_NAME% durch den vollqualifizierten Namen des Computers, auf dem der Dienst ausgeführt wird\).  
  
    5.  Importieren Sie das Dienstzertifikat, indem Sie eine Visual Studio\-Eingabeaufforderung mit Administratorrechten öffnen und den folgenden Befehl auf dem Clientcomputer ausführen \(ersetzen Sie %SERVER\_NAME% durch den vollqualifizierten Namen des Computers, auf dem der Dienst ausgeführt wird\):  
  
        ```  
        certmgr.exe -add -c %SERVER_NAME%.cer -s -r CurrentUser TrustedPeople  
        ```  
  
         Die Schritte c, d und e sind nicht erforderlich, wenn das Zertifikat von einem vertrauenswürdigen Aussteller stammt.  
  
    6.  Ändern Sie die Datei App.config des Clients wie folgt:  
  
        ```  
        <client>  
            <endpoint name="default"  
                address="net.tcp://ReplaceThisWithServiceMachineName:8000/ServiceModelSamples/Service"   
                binding="customBinding"   
                bindingConfiguration="Binding1"   
                contract="Microsoft.ServiceModel.Samples.ICalculatorDuplex"  
        behaviorConfiguration="CalculatorClientBehavior" />  
        </client>  
  
        ```  
  
    7.  Wenn der Dienst unter einem anderen Konto als NetworkService oder LocalSystem in einer Domänenumgebung ausgeführt wird, müssen Sie möglicherweise die Endpunktidentität für den Dienstendpunkt in der Datei App.config des Clients ändern und den entsprechenden UPN oder SPN für das Konto angeben, unter dem der Dienst ausgeführt wird.Weitere Informationen zur Endpunktidentität finden Sie im Thema [Dienstidentität und Authentifizierung](../../../../docs/framework/wcf/feature-details/service-identity-and-authentication.md).  
  
    8.  Führen Sie an einer Eingabeaufforderung Client.exe aus.  
  
### So stellen Sie den Zustand vor Ausführung des Beispiels wieder her  
  
-   Führen Sie Cleanup.bat im Beispielordner aus, nachdem Sie die Ausführung des Beispiels abgeschlossen haben.  
  
## Siehe auch