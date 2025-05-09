---
title: Execute Bound Action
description: 
published: true
date: 2025-05-09T09:31:21.115Z
tags: 
editor: markdown
dateCreated: 2025-05-09T09:31:21.115Z
---

# Header
Your content here

var ESHV = ESHV || {};

ESHV.CopyCroppingProgramDefinition = function (primaryControl) {
  Xrm.Utility.showProgressIndicator("Copying Cropping Program Definition...");

  var formContext = primaryControl;
  var id = formContext.data.entity.getId();
  
  var target = {};
  target.entityType = "eshv_croppingprogramdefinition";
  target.id = id.replace("{", "").replace("}", "");

  var req = {};
  req.entity = target;
  req.getMetadata = function () {
    return {
      boundParameter: "entity",
      parameterTypes: {
        entity: {
          typeName: "eshv_croppingprogramdefinition",
          structuralProperty: 5,
        },
      },
      operationType: 0,
      operationName: "eshv_CopyCroppingProgramDefinition",
    };
  };

  Xrm.WebApi.online.execute(req).then(
    function (result) {
      
      if (result.ok) {
        result.json().then(function (response) {
          console.log(response);
      
          Xrm.Utility.closeProgressIndicator();

          if (response.eshv_Success) {
            // navigate to new cropping program definition
            var pageInput = {
              pageType: "entityrecord",
              entityName: "eshv_croppingprogramdefinition",
              entityId: response.eshv_CroppingProgramDefinitionId,
            };

            var navigationOptions = {
              target: 1, // Open inline
            };

            Xrm.Navigation.navigateTo(pageInput, navigationOptions);
          } else {
            Xrm.Utility.closeProgressIndicator();
            alert("Error occurred copying cropping program definition");
          }
        });
      } // result not ok
      else {
        console.log(result);
        alert("Error occurred copying cropping program definition");
        Xrm.Utility.closeProgressIndicator();
      }
    },
    function (error) {
      Xrm.Utility.closeProgressIndicator();
      console.log(error);
      alert(error.message);
    }
  );
};
