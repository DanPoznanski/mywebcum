



## Making GET Requests and Handling Responses in Go

## Making GET Requests and Handling Responses

Welcome to the second lesson in our journey of interacting with `APIs` using Go. In the previous lesson, we established a strong understanding of `RESTful APIs` and how `HTTP requests` facilitate interactions with them. We used `curl` to interact directly with an API endpoint. Now, we will automate this process using Go's `net/http` package. This lesson will guide you through making HTTP requests in Go, equipping you with essential skills for web development and API integration.

**Setting Up the Environment**

To make HTTP requests in Go, we'll use the standard library's `net/http` package, which is powerful and does not require any third-party installation. If you're developing locally, make sure you have Go installed on your system. You can download and install it from the official website. Once installed, verify your installation by running:

```bash
go version
```
This command should show the currently installed version of Go. With Go set up, you're ready to write and run scripts that automate HTTP requests, saving time and boosting efficiency.

**Defining the Base URL**

In Go, we'll define a **base URL** for the API service we are using. This approach keeps our code modular and maintainable.

```Go
package main

import (
    "fmt"
)

const baseURL = "http://localhost:8000"

func main() {
    fmt.Println("Base URL is set to:", baseURL)
}
```
By defining the base URL this way, we can easily append endpoints for different API services, keeping our code clean and adaptable.


**Performing a Basic GET Request**
Let's move on to fetching data from an API using Go's `http.Get()` function. Our goal is to retrieve a list of to-do items from the `/todos` endpoint.
```Go
package main

import (
    "fmt"
    "io"
    "log"
    "net/http"
)

func main() {
    resp, err := http.Get(baseURL + "/todos")
    if err != nil {
        log.Fatalf("Failed to get todos: %v", err)
    }

    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read response body: %v", err)
    }

    fmt.Println("Raw Response:")
    fmt.Println(string(body))
}
```
Here, `http.Get()` sends the GET request, and `io.ReadAll()` reads the response body. We print the raw response as a string, giving us the immediate result returned by the server.

**Handling Successful Requests (Status Code: 200)**

When the server returns a **200** status code, it indicates a successful request. Below is an example of retrieving and displaying the response body.

```Go
package main

import (
    "fmt"
    "io"
    "log"
    "net/http"
)

func main() {
    resp, err := http.Get(baseURL + "/todos")
    if err != nil {
        log.Fatalf("Failed to get todos: %v", err)
    }

    // http.StatusOK == 200
    if resp.StatusCode == http.StatusOK {
        body, err := io.ReadAll(resp.Body)
        if err != nil {
            log.Fatalf("Failed to read response body: %v", err)
        }

        fmt.Println("Response received successfully:")
        fmt.Println(string(body))
    }
}
```
In Go, `http.StatusOK` is interchangeable with its integer representation (`200`). Using the named constant improves readability and helps avoid magic numbers in your code, making it more maintainable and self-explanatory.

This ensures the response is successfully retrieved and displayed as a string. Here’s an example of what the raw response might look like:
```Go
Raw Response:
[
  {
    "description": "Milk, eggs, bread, and coffee",
    "done": false,
    "id": 1,
    "title": "Buy groceries"
  },
  {
    "description": "Check in and catch up",
    "done": true,
    "id": 2,
    "title": "Call mom"
  },
  {
    "description": "Summarize Q4 performance metrics",
    "done": false,
    "id": 3,
    "title": "Finish project report"
  },
  {
    "description": "30 minutes of cardio",
    "done": true,
    "id": 4,
    "title": "Workout"
  }
]
```
By reading the response body and printing it, we can inspect the returned data before processing it further.

**Handling Bad Requests (Status Code: 400)**

If the request is malformed, a **400** status code is returned to signify this. Let's handle it gracefully in Go.
```Go
// http.StatusBadRequest == 400
else if resp.StatusCode == http.StatusBadRequest {
    fmt.Println("\nBad Request. The server could not understand the request due to invalid syntax.")
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read error response: %v", err)
    }
    fmt.Printf("Error Details: %s\n", body)
}
```

**Handling Unauthorized Requests (Status Code: 401)**

For cases involving a **401 Unauthorized** status code, which typically means authorization issues, we'll handle them as follows.
```Go
// http.StatusUnauthorized == 401
else if resp.StatusCode == http.StatusUnauthorized {
    fmt.Println("\nUnauthorized. Access is denied due to invalid credentials.")
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read error response: %v", err)
    }
    fmt.Printf("Error Details: %s\n", body)
}
```

**Handling Not Found Errors (Status Code: 404)**

A **404** status code indicates that the requested resource was not found.
```Go
// http.StatusNotFound == 404
else if resp.StatusCode == http.StatusNotFound {
    fmt.Println("\nNot Found. The requested resource could not be found on the server.")
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read error response: %v", err)
    }
    fmt.Printf("Error Details: %s\n", body)
}
```

**Handling Internal Server Errors (Status Code: 500)**

The 500 status code denotes an internal server error, often requiring server-side fixes.
```Go
// http.StatusInternalServerError == 500
else if resp.StatusCode == http.StatusInternalServerError {
    fmt.Println("\nInternal Server Error. The server has encountered a situation it doesn't know how to handle.")
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read error response: %v", err)
    }
    fmt.Printf("Error Details: %s\n", body)
}
```

**Handling Unexpected Status Codes**

Finally, for any unexpected status codes, we generalize error handling.
```Go
else {
    fmt.Printf("\nUnexpected Status Code: %d\n", resp.StatusCode)
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read error response: %v", err)
    }
    fmt.Printf("Error Details: %s\n", body)
}
```
!









Example : Performing a Basic GET Request in Go
```Go
package main

import (
    "fmt"
    "io"
    "log"
    "net/http"
)

// TODO: Define the base URL for the API
const baseURL = "http://localhost:8000"


func main() {
    // TODO: Fetch all todos using the Get method
    resp, err := http.Get(baseURL + "/todos")
    if err != nil {
        log.Fatalf("Failed to get todos: %v", err)
    }
    defer resp.Body.Close()
    
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read response body: %v", err)
    }
    
    
    // TODO: Print raw response
    fmt.Println("Base URL is set to:", baseURL)
    fmt.Println(string(body))
    fmt.Println(string(body))
}

```

Example : Interact with APIs using Go: Fetching and Processing Data
```Go
package main

import (
    "fmt"
    "io"
    "log"
    "net/http"
)

const baseURL = "http://localhost:8000"

func main() {
    resp, err := http.Get(baseURL + "/todos")
    if err != nil {
        log.Fatalf("Failed to get todos: %v", err)
    }

    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read response body: %v", err)
    }

    // TODO: Check if the response status code is 200
    if resp.StatusCode == http.StatusOK {
        fmt.Println(string(body))
        if err != nil {
            log.Fatalf("Failed to read response body: %v", err)
        }
    }
        // TODO: Print the response data
    
}
```

