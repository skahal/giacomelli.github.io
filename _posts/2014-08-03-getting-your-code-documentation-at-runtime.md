---
published: true
layout: post
title: Getting your code documentation at runtime
---

In some situations, like generate a live console for a web api or generate code using T4 template we need a way to read the code documentation at runtime.

Some time ago I've faced that situation again and I thought: _"Should be an easy way to read this code documentation!"_. 

After some googled I found a code from Jim Blackler that allowed developers read C# code documentation at runtime, but at that time the code was just a downloadable .zip in Jim's blog. I asked him if I could put the source code on GitHub to allow better code improvements and community collaboration, he said: "Please go ahead with your plan".

So, I created the project at GitHub, **DocsByReflection**: [https://github.com/giacomelli/DocsByReflection](https://github.com/giacomelli/DocsByReflection)

### DocsByReflection
With DocsByReflection you can easy get your code documentation at runtime in many ways, like:

```csharp
// From type.
var typeDoc = DocsService.GetXmlFromType(typeof(Stub));

// From property.
var propertyInfo = typeof(Stub).GetProperty("PropertyWithDoc");
var propertyDoc = DocsService.GetXmlFromMember(propertyInfo);

// From method.
var methodInfo = typeof(Stub).GetMethod("MethodWithGenericParameter");
var methodDoc = DocsService.GetXmlFromMember(methodInfo);

// From assembly.
var assemblyDoc = DocsService.GetXmlFromAssembly(typeof(Stub).Assembly);
```

If you want colaborate, just [fork it at GitHub](https://github.com/giacomelli/DocsByReflection/fork).

#### Nuget
If you want just use it, there is a NuGet package with latest binaries version:

```
Install-Package DocsByReflection
```
