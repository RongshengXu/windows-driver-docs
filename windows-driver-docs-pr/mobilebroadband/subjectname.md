---
title: SubjectName
description: SubjectName
ms.assetid: 1aa9c826-c95a-4e89-931f-4e296122edc3
ms.author: windowsdriverdev
ms.date: 04/20/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
---

# SubjectName


The SubjectName element specifies the Subject Name of a trusted certificate.

## <span id="Usage"></span><span id="usage"></span><span id="USAGE"></span>Usage


``` syntax
<SubjectName>
  text
</SubjectName>
```

## <span id="Attributes"></span><span id="attributes"></span><span id="ATTRIBUTES"></span>Attributes


There are no attributes.

## <span id="Text_value"></span><span id="text_value"></span><span id="TEXT_VALUE"></span>Text value


A string with the Subject Name of the certificate.

## <span id="Child_elements"></span><span id="child_elements"></span><span id="CHILD_ELEMENTS"></span>Child elements


There are no child elements.

## <span id="Parent_elements"></span><span id="parent_elements"></span><span id="PARENT_ELEMENTS"></span>Parent elements


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>Element</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>[TrustedCertificate](trustedcertificate.md)</p></td>
<td><p>Specifies a trusted certificate.</p></td>
</tr>
</tbody>
</table>

 

## <span id="XSD"></span><span id="xsd"></span>XSD


``` syntax
<xs:element name="SubjectName" type="xs:string" />
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Remarks


The SubjectName element is optional.

 

 