Example: Handling 404 Errors in Go
```Go
package main

import (
    "fmt"
    "io"
    "log"
    "net/http"
)

const baseURL = "http://localhost:8000"

func main() {
    // TODO: Use an incorrect endpoint like '/todoos' to trigger a 404 error
    resp, err := http.Get(baseURL + "/todoos")
    if err != nil {
        log.Fatalf("Failed to get todos: %v", err)
    }
    defer resp.Body.Close()
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read response body: %v", err)
    }
    
    if resp.StatusCode == http.StatusOK {
        fmt.Println("Todos retrieved successfully:")
        fmt.Println(string(body))
     // TODO: Handle the 404 status code
    }else if resp.StatusCode == http.StatusNotFound {
    // - Output an appropriate message
    fmt.Println("\nNot Found. The requested resource could not be found on the server.")
    // - Display the error details
    fmt.Printf("Error Details: %s\n", body)
    }
}
```

Example: Performing GET Requests with Go
```Go
package main

import (
    "fmt"
    "io"
    "log"
    "net/http"
)

const baseURL = "http://localhost:8000"

func main() {
    resp, err := http.Get(baseURL + "/todos")
    if err != nil {
        log.Fatalf("Failed to get todos: %v", err)
    }
    defer resp.Body.Close()

    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read response body: %v", err)
    }

    if resp.StatusCode == http.StatusOK {
        fmt.Println("Todos retrieved successfully:")
        fmt.Println(string(body))
    } else if resp.StatusCode == http.StatusNotFound {
        fmt.Println("\nNot Found. The requested resource could not be found on the server.")
        fmt.Printf("Error Details: %s\n", body)
    } else {
        fmt.Printf("Unexpected status code: %d\n", resp.StatusCode)
        fmt.Printf("Error Details: %s\n", body)
    }
}
```


### Introduction to Path and Query Parameters in Go

**Introduction to Path and Query Parameters**

Welcome to another lesson of this course. In our previous lessons, we established a foundation by learning about **RESTful APIs** and making `GET` requests using Go's standard library. Now, we will shift our focus to **path** and **query parameters**, essential tools for refining API requests and fetching specific data.

Path and query parameters play a crucial role in making your API requests more precise and efficient. Imagine you are shopping online: selecting a specific item using its ID is akin to a path parameter, while filtering items by categories like price or color resembles query parameters. In this lesson, we'll explore these concepts with practical examples in Go, empowering you to extract just the information you need from an API.


**Understanding Path Parameters**

Path parameters are part of the URL used to access specific resources within an API, acting like unique identifiers. For example, if you want to retrieve a to-do item with ID `3`, the URL would be structured as follows:
```Go
http://localhost:8000/todos/3
```
In this case, `3` is the path parameter specifying the particular item you wish to access.

**Understanding Query Parameters**

Query parameters are added to the URL after a ? to filter or modify the data returned by the API. They are formatted as key-value pairs. For instance, if you want to list only the completed to-do tasks, your request would be:
```GO
http://localhost:8000/todos?done=true
```
Here, `done=true` is the query parameter filtering the results to include only tasks that are marked as completed.

**Setup Recap**

Before jumping into implementing these parameters using Go, let's make sure everything's set up. Start by importing the necessary packages and defining the base URL:
```GO
package main

import (
    "fmt"
    "net/http"
    "io"
    "log"
)

// Base URL for the API
const baseURL = "http://localhost:8000"

func main() {
    // Entry point
}
```
Now we're all set to dive into the code examples!

**Fetching Data with Path Parameters**

Path parameters are used to target specific resources within an API, allowing you to access individual items directly. They are appended directly to the endpoint URL. For instance, if you want to fetch details of a specific to-do item using its ID, you'd use a path parameter.

Here's a practical example using ID `3`:
```GO
func fetchTodoByID(todoID int) {
    url := fmt.Sprintf("%s/todos/%d", baseURL, todoID)

    resp, err := http.Get(url)
    if err != nil {
        log.Fatalf("Error fetching the todo with path parameter: %v", err)
    }

    if resp.StatusCode == http.StatusOK {
        data, _ := io.ReadAll(resp.Body)
        fmt.Printf("Todo Item: %s\n", data)
    } else {
        log.Printf("Failed to fetch todo. Status Code: %d", resp.StatusCode)
    }
}

func main() {
    fetchTodoByID(3)
}
```
In this example, `todoID` is a path parameter specifying the to-do item with ID `3`. If successful, the function prints the details of that specific item. This demonstrates how path parameters enable you to access individual resources accurately.


**Filtering Data with Query Parameters**

Query parameters are attached to the URL to filter or modify the results returned by the API. They're especially useful for searching or narrowing down data without altering the overall resource structure.

Let's filter the to-do items to list only those marked as done:
```GO
func fetchCompletedTodos() {
    url := fmt.Sprintf("%s/todos?done=true", baseURL)

    resp, err := http.Get(url)
    if err != nil {
        log.Fatalf("Error fetching todos with query parameter: %v", err)
    }

    if resp.StatusCode == http.StatusOK {
        data, _ := io.ReadAll(resp.Body)
        fmt.Printf("Completed Todos: %s\n", data)
    } else {
        log.Printf("Failed to fetch todos. Status Code: %d", resp.StatusCode)
    }
}

func main() {
    fetchCompletedTodos()
}
```
Here, the query parameter done=true is used to filter the results. The function focuses only on completed tasks, demonstrating how query parameters can streamline outputs by highlighting specific criteria.

**Using Multiple Query Parameters**

To refine data retrieval further, you can combine multiple query parameters. For instance, you might want to fetch to-do items that are marked as done and also have titles that start with a specific prefix.

Here's how you can filter to-do items that are completed and have titles starting with the prefix "c":

```GO
func fetchFilteredTodos(doneStatus, titlePrefix string) {
    url := fmt.Sprintf("%s/todos?done=%s&title=%s", baseURL, doneStatus, titlePrefix)

    resp, err := http.Get(url)
    if err != nil {
        log.Fatalf("Error fetching todos with multiple query parameters: %v", err)
    }

    if resp.StatusCode == http.StatusOK {
        data, _ := io.ReadAll(resp.Body)
        fmt.Printf("Filtered Todos: %s\n", data)
    } else {
        log.Printf("Failed to fetch todos. Status Code: %d", resp.StatusCode)
    }
}

func main() {
    fetchFilteredTodos("true", "c")
}
```
n this example, the query parameters `done=true` and `title=c` are used together to filter the results. If successful, the function retrieves items that are both completed and begin with `"c"`.

