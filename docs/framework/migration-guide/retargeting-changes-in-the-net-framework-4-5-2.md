---
title: "Änderungen der Neuzuweisungen in .NET Framework 4.5.2 | Microsoft-Dokumentation"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 2be2a828-9052-4738-a965-d4a836cc6384
caps.latest.revision: 5
author: rpetrusha
ms.author: ronpet
manager: wpickett
translationtype: Human Translation
ms.sourcegitcommit: 9f5b8ebb69c9206ff90b05e748c64d29d82f7a16
ms.openlocfilehash: 10af942724ce0207bc6e64f1ebabfdcd2d3488bd
ms.lasthandoff: 04/18/2017

---
# <a name="retargeting-changes-in-the-net-framework-452"></a>Neuzuweisung von Änderungen in .NET Framework 4.5.2
In seltenen Fällen wirken sich Neuausrichtungsänderungen auf Apps aus, die für das .NET Framework 4.5.2 neu kompiliert wurden. Sofern in den folgenden Tabellen nicht anderweitig angegeben, wirken sich diese Änderungen nicht auf Binärdateien aus, die eine frühere Version von .NET Framework als Ziel haben, jedoch in Version 4.5.2 ausgeführt werden. Das .NET Framework 4.5.2 umfasst Neuausrichtungsänderungen in den folgenden Bereichen:  
  
-   [Entity Framework](#EF)  
  
-   [Windows Forms](#WinForms)  
  
<a name="EF"></a>   
## <a name="entity-framework-retargeting-changes"></a>Entity Framework-Neuausrichtungsänderungen  
  
|Funktion|Änderung|Auswirkungen|Bereich|  
|-------------|------------|------------|-----------|  
|Einfachere Abfragen mit unsortierten Einschränkungsoperationen|Abfragen, die `JOIN`-Anweisungen erzeugen und einen Aufruf an eine Einschränkungsoperation enthalten, ohne zuvor <xref:System.Data.Objects.ObjectQuery%601.OrderBy%2A> zu verwenden, generieren nun einfacheres SQL. Nach dem Upgrade auf [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] generieren diese Abfragen komplizierteres SQL als vorherige Versionen.|Dieses Feature ist standardmäßig deaktiviert. Wenn Entity Framework zusätzliche `JOIN`-Anweisungen generiert, die Leistungseinbußen verursachen, können Sie dieses Feature aktivieren, indem Sie den folgenden Eintrag zum `<appSettings>`-Bereich der Anwendungskonfigurationsdatei (app.config) hinzufügen:<br /><br /> `<add key="EntityFramework_SimplifyLimitOperations" value="true" />`|Gering|  
  
<a name="WinForms"></a>   
## <a name="windows-forms-retargeting-changes"></a>Windows Forms-Neuausrichtungsänderungen  
  
|Funktion|Änderung|Auswirkungen|Bereich|  
|-------------|------------|------------|-----------|  
|Abrufen von Daten im HTML-Format aus der Zwischenablage mit der <xref:System.Windows.Forms.DataObject.GetData%2A?displayProperty=fullName>-Methode|Für Apps, die auf [!INCLUDE[net_v40_short](../../../includes/net-v40-short-md.md)] ausgerichtet sind oder unter .NET Framework 4.5.1 oder älteren Versionen ausgeführt werden, ruft <xref:System.Windows.Forms.DataObject.GetData%2A?displayProperty=fullName> entsprechende HTML-formatierte Daten als ASCII-Zeichenfolge ab. Als Resultat werden nicht-ASCII-Zeichen (Zeichen mit ASCII-Codes größer als 0x7F) durch zwei zufällige Zeichen dargestellt. é (0xE9) wird z. B. als Ã© (0xC3 0xA9) dargestellt.<br /><br /> In Apps, die [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] oder eine neuere Version als Ziel verwenden und unter .NET Framework 4.5.2 ausgeführt werden, ruft <xref:System.Windows.Forms.DataObject.GetData%2A?displayProperty=fullName> HTML-formatierte Daten als UTF-8 ab und stellt Zeichen größer als 0x7F korrekt dar.|Wenn Sie eine Problemumgehung für das Codierungsproblem mit HTML-formatierten Zeichenfolgen implementiert haben (z. B. durch explizites Codieren der HTML-Zeichenfolge aus der Zwischenablage durch Übergabe an die <xref:System.Text.UTF8Encoding.GetString%2A?displayProperty=fullName>-Methode) und das Ziel Ihrer App von Version 4 zu 4.5 ändern, sollten Sie diese Problemumgehung entfernen.|Gering|  
  
## <a name="see-also"></a>Siehe auch  
 [Laufzeitänderungen](../../../docs/framework/migration-guide/runtime-changes-in-the-net-framework-4-5-2.md)   
 [Anwendungskompatibilität in 4.5](../../../docs/framework/migration-guide/application-compatibility-in-the-net-framework-4-5.md)   
 [Anwendungskompatibilität in 4.5.1](../../../docs/framework/migration-guide/application-compatibility-in-the-net-framework-4-5-1.md)
