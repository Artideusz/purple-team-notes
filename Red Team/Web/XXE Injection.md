# XML External Entity Injection

XML External Entity Injection is a attack that exploits weakly configured XML parsers in an application.

## What is XML?

Extensible Markup Language (XML) is a text-based format used for representing information. XML is a predecessor of JSON, which is simpler and requires less processing time and memory. XML's structure is very similar to HTML, but with a couple of differences, such as:

- All elements must be closed with a corresponding closing tag or marked as empty.
- Empty elements are marked as follows: `<my-element />`.
- Attributes values must be quoted.
- There are no built-in names. Only tags starting with *xml* have special meanings.
- There are only 5 character entities: 
    - `&lt;` for `<`
    - `&gt;` for `>`
    - `&amp;` for `&`
    - `&quot;` for `"`
    - `&apos;` for `'`
- The user is able to create their own entities, thanks to DTDs.

An example XML would look like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<letter>
    <metadata>
        <address>
            <address_to>123 Example St.</address_to>
            <address_from>321 Pwner Ave.</address_from>
        </address>
        <author>Phisher</author>
        <reciever>Bob</reciever>
    </metadata>
    <body>
        <title>IMPORTANT - Letter from Boss </title>
        <content>
            Hey Bob,
            
            I have a crucial meeting with an important client today and unfortunately,
            I forgot my password for the administration portal of our site,
            could you please restart it for me as soon as possible and send it to my phone number? 
            
            I should also tell you that this is my new number for the office `123-123-1234`,
            since I lost the other number. Be sure to change it in your contacts.
            
            Please be swift with this task, this is very urgent,
            Boss.
        </content>
    </body>
</letter>
```

## What are DTDs?

Document Type Definitions (DTD) are special XML rules that define entities and specific rules for specific XML documents. *Well-formed* or *valid* XML documents are those which are validated against a DTD, we can validate files by using the `<!DOCTYPE>` tag; consider the following XML example:

example.xml (sent by the client machine)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example
    [
        <!ELEMENT example (value)>
        <!ELEMENT value (#PCDATA)>
    ]
>
<example>
    <value>Hello!</value>
</example>
```

The above example shows a type of DTD declaration called an "Internal DTD Declaration". The structure of the DTD is interpreted like this:
- "!DOCTYPE &lt;name&gt;" - This defines the root element of the document (which is example in our case).
- "!ELEMENT &lt;name&gt; (&lt;data&gt;)" - This defines an element in the document that must contain elements of data OR be of type "#PCDATA", which is also called "parseable character data".

This is another example of DTD usage, but this time, the DTD is external:

example.xml (sent by the client machine)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example SYSTEM "example.dtd">
<example>
    <value>Hello!</value>
</example>
```

example.dtd (hosted by the server)
```xml
<!DOCTYPE example
    [
        <!ELEMENT example (value)>
        <!ELEMENT value (#PCDATA)>
    ]
>
```

## What are entities?

Entities are special tags used by DTD to replace identifiers inside an XML document with other data, almost like a `include` function in PHP or C++. There are 3 types of entities:

### Internal Entities

Internal entities are those whose definitions can be found entirely within a document's DTD. An example internal entity in an DTD document looks like this:

```xml
<!DOCTYPE example
    [
        <!ENTITY entity_name "entity_value_text">
        
    ]
>
```

### External Entities

External entities

## How does the attack work?

TODO

## Resources

- [OWASP - XML External Entity](https://owasp.org/www-community/vulnerabilities/XML_External_Entity_(XXE)_Processing)
- [W3C - XML Essentials](https://www.w3.org/standards/xml/core)
- [W3C - XML Entities](https://www.w3resource.com/xml/entities.php)
- [PortSwigger - Blind XXE](https://portswigger.net/web-security/xxe/blind)
- [PwnFunction - XXE Explained](https://www.youtube.com/watch?v=gjm6VHZa_8s)