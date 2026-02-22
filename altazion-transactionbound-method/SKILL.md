---
name: altazion-transactionbound-method
description: Guide for a method that uses transaction-bound operations in an Altazion backend project.
---

A transaction bound method is a method that performs multiple operations that should be treated as a single unit of work. 

Creating a transaction bound consist of :
- Getting a access to the database context. If you are inside a Bll object, you can use the MyDataManager object, if you are outside a Bll object, you can use : 

  var dataManager = DataManager.GetDataManager();

- You should then use the dataManager to create a transaction context :

  using(var ctx = new TransactionContext(dataManager))
  {
      // your code here
  }

- Inside the transaction context, you can perform your database operations. If any of the operations fail, the transaction will be rolled back automatically, ensuring data integrity. 

- If all operations succeed, you can commit the transaction by calling ctx.Commit() at the end of the using block. Any transaction context disposed without calling Commit() will be rolled back automatically.

To create a method that uses transaction-bound operations in an Altazion backend project, follow these steps:

1. Identify the specific use case for the transaction-bound method. This could be a scenario where you need to perform multiple database operations that should either all succeed or all fail together.
2. Use the Altazion MCP server to get the details of Bll objects and DataClasses. Check if there are static methods available in the Bll objects that can be used for your transaction-bound method. 
3. if you find suitables method in the Bll objects, use them in your transaction-bound method.
4. If you do not find suitable methods, check with the user or your instructions for whether you can create new methods in the Bll objects or if you should perform the database operations directly in the transaction-bound method.
5. Implement the transaction-bound method using the transaction context as described above, ensuring that all database operations are performed within the context.
6. Do only one commit at the end of the transaction context. Using more than one commit in the same transaction will lead to an error. If you need to perform multiple commits, you should create multiple transaction contexts.
7. Do only minimal exception handling inside the transaction context. If an exception occurs, it will automatically trigger a rollback, so you should only catch exceptions if you need to perform specific actions before the rollback occurs. Otherwise, let the exception propagate and be handled by the calling code.