---
type: fundamentals
tags: SOLID
---

# Open-closed principle

The open-closed principle is the second letter of the [[solid-principles]].

## Meaning

Robert Martin cites Bertrand Meyer's definition for the open-closed principle.

> "A module should be open for extension but closed for modification".

The idea is that it should be easy to extend the behavior of an existing module without having to modify it's source code. Having to modify the source code to change behavior means recompilation and redeployment of the changes, which is an indication of rigidity.

## Example

Polymorphic open-closed principle is the preferred approach.

Imagine that we had the following example:

```java
class SimplePrinter {
  void print(String content) {
    ...
  }
}

class ReportProcessor {
  private SimplePrinter printer;

  public ReportProcessor(SimplePrinter printer) {
    this.printer = printer;
  }

  void printReport() {
    String content = generateReportContent();
    this.printer.print(content)
  }
}
```

There is a `SimplePrinter` class which exposes a `print()` function that prints out a string using a basic printer machine. This class is currently used directly in a `ReportProcessor` class.

Imagine in the future that there's new printers coming into the picture. Ideally, the same content can be printed on many different printers. However, with the implementation as is, switching to another printer (say `SophisticatedPrinter`) involves having to change the code in `ReportProcessor`.

A different design that adheres to the open-closed principle would be to extract a `Printer` interface and then have many different classes that implement the interface:

```java
public interface Printer {
  void print(String content);
}

class SimplePrinter implements Printer {
  void print(String content) {
    ...
  }
}

class AdvancedPrinter implements Printer {
  void print(String content) {
    ...
  }
}

class InkjetPrinter implements Printer {
  void print(String content) {
    ...
  }
}
```

Clients of various `Printer`s will use the interface rather than an implementation of it:

```java
public class ReportProcessor {
  private Printer printer;

  public ReportProcessor(Printer printer) {
    this.printer = printer;
  }

  void printReport() {
    String content = generateReportContent();
    this.printer.print(content)
  }
}
```

With this design, we can support all kinds of printers in the future and we would not need to modify the code in `ReportProcessor` in order to use it, as long as they implement the `Printer` interface.

This example is an instance of a polymorphic open-closed principle. The declared interface is closed to modifications but is open to extension since it is possible to create a new class that, at minimum, implements that interface. It is common to leverage a programming technique called [[dependency-injection]] with this type of design.

[//begin]: # "Autogenerated link references for markdown compatibility"
[solid-principles]: solid-principles.md "SOLID principles"
[//end]: # "Autogenerated link references"