Example : Fetching Todo Item Using Path Parameters in Go
```Go
package main

import (
    "fmt"
    "io"
    "log"
    "net/http"
)

// Base URL for the API
const baseURL = "http://localhost:8000"

func main() {
    // Define the ID of the todo item you want to retrieve
    todoID := 2

    // Make a GET request using the path parameter
    url := fmt.Sprintf("%s/todos/%d", baseURL, todoID)

    // Send GET request
    resp, err := http.Get(url)
    if err != nil {
        log.Fatalf("Error fetching the todo with path parameter: %v", err)
    }

    // Close response body when function ends
    defer resp.Body.Close()

    // Check if the request was successful
    if resp.StatusCode == http.StatusOK {

        // Read response body
        data, err := io.ReadAll(resp.Body)
        if err != nil {
            log.Fatalf("Failed to read response body: %v", err)
        }

        // Print todo item
        fmt.Printf("Todo Item: %s\n", data)

    } else {

        // Print error status code
        log.Printf("Failed to fetch todo. Status Code: %d", resp.StatusCode)
    }
}
```

Example 2: Modifying Path Parameters to Simulate a Not Found Item in Go
```Go
package main

import (
    "fmt"
    "net/http"
    "io"
    "log"
)

// Base URL for the API
const baseURL = "http://localhost:8000"

func main() {
    // TODO: Change the todoID to 999 to intentionally not find the item
    todoID := 2

    // Make a GET request using the path parameter for the specific todo item
    url := fmt.Sprintf("%s/todos/%d", baseURL, todoID)

    resp, err := http.Get(url)
    if err != nil {
        log.Fatalf("Error fetching the todo with path parameter: %v", err)
    }
    defer resp.Body.Close()

    if resp.StatusCode == http.StatusOK {
        data, _ := io.ReadAll(resp.Body)
        fmt.Printf("Todo Item: %s\n", data)
    } else {
        fmt.Println("Error fetching the todo with path parameter")
        fmt.Printf("Status Code: %d\n", resp.StatusCode)
        data, _ := io.ReadAll(resp.Body)
        fmt.Printf("Error Details: %s\n", data)
    }
}
```

```Go
package main

import (
    "fmt"
    "log"
    "net/http"
    "io"
)

const baseURL = "http://localhost:8000"

func main() {
    // TODO: Include a query parameter to filter for not done items
    url := fmt.Sprintf("%s/todos?done=false", baseURL)

    resp, err := http.Get(url)
    if err != nil {
        log.Fatalf("Error fetching todos: %v", err)
    }
    defer resp.Body.Close()

    if resp.StatusCode == http.StatusOK {
        data, _ := io.ReadAll(resp.Body)
        fmt.Printf("Todos: %s\n", data)
    } else {
        log.Printf("Failed to fetch todos. Status Code: %d", resp.StatusCode)
    }
}
```

example: Modifying Query Parameters in Go
```Go
package main

import (
    "fmt"
    "net/http"
    "io"
    "log"
)

const baseURL = "http://localhost:8000"

func main() {
    // TODO: Change the parameters to filter todos that are done and start with 'w'
    url := fmt.Sprintf("%s/todos?done=true&title=w", baseURL)

    resp, err := http.Get(url)
    if err != nil {
        log.Fatalf("Error fetching todos with query parameters: %v", err)
    }
    defer resp.Body.Close()

    if resp.StatusCode == http.StatusOK {
        data, _ := io.ReadAll(resp.Body)
        fmt.Printf("Filtered Todos: %s\n", data)
    } else {
        log.Printf("Failed to fetch todos. Status Code: %d", resp.StatusCode)
    }
}
```



### Sending Data with POST Requests in Go

**Sending Data with POST Requests**

Welcome to this lesson on sending data with **POST requests** using Go's net/http package. As you continue your exploration of interacting with **RESTful APIs**, you'll learn how to send data to a server using the POST method. **POST requests** are essential when you want to create new resources or submit data, such as filling out a web form or adding a new entry to a database. Unlike GET requests, which allow you to retrieve data, POST requests transmit data to an API.

Understanding these differences is crucial as you expand your skill set in HTTP methods. Let’s dive deeper into utilizing **POST requests** to comprehend how they stand apart from **GET requests**.

**Key Differences Between GET and POST**

Before diving into POST requests, let's briefly compare them to GET requests:

- **GET Requests**:

    - **Purpose**: Retrieve data from a server.

    - **Data Location**: Data is sent in the URL as path or query parameters.

    - **Success Status**: Expect a http.StatusOK status code for successful data retrieval.

- **POST Requests**:

    - **Purpose**: Send data to a server to create or update a resource, such as submitting a form, uploading a file, or adding a new item to a database.

    - **Data Location**: Data is sent in the request body.

    - **Success Status**: Expect a http.StatusCreated status code for successful resource creation.

These differences clarify when to use each method. **POST requests**, in particular, require careful handling of the request body.



**Understanding the Request Body**

For **POST requests**, the request body is crucial as it holds the data you want to send to the server. This data is usually structured in formats like JSON or XML, with JSON being a common choice due to its readability and compatibility.

Here’s an example of a JSON request body represented as a string:

```Go
jsonBody := `{
  "title": "Learn Go http requests",
  "description": "Complete a course on Go API calls.",
  "done": false
}`
```
This represents a new todo item, including a title, completion status, and description. Noticeably, we are not sending an `id`, as this is typically managed by the server upon resource creation.



**Crafting a Post Request: Adding a New Todo**

Let's walk through an example of how to craft a POST request to add a new todo item to our API using Go's `net/http` package.

First, we will need to prepare the data we wish to send. Here, we'll be adding a new todo item with a specific title, a completion status, and a description. The data is represented as a JSON string:
```Go
// New todo data structured as JSON string
jsonBody := `{
  "title": "Learn Go http requests",
  "description": "Complete a course on Go API calls.",
  "done": false
}`

// Base URL for the API
baseURL := "http://localhost:8000"

// Complete endpoint URL
endpoint := baseURL + "/todos"

// Send the POST request
resp, err := http.Post(endpoint, "application/json", bytes.NewBufferString(jsonBody))
if err != nil {
    log.Fatalf("Failed to send POST request: %v", err)
}
```

**Handling Responses and Error Management**

Interpreting the response from a POST request is an integral part of the process. After sending the request, the server provides a response indicating whether the operation was successful. A `http.StatusCreated` status code signifies successful resource creation.

```Go
// Check if the request was successful
if resp.StatusCode == http.StatusCreated {
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read response body: %v", err)
    }
    fmt.Println("New todo added successfully!")
    fmt.Println(string(body))
} else {
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read error response body: %v", err)
    }
    fmt.Printf("Failed to add a new todo\nStatus Code: %d\nError Details: %s\n", resp.StatusCode, string(body))
}
```
The code assesses the server's response: a `http.StatusCreated` status code confirms successful creation, allowing you to print and use the details of the new todo, typically included in the server's response. Otherwise, the code handles potential errors by outputting relevant error information.

