---
title: 'Vorgehensweise: Steuern des Typs einer Projektion (C#) Microsoft-Dokumentation'
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: e4db6b7e-4cc9-4c8f-af85-94acf32aa348
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 465a04338de0a092bf60e15ce07a9aace160bbe3
ms.lasthandoff: 03/13/2017


---
# <a name="how-to-control-the-type-of-a-projection-c"></a>Vorgehensweise: Steuern des Typs einer Projektion (C#)
Bei einer Projektion wird ein Satz von Daten gefiltert und in der Form und sogar im Typ geändert. Die meisten Abfrageausdrücke führen Projektionen aus. Bei den meisten Abfrageausdrücken in diesem Abschnitt ist das Ergebnis der Auswertung <xref:System.Collections.Generic.IEnumerable%601> von <xref:System.Xml.Linq.XElement>, aber Sie können festlegen, welcher Projektionstyp verwendet werden soll, um Auflistungen eines anderen Typs zu erstellen. In diesem Thema wird gezeigt, wie Sie dazu vorgehen müssen.  
  
## <a name="example"></a>Beispiel  
 Das folgende Beispiel definiert einen neuen Typ: `Customer`. Der Abfrageausdruck instanziiert dann in der `Customer`-Klausel neue `Select`-Objekte. Dadurch wird der Typ des Abfrageausdrucks <xref:System.Collections.Generic.IEnumerable%601> von `Customer`.  
  
 In diesem Beispiel wird das folgende XML-Dokument verwendet: [Beispiel-XML-Datei: Kunden und Bestellungen (LINQ to XML)](../../../../csharp/programming-guide/concepts/linq/sample-xml-file-customers-and-orders-linq-to-xml-2.md).  
  
```csharp  
public class Customer  
{  
    private string customerID;  
    public string CustomerID{ get{return customerID;} set{customerID = value;}}  
  
    private string companyName;  
    public string CompanyName{ get{return companyName;} set{companyName = value;}}  
  
    private string contactName;  
    public string ContactName { get{return contactName;} set{contactName = value;}}  
  
    public Customer(string customerID, string companyName, string contactName)  
    {  
        CustomerID = customerID;  
        CompanyName = companyName;  
        ContactName = contactName;  
    }  
  
    public override string ToString()  
    {  
        return String.Format("{0}:{1}:{2}", this.customerID, this.companyName, this.contactName);  
    }  
}  
  
class Program  
{  
    static void Main(string[] args)  
    {  
        XElement custOrd = XElement.Load("CustomersOrders.xml");  
        IEnumerable<Customer> custList =  
            from el in custOrd.Element("Customers").Elements("Customer")  
            select new Customer(  
                (string)el.Attribute("CustomerID"),  
                (string)el.Element("CompanyName"),  
                (string)el.Element("ContactName")  
            );  
        foreach (Customer cust in custList)  
            Console.WriteLine(cust);  
    }  
}  
```  
  
 Dieser Code erzeugt die folgende Ausgabe:  
  
```  
GREAL:Great Lakes Food Market:Howard Snyder  
HUNGC:Hungry Coyote Import Store:Yoshi Latimer  
LAZYK:Lazy K Kountry Store:John Steel  
LETSS:Let's Stop N Shop:Jaime Yorres  
```  
  
## <a name="see-also"></a>Siehe auch  
 <xref:System.Linq.Enumerable.Select%2A>   
 [Projektionen und Transformationen (LINQ to XML) (C#)](../../../../csharp/programming-guide/concepts/linq/projections-and-transformations-linq-to-xml.md)
