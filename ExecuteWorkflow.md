---
title: Execute Workflow
description: 
published: true
date: 2025-05-09T09:32:44.262Z
tags: 
editor: markdown
dateCreated: 2025-05-09T09:32:44.262Z
---

# Header
Your content here

ESHV.Export = function (primaryControl) {
  console.log("Export");

  var formContext = primaryControl;
  var id = formContext.data.entity.getId();
  id = id.replace("{", "").replace("}", "");

  var entityId = {};
  entityId.guid = id;

  var target = {};
  target.entityType = "workflow";
  target.id = "365a8cd9-98c6-ef11-b8e9-000d3ae6d3a4"; // generate sales export workflow id

  var req = {};
  req.EntityId = entityId;
  req.entity = target;
  req.getMetadata = function () {
    return {
      boundParameter: "entity",
      parameterTypes: {
        entity: {
          typeName: "Microsoft.Dynamics.CRM.workflow",
          structuralProperty: 5,
        },
        EntityId: {
          typeName: "Edm.Guid",
          structuralProperty: 1,
        },
      },
      operationType: 0,
      operationName: "ExecuteWorkflow",
    };
  };

  // update status
  var salesExportUpdate = {
    statuscode: 776600001 // generating
  };

  Xrm.WebApi.updateRecord("eshv_salesexport", id, salesExportUpdate).then(
    function (success) {
      // refresh form without save
      formContext.data.refresh(false);
      formContext.ui.refreshRibbon();

      // run workflow async
      Xrm.WebApi.online.execute(req).then(
        function (result) {
          console.log(result);

          var alertStrings = {
            confirmButtonLabel: "OK",
            text: "Sales Export file generation in progress. Refresh the page after a few minutes to download the file.",
            title: "Sales Export",
          };

          var alertOptions = { height: 200, width: 300 };
          Xrm.Navigation.openAlertDialog(alertStrings, alertOptions);
        },
        function (error) {
          console.log(error);
          alert(error.message);
        }
      );
    },
    function (error) {
      console.log(error);
      alert(error.message);
    }
  );
};