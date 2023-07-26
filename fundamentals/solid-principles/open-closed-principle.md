---
type: fundamentals
tags: fundamentals open-closed-principle
---

# Open-closed principle

The open-closed principle is the second letter of the SOLID principles.

## Meaning

Robert "Uncle Bob" Martin cites Bertrand Meyer's definition for the open-closed principle. To paraphrase it: "a module should be open for extension but closed for modification".

The core idea is that it should be easy to extend the behavior of an existing module without having to modify it's source code. Having to modify the source code to change behavior means recompilation and redeployment of the changes, which is an indication of rigidity.

## Example

Polymorphic open-closed principle is the preferred approach.

Imagine that we had the following example:

```java
public class SimplePrinter {
  void print(String content) {
    ...
  }
}

public class ReportProcessor {
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

There is a `SimplePrinter` class which exposes a `print()` function for printing out a string using a basic printer machine. This class is currently used directly in a `ReportProcessor` class.

Imagine in the future that there might new printer machines coming into the picture. Ideally, the same content can be printed on various kinds of new printer machines. However, with the current implementation, switching to another printer involves having to change the code in `ReportProcessor`.

A different design that adheres to the open-closed principle would be to extract a `Printer` interface and then have various types of printer machines implement this interface:

```java
public interface Printer {
  void print(String content);
}

public class SimplePrinter implements Printer {
  void print(String content) {
    ...
  }
}

public class AdvancedPrinter implements Printer {
  void print(String content) {
    ...
  }
}

public class InkjetPrinter implements Printer {
  void print(String content) {
    ...
  }
}
```

Clients of various `Printer`s will work with the interface rather that an implementation of it:

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

With this, we can add all kinds of printers in the future and we would not need to modify the code in `ReportProcessor` in order to use it, so long as the implement adheres to the `Printer` interface.
