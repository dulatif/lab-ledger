# 12. Legacy Systems: A Brief Look at SOAP

## 🎯 Learning Goal

Appreciate history (and Enterprise systems).

## 🧠 Concept

**SOAP (Simple Object Access Protocol)**.

- Uses XML.
- Strict WSDL (Web Services Description Language) contract.
- Heavy, but secure and ACID compliant.

## 💻 Implementation

```xml
<soap:Envelope>
  <soap:Body>
    <GetUser>
      <Id>1</Id>
    </GetUser>
  </soap:Body>
</soap:Envelope>
```

## 🧩 Activity / Challenge

1.  Compare the XML above to the JSON in the previous lesson.
2.  Observe the verbosity.

## 🔑 Key Takeaways

- You will encounter SOAP in banking and government integrations.
