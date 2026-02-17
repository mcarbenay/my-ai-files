---
name: altazion-office-apicontroller
description: Guide for creating API controllers in an Altazon project.
---

To create an API controller in an Altazion project, follow these steps:
1. Identify the entity or functionality that the API controller will manage. For example, if you have an entity called "Product", you will be creating an API controller for managing products.
2. Use the Altazion MCP Server to get details about the data class that represents the entity, as well as the Bll class that contains the business logic for that entity. This will help you understand the structure of the data and the operations that can be performed on it.
3. Create a new class that extends the ApiControllerBase class provided by Altazion (namespace : Altazion.Core.Api). This class will represent the API controller for the entity.
4. Define the necessary API endpoints (e.g., GET, POST, PUT, DELETE) in the controller class to perform CRUD operations on the entity. Use the Bll class to implement the business logic for each endpoint.
5. Add appropriate routing attributes to the controller and its methods to define the API routes. Follow the Altazion routing conventions for consistency : 
```
[Route("api/{project domain}/{entity name}")]
```")]
6. Authentication is handled by the Altazion framework, add the [UserAuthorize] attribute to the controller or specific endpoints to ensure with the correct permission. Ask the user for the necessary permissions if not already defined.
7. Except for checking parameters, avoid returning any direct error codes with BadRequest() or similar methods. Instead, throw exceptions with clear and descriptive messages when an error occurs. The Altazion framework will handle the exceptions and return appropriate error responses to the client.
8. Add XML comments to the controller and its methods in French, describing their purpose and any relevant information. This will help other developers understand the controller and its endpoints when they read the code or use intellisense in their IDE.
9. When using DataClasses that inherits from DataObjectBase, use the appropriate FromData/FromDataRow methods to convert between the DataSet/DataTable returned by the Bll and the DataClass used in the API controller. This will ensure that the data is correctly mapped and handled in the API controller.
ex :
```
var results = Country.FromData<Country>(ds.params_pays);
```
10. For create/update or upsert operations, use the DataValidation tool to validate the data before performing the operation. This will help ensure that the data being processed is valid and meets the necessary constraints defined in the DataClass.
```

      var val = new DataValidation();

      // use ValidateData() for objects that implements IValidateData and for classes that inherits from DataObjectBase.
      val.ValidateData("request", request);

      // or AddError or Must*** for individual checks
      if (!request.RecipientGroupGuid.HasValue)
          val.AddError(MainApiStrings.Error_RecipientGroupGuidRequired, "request", "RecipientGroupGuid");
      if (request.TargetGuid.HasValue)
          val.MustNotBeNull(request.TargetType, MainApiStrings.Error_TargetTypeRequired, "request", "TargetType");

      val.Validate();
```
Validate() will throw an exception if any validation error has been added, with a clear message containing all the errors added.