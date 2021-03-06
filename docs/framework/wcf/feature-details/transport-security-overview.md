---
title: "&#220;bersicht &#252;ber die Transportsicherheit | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 00959326-aa9d-44d0-af61-54933d4adc7f
caps.latest.revision: 23
author: "BrucePerlerMS"
ms.author: "bruceper"
manager: "mbaldwin"
caps.handback.revision: 23
---
# &#220;bersicht &#252;ber die Transportsicherheit
Transportsicherheitsmechanismen in [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] hängen von der Bindung und dem verwendeten Transport ab. Z. B. bei Verwendung der <xref:System.ServiceModel.WSHttpBinding> -Klasse, der Transport ist HTTP, und der primäre Mechanismus zum Sichern des Transports ist Secure Sockets Layer (SSL) über HTTP, Allgemein als HTTPS bezeichnet. In diesem Thema werden die wichtigsten Sicherheitsmechanismen beschrieben, die für die vom [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]-System bereitgestellten Bindungen verwendet werden.  
  
> [!NOTE]
>  Wenn SSL-Sicherheit in Verbindung mit .NET Framework 3.5 und höher verwendet wird, verwenden [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]-Clients später zur Validierung des Dienstzertifikats mittels einer Zertifikatskette sowohl die Zwischenzertifikate aus dem lokalen Zertifikatspeicher als auch die Zwischenzertifikate, die sie im Rahmen der SSL-Verhandlung empfangen haben. Bei .NET Framework 3.0 werden nur die im lokalen Zertifikatspeicher installierten Zwischenzertifikate verwendet.  
  
> [!WARNING]
>  Wenn transportsicherheit verwendet wird, die <xref:System.Treading.Thread.CurrentPrincipal%2A> -Eigenschaft kann überschrieben werden. Um dies zu verhindern legen die <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.PrincipalPermission%2A> auf None. <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> ist ein Dienstverhalten, die für die dienstbeschreibung festgelegt werden kann.  
  
## <a name="basichttpbinding"></a>BasicHttpBinding  
 In der Standardeinstellung die <xref:System.ServiceModel.BasicHttpBinding> -Klasse keine Sicherheit geboten. Diese Bindung ist für die Interoperabilität mit Webdienstanbietern ausgelegt, die keine Sicherheit implementieren. Sie können jedoch für die Sicherheit wechseln, durch Festlegen der <xref:System.ServiceModel.BasicHttpSecurity.Mode%2A> -Eigenschaft auf einen beliebigen Wert außer <xref:System.ServiceModel.BasicHttpSecurityMode>. Um transportsicherheit zu aktivieren, legen Sie die Eigenschaft auf <xref:System.ServiceModel.BasicHttpSecurityMode>.  
  