**Example of Unsuccessful POST Request**

A POST request may fail if required fields are missing. For example, omitting a "title" might lead to an error response:
```Go
// New todo data with missing title
jsonBody := `{
  "description": "Complete a course on Go API calls.",
  "done": false
}`

// Send the POST request
resp, err := http.Post(endpoint, "application/json", bytes.NewBufferString(jsonBody))
if err != nil {
    log.Fatalf("Failed to send POST request: %v", err)
}

// Check if the request was successful
if resp.StatusCode == http.StatusCreated {
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read response body: %v", err)
    }
    fmt.Println("New todo added successfully!")
    fmt.Println(string(body))
} else {
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read error response body: %v", err)
    }
    fmt.Printf("Failed to add a new todo\nStatus Code: %d\nError Details: %s\n", resp.StatusCode, string(body))
}
```
Running this code could produce an error message similar to the following, depending on the server's implementation:
```
Failed to add a new todo
Status Code: 400
Error Details: {"error": "Invalid request. 'title' is required."}
```
The `400` status code signifies a bad request, which is often due to missing or incorrect data in the request body. In this case, the error message specifies that the "title" field is required, making it clear what needs to be corrected for the POST request to succeed. This feedback allows you to quickly identify and amend the missing or erroneous part of the data before resending the request.





Example : Fixing Bugs in a Go POST Request Script
```Go
package main

import (
    "fmt"
    "io"
    "log"
    "net/http"
    "bytes"
)

func main() {
    // Base URL for the API
    baseURL := "http://localhost:8000"

    // New todo data structured as JSON string
    jsonBody := `{
        "title": "Learn Go http package",
        "description": "Complete a course on Go API calls.",
        "done": false
    }`

    // Complete endpoint URL
    endpoint := baseURL + "/todos"

    // Create a new GET request (this is incorrect)
    resp, err := http.Post(endpoint, "application/json", bytes.NewBufferString(jsonBody))
    if err != nil {
        log.Fatalf("Failed to create request: %v", err)
    }
    
    // Check if the request was successful (incorrect status code check)
    if resp.StatusCode == http.StatusCreated {
        body, err := io.ReadAll(resp.Body)
        if err != nil {
            log.Fatalf("Failed to read response body: %v", err)
        }
        fmt.Println("New todo added successfully!")
        fmt.Println(string(body))
    } else {
        body, err := io.ReadAll(resp.Body)
        if err != nil {
            log.Fatalf("Failed to read error response body: %v", err)
        }
        fmt.Printf("Failed to add a new todo\nStatus Code: %d\nError Details: %s\n", resp.StatusCode, string(body))
    }
}
```

Example : Verifying Todo Addition with POST and GET Requests in Go
```Go
package main

import (
    "bytes"
    "fmt"
    "io"
    "log"
    "net/http"
)

func main() {
    // Base URL for the API
    baseURL := "http://localhost:8000"

    // New todo data structured as JSON string
    jsonBody := `{
        "title": "Learn Go http requests",
        "description": "Complete a course on Go API calls.",
        "done": false
    }`

    // Send POST request
    resp, err := http.Post(baseURL+"/todos", "application/json", bytes.NewBufferString(jsonBody))
    if err != nil {
        log.Fatalf("Failed to send POST request: %v", err)
    }

    // Check if the request was successful
    if resp.StatusCode == http.StatusCreated {
        fmt.Println("New todo added successfully!")
        body, err := io.ReadAll(resp.Body)
        if err != nil {
            log.Fatalf("Failed to read response body: %v", err)
        }
        fmt.Println(string(body))

        // TODO: Perform a GET request
        url := fmt.Sprintf(baseURL + "/todos")
        resp, err := http.Get(url)
        if err != nil {
        log.Fatalf("Error fetching the todo with path parameter: %v", err)
        }
        defer resp.Body.Close()
        // TODO: Check if the GET request was successful and print the new todo item
        if resp.StatusCode == http.StatusOK {
        data, _ := io.ReadAll(resp.Body)
        fmt.Printf("Filtered Todos: %s\n", data)
        }
        // TODO: Handle potential errors from the GET request
        
    } else {
        // Handle potential errors from the POST request
        body, err := io.ReadAll(resp.Body)
        if err != nil {
            log.Fatalf("Failed to read error response body: %v", err)
        }
        fmt.Printf("Failed to add a new todo\nStatus Code: %d\nError Details: %s\n", resp.StatusCode, string(body))
    }
}
```


Example: Crafting a POST Request in Go
```Go
package main



import (
    // TODO: Import necessary packages
    "fmt"
    "io"
    "log"
    "net/http"
    "bytes"
)

func main() {
    // Base URL for the API
    baseURL := "http://localhost:8000"

    // TODO: Define the new todo data as a JSON string
    jsonBody := `{
        "title": "Learn Go http package",
        "description": "Complete a course on Go API calls.",
        "done": false
    }`
    // TODO: Use http.NewRequest to create a POST request to the endpoint {baseURL}/todos
    endpoint := baseURL + "/todos"
    // - Use the JSON data as the body of your request
    req, err := http.NewRequest("POST", endpoint, bytes.NewBufferString(jsonBody))
    if err != nil {
        log.Fatalf("Failed to send POST request: %v", err)
    }
    req.Header.Set("Content-Type", "application/json")
    // TODO: Send the POST request and handle the response
    // - Use http.Client to send the request
    client := &http.Client{}
    resp, err := client.Do(req)
    if err != nil {
        log.Fatalf("Failed to send POST request: %v", err)
    }
    defer resp.Body.Close()
    
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        log.Fatalf("Failed to read response body: %v", err)
    }
    
    // - Check if the response status code is http.StatusCreated
    if resp.StatusCode == http.StatusCreated {
        //   - If it is, print a success message and the response body
        fmt.Println("New todo added successfully!")
        fmt.Println(string(body))
        //   - If not, print an error message with the status code and response body
        } else {
        fmt.Printf("Request failed. Status Code: %d\n", resp.StatusCode)
        fmt.Printf("Error Details: %s\n", string(body))
        }
}
```









### Updating and Deleting Resources with PUT, PATCH, and DELETE

Welcome to this lesson focusing on enhancing your skills in interacting with **RESTful APIs** by updating and deleting resources. In previous lessons, you learned how to retrieve and create resources using the `GET` and `POST` methods. Now, you will explore the `PUT`, `PATCH`, and `DELETE` methods, which are crucial for modifying existing resources or removing them when they are no longer needed.


**Understanding PUT, PATCH, and DELETE Methods**

To effectively manage resources within an API, it's essential to understand how to use the following methods:

**PUT**: This method is used for completely updating a resource. You replace the current resource with the new data you provide in the request body. For example, if you're updating a to-do item, you'll include all the new details of the item in the body of the request and specify the resource ID in the request path to indicate which item you are updating.

