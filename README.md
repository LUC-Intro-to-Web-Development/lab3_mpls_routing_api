#  MPLS Dog Boarding App - Configuring Routes and Implementing REST API (Lab 3) 
You are going to extend the functionality of an existing application by adding additional routes to the MPLS Dog boarding application and implementing a REST API that will allow you to Create and View random dogs who reside at MPLS Dog Boarding. 

![Image of Lab 3 Functionality](https://instructorc.github.io/site/slides/database/images/lab3/cpst342_lab3_outputvid.gif)
## Requirements

### Step 1 - Setup project folder and start-up server
1.  Pull down the contents of your assigned repository and install the necessary dependencies needed to run the application.  The package.json file has already been created and identifies the packages your repository will depend upon.
2.  Start the server and make sure you are able to render the application in the browser before making any changes. Run command ```npm start`` in your terminal.

**Throughout this lab, make a commit for each step to show the progression of your work.**
### Step 2 - Adding Route
Listed below are two steps that will help you accomplish adding route to your server.js file.
1.  Adding routes to server.js file.
	1.  Add a route that listens for a get request to file path ```/search_breeds```, and respond back to the client rendering the ```dog_breeds.html``` page
        ```javascript
	      app.get('/search_breeds', function (req, res) {

             res.render("dog_breeds.html");
	      })
	    ```
2. If page properly renders, kill server and proceed to step 3. 
 

### Step 3 - Implement REST API
1.  Sign-up for an API key by creating an account at ```https://thedogapi.com/signup```.  You should receive an email that provides you with an API key. Test to see if you can retrieve information using the API by using a tool such as ```hoppscotch.io```.  You will need to include your API key as a key/value pair for the header request.  The picture below should help provides an example of how to specify the key for the header request.
![Image of Lab 3 API Request](https://instructorc.github.io/site/slides/database/images/lab3/forlab3_readme.PNG)

2.  Navigate to the server.js file and locate the route that listens for a get request to URL path ```/search_breeds```, adjust the route to include the code below.  Make sure to add your API key.
        ```javascript
	      app.get('/search_breeds ', async function (req, res) {
    			
              // API URL
	          const API_URL = "https://api.thedogapi.com/v1/breeds";  // <---- Change this to your api key
				
	          // YOUR API KEY
	          const API_KEY = "[YOUR_API_KEY]"; // <---- Change this to your api key
	    
			  try {
			    const response = await fetch(API_URL, {
			      method: "GET",
			      headers: {
			        "x-api-key": API_KEY,
			        "Content-Type": "application/json"
			      }
			    });
			
			    if (!response.ok) {
			      return res.status(response.status).json({ error: "API request failed" });
			    }
			
			    const data = await response.json();
			    res.json(data);
			
			  } catch (error) {
			    console.error("Fetch error:", error);
			    res.status(500).json({ error: "Internal Server Error" });
			  }
	  })
	    ```
4. Render your application in the browser and naviage to route ```/search_breeds``` to make sure you are successfully returning JSON data.  If data is not displaying, you can troubleshoot by viewing possible error in browser inspect elements console area.
   
### Step 4 - Adjust HTML to display data returned from API 
1.  Navigate to the server.js and replace  ```res.json(data);`` with ```res.render('dog_breeds', {breeds: data});```
2.  Navigate to ```dog_breeds.html``` and adjust the HTML below the closing form tag. 
	```html
		<section id="breed-results">
			{{#if breeds}}
				{{#each breeds}}
					<div class="breed-card">
						{{#if image}}
						   <img src="{{image.url}}" alt="{{name}}">

						{{else}}
							{{#if reference_image_id}}
								<img src="https://cdn2.thedogapi.com/images/{{reference_image_id}}.jpg" alt="{{name}}">

							{{/if}}
						{{/if}}

						<h3>{{name}}</h3>

						{{#if breed_group}}<div>Group: {{breed_group}}</div>{{/if}}
						{{#if bred_for}}<div>Bred for: {{bred_for}}</div>{{/if}}
						{{#if temperament}}<div>Temperament: {{temperament}}</div>{{/if}}
						{{#if origin}}<div>Origin: {{origin}}</div>{{/if}}
						{{#if life_span}}<div>Life span: {{life_span}}</div>{{/if}}

						{{#if weight.metric}}
							<div>Weight: {{weight.metric}} kg</div>
						{{else}}
							{{#if weight.imperial}}<div>Weight: {{weight.imperial}} lb</div>{{/if}}
						{{/if}}

						{{#if height.metric}}
							<div>Height: {{height.metric}} cm</div>
						{{else}}
							{{#if height.imperial}}<div>Height: {{height.imperial}} in</div>{{/if}}
						{{/if}}
					</div>
				{{/each}}
			{{else}}
				<div>No breeds to display.</div>
			{{/if}}
		</section>

 	``` 
 3. Save all adjusted files and start server to render application in browser.  Test out application to make sure you are able to filter by name and breed group.
 

### STEP 5 - Adjust README.md file and package.json file 
1.  Adjust your package.json file to include your name, application keywords and GitHub repository for the designated key/value pairs.
  

### STEP 6 - Submission 
1.  Make sure your branch is clean by making all necessary commits and push up your final changes.
2.  In Sakai, submit the URL to your repository.

### Lab Resources
[The Dog API Signup ](https://thedogapi.com/en/students)
[Handlebars Documentation](https://handlebarsjs.com/api-reference/)