### <a name="interoperation-with-iis"></a>Interoperieren mit IIS  
 Die <xref:System.ServiceModel.BasicHttpBinding> Klasse wird hauptsächlich für die Interoperabilität mit vorhandenen Webdiensten und viele dieser Dienste von Internet Information Services (IIS) gehostet werden. Deshalb ist die Transportsicherheit für diese Bindung auf die nahtlose Interoperation mit IIS-Websites ausgerichtet. Dies geschieht durch Festlegen des Sicherheitsmodus auf <xref:System.ServiceModel.BasicHttpSecurityMode> festlegen und anschließend die Client-Anmeldeinformationstyp. Die Werte für die Anmeldeinformationstypen entsprechen den IIS-Verzeichnissicherheitsmechanismen. Der folgende Code zeigt den festgelegten Modus und einen Anmeldeinformationstyp, der auf Windows festgelegt ist. Sie können diese Konfiguration verwenden, wenn sich Client und Server in derselben Windows-Domäne befinden.  
  
 [!code-csharp[c_ProgrammingSecurity#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#10)]
 <!-- TODO: review snippet reference [!code-vb[c_ProgrammingSecurity#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#10)]  -->  
  
 Oder in der Konfiguration:  
  
```xml  
<bindings>  
  <basicHttpBinding>  
    <binding name="SecurityByTransport">  
      <security mode="Transport">  
        <transport clientCredentialType="Windows" />  
       </security>  
     </binding>  
  </basicHttpBinding>  
</bindings>  
```  
  
 In den folgenden Abschnitten werden andere Client-Anmeldeinformationstypen erläutert.  
  
#### <a name="basic"></a>Standard  
 Dies entspricht der Authentifizierungsmethode Standard in IIS. Wenn Sie diesen Modus verwenden, muss der IIS-Server mit Windows-Benutzerkonten und den entsprechenden NTFS-Dateisystemberechtigungen konfiguriert sein. [!INCLUDE[crabout](../../../../includes/crabout-md.md)][!INCLUDE[iis601](../../../../includes/iis601-md.md)], finden Sie unter [Aktivieren der Standardauthentifizierung und Konfigurieren des Bereichsnamens](http://go.microsoft.com/fwlink/?LinkId=88592). [!INCLUDE[crabout](../../../../includes/crabout-md.md)][!INCLUDE[iisver](../../../../includes/iisver-md.md)], finden Sie unter [IIS 7.0 Beta: Konfigurieren der Standardauthentifizierung](http://go.microsoft.com/fwlink/?LinkId=88593).  
  
#### <a name="certificate"></a>Zertifikat  
 IIS verfügt über eine Option, mit der sich die Clients mit einem Zertifikat anmelden müssen. Mit dieser Funktion können die Internetinformationsdienste auch einem Windows-Konto ein Clientzertifikat zuordnen. [!INCLUDE[crabout](../../../../includes/crabout-md.md)][!INCLUDE[iis601](../../../../includes/iis601-md.md)], finden Sie unter [Aktivieren von Clientzertifikaten in IIS 6.0](http://go.microsoft.com/fwlink/?LinkId=88594). [!INCLUDE[crabout](../../../../includes/crabout-md.md)][!INCLUDE[iisver](../../../../includes/iisver-md.md)], finden Sie unter [IIS 7.0 Beta: Konfigurieren von Serverzertifikaten in IIS 7.0](http://go.microsoft.com/fwlink/?LinkId=88595).  
  
#### <a name="digest"></a>Hashwert  
 Die Hashwertauthentifizierung ähnelt der Standardauthentifizierung, bietet jedoch den Vorteil, die Anmeldeinformationen als Hash zu senden und nicht als Klartext. [!INCLUDE[crabout](../../../../includes/crabout-md.md)][!INCLUDE[iis601](../../../../includes/iis601-md.md)], finden Sie unter [die Digestauthentifizierung in IIS 6.0](http://go.microsoft.com/fwlink/?LinkID=88443). [!INCLUDE[crabout](../../../../includes/crabout-md.md)][!INCLUDE[iisver](../../../../includes/iisver-md.md)], finden Sie unter [IIS 7.0 Beta: Konfigurieren der Digestauthentifizierung](http://go.microsoft.com/fwlink/?LinkId=88596).  
  
#### <a name="windows"></a>Windows  
 Dies entspricht der integrierten Windows-Authentifizierungsmethode in IIS. Bei dieser Methode muss sich auch der Server in einer Windows-Domäne befinden, die das Kerberos-Protokoll als Domänencontroller verwendet. Falls sich der Server nicht in einer Kerberos-Domäne befindet oder falls das Kerberos-System fehlschlägt, können Sie den im nächsten Abschnitt beschriebenen NTLM-Wert verwenden. [!INCLUDE[crabout](../../../../includes/crabout-md.md)][!INCLUDE[iis601](../../../../includes/iis601-md.md)], finden Sie unter [integrierte Windows-Authentifizierung in IIS 6.0](http://go.microsoft.com/fwlink/?LinkId=88597). [!INCLUDE[crabout](../../../../includes/crabout-md.md)][!INCLUDE[iisver](../../../../includes/iisver-md.md)], finden Sie unter [IIS 7.0 Beta: Konfigurieren von Serverzertifikaten in IIS 7.0](http://go.microsoft.com/fwlink/?LinkId=88595).  
  
#### <a name="ntlm"></a>NTLM  
 Dadurch kann der Server NTLM für die Authentifizierung verwenden, falls das Kerberos-Protokoll fehlschlägt. [!INCLUDE[crabout](../../../../includes/crabout-md.md)]Konfigurieren von IIS in [!INCLUDE[iis601](../../../../includes/iis601-md.md)], finden Sie unter [Erzwingen der NTLM-Authentifizierung](http://go.microsoft.com/fwlink/?LinkId=88598). Im Falle von [!INCLUDE[iisver](../../../../includes/iisver-md.md)] schließt die Windows-Authentifizierung die NTLM-Authentifizierung ein. [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][IIS 7.0 Beta: Konfigurieren von Serverzertifikaten in IIS 7.0](http://go.microsoft.com/fwlink/?LinkID=88595).  
  
## <a name="wshttpbinding"></a>WsHttpBinding  
 Die <xref:System.ServiceModel.WSHttpBinding> Klasse dient zur Zusammenarbeit mit Diensten, die Implementierung von WS-* Spezifikationen. Die Transportsicherheit für diese Bindung ist SSL (Secure Sockets Layer) über HTTP oder HTTPS. Um eine [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]-Anwendung zu erstellen, die SSL verwendet, verwenden Sie IIS, um die Anwendung zu hosten. Wenn Sie eine selbst gehostete Anwendung erstellen, können Sie mit dem Tool HttpCfg.exe ein X.509-Zertifikat an einen bestimmten Anschluss eines Computers binden. Die Anschlussnummer wird als Teil der [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]-Anwendung als Endpunktadresse angegeben. Bei Verwendung des Transportmodus muss die Endpunktadresse das HTTPS-Protokoll enthalten. Andernfalls wird zur Laufzeit eine Ausnahme ausgelöst. [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][HTTP-Transportsicherheit](../../../../docs/framework/wcf/feature-details/http-transport-security.md).  
  
 Legen Sie für die Clientauthentifizierung der <xref:System.ServiceModel.HttpTransportSecurity.ClientCredentialType%2A> Eigenschaft der <xref:System.ServiceModel.HttpTransportSecurity> -Klasse auf einen der der <xref:System.ServiceModel.HttpClientCredentialType> -Enumerationswerte. Die Enumerationswerte sind identisch mit den Typen von Clientanmeldeinformationen für <xref:System.ServiceModel.BasicHttpBinding> und für die Verwendung mit IIS-Dienste gehostet werden.  
  
 Das folgende Beispiel zeigt die mit einem Client-Anmeldeinformationstyp von Windows verwendete Bindung.  
  
 [!code-csharp[c_ProgrammingSecurity#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#11)]
 [!code-vb[c_ProgrammingSecurity#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#11)]  
  
## <a name="wsdualhttpbinding"></a>WSDualHttpBinding  
 Diese Bindung bietet lediglich Sicherheit auf Nachrichtenebene, nicht Sicherheit auf Transportebene.  
  
## <a name="nettcpbinding"></a>NetTcpBinding  
 Die <xref:System.ServiceModel.NetTcpBinding> -Klasse verwendet TCP für den Nachrichtentransport. Die Sicherheit für den Transportmodus wird über die Implementierung von TLS über TCP bereitgestellt. Die TLS-Implementierung wird vom Betriebssystem bereitgestellt.  
  
 Sie können auch die Client Anmeldeinformationstyp angeben, durch Festlegen der <xref:System.ServiceModel.TcpTransportSecurity.ClientCredentialType%2A> Eigenschaft der <xref:System.ServiceModel.TcpTransportSecurity> -Klasse auf einen der der <xref:System.ServiceModel.TcpClientCredentialType> Werte, wie im folgenden Code gezeigt.  
  
 [!code-csharp[c_ProgrammingSecurity#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#12)]
 [!code-vb[c_ProgrammingSecurity#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#12)]  
  
#### <a name="client"></a>Client  
 Auf dem Client müssen Sie angeben, ein Zertifikat mithilfe der <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> Methode der <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential> Klasse.  
  
> [!NOTE]
>  Wenn Sie die Windows-Sicherheit verwenden, ist kein Zertifikat erforderlich.  
  
 Der folgende Code verwendet den Fingerabdruck des Zertifikats, der es eindeutig identifiziert. [!INCLUDE[crabout](../../../../includes/crabout-md.md)]Zertifikate, finden Sie unter [arbeiten mit Zertifikaten](../../../../docs/framework/wcf/feature-details/working-with-certificates.md).  
  
 [!code-csharp[c_ProgrammingSecurity#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#13)]
 [!code-vb[c_ProgrammingSecurity#13](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#13)]  
  
 Alternativ können Sie das Zertifikat in der Konfiguration des Clients angeben mithilfe einer [ <> \> ](../../../../docs/framework/configure-apps/file-schema/wcf/clientcredentials.md) Element im verhaltensabschnitt.  
  
```xml  
<behaviors>  
  <behavior>  
   <clientCredentials>  
     <clientCertificate findValue= "101010101010101010101010101010000000000"   
      storeLocation="LocalMachine" storeName="My"   
      X509FindType="FindByThumbPrint"/>  
     </clientCertificate>  
   </clientCredentials>  
 </behavior>  
</behaviors>    
```  
  
## <a name="netnamedpipebinding"></a>NetNamedPipeBinding  
 Die <xref:System.ServiceModel.NetNamedPipeBinding> -Klasse kann für die effiziente Kommunikation; d. h. für Prozesse, die auf dem gleichen Computer ausgeführt wird, obwohl die named Pipe-Kanäle kann erstellt werden zwischen zwei Computern im selben Netzwerk. Diese Bindung bietet lediglich Sicherheit auf Transportebene. Wenn Sie mit dieser Bindung Anwendungen erstellen, müssen die Endpunktadressen "net.pipe" als Protokoll der Endpunktadresse enthalten.  
  
## <a name="wsfederationhttpbinding"></a>WSFederationHttpBinding  
 Wenn Sie transportsicherheit verwenden, verwendet diese Bindung SSL über HTTP, bekannt als HTTPS mit einem ausgestellten Token (<xref:System.ServiceModel.WSFederationHttpSecurityMode>). [!INCLUDE[crabout](../../../../includes/crabout-md.md)]verbundanwendungen finden Sie unter [Verbund und ausgestellte Token](../../../../docs/framework/wcf/feature-details/federation-and-issued-tokens.md).  
  
## <a name="netpeertcpbinding"></a>NetPeerTcpBinding  
 Die <xref:System.ServiceModel.NetPeerTcpBinding> Klasse ist ein sicherer Transport, die für eine effiziente Kommunikation, die die Peer-to-Peer-Netzwerkfunktion entwickelt wurde. Wie anhand des Namens der Klasse und der Bindung angegeben, ist TCP das Protokoll. Wenn der Sicherheitsmodus auf Transport festgelegt ist, implementiert die Bindung TLS über TCP. [!INCLUDE[crabout](../../../../includes/crabout-md.md)]die Peer-to-Peer-Funktion finden Sie unter [Peer-zu-Peer-Netzwerk](../../../../docs/framework/wcf/feature-details/peer-to-peer-networking.md).  
  
## <a name="msmqintegrationbinding-and-netmsmqbinding"></a>MsmqIntegrationBinding und NetMsmqBinding  
 Eine vollständige Erläuterung der transportsicherheit mit Message Queuing (früher als MSMQ bezeichnet), finden Sie unter [Sichern von Nachrichten mit Transportsicherheit](../../../../docs/framework/wcf/feature-details/securing-messages-using-transport-security.md).  
  
## <a name="see-also"></a>Siehe auch  
 [Programmieren der WCF-Sicherheit](../../../../docs/framework/wcf/feature-details/programming-wcf-security.md)