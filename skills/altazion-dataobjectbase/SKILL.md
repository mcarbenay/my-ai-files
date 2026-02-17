---
name: altazion-dataobjectbase
description: Guide for creating data classes in an Altazon project that map a SQL / Dataset table.
---

To creative a data class in an Altazion project that maps a SQL / Dataset table, follow these steps:

1. check the datatable you want to map and identify the fields and their types. You should ignore any field that ends with "_rjs_id" as these fields are used for internal purpose (it's the tenant id in a multi-tenant database) and should not be included in the data class.
2. Create a new class that extends the DataObjectBase class provided by Altazion (namespace : Altazion.Core.Model). This class will represent the data object that maps to the SQL / Dataset table.
3. Define the properties of the class that correspond to the fields in the SQL / Dataset table. Make sure to use english for then name of the class and its properties, even if the original table is in another language.
4. Add xml comments to the class and its properties in french, describing their purpose and any relevant information. This will help other developers understand the class and its properties when they read the code or use intellisense in their IDE.
5. Implement any necessary methods for the class.
6. Add a SqlDataConcept (from namespace Altazion.Core.Metadata) attribute : [SqlDataConcept("{domain}", "{name of the entity}", "{name of the table in the database}")]
7. Implement a basic validation for the class with the constraints from the Datatable, by overriding the OnValidate() method. Identify if the project have a resource file for error messages, if so, add any error message you need to the resource file and use it in the validation, otherwise, you can directly add the error message in the AddError() method.
```
protected override void OnValidate()
{
    if (Guid == Guid.Empty)
        AddError(nameof(Guid), MyProjectStrings.GuidCannotBeEmpty);
}
```
8. If you edited the resource file, ask the user to run the command to regenerate the resource designer file, so the new messages are available in the code.