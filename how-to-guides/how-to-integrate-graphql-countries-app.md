# How to work with GraphQL on Appsmith

In this tutorial, you'll see how to integrate a GraphQL API with Appsmith. Basic familiarity with GraphQL API queries and Appsmith platform is assumed. You can learn more about GraphQL [here](https://graphql.org/learn/) and Appsmith [here](https://docs.appsmith.com/).

{% embed url="https://youtu.be/W0Pa7OJzk5E" %}

After completing this guide, we'll build the finished app shown below:

![App Demo](../.gitbook/assets/00.graphql-countries-demo.gif)

## Setting Up GraphQL with Appsmith

We will be using a public GraphQL API from [here](https://countries.trevorblades.com/). You can explore the SCHEMA and DOCS to know more about this API. It gives information about continents and countries.

![API Schema](../.gitbook/assets/01.schema-api.png)

### Creating a new Appsmith application

* Create a new account on Appsmith (it’s free!). If you are already an existing user, you can login to your Appsmith account.
* Create a new application by clicking on the **NEW** button on your Appsmith dashboard.
* You will now see a new Appsmith app with an empty canvas and a sidebar with Widgets and Datasources.
* In the top left, click on your application name and rename it to "World Explorer".

![New app](../.gitbook/assets/02.new-app.png)

### Creating Continents page

Let's get started with creating our first page which will show all continents in a table.

* Click on the + icon next to the Datasources then click on **Create New** button. 
* Since we want to add a new GraphQL API, click on **Create new API**.
![Create API](../.gitbook/assets/03.a-create-api.png)

* Rename the API to `Get_All_Continents` and change the request type to **POST**.
* The API request URL will be: `https://countries.trevorblades.com`.
* In the **Body** of the request, put the GraphQL query to get all continents.
```
{
  "query": "{
    continents {
      code
      name
    }
  }"
}
```
* You can **RUN** the API to see the response.
![Get_All_Continents](../.gitbook/assets/03.get-all-continents.png)

Now that we have setup the datasource for our first page, it's time to display the API response in a table.

* Click on the + icon next to the **Widgets** and drag the **TABLE** widget to the canvas.
* Go to the table widget properties and change Table Data to:
```
{{Get_All_Continents.data.data.continents}}
```
* We don't need to display code column to user. It will just be used for our further GraphQL queries. To
hide this column click on the 👁 (eye) icon.
![Table data](../.gitbook/assets/04.table1-data.png)

Yay! Now you can see all the continents in the API response in a table.

### Creating Countries page

Now, we want users to be able to see all countries in a continent by clicking on the continent name in the table.

* Click on the + icon next to the **Pages**. This will create a new page named Page2. Leave this page as of now, we will come back to it later.
* Now go back to Table1 of Page1 and click on table properties.
* We need to add logic for page change in our table's **onRowSelected** action. Click on the **JS**   button to add below custom JS code which will get called whenever a row is clicked.
```
{{
(function() {
  storeValue('code', Table1.selectedRow.code)
  navigateTo('Page2', {})
}) ()
}}
```
The `storeValue()` function is used to store a value in localStorage. Here, we are storing code of selected country.
`navigateTo()` function is used to navigate to Page2.
![Table 1 onRowSelected](../.gitbook/assets/05.table1-on-row-selected.png)

* Click on any row of the table and you should be redirected to an empty Page2.

Now, it's time to go back to Page2 and add a new datasource for fetching countries in the selected continent.

* Add a new POST API named `Get_All_Countries_In_Continent` with URL `https://countries.trevorblades.com`.
* Go to settings and uncheck `Smart JSON Substitution` option.
![Get_All_Countries_In_Continent](../.gitbook/assets/06.get-all-countries-api.png)

* Put the following query in the body of this API:
```
{
  "query": "{
         continent (code: {{(function() {return "\\\\"+`\"${appsmith.store.code}`+"\\\\" + `\"`})() }}) {
    countries {
      name
           capital
           code
    }
  }
  }"
}
```
Here, we are passing the selected continent code stored in localStorage to the GraphQL query through `${appsmith.store.code}`. This will fetch the details of the selected continent.

* You can **RUN** the API to see the response.
![Get_All_Countries_In_Continent run](../.gitbook/assets/07.country-api-body-and-run.png)

* Create a new table widget in Page2 and in **Table Data** property bind the response of `Get_All_Countries_In_Continent` like this:
```
{{Get_All_Countries_In_Continent.data.data.continent.countries}}
```
* Hide the code column by clicking on the 👁 (eye) icon.
![Country Table](../.gitbook/assets/08.country-table.png)

* To know which country user clicked on we will store it's code in localStorage. Add the following **JS** code to **onRowSelected**.
```
{{
(function() {
  storeValue('country',Table1.selectedRow.code)
}) ()
}}
```
![Country Table onRowSelected](../.gitbook/assets/09.country-table-on-row-selected.png)

* Let's add a Datasource to fetch more details like currency, phone, language of selected country.
* Add a new POST API named `Get_Country_Details` with URL `https://countries.trevorblades.com`.
* Go to settings and uncheck `Smart JSON Substitution` option.
![Get_Country_Details](../.gitbook/assets/10.country-details-api.png)

