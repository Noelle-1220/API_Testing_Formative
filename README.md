**GoREST API Testing & Automation with Postman**

**Names**: INGABIRE Marie Noelle

**Module**: Web Infrastructure\_ Formative Assignment

**API Used**: GoREST - Public REST API with Bearer Token Authentication

**Base URL**: https://gorest.co.in/public/v2

**Github Repository**:
https://github.com/Noelle-1220/API_Testing_Formative

**1.API Choice**

To carry out this assignment , I used GoREST as it is the best API
because it is free, supports full CRUD(Create, Read, Update and Delete)
and also uses a simpler Bearer Token instead of a complex OAuth2 flow.

GoREST uses a personal access token model unlike other API, GoREST
allows a user to log in with a username/password and get a static token
(generated at
[[gorest.co.in/public/v2]{.underline}](http://gorest.co.in/public/v2) )
tied to the account (either Github or Google). The token acts as your
authorization proof to carry out requests like GET, POST, PUT/PATCH and
DELETE.

**2. GoREST API Endpoint Documentation**

+-----------+-----------+--------------------------------+--------------------+------------+
| Purpose   | Method    | Endpoint                       | Auth Required?     | Request    |
|           |           |                                |                    | Body       |
+-----------+-----------+--------------------------------+--------------------+------------+
| List all  | GET       | {{base_url}}/users             | None (Public read) | None       |
| users     |           |                                |                    |            |
+-----------+-----------+--------------------------------+--------------------+------------+
| Get one   | GET       | {{base_url}}/User/             | None (public read) | None       |
| user      |           |                                |                    |            |
|           |           | \[{user_id}}                   |                    |            |
+-----------+-----------+--------------------------------+--------------------+------------+
| Create    | POST      | {{base_url}}/users             | **Authorization**: | **JSON**:  |
| user      |           |                                | Bearer             | name,      |
|           |           |                                | {{auth_token}}     | email,     |
|           |           |                                | **Content-Type**:  | gender,    |
|           |           |                                | application/json   | status     |
+-----------+-----------+--------------------------------+--------------------+------------+
| Update    | PUT/PATCH | {{base_url}}/users/{{user_id}} | **Authorization**: | JSON with  |
| user      |           |                                | Bearer             | fields to  |
|           |           |                                | {{auth_token}},    | update     |
|           |           |                                | **Content-Type**:  |            |
|           |           |                                | application/json   |            |
+-----------+-----------+--------------------------------+--------------------+------------+
| Delete    | DELETE    | {{base_url}}/users/{{user_id}} | **Authorization**: | None       |
| user      |           |                                | Bearer             |            |
|           |           |                                | {{auth_token}}     |            |
+-----------+-----------+--------------------------------+--------------------+------------+

**3. Authentication Process**

**Method Used**: Bearer Token (GoREST Personal Access Token)

The token generated was added in the GoREST Env (a Postman Environment)
as auth_token. At the collection/folder level,I set the Authorization
tab to Bear Token using {{auth_token}} as the token value. Setting it at
the Collection level allows every request inside the folder to
automatically inherit the exact authentication without repeating it per
each request I make.

This way of authentication satisfies the underlying goal as a
login-based pre-request script, but abides by GoREST's actual token
model rather than simulating a login flow.

![](media/image6.png){width="6.267716535433071in"
height="2.5416666666666665in"}

**4.Postman Environment & Variables**

**GoREST Env** is a postman environment created to contain following
variables**:**

  ----------------------- ---------------------------------------------
  **Variable**            **Purpose**

  **base_url**            Stores the API's base url, making variable
                          reuse easier since it's updated in one place.

  **auth_token**          Stores the Bearer Token used to authenticate
                          write operations.

  **user_id**             Initially left empty then populated after the
                          user is created (POST) and reused across the
                          remaining CRUD operations.
  ----------------------- ---------------------------------------------

All requests reference these variables using postman's {{variable_name}}
syntax. Using this method allows us to only update changes in the
environment instead of updating each request individually.

![](media/image7.png){width="6.267716535433071in"
height="4.069444444444445in"}

**5. CRUD Operation \_ Explanation and Screenshots**

A.  **Get all User:**

- Request: GET {{base_url}}/users retrieves all users that used the
  GoREST API. I carried out this step to ensure the environment and
  auth_token are functioning properly

- Response: the request returned a 200 status indicating all were in
  place and green flagged. The result listed all users as I requested.

- Screenshot:

> ![](media/image4.png){width="6.140625546806649in" height="6.25in"}

B.  **POST - Creating my user**

- Request: POST {{base_url}}/users

- Body:

> {
>
> \"name\": \"YOUR_NAME_HERE\",
>
> \"gender\": \"male\",
>
> \"email\": \"YOUR_EMAIL_HERE\_{{\$randomInt}}@example.com\",
>
> \"status\": \"active\"
>
> }

- Response: a 201 indicating the user was created successfully. The
  result comes bearing an Id which will be considered a unique user id
  and the value for variable {user_id} . "{\$randomInt}}" is a Postman
  dynamic variable that ensures there is not similar email which keeps
  my email accepted by GoREST.

- Post-response script:

> ![](media/image5.png){width="5.765625546806649in"
> height="2.4166666666666665in"}

- Screenshot:

> ![](media/image11.png){width="6.267716535433071in"
> height="4.527777777777778in"}

C.  **Get Single User**

- Request: {{base_url}}/users/{{user_id}}

- Script:

> ![](media/image10.png){width="6.267716535433071in"
> height="2.3194444444444446in"}

- Response: a 200 indicating the request was successful. Reference to
  the image below.

- Screenshot:

> ![](media/image2.png){width="6.267716535433071in"
> height="4.861111111111111in"}

D.  **PUT/PATCH - Update the user**

- Request:PATCH {{base_url}}/users/{{user_id}} , I used PATCH to only
  update and inside specific areas of my request instead of PUT as I
  would have to write all the body details even those I do not intend to
  change.

- Body:

> raw=\>JSON
>
> ![](media/image8.png){width="6.267716535433071in"
> height="2.1527777777777777in"}

- Script:

> ![](media/image9.png){width="6.267716535433071in"
> height="2.3472222222222223in"}

- Response: a 200 indicating it was successful. More details reference
  to the below image.

- Screenshot:

> ![](media/image3.png){width="6.267716535433071in"
> height="4.916666666666667in"}

E.  **DELETE - Remove the User**

- Request: DELETE {{base_url}}/users/{{user_id}}

- Script:

> ![](media/image13.png){width="6.267716535433071in"
> height="2.0555555555555554in"}

- Response: a 204 code meaning No Content which indicates the deletion
  was a success, more details reference to the image below.

- Screenshot:

> ![](media/image12.png){width="6.267716535433071in"
> height="5.958333333333333in"}

**6. Successful Collection Runner**

![](media/image1.png){width="6.267716535433071in" height="6.875in"}

**7. Deliverable Included in this Repository**

- GoREST API Testing - IMN_collection.JSON

- GoREST Env_environment.JSON

- Screenshots Folder
