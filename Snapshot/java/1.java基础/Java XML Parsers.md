# Java解析XML文件

## Java XML Overview

### What is XML?

XML is a simple text-based language which was designed to **store and transport data in plain text format**. It stands for **Extensible Markup Language**. Following are some of the salient(凸角) features of XML.

- XML is a markup language.
- XML is a tag based language like HTML.
- XML tags are not predefined(预先确定) like HTML.
- You can define your own tags which is why it is called extensible(可延长的) language.
- XML tags are designed to be self-descriptive.
- XML is W3C Recommendation for data storage and data transfer.

```xml
<?xml version = "1.0"?>
<Class>
   <Name>First</Name>
   <Sections>
      <Section>
         <Name>A</Name>
         <Students>
            <Student>Rohan</Student>
            <Student>Mohan</Student>
            <Student>Sohan</Student>
            <Student>Lalit</Student>
            <Student>Vinay</Student>
         </Students>
      </Section>
      
      <Section>
         <Name>B</Name>
         <Students>
            <Student>Robert</Student>
            <Student>Julie</Student>
            <Student>Kalie</Student>
            <Student>Michael</Student>
         </Students>
      </Section>
   </Sections>
</Class>
```

### Advantages

Following are the advantages that XML provides −

- **Technology agnostic** − Being plain text, XML is technology independent. It can be used by any technology for data storage and data transfer purpose.
- **Human readable** − XML uses simple text format. It is human readable and understandable.
- **Extensible** − In XML, custom tags can be created and used very easily.
- **Allow Validation** − Using XSD, DTD and XML structures can be validated easily.

### Disadvantages

Following are the disadvantages of XML usage −

- **Redundant Syntax** − Normally XML files contain a lot of repetitive(重复的) terms.
- **Verbose** − Being a verbose(冗长的) language, XML file size increases the transmission and storage costs.





## Java XML Parsers

### What is MXL Parsing

XML Parsing refers to going through an XML document in order to access or modify data.

### What is XML Parser

XML Parser provides a way to access or modify data in an XML document. Java provides multiple options to parse XML documents. Following are the various types of parsers which are commonly used to parse XML documents.

- **Dom Parser** − Parses an XML document by loading the complete contents of the document and creating its complete hierarchical tree in memory.
- **SAX Parser** − Parses an XML document on event-based triggers. Does not load the complete document into the memory.
- **JDOM Parser** − Parses an XML document in a similar fashion to DOM parser but in an easier way.
- **StAX Parser** − Parses an XML document in a similar fashion to SAX parser but in a more efficient way.
- **XPath Parser** − Parses an XML document based on expression and is used extensively in conjunction with XSLT.
- **DOM4J Parser** − A java library to parse XML, XPath, and XSLT using Java Collections Framework. It provides support for DOM, SAX, and JAXP.

## Java DOM4J Parser 

### Steps to Using DOM4J

Following are the steps used while parsing a document using DOM4J Parser.

- Import XML-related packages.
- Create a SAXReader.
- Create a Document from a file or stream.
- Get the required nodes using XPath Expression by calling document.selectNodes()
- Extract the root element.
- Iterate over the list of nodes.
- Examine attributes.
- Examine sub-elements.