**PATCH**: Use this method to make partial updates to a resource. You only need to include the specific fields you're updating in the request body. For instance, if you're only changing the description of a to-do item, you'll pass just the new description in the body. The resource ID is specified in the request path to identify which item is being modified.

**DELETE**: This straightforward method removes a resource from the server. You specify which resource to delete by including its identifier in the request path. No request body is needed since you're not sending any data, just instructing the server to remove the resource corresponding to that ID.

Successful requests using these methods typically return status codes of `200` (OK) or `204` (No Content). A `200` status code means the operation was successful and also returns content, such as a representation of the updated resource. In contrast, a `204` status code indicates success but with no content returned, meaning the server successfully processed the request but isn't providing any additional information. These methods are crucial for performing full **CRUD** (Create, Read, Update, Delete) functionality in API management.

**Setting Up the Go Environment**

Before executing HTTP requests, let's set up our Go environment by importing the necessary Go packages and defining the base URL for our API interactions:
```Go
package main

import (
    "bytes"
    "fmt"
    "io"
    "net/http"
)

// Base URL for the API
const baseUrl = "http://localhost:8000"
```
With this setup in place, you're well-prepared to explore `PUT`, `PATCH`, and `DELETE` operations for resource management.


**Using `http.NewRequest` for Custom HTTP Requests**

So far, we’ve used `http.Get` and `http.Post` to interact with APIs. These functions work well for simple requests, but sometimes we need more control—especially when using **PUT**, **PATCH**, and **DELETE**, which don’t have built-in helper functions in Go.

For these cases, Go provides the `http.NewRequest` function, which allows us to create a customizable HTTP request.


**Why Use `http.NewRequest`?**

