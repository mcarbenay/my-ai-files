---
name: altazion-react-front-end-listpage
description: Guide for creating a React front-end list page in an Altazion backend project.
---

To create a React front-end list page in an Altazion backend project, follow these steps:

1. Be sure to have the entity name you want to create the list page for. For example, if you have an entity called "Product", you will be creating a list page for that entity.
2. use the Altazion MCP server to get the Development Guide for ListPage and for the Zustand store. You can also request the MCP to create the element for the base template.
3. Check with your instructions or ask the user for specific requirements for the list page, such as filters, sorting options, pagination, and any specific fields to display.
4. If a swagger.json file is available on the projet, use this file to understand the API endpoints and data structure for fetching the list of entities. 
5. When implementing the store, use fetch and avoid tools like axios.
6. Implement the correct flow for checking if user is authenticated in the store : if the checkIfAuth() function returns false, retry the check 2 times with a delay of 1 second between each retry. If the check still fails after the retries, redirect the user to the login page.