* Put the following query in the body of this API:
```
{
  "query": "{
         country (code: {{(function() {return "\\\\"+`\"${appsmith.store.country}`+"\\\\" + `\"`})() }}) {
      name 
    capital
     currency
    phone
    native
    languages {
      code
      native
      name
      rtl
    }
  }
  }"
}
```
Here, we are passing the selected country code stored in localStorage to the GraphQL query through `${appsmith.store.country}`. This will fetch the details of the selected country.

* You can **RUN** the API to see the response.
![Country detail body](../.gitbook/assets/11.country-detail-body.png)

* Go to Page2 table's **onRowSelected** property. Call `Get_Country_Details` API when user selects a country. This will fetch country details of the selected country every time.
```
{{
(function() {
  storeValue('country',Table1.selectedRow.code)
  Get_Country_Details.run()
}) ()
}}
```
![Call country details](../.gitbook/assets/12.call-country-details.png)


### Creating Country Details Modal

Now, we will show more details of the selected country when user clicks on it. We will show these details in a **MODAL**.

* Click on the + icon next to the **Widgets** in Page2 and drag the **MODAL** widget to the canvas.
![Create modal](../.gitbook/assets/13.create-modal.png)

* Drag **TEXT** widgets to the modal and bind them to different properties in our `Get_Country_Details` response.
* For each text widget, write the following JS code in **Text** property. 
Heading
```
Country Details for {{Get_Country_Details.data.data.country.name}} :-
```
![Modal country details](../.gitbook/assets/14.modal-country-details.png)

Domestic Name Value
```
{{Get_Country_Details.data.data.country.native}}
```
![Modal domestic name](../.gitbook/assets/15.modal-domestic-name.png)

Currency Value
```
{{Get_Country_Details.data.data.country.currency}}
```
![Modal currency value](../.gitbook/assets/16.modal-currency.png)

Country code Value
```
{{Get_Country_Details.data.data.country.phone}}
```
![Modal country code value](../.gitbook/assets/17.modal-country-code.png)

Language Value
```
{{
function() {
  const languages = Get_Country_Details.data.data.country.languages;
  string result = "";
  if(languages[0]) {
    result += languages[0].name;
  }
  if(languages[1]) {
    result += " , " + languages[1].name;
  }
  return result;
} ()
}}
```
![Modal languages](../.gitbook/assets/18.modal-languages.png)

![Modal ready](../.gitbook/assets/19.modal-ready.png)

Our modal is ready, only thing left is to show it.

* Go to Page2 table's **onRowSelected** property. Call `showModal('Modal1')` when user selects a country.
```
{{
(function() {
  storeValue('country',Table1.selectedRow.code)
  Get_Country_Details.run()
  showModal('Modal1')
}) ()
}}
```
![Show modal](../.gitbook/assets/20.show-modal.png)

Congratulations! You have successfully integrated a GraphQL API with Appsmith. Explore the app and learn something new about a country!
