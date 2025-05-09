---
title: Lookup Custom View
description: 
published: true
date: 2025-05-09T09:28:22.963Z
tags: 
editor: markdown
dateCreated: 2025-05-09T09:28:22.963Z
---

# Header
Your content here

ESHV.AddLicenseeBrandCustomView = function (executionContext) {
  const formContext = executionContext.getFormContext();
  var licensee = formContext.getAttribute("eshv_licensee").getValue();

  var fetchXml = `<fetch>
  <entity name="eshv_brand">
    <attribute name="eshv_name" />
    <attribute name="statuscode" />
    <attribute name="eshv_description" />
    <filter>
      <condition attribute="statecode" operator="eq" value="0" />
    </filter>
    <order attribute="eshv_name" />
    <link-entity name="eshv_stakeholderbrands" from="eshv_brand" to="eshv_brandid" alias="sb">
      <filter>
        <condition attribute="eshv_account" operator="eq" value="${licensee[0].id}" uitype="account" />
      </filter>
    </link-entity>
  </entity>
</fetch>`;

  var layoutXml = `<grid name="resultset" object="11149" jump="eshv_name" select="1" icon="1" preview="1">
  <row name="result" id="eshv_brandid">
    <cell name="eshv_name" width="100" />
    <cell name="eshv_description" width="100" />
    <cell name="statuscode" width="100" />
  </row>
</grid>`;

  formContext
    .getControl("eshv_brand")
    .addCustomView(
      "{d47c00e7-7a68-4c44-b9d6-9c7e20e2bc91}",
      "eshv_brand",
      "Brands",
      fetchXml,
      layoutXml,
      true
    );

  formContext
    .getControl("eshv_brand")
    .setDefaultView("{d47c00e7-7a68-4c44-b9d6-9c7e20e2bc91}");
};