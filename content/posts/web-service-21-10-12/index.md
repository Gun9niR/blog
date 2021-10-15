---
title: "Web Service : First attempt"
date: 2021-10-12T15:33:48+08:00
draft: false
author: Gun9niR
resources:
- name: "featured-image-preview"
  src: "featured-image-preview.jpeg"  
tags: ["Web Service", "SOAP"]
categories: ["Web"]

lightgallery: true
---

This article records my first attempt with web service in course SE3353.

## 1. SOAP

SOAP is an XML-based application-level protocol for accessing web services. It is normally sent on HTTP, but could be sent over SMTP or even FTP.

[Anatomy of SOAP protocol](https://zhuanlan.zhihu.com/p/29819666)

### 1.1. SOAP vs REST

They are not mutual-exclusive.

[Detailed comparison](https://www.javatpoint.com/soap-vs-rest-web-services)

## 2. WSDL

WSDL, or *Web Service Description Language*, is an XML based definition language. It’s used for describing the functionality of a SOAP based web service, some important ones being:

- `schema`: Describes the parameters and return type of services.
- `portType`: Describes
  1. Name of the service
  2. The operations included in the service
  3. The input and output for each operation
- `binding`: Describes the transport protocol that the operations use.
- `service`: Describes
  1. `address`: Where to send HTTP request to call the webservice.

## 3. SOAP with JS

Using SOAP with JS is quite troublesome, because the xml has to be hand-written or generated with third-party library, and there's no tool to process WSDL (that I've found), you have to read it word by word.

### 3.1. Preflight `OPTIONS` request

Using [jquery.soap](https://github.com/doedje/jquery.soap).

[Explanation1](https://stackoverflow.com/questions/1256593/why-am-i-getting-an-options-request-instead-of-a-get-request)
[GitHub Issue](https://github.com/doedje/jquery.soap/issues/97)

When sending `xml` with `POST` request, a preflight request will be sent using `OPTIONS` method.

[Solution](https://www.freesion.com/article/61481414215/)

### 3.2. Request namespace

The **targetNamespace** should be specified for a method call. In `jquery.soap`, just add:

```javascript
namespaceURL: <your-target-namespace>
```

In the parameters, namespace are also required (server side). See [details](https://www.cnblogs.com/HuuuWnnn/p/14174627.html).

### 3.3. CXF pitfalls

- Java class for `complexType` in xml must have setters to be generated properly.

### 3.4. Handling SOAP response xml with JS

I'm using `xml2json` jquery plugin to parse xml to json.

Pitfalls:

1. When an xml array contains only one element, the library will parse it as a single object.
2. All fields of the obejct are of type `string`.
