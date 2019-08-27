---
layout: post
title: Фасада (Facade Pattern)
---

# Вовед
Оваа шема дефинира начин преку кој ќе се упрости пристапот кон под-системите во еден софтвер.
Со други зборови, имаме едноставен интерфејс преку кој пристапуваме кон другите, посложени интерфејси во целиот систем.

![facade-pattern](/assets/img/facade.jpg)

# Примери

* Наједноставен пример за Facade е вклучување на компјутер. Ние го притискаме копчето и не се грижиме за процесите што се случуваат внатре во куќиштето. Ова е пример за фасада.

* Во Java, за да направиме query до база, прво треба да земеме инстанца од конекцијата со методот `dataSource.getConnection()`.
 Овој метод позади себе содржи лоадирање на драјверот, креирање конекција, update на конфигурациите и слично, ама на крај ние ја добиваме само инстанцата која ќе ја користиме за повик до база. Не се грижиме за внатрешните работи.

# Кога да се користи

1. Кога имаме потреба од едноставен интерфејс на некој комплициран под-систем.
2. Кога сакаме да креираме нивоа во нашиот софтвер
3. За полесна интеракција помеѓу клиентот и нашиот софтвер.

# Talk is cheap, show me the code

За пример ќе креираме едноставен систем за генерирање извештаи во PDF или Word формат.
Овој систем ќе користи различни класи за креирање header, footer и data на извештајот.

За да го поедноставиме креирањето извештај, ќе креираме `ReportGeneratorFacade` класа која ќе ги повика другите методи во другите класи и на крај ќе врати комплетен извештај.

`Report.java`

```java
public class Report {

    private ReportHeader header;
    private ReportData data;
    private ReportFooter footer;

    public ReportHeader getHeader() {
        return header;
    }
    public void setHeader(ReportHeader header) {
        System.out.println("Setting report header");
        this.header = header;
    }
    public ReportData getData() {
        return data;
    }
    public void setData(ReportData data) {
        System.out.println("Setting report data");
        this.data = data;
    }
    public ReportFooter getFooter() {
        return footer;
    }
    public void setFooter(ReportFooter footer) {
        System.out.println("Setting report footer");
        this.footer = footer;
    }
}
```

`ReportHeader.java`

```java
public class ReportHeader {
}
```

`ReportFooter.java`

```java
public class ReportFooter {
}
```

`ReportData.java`

```java
public class ReportData {
}
```

`ReportType.java`

```java
public enum ReportType
{
    PDF, HTML
}
```

`ReportWriter.java`

```java
public class ReportWriter {

    public void writeHtmlReport(Report report, String location) {
        System.out.println("HTML Report written");
        //implementation
    }

    public void writePdfReport(Report report, String location) {
        System.out.println("Pdf Report written");
        //implementation
    }
}
```

`ReportGeneratorFacade.java`

```java
import javax.activation.DataSource;

public class ReportGeneratorFacade
{
    public static void generateReport(ReportType type, DataSource dataSource, String location)
    {
        if(type == null || dataSource == null)
        {
            //throw some exception
        }
        //Create report
        Report report = new Report();

        report.setHeader(new ReportHeader());
        report.setFooter(new ReportFooter());

        //Get data from dataSource and set to ReportData object

        report.setData(new ReportData());

        //Write report
        ReportWriter writer = new ReportWriter();
        switch(type)
        {
            case HTML:
                writer.writeHtmlReport(report, location);
                break;

            case PDF:
                writer.writePdfReport(report, location);
                break;
        }
    }
}
```

`Main.java`
```java
import com.howtodoinjava.facade.ReportGeneratorFacade;
import com.howtodoinjava.facade.ReportType;
 
public class Main
{
    public static void main(String[] args) throws Exception
    {
        ReportGeneratorFacade reportGeneratorFacade = new ReportGeneratorFacade();
         
        reportGeneratorFacade.generateReport(ReportType.HTML, null, null);
         
        reportGeneratorFacade.generateReport(ReportType.PDF, null, null);
    }
}
```

And the program output:
```
Setting report header
Setting report footer
Setting report data
HTML Report written
 
 
Setting report header
Setting report footer
Setting report data
Pdf Report written
```

Ова е еден од таканаречените *Structural patterns*, дел од шемите кои се разработени во најпознатата книга за шеми на креирање на софтвер: **Design Patterns: Elements of Reusable Object-Oriented Software**.

За секој кој бара повеќе информации или сака да научи нешто повеќе, оваа книга е must read.