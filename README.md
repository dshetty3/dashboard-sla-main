<h3> Front-End Application Enhancement Project </h3>

<br/>
<br/>

<h4> This project aims to enhance an existing front-end application by refactoring and adding new features to improve the user experience. The following tasks have been accomplished:
 </h4>

<h5>To run the enhanced front-end application on your local machine, follow these instructions:</h5>

Prerequisites: <code>Install npm or yarn</code>

<code> 1. Clone the given repository  https://github.com/rayyach/dashboard-sla/ </code>

    git clone  https://github.com/rayyach/dashboard-sla/

<code> Navigate to the project directory using your command line or terminal: </code>
    
     cd your-repo

<code> 2. Install the necessary dependencies using yarn: </code>

    yarn install

<code> 3. Run the application: </code>

     yarn run dev

<h5>The application should now be accessible in your web browser at http://localhost:3000</h5> 


<h5> Task Overview </h5>

<code> For Refactor and Enhancement, I have converted the application from Vue 2 to Vue 3 by installing all the necessary dependencies. I prefered yarn </code>

<code> 1. Pagination: I have created a Pagination component to display 100 rows/page to make it more user freindly.
Changes have been implemented in the existing productDataBystatus() </code>

<code> 2. Color Coding: Color Coded the Products based on the Status:
       Here, <font color="Lightblue"> Launched </font>
       Discontinued - LightGreen
       Launched (with IPU) -Yellow
       Announced - Light Gray
</code>

<code> 3. SearchBar (Optional) : Implement Search Bar to filter out rows (I have used Product as a criteria - can be changed later)    
    The search bar is the top right hand side of the page. Once the user enters the product name - it will start filtering out the 
    names present from the product column. </code> 




