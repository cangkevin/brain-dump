---
type: fundamentals
tags: fundamentals single-responsibility-principle
---

# Single responsibility principle

The single responsibility principle is the first letter of the SOLID principles.

## Meaning

Robert "Uncle Bob" Martin simply states this as "a class should have only one reason to change". However, the term "reason" is a bit ambiguous.

Rather, it's better to think about the role that a class serves in the context of what it does for the business or product. If a class is responsibility for more than one role and those roles do not intersect in any meaningful or logical way, then there is a possibility that it can change for more than one reason. Hence, this is a violation of the single responsibility principle.

## Example

Imagine that we have a class like the following:
```java
public class Invoice {
  public InvoiceDto parseInvoiceDocument(InputStream inputStream) {...}
  public void persistInvoice() {...}
  public void prepareInvoiceSummaryReport() {...}
}
```

`Invoice` is a class that:
- Takes as input an invoice file from a seller and parses it into an in-memory representation
- Saves the content of the invoice to a database
- Transforms and writes out the invoice data into a different format to be used internally

Imagine that there were changes requested that puts this class under scope of those requests. What kind of changes can happen?
- The input invoice file could be changed in that information could be added, removed, renamed, etc.
- Saving the invoice data to a database could be changed in that the database technologies could change or there is a need to change the table schema
- The internal format that the invoice data is transformed to could be changed in that users could request modifications to what information gets written out and how

This means that any change to one of these functionalities could inadvertently impact other functionality. This leads to rigidity and fragility in this class. Hence, it's a good idea to separate these out into their own classes.

There is no silver bullet for handling these situations as there are many different ways of going about it. This is likely where experimentation, experience, intuition, and knowledge of the business domain can help in arriving at a reasonable solution. Whatever the approach, the final result should be able to pass the above litmus test to determine if the classes are appropriately scoped.