- **Supports all HTTP methods** (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`, etc.).

- Allows us to **set headers** (like `"Content-Type"`: `"application/json"`).

- Gives more control over the request body.


**Creating a Custom HTTP Request**

Here’s an example of how to create a request using `http.NewRequest`:
```Go
req, err := http.NewRequest(http.MethodPut, "http://example.com/resource", nil)
if err != nil {
    log.Fatalf("Failed to create request: %v", err)
}
```
Once the request is created, we send it using an http.Client:

```Go
client := &http.Client{}
resp, err := client.Do(req)
if err != nil {
    log.Fatalf("Failed to send request: %v", err)
}
```
Since PUT, PATCH, and DELETE require us to send structured data, we use http.NewRequest to define these requests, then send them using an http.Client.

Next, let’s see http.NewRequest in action as we update resources with the PUT method.

**Updating Resources with PUT**

The `PUT` method is utilized when you need to update an entire resource. Imagine you have a to-do item that needs a complete overhaul, such as updating the task description, adding more details, or marking it as completed. The `PUT` request replaces the current resource representation with the new one you provide.

Here's an example of how to send a `PUT` request to update a to-do item:
```Go
func updateTodoWithPut(todoID int) {
    // Updated todo data for PUT
    updatedTodo := `{
        "title": "Buy groceries and snacks", 
        "done": true, 
        "description": "Milk, eggs, bread, coffee, and chips."
    }`

    // Send PUT request
    request, err := http.NewRequest(http.MethodPut, baseUrl+"/todos/"+fmt.Sprint(todoID), bytes.NewBufferString(updatedTodo))
    if err != nil {
        fmt.Println("Error creating request:", err)
        return
    }
    request.Header.Set("Content-Type", "application/json")

    client := &http.Client{}
    response, err := client.Do(request)
    if err != nil {
        fmt.Println("Error sending request:", err)
        return
    }

    // Check if the request was successful
    if response.StatusCode == http.StatusOK {
        body, _ := io.ReadAll(response.Body)
        fmt.Println("Todo updated successfully with PUT!")
        fmt.Println(string(body))
    } else {
        // Handle potential errors
        fmt.Println("Error updating the todo with PUT")
        fmt.Printf("Status Code: %d\n", response.StatusCode)
        body, _ := io.ReadAll(response.Body)
        fmt.Println("Error Details:", string(body))
    }
}
```
n this code, we specify the complete new data for the to-do item and send the request using Go's `http.NewRequest` method. Upon receiving a `200` status code using `http.StatusOK`, you know the update was successful, and you can print the new representation of the to-do item. It's important to note that for our API, a `PUT` request requires all fields (`title`, `description`, and `done`) to be provided. If any field is missing, the request will fail and return a status code of `400`. In cases where you only need to update specific fields, rather than the entire resource, the `PATCH` method is more suitable.


**Partial Updates with PATCH**

Unlike `PUT`, the `PATCH` method is specifically designed for partial updates. When you need to modify only a specific field of a resource, PATCH ensures system efficiency and data integrity without the need to send a full representation.

Here's an example of performing a `PATCH` request on a resource:

```Go
func updateTodoWithPatch(todoID int) {
    // Partial updated data
    patchData := `{
        "description": "Updated description"
    }`

    // Send PATCH request
    request, err := http.NewRequest(http.MethodPatch, baseUrl+"/todos/"+fmt.Sprint(todoID), bytes.NewBufferString(patchData))
    if err != nil {
        fmt.Println("Error creating request:", err)
        return
    }
    request.Header.Set("Content-Type", "application/json")

    client := &http.Client{}
    response, err := client.Do(request)
    if err != nil {
        fmt.Println("Error sending request:", err)
        return
    }

    // Check if the request was successful
    if response.StatusCode == http.StatusOK {
        body, _ := io.ReadAll(response.Body)
        fmt.Println("Todo updated successfully with PATCH!")
        fmt.Println(string(body))
    } else {
        // Handle potential errors
        fmt.Println("Error updating the todo with PATCH")
        fmt.Printf("Status Code: %d\n", response.StatusCode)
        body, _ := io.ReadAll(response.Body)
        fmt.Println("Error Details:", string(body))
    }
}
```
In this scenario, only the `description` field is updated. After sending the `PATCH` request using Go's `http.NewRequest` method, we can check the response status code to confirm success. Successful updates with a status code of `200` allow you to view the updated resource fields, while any errors are managed by inspecting and printing error details.


**Deleting Resources with DELETE**

After learning how to update resources using `PUT` and `PATCH`, let's move on to deleting resources with the `DELETE` method. This method is straightforward - it's used to remove a resource from the server, which is important for keeping your data clean and relevant.

Here's how you can perform a `DELETE` request:
```Go
func deleteTodoWithDelete(todoID int) {
    // Send DELETE request
    request, err := http.NewRequest(http.MethodDelete, baseUrl+"/todos/"+fmt.Sprint(todoID), nil)
    if err != nil {
        fmt.Println("Error creating request:", err)
        return
    }

    client := &http.Client{}
    response, err := client.Do(request)
    if err != nil {
        fmt.Println("Error sending request:", err)
        return
    }

    // Check if the request was successful
    if response.StatusCode == http.StatusNoContent {
        fmt.Println("Todo deleted successfully!")
    } else {
        // Handle potential errors
        fmt.Println("Error deleting the todo")
        fmt.Printf("Status Code: %d\n", response.StatusCode)
        body, _ := io.ReadAll(response.Body)
        fmt.Println("Error Details:", string(body))
    }
}
```
In this example, we're sending a `DELETE` request to remove a specific to-do item by specifying its ID in the request path. When the server successfully deletes the resource, you will typically receive a `204` status code using `http.StatusNoContent`. This code means "No Content," indicating that the deletion was successful, but there's no additional data to send back.

However, it's important to note that some services might return a `200` status code along with some content, such as a message. This can vary depending on how the service is designed, so always check the API documentation for specifics on how it handles deletions.


example 1 : Implementing PUT Method for Todo Update in Go
```go
package main

import (
    "bytes"
    "fmt"
    "io"
    "net/http"
)

func main() {
    // Base URL for the API
    const baseUrl = "http://localhost:8000"

    // Todo ID to update
    todoID := 1

    // TODO: Define a json string with the updated todo data, including "title", "description", and "done"
    updatedTodo := `{
        "title": "Buy groceries and snacks", 
        "done": true, 
        "description": "Milk, eggs, bread, coffee, and chips."
    }`
    

    // TODO: Send a PUT request to update the todo with the specified ID using the updated data
    request, err := http.NewRequest(http.MethodPut, baseUrl+"/todos/"+fmt.Sprint(todoID), bytes.NewBufferString(updatedTodo))
    if err != nil {
        fmt.Println("Error creating request:", err)
        return
    }
    request.Header.Set("Content-Type", "application/json")
    
    client := &http.Client{}
    response, err := client.Do(request)
    if err != nil {
        fmt.Println("Error sending request:", err)
        return
    }

    
    // TODO: Check if the request was successful (status code 200) and print a success message along with the updated todo
        if response.StatusCode == http.StatusOK {
        body, _ := io.ReadAll(response.Body)
        fmt.Println("Todo updated successfully with PUT!")
        fmt.Println(string(body))
        } else {
    // TODO: Handle potential errors by printing an error message, status code, and error details if the update fails
        fmt.Println("Error updating the todo with PUT")
        fmt.Printf("Status Code: %d\n", response.StatusCode)
        body, _ := io.ReadAll(response.Body)
        fmt.Println("Error Details:", string(body))    
    }       
}
   
```

Example 2 : Partial Update of a Todo Item in Go
```Go
package main

import (
    "bytes"
    "fmt"
    "io"
    "net/http"
)

func main() {
    // Base URL for the API
    const baseUrl = "http://localhost:8000"

    // Todo ID to update
    todoID := 1

    // TODO: Modify the data to send only the updated 'done' status, remove 'title' and 'description'
    patchData := `{
        "done": true
    }`

    // TODO: Change the request method from PUT to PATCH
    request, err := http.NewRequest(http.MethodPatch, baseUrl+"/todos/"+fmt.Sprint(todoID), bytes.NewBufferString(patchData))
    if err != nil {
        fmt.Println("Error creating request:", err)
        return
    }
    request.Header.Set("Content-Type", "application/json")

    client := &http.Client{}
    response, err := client.Do(request)
    if err != nil {
        fmt.Println("Error sending request:", err)
        return
    }
    defer response.Body.Close()

    // Check if the request was successful
    if response.StatusCode == http.StatusOK {
        body, _ := io.ReadAll(response.Body)
        fmt.Println("Todo updated successfully!")
        fmt.Println(string(body))
    } else {
        // Handle potential errors
        fmt.Println("Error updating the todo")
        fmt.Printf("Status Code: %d\n", response.StatusCode)
        body, _ := io.ReadAll(response.Body)
        fmt.Println("Error Details:", string(body))
    }
}
```

example: Changing PATCH to DELETE in Go API Request
```Go
package main

import (
    "fmt"
    "net/http"
)

// Base URL for the API
const baseUrl = "http://localhost:8000"

func main() {
    // Todo ID to update
    todoID := 3


    // TODO: Modify the PATCH request to a DELETE request and remove patchData
    request, err := http.NewRequest(http.MethodDelete, fmt.Sprintf("%s/todos/%d", baseUrl, todoID), nil)
    if err != nil {
        fmt.Println("Error creating request:", err)
        return
    }

    client := &http.Client{}
    response, err := client.Do(request)
    if err != nil {
        fmt.Println("Error sending request:", err)
        return
    }
    defer response.Body.Close()

    // TODO: Update response handling to support DELETE method and 204 status code
    if response.StatusCode == http.StatusNoContent {
        fmt.Println("Todo delete successfully!")
    } else {
        // Handle potential errors
        fmt.Println("Error deleting the todo")
        fmt.Printf("Status Code: %d\n", response.StatusCode)
    }
}
```





example : Implementing PUT, PATCH, and DELETE HTTP Requests in Go
```Go
package main

import (
    "bytes"
    "fmt"
    "io"
    "net/http"
    "log"
)

const baseUrl = "http://localhost:8000"

func handleError(response *http.Response) {
    fmt.Println("Error occurred")
    fmt.Printf("Status Code: %d\n", response.StatusCode)
    body, _ := io.ReadAll(response.Body)
    fmt.Println("Error Details:", string(body))
}

// TODO: Define a function named `putTodo` to handle PUT requests.
func putTodo(todoID int, data string) {
    // - The function should take `todoID` and `data` as parameters.   
    // - Construct the PUT request URL using `baseUrl` and `todoID`, send the request, and check the response status.
    request, err := http.NewRequest(http.MethodPut, baseUrl+"/todos/"+fmt.Sprint(todoID), bytes.NewBufferString(data))
    if err != nil {
        log.Fatalf("Failed to create request: %v", err)
        return
    }

    request.Header.Set("Content-Type", "application/json")

    client := &http.Client{}
    response, err := client.Do(request)
    if err != nil {
        fmt.Println("Error sending request:", err)
        return
    }
    defer response.Body.Close()

    // - Print a success message and the response body if the status code is 200, otherwise call `handleError`.
    if response.StatusCode == http.StatusOK {
        body, _ := io.ReadAll(response.Body)
        fmt.Println("Todo updated successfully with PUT!")
        fmt.Println(string(body))
    } else {
        handleError(response)
    }
}



// TODO: Define a function named `patchTodo` to handle PATCH requests.
func patchTodo(todoID int, data string) {
// - The function should take `todoID` and `data` as parameters.

// - Construct the PATCH request URL using `baseUrl` and `todoID`, send the request, and check the response status.
    request, err := http.NewRequest(http.MethodPatch, baseUrl+"/todos/"+fmt.Sprint(todoID), bytes.NewBufferString(data))
    if err != nil {
        log.Fatalf("Failed to create request: %v", err)
        return
    }
    
    request.Header.Set("Content-Type", "application/json")
    client := &http.Client{}
	response, err := client.Do(request)
	if err != nil {
		log.Fatalf("Error sending PATCH request: %v", err)
	}
	defer response.Body.Close()
    
// - Print a success message and the response body if the status code is 200, otherwise call `handleError`.   
    if response.StatusCode == http.StatusOK {
        body, _ := io.ReadAll(response.Body)
        fmt.Println("Todo updated successfully with PATCH!")
        fmt.Println(string(body))
    } else {
        handleError(response)
    }
}

// TODO: Define a function named `deleteTodo` to handle DELETE requests.
func deleteTodo(todoID int) {
// - The function should take `todoID` as a parameter. 
// - Construct the DELETE request URL using `baseUrl` and `todoID`, send the request, and check the response status.
    request, err := http.NewRequest(http.MethodDelete, baseUrl+"/todos/"+fmt.Sprint(todoID), nil)
    if err != nil {
        log.Fatalf("Failed to create request: %v", err)
        return
    }
    client := &http.Client{}
	response, err := client.Do(request)
	if err != nil {
		log.Fatalf("Error sending DELETE request: %v", err)
	}
	defer response.Body.Close()
// - Print a success message if the status code is 204, otherwise call `handleError`.
    if response.StatusCode == http.StatusNoContent {
        fmt.Println("Todo delete succsessfully!")
    } else {
        handleError(response)
    }
}




func main() {
    putTodo(1, `{"title": "Buy groceries and snacks", "description": "Milk, eggs, bread, coffee, and chips.", "done": true}`)
    patchTodo(2, `{"description": "Updated description"}`)
    deleteTodo(3)
}
``` 


example : Managing Todo List with HTTP Methods in Go
```Go
package main

import (
    "bytes"
    "fmt"
    "io"
    "net/http"
)

const baseUrl = "http://localhost:8000"

func handleGetAllTodos() {
    resp, err := http.Get(baseUrl + "/todos")
    if err != nil {
        fmt.Println("Error getting todos:", err)
        return
    }
    defer resp.Body.Close()

    if resp.StatusCode == http.StatusOK {
        body, _ := io.ReadAll(resp.Body)
        fmt.Println("\nAll todos retrieved successfully!")
        fmt.Println(string(body))
    } else {
        handleError(resp)
    }
}

func handlePost(data string) {
    resp, err := http.Post(baseUrl+"/todos", "application/json", bytes.NewBufferString(data))
    if err != nil {
        fmt.Println("Error posting todo:", err)
        return
    }
    defer resp.Body.Close()

    if resp.StatusCode == http.StatusCreated {
        body, _ := io.ReadAll(resp.Body)
        fmt.Println("Todo created successfully!")
        fmt.Println(string(body))
    } else {
        handleError(resp)
    }
}

func handlePut(todoID int, data string) {
    request, err := http.NewRequest(http.MethodPut, fmt.Sprintf("%s/todos/%d", baseUrl, todoID), bytes.NewBufferString(data))
    if err != nil {
        fmt.Println("Error creating request:", err)
        return
    }
    request.Header.Set("Content-Type", "application/json")

    client := &http.Client{}
    resp, err := client.Do(request)
    if err != nil {
        fmt.Println("Error sending request:", err)
        return
    }
    defer resp.Body.Close()

    if resp.StatusCode == http.StatusOK {
        body, _ := io.ReadAll(resp.Body)
        fmt.Println("Todo updated successfully with PUT!")
        fmt.Println(string(body))
    } else {
        handleError(resp)
    }
}

func handlePatch(todoID int, data string) {
    request, err := http.NewRequest(http.MethodPatch, fmt.Sprintf("%s/todos/%d", baseUrl, todoID), bytes.NewBufferString(data))
    if err != nil {
        fmt.Println("Error creating request:", err)
        return
    }
    request.Header.Set("Content-Type", "application/json")

    client := &http.Client{}
    resp, err := client.Do(request)
    if err != nil {
        fmt.Println("Error sending request:", err)
        return
    }
    defer resp.Body.Close()

    if resp.StatusCode == http.StatusOK {
        body, _ := io.ReadAll(resp.Body)
        fmt.Println("Todo updated successfully with PATCH!")
        fmt.Println(string(body))
    } else {
        handleError(resp)
    }
}

func handleDelete(todoID int) {
    request, err := http.NewRequest(http.MethodDelete, fmt.Sprintf("%s/todos/%d", baseUrl, todoID), nil)
    if err != nil {
        fmt.Println("Error creating request:", err)
        return
    }

    client := &http.Client{}
    resp, err := client.Do(request)
    if err != nil {
        fmt.Println("Error sending request:", err)
        return
    }
    defer resp.Body.Close()

    if resp.StatusCode == http.StatusNoContent {
        fmt.Println("Todo deleted successfully!")
    } else {
        handleError(resp)
    }
}

func handleError(resp *http.Response) {
    fmt.Println("Error occurred")
    fmt.Printf("Status Code: %d\n", resp.StatusCode)
    body, _ := io.ReadAll(resp.Body)
    fmt.Println("Error Details:", string(body))
}

// Call appropriate functions based on the provided scenarios
// Use:
// - handlePatch() for partial updates
// - handlePut() for full updates
// - handleDelete() for removing items
// - handlePost() for adding new items
// - Verify the final state with handleGetAllTodos()

func main() {
    handlePatch(1, `{"title": "Buy groceries and snacks", "description": "Milk, eggs, bread, coffee, and chips", "done": false}`)
    handleDelete(2)
    handlePatch(3, `{"title": "Complete project report", "done": true}`)
    handleDelete(4)
    handlePost(`{"title": "Read new book", "description": "Start reading 'Atomic Habits'", "done": false}`)
    handlePost(`{"title": "Attend yoga class", "description": "Morning session", "done": false}`)
    handleGetAllTodos()
}
```



### Introduction to JSON and Go Structs

Welcome to the first lesson of our course on handling JSON in Go. JSON, which stands for JavaScript Object Notation, is a lightweight data interchange format that is easy for humans to read and write and easy for machines to parse and generate. It is widely used in web services for data exchange. In this lesson, we will explore how Go, a statically typed language, uses structs to manage and organize JSON data. Understanding how to work with JSON and Go structs is crucial for interacting with APIs, as it allows you to seamlessly integrate and manipulate data within your Go application


**Defining Structs in Go**
In Go, a struct is a composite data type that groups together variables under a single name. These variables, known as fields, can be of different types. Structs are particularly useful when working with JSON data, as they allow you to define a clear structure for the data you expect to receive or send.

To define a struct in Go, you use the type keyword followed by the struct name and the struct keyword. Each field in the struct is defined with a name and a type. Additionally, you can use struct tags to specify how the fields should be serialized or deserialized when working with JSON. These tags are placed after the field type and are enclosed in backticks.

For example, consider the following struct definition:
```Go
type Todo struct {
    ID        int    `json:"id"`
    Title     string `json:"title"`
    Completed bool   `json:"completed"`
}
```
In this example, the `Todo` struct has three fields: `ID`, `Title`, and `Completed`. The struct tags specify the JSON keys that correspond to each field. This means that when the struct is serialized to `JSON`, the `ID` field will be represented as `"id"`, the `Title` field as `"title"`, and the `Completed` field as `"completed"`.

**Example: Creating and Using a Go Struct for JSON Data**

Let's walk through an example to see how you can create and use a Go struct for JSON data. We'll use the `Todo` struct we defined earlier.

First, we initialize an instance of the `Todo` struct with some sample data:
```Go
myTodo := Todo{
    ID:        1,
    Title:     "Get groceries",
    Completed: false,
}
```
Here, we create a myTodo variable of type `Todo` and assign values to its fields. The `ID` is set to `1`, the `Title` is set to `"Get groceries"`, and `Completed` is set to `false`.

Next, we print the struct data using the `fmt.Printf` function:
```Go
fmt.Printf("%+v\n", myTodo)
```

The `%+v` verb in `fmt.Printf` is used to print the struct with field names. The output of this code will be:
```
{ID:1 Title:Get groceries Completed:false}
```

**Accessing Struct Fields in Go**

Once you have created a struct instance, you can access its fields using **dot notation** (`.`). This allows you to retrieve and display the values of specific fields easily. Printing struct fields is useful for debugging and understanding the data stored in a struct.

For example, using the `Todo` struct, you can print individual fields like this:
```Go
myTodo := Todo{
    ID:        1,
    Title:     "Get groceries",
    Completed: false,
}

fmt.Println("Todo ID:", myTodo.ID)
fmt.Println("Todo Title:", myTodo.Title)
fmt.Println("Is Completed?", myTodo.Completed)
```
This will output:
```Go
Todo ID: 1
Todo Title: Get groceries
Is Completed? false
```
Using this approach, you can display structured data clearly and effectively in your Go programs.

**Serializing and Deserializing JSON in Go (Brief Overview)**

While we won't delve deeply into serialization and deserialization in this lesson, it's important to understand these concepts at a high level. Serialization, also known as marshaling, is the process of converting a Go struct into JSON format. Deserialization, or unmarshaling, is the reverse process, where JSON data is converted back into a Go struct. These processes are essential for working with JSON data in Go and will be covered in detail in future lessons.


Example: Structs in Action with Go
```Go
package main

import "fmt"

// TODO: Define the struct data type for Todo
type Todo struct {
    ID        int    `json:"id"`
    Title     string `json:"title"`
    Completed bool   `json:"completed"`
}

func main() {
    // TODO: Initialize a struct instance with ID, Title, and Completed fields
    myTodo := Todo{
        ID: 1,
        Title : "hello word",
        Completed: true,
    }

    // Printing struct data
    fmt.Printf("%+v\n", myTodo)
}
```


example: Master JSON Encoding in Go
```Go
package main

import (
    "encoding/json"
    "log"
    "bytes"
    "net/http"
)

const baseUrl = "http://localhost:8000"

// TODO: Define the Todo struct with fields Title, Done, and Description. Use JSON tags for each field.
type Todo struct {
    Title       string      `json:"title"`
    Done        bool        `json:"done"`
    Description string      `json:"description"`
}

func main() {
    
    
    // TODO: Create a new instance of Todo with appropriate values for Title, Done, and Description.
    todo := Todo {
        Title:  "title",
        Done:   false,
        Description: "description",
    }
    
    // TODO: Marshal the Todo instance into JSON format. Handle any errors that occur during this process.
    jsonTodo, err := json.Marshal(todo)
    if err != nil {
        log.Fatalf("Error occurred during marshalling %s", err.Error())
    }
    // TODO: Log the JSON object to verify the marshaling process.
    log.Printf("JSON object: %s", jsonTodo)

    // TODO: Send the JSON data via an HTTP POST request to the "/todos" endpoint. Handle any errors that occur during this process.
    err = sendJSONRequest(jsonTodo)
    if err != nil {
		log.Fatalf("Error occurred during HTTP request: %s", err.Error())
	}
}

// TODO: Implement the sendPOSTRequest function to send a JSON-encoded request to the specified endpoint. Set the Content-Type header to application/json and log the HTTP response status.
func sendJSONRequest(jsonData []byte) error {
    req, err := http.NewRequest("POST", baseUrl+"/todos", bytes.NewBuffer(jsonData))
    if err != nil {
        return err
    }

    // Set the content type to application/json
    req.Header.Set("Content-Type", "application/json")

    // Send the HTTP request
    client := &http.Client{}
    resp, err := client.Do(req)
    if err != nil {
        return err
    }
    defer resp.Body.Close()

    // Print the HTTP response status
    log.Printf("HTTP response status: %s", resp.Status)
    return nil
}
```





###






Example: Decoding JSON into Go Structs
```Go
package main

import (
    "encoding/json"
    "fmt"
    "io"
    "net/http"
)

type Todo struct {
    Title       string `json:"title"`
    Done        bool   `json:"done"`
    Description string `json:"description"`
}

func fetchTodos(url string) ([]byte, error) {
    response, err := http.Get(url)
    if err != nil {
        return nil, fmt.Errorf("request to %s failed: %w", url, err)
    }
    defer response.Body.Close()

    if response.StatusCode != http.StatusOK {
        return nil, fmt.Errorf("request failed with status: %s", response.Status)
    }

    body, err := io.ReadAll(response.Body)
    if err != nil {
        return nil, fmt.Errorf("failed to read response body: %w", err)
    }
    return body, nil
}

func parseTodos(data []byte) ([]Todo, error) {
    var todos []Todo
    err := json.Unmarshal(data, &todos)
    if err != nil {
        return nil, fmt.Errorf("JSON decoding failed: %w", err)
    }
    return todos, nil
}

func printTodos(todos []Todo) {
    fmt.Println("Decoded todo items from JSON:")
    for _, todo := range todos {
        fmt.Println("Title: ", todo.Title)
        fmt.Println("Done: ", todo.Done)
        fmt.Println("Description: ", todo.Description)
        fmt.Println("-----------------------------")
    }
}

func main() {
    url := "http://localhost:8000/todos"
    body, err := fetchTodos(url)
    if err != nil {
        fmt.Println(err)
        return
    }

    todos, err := parseTodos(body)
    if err != nil {
        fmt.Println(err)
        return
    }

    printTodos(todos)
}
```
