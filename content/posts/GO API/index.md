---
title: "GO API"
date: 2021-05-19
description: "GO"
tags: ["GO","Golang"]
type: post
weight: 20
showTableOfContents: true
---






![img01](images/01.webp)

&nbsp;

## Making GET Requests and Handling Responses in Go

&nbsp;


### Making GET Requests and Handling Responses

Welcome to the second lesson in our journey of interacting with `APIs` using Go. In the previous lesson, we established a strong understanding of `RESTful APIs` and how `HTTP requests` facilitate interactions with them. We used `curl` to interact directly with an API endpoint. Now, we will automate this process using Go's `net/http` package. This lesson will guide you through making HTTP requests in Go, equipping you with essential skills for web development and API integration.

&nbsp;

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

&nbsp;

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

&nbsp;

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

&nbsp;

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

&nbsp;

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

&nbsp;

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

&nbsp;

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

&nbsp;

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

&nbsp;

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

&nbsp;

Example 2 : Interact with APIs using Go: Fetching and Processing Data
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

&nbsp;

Example 3: Handling 404 Errors in Go
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

&nbsp;

Example 4: Performing GET Requests with Go
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

&nbsp;

&nbsp;

&nbsp;

### Introduction to Path and Query Parameters in Go

&nbsp;

**Introduction to Path and Query Parameters**
Welcome to another lesson of this course. In our previous lessons, we established a foundation by learning about **RESTful APIs** and making GET requests using Go's standard library. Now, we will shift our focus to **path** and **query parameters**, essential tools for refining API requests and fetching specific data.

Path and query parameters play a crucial role in making your API requests more precise and efficient. Imagine you are shopping online: selecting a specific item using its ID is akin to a path parameter, while filtering items by categories like price or color resembles query parameters. In this lesson, we'll explore these concepts with practical examples in Go, empowering you to extract just the information you need from an API.

&nbsp;

**Understanding Path Parameters**
Path parameters are part of the URL used to access specific resources within an API, acting like unique identifiers. For example, if you want to retrieve a to-do item with ID `3`, the URL would be structured as follows:

```
http://localhost:8000/todos/3
```
In this case, `3` is the path parameter specifying the particular item you wish to access.

&nbsp;

**Understanding Query Parameters**

Query parameters are added to the URL after a `?` to filter or modify the data returned by the API. They are formatted as key-value pairs. For instance, if you want to list only the completed to-do tasks, your request would be:
```
http://localhost:8000/todos?done=true
```
Here, done=true is the query parameter filtering the results to include only tasks that are marked as completed.

&nbsp;

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

&nbsp;

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

&nbsp;

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

&nbsp;

**Using Multiple Query Parameters**

To refine data retrieval further, you can combine multiple query parameters. For instance, you might want to fetch to-do items that are marked as done and also have titles that start with a specific prefix.

Here's how you can filter to-do items that are completed and have titles starting with the prefix `"c"`:

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

&nbsp;

Example 1: Fetching Todo Item Using Path Parameters in Go
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

&nbsp;

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

&nbsp;

Example 3: Filtering To-Do Items with Query Parameters in Go
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

&nbsp;

Example 4: Modifying Query Parameters in Go
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

&nbsp;

&nbsp;

&nbsp;

### Sending Data with POST Requests in Go

&nbsp;

**Sending Data with POST Requests**

Welcome to this lesson on sending data with **POST requests** using Go's net/http package. As you continue your exploration of interacting with **RESTful APIs**, you'll learn how to send data to a server using the POST method. **POST requests** are essential when you want to create new resources or submit data, such as filling out a web form or adding a new entry to a database. Unlike GET requests, which allow you to retrieve data, POST requests transmit data to an API.

Understanding these differences is crucial as you expand your skill set in HTTP methods. Let’s dive deeper into utilizing **POST requests** to comprehend how they stand apart from **GET requests**.

&nbsp;

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


&nbsp;

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


&nbsp;

&nbsp;


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

&nbsp;

&nbsp;

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

&nbsp;

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

&nbsp;

&nbsp;


Example 1: Fixing Bugs in a Go POST Request Script
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

&nbsp;

Example 2: Verifying Todo Addition with POST and GET Requests in Go
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

&nbsp;

Example 4: Crafting a POST Request in Go
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


&nbsp;

&nbsp;

&nbsp;


### Updating and Deleting Resources with PUT, PATCH, and DELETE

Welcome to this lesson focusing on enhancing your skills in interacting with **RESTful APIs** by updating and deleting resources. In previous lessons, you learned how to retrieve and create resources using the `GET` and `POST` methods. Now, you will explore the `PUT`, `PATCH`, and `DELETE` methods, which are crucial for modifying existing resources or removing them when they are no longer needed.

&nbsp;

**Understanding PUT, PATCH, and DELETE Methods**

To effectively manage resources within an API, it's essential to understand how to use the following methods:

**PUT**: This method is used for completely updating a resource. You replace the current resource with the new data you provide in the request body. For example, if you're updating a to-do item, you'll include all the new details of the item in the body of the request and specify the resource ID in the request path to indicate which item you are updating.

**PATCH**: Use this method to make partial updates to a resource. You only need to include the specific fields you're updating in the request body. For instance, if you're only changing the description of a to-do item, you'll pass just the new description in the body. The resource ID is specified in the request path to identify which item is being modified.

**DELETE**: This straightforward method removes a resource from the server. You specify which resource to delete by including its identifier in the request path. No request body is needed since you're not sending any data, just instructing the server to remove the resource corresponding to that ID.

Successful requests using these methods typically return status codes of `200` (OK) or `204` (No Content). A `200` status code means the operation was successful and also returns content, such as a representation of the updated resource. In contrast, a `204` status code indicates success but with no content returned, meaning the server successfully processed the request but isn't providing any additional information. These methods are crucial for performing full **CRUD** (Create, Read, Update, Delete) functionality in API management.

&nbsp;

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

&nbsp;

**Using `http.NewRequest` for Custom HTTP Requests**

So far, we’ve used `http.Get` and `http.Post` to interact with APIs. These functions work well for simple requests, but sometimes we need more control—especially when using **PUT**, **PATCH**, and **DELETE**, which don’t have built-in helper functions in Go.

For these cases, Go provides the `http.NewRequest` function, which allows us to create a customizable HTTP request.

&nbsp;

**Why Use `http.NewRequest`?**

- **Supports all HTTP methods** (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`, etc.).

- Allows us to **set headers** (like `"Content-Type"`: `"application/json"`).

- Gives more control over the request body.

&nbsp;

**Creating a Custom HTTP Request**

Here’s an example of how to create a request using `http.NewRequest`:
```Go
req, err := http.NewRequest(http.MethodPut, "http://example.com/resource", nil)
if err != nil {
    log.Fatalf("Failed to create request: %v", err)
}
```

&nbsp;

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

&nbsp;

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


&nbsp;

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

&nbsp;


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

&nbsp;

&nbsp;

Example 1 : Implementing PUT Method for Todo Update in Go
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
&nbsp;

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

Example 3: Changing PATCH to DELETE in Go API Request
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

&nbsp;

Example 4: Implementing PUT, PATCH, and DELETE HTTP Requests in Go
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

&nbsp;

Example 5: Managing Todo List with HTTP Methods in Go
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

&nbsp;

&nbsp;

&nbsp;

![img02](images/02.webp)

## Introduction to JSON and Go Structs

&nbsp;

### Introduction to JSON and Go Structs
Welcome to the first lesson of our course on handling **JSON** in **Go**. JSON, which stands for JavaScript Object Notation, is a lightweight data interchange format that is easy for humans to read and write and easy for machines to parse and generate. It is widely used in web services for data exchange. In this lesson, we will explore how Go, a statically typed language, uses structs to manage and organize JSON data. Understanding how to work with JSON and Go structs is crucial for interacting with APIs, as it allows you to seamlessly integrate and manipulate data within your Go application

&nbsp;

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

&nbsp;

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

&nbsp;

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

&nbsp;

**Serializing and Deserializing JSON in Go (Brief Overview)**

While we won't delve deeply into serialization and deserialization in this lesson, it's important to understand these concepts at a high level. Serialization, also known as marshaling, is the process of converting a Go struct into JSON format. Deserialization, or unmarshaling, is the reverse process, where JSON data is converted back into a Go struct. These processes are essential for working with JSON data in Go and will be covered in detail in future lessons.

&nbsp;

Example 1: Structs in Action with Go
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

&nbsp;

&nbsp;

&nbsp;


### Encoding Structs into JSON in Go

**Introduction to JSON Encoding in Go**

Welcome back! In the previous lesson, we explored the basics of **JSON** and how **Go** uses structs to manage and organize JSON data. You learned how to define a struct in Go and use struct tags for JSON serialization. This foundational knowledge is crucial as we move forward to more advanced topics. In this lesson, we will focus on encoding Go structs into JSON format, a process known as marshaling. This is an essential skill for interacting with APIs, as JSON is the primary data format for most web services. By the end of this lesson, you will be able to seamlessly convert Go structs into JSON, preparing you for effective API communication.



**Encoding Go Structs into JSON**


To encode a Go struct into JSON, we use the encoding/json package, which provides the json.Marshal function. This function takes a Go struct and converts it into a JSON-encoded byte slice. Let's revisit the Todo struct from the previous lesson and see how we can marshal it into JSON.

```Go
Copy to clipboard
package main

import (
    "encoding/json"
    "log"
)

// Todo structure represents a task with 'title' and 'done' status
type Todo struct {
    Title       string `json:"title"`
    Done        bool   `json:"done"`
    Description string `json:"description"`
}

func main() {
    // Create a new instance of Todo
    todo := Todo{
        Title:       "Walking the dog",
        Done:        false,
        Description: "Walking the dog in the park",
    }

    // Convert Todo instance to JSON format
    jsonTodo, err := json.Marshal(todo)
    if err != nil {
        log.Fatalf("Error occurred during marshalling: %s", err.Error())
    }

    // Print the JSON object
    log.Printf("JSON object: %s", jsonTodo)
}
```
In this example, we define a `Todo` struct with fields `Title`, `Done`, and `Description`. Each field has a JSON tag that specifies the key name in the resulting JSON object. We create an instance of Todo and use `json.Marshal` to convert it into JSON. If successful, the `JSON` object is printed. The output will be a JSON string representing the Todo object, such as:
```
JSON object: {"title":"Walking the dog","done":false,"description":"Walking the dog in the park"}
```

&nbsp;


**Practical Example: Sending JSON Data via HTTP**


**Step 1: Convert Struct to JSON**

Before sending data, we need to marshal our Todo struct into JSON format.

&nbsp;

```Go
jsonTodo, err := json.Marshal(todo)
if err != nil {
    log.Fatalf("Error occurred during marshalling: %s", err.Error())
}
Step 2: Create an HTTP Request
Now, we create an HTTP request with the JSON data.

Go
Copy to clipboard
req, err := http.NewRequest("POST", "http://localhost:8000/todos", bytes.NewBuffer(jsonTodo))
if err != nil {
    log.Fatalf("Error occurred during creating HTTP request: %s", err.Error())
}
Step 3: Set Headers and Send Request
We need to specify the Content-Type and send the request using an HTTP client.
```
&nbsp;

```Go
req.Header.Set("Content-Type", "application/json")
client := &http.Client{}
resp, err := client.Do(req)
if err != nil {
    log.Fatalf("Error occurred during sending HTTP request: %s", err.Error())
}
defer resp.Body.Close()
Step 4: Handle HTTP Response
Finally, we check the response status to verify whether the request was successful.

Go
Copy to clipboard
body, err := io.ReadAll(resp.Body)
if err != nil {
    return err
}

if resp.StatusCode >= 400 {
    return fmt.Errorf("HTTP request failed with status %d: %s", resp.StatusCode, string(body))
}

log.Printf("Request successful! Response: %s", string(body))
Complete Code Example
Here is the final version of our code incorporating all the steps:

```Go
package main

import (
    "encoding/json"
    "log"
    "bytes"
    "net/http"
)

// Todo structure represents a task with 'title' and 'done' status
type Todo struct {
    Title       string `json:"title"`
    Done        bool   `json:"done"`
    Description string `json:"description"`
}

func main() {
    // Create a new instance of Todo
    todo := Todo{
        Title:       "Walking the dog",
        Done:        false,
        Description: "Walking the dog in the park",
    }

    // Convert Todo instance to JSON format
    jsonTodo, err := json.Marshal(todo)
    if err != nil {
        log.Fatalf("Error occurred during marshalling: %s", err.Error())
    }
    log.Printf("JSON object: %s", jsonTodo)

    // Send JSON data via HTTP
    if err := sendJSONRequest(jsonTodo); err != nil {
        log.Fatalf("Error occurred during HTTP request: %s", err.Error())
    }
}

// sendJSONRequest sends a JSON-encoded request to the specified endpoint.
func sendJSONRequest(jsonData []byte) error {
    req, err := http.NewRequest("POST", "http://localhost:8000/todos", bytes.NewBuffer(jsonData))
    if err != nil {
        return err
    }

    req.Header.Set("Content-Type", "application/json")

    client := &http.Client{}
    resp, err := client.Do(req)
    if err != nil {
        return err
    }
    defer resp.Body.Close()

    body, err := io.ReadAll(resp.Body)
    if err != nil {
        return err
    }

    if resp.StatusCode >= 400 {
        return fmt.Errorf("HTTP request failed with status %d: %s", resp.StatusCode, string(body))
    }

    log.Printf("Request successful! Response: %s", string(body))
    return nil
}
```
In this code, we first marshal the Todo struct into JSON. We then create a new HTTP POST request to the `/todos` endpoint, using the JSON data as the request body. The Content-Type header is set to `application/json` to indicate that the request body contains JSON data. We use an http.Client to send the request and log the response status. This process is crucial for sending data to web services and APIs.

&nbsp;

**Handling HTTP Responses**

After sending the JSON data, it's important to handle the HTTP response. This involves checking the response status and handling any potential errors. In the example above, we log the response status using resp.Status. This provides valuable feedback on whether the request was successful or if there were any issues. Properly handling responses is key to robust API interactions, allowing you to debug and ensure your application behaves as expected.


&nbsp;

&nbsp;


Example 1: Master JSON Encoding in Go

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

&nbsp;

&nbsp;

&nbsp;

### Decoding JSON into Structs in Go

**Introduction to Decoding JSON in Go**

Welcome back! In the previous lesson, you learned how to encode Go structs into JSON format, a process known as ***marshaling***. This skill is crucial for sending data to APIs, as JSON is the primary data format for most web services. Now, we will focus on the reverse process: decoding JSON data into Go structs, also known as ***unmarshaling***. This is an essential skill for receiving and processing data from APIs. By the end of this lesson, you will be able to seamlessly convert JSON data into Go structs, preparing you for effective API communication.

&nbsp;

**Example: Decoding JSON into Go Structs**
Let's walk through an example to see how decoding JSON into Go structs works in practice. We'll use the Todo struct, which you are already familiar with from previous lessons. Here's a complete example that demonstrates how to fetch JSON data from an API and decode it into a Go struct.

&nbsp;

**Step 1: Defining the Todo Struct**
```Go
type Todo struct {
    Title       string `json:"title"`
    Done        bool   `json:"done"`
    Description string `json:"description"`
}
```
The Todo struct defines the expected structure of the JSON data. The struct tags (`json:"title"`, etc.) map the JSON keys to Go struct fields.
&nbsp;


&nbsp;

**Step 2: Fetching JSON Data from an API**
```Go
func fetchTodos(url string) ([]byte, error) {
    response, err := http.Get(url)
    if err != nil {
        return nil, fmt.Errorf("request to %s failed: %w", url, err)
    }
    defer response.Body.Close()

    if response.StatusCode != http.StatusOK {
        return nil, fmt.Errorf("request failed with status: %s", response.Status)
    }

    body, err := ioutil.ReadAll(response.Body)
    if err != nil {
        return nil, fmt.Errorf("failed to read response body: %w", err)
    }
    return body, nil
}
```
This function fetches JSON data from the given API URL and returns the raw JSON response body.

&nbsp;

**Step 3: Parsing JSON Data into Go Structs **
```go
func parseTodos(data []byte) ([]Todo, error) {
    var todos []Todo
    err := json.Unmarshal(data, &todos)
    if err != nil {
        return nil, fmt.Errorf("JSON decoding failed: %w", err)
    }
    return todos, nil
}
```
Here, we use `json.Unmarshal` to convert the JSON data into a slice of Todo structs.

**Step 4: Printing Decoded Todo Items**
```Go
func printTodos(todos []Todo) {
    fmt.Println("Decoded todo items from JSON:")
    for _, todo := range todos {
        fmt.Println("Title: ", todo.Title)
        fmt.Println("Done: ", todo.Done)
        fmt.Println("Description: ", todo.Description)
        fmt.Println("-----------------------------")
    }
}
```
This function iterates over the parsed `todos` and prints each one.

**Step 5: Running the Complete Program**
```Go
import (
    "encoding/json"
    "fmt"
    "io"
    "net/http"
)

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
This is the main function that ties everything together. It fetches JSON data, parses it into Go structs, and prints the results.

**Common Pitfalls and Error Handling**

When decoding JSON, it's important to handle potential errors effectively. Common errors include network issues, incorrect JSON structure, and mismatched struct fields. In the example above, we handle errors at each step of the process, from making the HTTP request to reading the response body and decoding the JSON. Proper error handling ensures that your application can gracefully handle unexpected situations and provides valuable feedback for debugging.

&nbsp;

Example 1: Decoding JSON into Go Structs
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

&nbsp;

&nbsp;

&nbsp;

### Handling Nested and Optional JSON Fields in Go


**Introduction to Handling Nested and Optional JSON Fields**

Welcome back! In the previous lesson, you learned how to decode JSON data into Go structs, a process known as ***unmarshaling***. This skill is essential for receiving and processing data from APIs. Now, we will build on that knowledge by focusing on handling **nested and optional JSON fields**. These are common in real-world JSON data, where you might encounter complex structures and fields that may or may not be present. By the end of this lesson, you will be able to parse and handle nested and optional JSON fields in Go, enhancing your ability to work with diverse JSON data structures.

&nbsp;

**Defining Structs for Nested JSON Fields**

In Go, handling nested JSON fields involves defining structs that mirror the JSON structure. This means creating nested structs within your main struct to represent the hierarchy of the JSON data. Let's consider a JSON structure that includes a nested object:
```json
{
  "title": "Learn Go",
  "details": {
    "description": "A comprehensive guide to Go programming",
    "author": "John Doe"
  }
}
```
To map this JSON structure in Go, you would define a struct with a nested struct for the details field:
```Go
type Details struct {
    Description string `json:"description"`
    Author      string `json:"author"`
}

type Todo struct {
    Title   string  `json:"title"`
    Details Details `json:"details"`
}
```
Here, the `Details` struct is nested within the `Todo` struct, reflecting the JSON hierarchy. The struct tags ensure that the JSON keys are correctly mapped to the Go struct fields.

```Go
jsonData := `{"title": "Learn Go", "details": {"description": "A comprehensive guide to Go programming", "author": "John Doe"}}`
var todo Todo
err := json.Unmarshal([]byte(jsonData), &todo)
if err != nil {
    fmt.Println("Error decoding JSON:", err)
    return
}
fmt.Printf("Title: %s\nDescription: %s\nAuthor: %s\n", todo.Title, todo.Details.Description, todo.Details.Author)
```

&nbsp;

In this example, `json.Unmarshal` decodes the JSON string into the todo variable, which is of type `Todo`. The nested details object is automatically mapped to the Details struct within Todo. The output will be:
```Go
Title: Learn Go
Description: A comprehensive guide to Go programming
Author: John Doe
```
This demonstrates how Go's json package handles nested JSON fields seamlessly, allowing you to work with complex data structures efficiently.

&nbsp;


**Handling Optional JSON Fields**

Optional JSON fields are those that may or may not be present in the JSON data. In Go, you can handle these fields using pointers and the omitempty struct tag. The `omitempty` tag tells the `json` package to omit the field if it is empty or nil. Let's modify our previous example to include an optional `author` field:

```Go
type Details struct {
    Description string  `json:"description"`
    Author      *string `json:"author,omitempty"`
}
```
Here, the `Author` field is a pointer to a string, allowing it to be nil if the author key is not present in the JSON data.

&nbsp;

**Why Use a Pointer for Optional Fields?**

Using a pointer (*string) for optional fields helps distinguish between **a missing field and an empty value. If Author were a regular string**, it would default to `""` even if absent in the JSON. With a pointer, it remains `nil`, allowing you to check if the field was truly missing.

Additionally, the omitempty tag ensures that when marshaling back to JSON, the field is excluded if it’s nil, keeping the output cleaner.

Now let's see how this works in practice:
```Go
jsonData := `{"title": "Learn Go", "details": {"description": "A comprehensive guide to Go programming"}}`
var todo Todo
err := json.Unmarshal([]byte(jsonData), &todo)
if err != nil {
    fmt.Println("Error decoding JSON:", err)
    return
}
fmt.Printf("Title: %s\nDescription: %s\n", todo.Title, todo.Details.Description)
if todo.Details.Author != nil {
    fmt.Printf("Author: %s\n", *todo.Details.Author)
} else {
    fmt.Println("Author: not provided")
}
```
In this example, the JSON data does not include the `author` field. The `Author` field in the `Details` struct is nil, and the output will be:
```Go
Title: Learn Go
Description: A comprehensive guide to Go programming
Author: not provided
```

&nbsp;

&nbsp;

&nbsp;


### Working with Dynamic or Unknown JSON Structures in Go

**Introduction to Dynamic JSON Handling**

Welcome to the final lesson in your journey of mastering **JSON handling** in Go! In the previous lessons, you learned how to decode JSON data into Go structs, handle nested and optional JSON fields, and manage complex JSON structures. Now, we will explore how to work with dynamic or unknown JSON structures. These are common in real-world API interactions where the JSON structure may not be fixed or known in advance. By the end of this lesson, you will be equipped to handle such dynamic data effectively, ensuring robust and flexible API communication.

&nbsp;

**Leveraging Go's Map Type for Dynamic JSON**

In Go, the map type is a powerful tool for handling dynamic JSON structures. Unlike structs, which require predefined fields, maps allow you to store key-value pairs without knowing the structure in advance. This flexibility makes maps ideal for working with JSON data that can vary in structure.

For example, consider a JSON response from an API that returns different fields based on the request:
```json 
{
  "name": "John Doe",
  "age": 30,
  "email": "john.doe@example.com"
}
```
In another scenario, the same API might return additional fields:

```json
{
  "name": "Jane Doe",
  "age": 25,
  "email": "jane.doe@example.com",
  "phone": "123-456-7890"
}
```
Using a `map` in Go, you can handle both responses without needing to define a new struct for each variation. This approach allows you to work with dynamic JSON data in a flexible and efficient manner.

&nbsp;

**Example: Fetching and Parsing Dynamic JSON from an API**

To understand how to work with **dynamic JSON data**, let’s go through an example where we **fetch JSON from an API**, **parse it into a flexible structure**, and **navigate its contents**.

&nbsp;

**Step 1: Define a Flexible Data Type**

Since the structure of the JSON response is **unknown** or **varies**, we use a **map with string keys and empty interface values** (`map[string]interface{}`), which allows us to store any kind of JSON data.
```Go
type dynamicJSON map[string]interface{}
```

&nbsp;

This means:

- The keys in the map will be **strings** (matching JSON field names).

- The values can be **any type** (numbers, strings, nested maps, arrays, etc.).

&nbsp;

**Step 2: Make an HTTP Request**

Next, we **fetch JSON data from an API**. We use http.Get() to send a GET request to "http://localhost:8000/docs".
```Go
resp, err := http.Get("http://localhost:8000/docs")
if err != nil {
    return nil, err
}
defer resp.Body.Close()
```

&nbsp;

Here’s what happens:

- **http.Get()** ** sends the request** to the API.

- **If there’s an error** (e.g., no internet, invalid URL), we return the error.

- **defer resp.Body.Close()** ** ensures** that the response body is closed after we finish reading it.

&nbsp;

**Step 3: Read and Parse the JSON Response**

Once we receive the HTTP response, we need to **read its body** and **convert it into a Go data structure**.
```Go
body, err := io.ReadAll(resp.Body)
if err != nil {
    return nil, err
}
```

- io.ReadAll(resp.Body) reads the full response into a byte slice.

- If an error occurs while reading, we **return the error**.

&nbsp;

Now, let’s **decode the JSON** into our dynamicJSON type.
```Go
var result dynamicJSON
err = json.Unmarshal(body, &result)
if err != nil {
    return nil, err
}
```

At this point, we have a **Go-friendly representation** of the JSON response, stored as a **map**.

&nbsp;

**Step 4: Access and Print JSON Data**

Now that we have the JSON data stored in a map[string]interface{}, we can **iterate through it** and print its contents.

&nbsp;

**Loop Through Top-Level Keys**
```Go
for path, detail := range result {
    fmt.Printf("Path: %s\n", path)
```

- path represents a **key** in the JSON (like "user", "posts", etc.).

- detail is the corresponding **value**, which could be another map or different data.

&nbsp;

**Use Type Assertions to Handle Nested Data**

Since the values are stored as interface{}, we need to **convert them into a map** to access nested fields.
```Go
details, ok := detail.(map[string]interface{})
if !ok {
    continue
}
```
- **Type assertion (.(map[string]interface{}))** ensures that detail is a nested map.

- **If it’s not a map**, we continue to the next iteration.

&nbsp;

**Loop Through Nested Keys**
Now that we know details is a map, we iterate over it:
```Go
Copy to clipboard
for method, info := range details {
    fmt.Printf("\nMethod: %s\n", method)
    infos, ok := info.(map[string]interface{})
    if !ok {
        continue
    }
```

- Each **method** (like "GET", "POST") corresponds to another nested map.

- **We assert that info is a map** before accessing its contents.

&nbsp;

**Print Inner Properties**
Finally, we iterate over the innermost fields and print them.

```Go
Copy to clipboard
for property, value := range infos {
    fmt.Printf("%s: %v\n", property, value)
}
fmt.Println("----------------------------------")
```
- We extract **key-value pairs** from the JSON.

- The %v format prints **any type** (string, number, list, etc.).

&nbsp;

**Final Code: Fetch and Parse JSON**

Putting all the steps together, we get:
```Go
Copy to clipboard
package main

import (
    "encoding/json"
    "fmt"
    "io"
    "net/http"
)

type dynamicJSON map[string]interface{}

func getJSONFromApi() (dynamicJSON, error) {
    resp, err := http.Get("http://localhost:8000/docs")
    if err != nil {
        return nil, err
    }
    defer resp.Body.Close()

    body, err := io.ReadAll(resp.Body)
    if err != nil {
        return nil, err
    }

    var result dynamicJSON
    err = json.Unmarshal(body, &result)
    if err != nil {
        return nil, err
    }

    return result, nil
}

func main() {
    doc, err := getJSONFromApi()
    if err != nil {
        fmt.Printf("Error: %v", err)
        return
    }

    for path, detail := range doc {
        fmt.Printf("Path: %s\n", path)
        details, ok := detail.(map[string]interface{})
        if !ok {
            continue
        }

        for method, info := range details {
            fmt.Printf("\nMethod: %s\n", method)
            infos, ok := info.(map[string]interface{})
            if !ok {
                continue
            }

            for property, value := range infos {
                fmt.Printf("%s: %v\n", property, value)
            }
        }
        fmt.Println("----------------------------------")
    }
}
```

In this code, we first make an HTTP GET request to the API endpoint. We then read the response body and unmarshal the JSON data into a dynamicJSON map. This allows us to handle any JSON structure returned by the API. Finally, we iterate over the map to print the JSON data, demonstrating how to access and work with dynamic JSON structures.

&nbsp;

&nbsp;

&nbsp;



![omg03](images/03.webp)

## Introduction to Error Handling in API Requests with Go


**Introduction to Error Handling in API Requests**

Welcome to the first lesson of ***Efficient API Interactions with Go***. In this course, you will learn how to handle common scenarios when working with APIs more effectively. One of the key aspects of working with APIs is **error handling**. Handling errors gracefully not only helps in building robust applications but also enhances the user experience by providing meaningful feedback when things go wrong. Our goal in this lesson is to help you manage these outcomes effectively.

&nbsp;

**Understanding HTTP Status Codes**
When you send a request to an API, the server responds with an **HTTP status code**. These codes indicate the result of your request. Understanding them is essential for effective error handling. Here's a brief overview:

- **2xx (Success)**: Indicates that the request was successfully received, understood, and accepted. For example, a 200 status code means OK.

- **4xx (Client Errors)**: Suggests that there was an error in the request made by your client. For example, 404 means the requested resource was not found.

- **5xx (Server Errors)**: Indicates that the server failed to fulfill a valid request. A common code here is 500, which means an internal server error.

By paying attention to these codes, you can determine whether your request succeeded or if there was a problem that needs addressing.

&nbsp;

**Handling HTTP Errors in Go**
In Go, **error handling** is done by checking the response StatusCode and handling errors accordingly. Unlike other languages that use exceptions, Go relies on explicit error checking using conditional statements.

Consider the following example, which fetches todo items from an API:
```Go
Copy to clipboard
package main

import (
    "errors"
    "fmt"
    "io/ioutil"
    "net/http"
)

func fetchTodos(apiURL string) error {
    resp, err := http.Get(apiURL)
    if err != nil {
        return fmt.Errorf("request error: %w", err)
    }
    defer resp.Body.Close()

    // Check if the response has a 4xx or 5xx status code
    if resp.StatusCode >= 400 {
        return errors.New(fmt.Sprintf("HTTP error occurred: %s", resp.Status))
    }

    // Read response body
    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        return fmt.Errorf("error reading response body: %w", err)
    }

    fmt.Println("Todos fetched successfully!")
    fmt.Println(string(body))
    return nil
}

func main() {
    baseURL := "http://localhost:8000/todos"

    // Call fetchTodos and handle any returned errors
    if err := fetchTodos(baseURL); err != nil {
        fmt.Println("Error:", err)
    }
}
```

In this example, we check the StatusCode of the response and handle any errors by printing an appropriate message if it falls within the 4xx or 5xx range. This approach ensures clear and effective **error handling**, making it easier to identify and troubleshoot issues in your application. To improve modularity and reusability, we encapsulate this logic within a dedicated function (`fetchTodos`), allowing for better organization and easier management of API requests and error handling separately.

Additionally, we use `errors.New()` to create well-formatted error messages, making it clear when something goes wrong. Instead of merely printing errors, we return them, enabling better error propagation and handling in real applications. This approach provides more flexibility in dealing with failures, making Go applications more robust and maintainable.

&nbsp;

**Examples: Non-existent Route**

Following our discussion on handling HTTP errors, let's delve into specific scenarios where errors might occur. In this first example, a `GET` request is sent to a non-existent route, leading to an `HTTP` error because a `404` Not Found status code is returned.

```Go
Copy to clipboard
package main

import (
    "errors"
    "fmt"
    "net/http"
)

func fetchData(apiURL string) error {
    resp, err := http.Get(apiURL)
    if err != nil {
        return fmt.Errorf("request error: %w", err)
    }
    defer resp.Body.Close()

    // Check for HTTP errors
    if resp.StatusCode >= 400 {
        return errors.New(fmt.Sprintf("HTTP error occurred: %s", resp.Status))
    }

    return nil
}

func main() {
    baseURL := "http://localhost:8000/invalid-route"

    if err := fetchData(baseURL); err != nil {
        fmt.Println("Error:", err)
    }
}
```

This will produce the following output indicating that the requested resource was not found:

```text
Error: HTTP error occurred: 404 Not Found
```

&nbsp;

**Examples: POST Request Without Required Field**

Continuing with **error handling**, the next scenario involves sending a POST request without a required field, the title, resulting in an HTTP error due to a `400 Bad Request`.


```Go

package main

import (
    "bytes"
    "errors"
    "fmt"
    "net/http"
)

func createTodo(apiURL string, data []byte) error {
    resp, err := http.Post(apiURL, "application/json", bytes.NewBuffer(data))
    if err != nil {
        return fmt.Errorf("request error: %w", err)
    }
    defer resp.Body.Close()

    // Check for HTTP errors
    if resp.StatusCode >= 400 {
        return errors.New(fmt.Sprintf("HTTP error occurred: %s", resp.Status))
    }

    return nil
}

func main() {
    baseURL := "http://localhost:8000/todos"
    payload := []byte("{}") // Missing required fields

    if err := createTodo(baseURL, payload); err != nil {
        fmt.Println("Error:", err)
    }
}
```

The following output shows a 400 Bad Request error, indicating missing required fields:
```text
Error: HTTP error occurred: 400 Bad Request
```

&nbsp;

**Examples: Handling Broader Request-Related Issues**

Finally, let's examine how to handle broader request-related issues. This example demonstrates a scenario where an error occurs due to connectivity issues or other problems external to the HTTP response itself.

```Go
package main

import (
    "fmt"
    "net/http"
)

func fetchWithInvalidURL() error {
    _, err := http.Get("http://invalid-url")
    if err != nil {
        return fmt.Errorf("Other error occurred: %v\n", err)
    }

    return nil
}

func main() {
    if err := fetchWithInvalidURL(); err != nil {
        fmt.Println("Error:", err)
    }
}
```
When a connection cannot be established, the following output will provide details about the connectivity issue:
```text
Error: Other error occurred: Get "http://invalid-url": dial tcp: lookup invalid-url: no such host
```

**These examples build on the principles of **error handling** we previously discussed, offering more detailed insights into managing errors effectively in different contexts within your API interactions.


&nbsp;

&nbsp;


Example 1: Managing HTTP Errors in Go API Requests
```go
package main

import (
    "fmt"
    "net/http"
    "errors"
)

// Base URL for the API
const baseURL = "http://localhost:8000"

func main() {
    // Attempt to perform a request with error handling
    err := fetchData()
    if err != nil {
        // TODO: Handle HTTP errors if the response was not successful
        fmt.Print("Error:", err)
    } else {
        // If no error was returned, print success message
        fmt.Println("Data fetched successfully!")
    }
}

func fetchData() error {
    // Send a GET request to an invalid route
    resp, err := http.Get(fmt.Sprintf("%s/invalid-route", baseURL))
    if err != nil {
        return err
    }
    defer resp.Body.Close()

    // TODO: Check for bad responses (4xx and 5xx status codes)
    if resp.StatusCode >= 400 {
        return errors.New(fmt.Sprintf("HTTP error occurred: %s", resp.Status))
    } else{
        fmt.Println("Todos fetched successfully!")
    }

    
    return nil
}
```

&nbsp;

Example : Streamlining Error Handling in Go API Requests

```Go
package main

import (
    "fmt"
    "net/http"
)

// TODO: Create a helper function to centralize error handling
// This function should take an HTTP response and an error as arguments
// and return a properly formatted error message.
func handleError(resp *http.Response, err error) error {
	if err != nil {
		return fmt.Errorf("request error: %w", err)
	}

	
	if resp.StatusCode >= http.StatusBadRequest {
		return fmt.Errorf("HTTP error occurred: %s", resp.Status)
	}

	return nil
}

func fetchWithInvalidURL() error {
    _, err := http.Get("http://localhost:8000/invalid-route")
    if err != nil {
        return fmt.Errorf("Other error occurred: %v\n", err)
    }

    return nil
}

func fetchData(apiURL string) error {
    resp, err := http.Get(apiURL)
    
    if err := handleError(resp, err); err != nil {
        return err
    }
    defer resp.Body.Close()
    fmt.Println("Data fetched successfully!")
    
    return nil

    
    
}

func main() {
    baseURL := "http://localhost:8000/invalid-route"

    // TODO: Ensure the new helper function is used to handle errors effectively
    if err := fetchData(baseURL); err != nil {
        fmt.Println("Error:", err)
    }
}
```

&nbsp;

&nbsp;

&nbsp;

## Downloading Files from an API with Go

**Downloading Files from an API**

Welcome back! Today's focus will be on downloading files from an API using Go. Understanding how to retrieve files efficiently not only enhances your technical skills but also broadens your application's capabilities. In this lesson, we'll explore a practical scenario using our **to-do list API**, which, in addition to managing tasks, supports handling text files such as notes. These notes can be downloaded or uploaded through the /notes endpoint, allowing functionality for storing supplementary information. For example, users might keep notes about a meeting or important reminders. By understanding how to interact with this endpoint, you can effectively manage notes within your application. By the end of this lesson, you'll know how to request a file from an API, save it locally, and verify its contents.

Let's dive into downloading files with precision and confidence!

&nbsp;

**Basic File Download with GET Requests**

**GET requests** are fundamental for retrieving files from an API. When you send a GET request using Go's net/http package, your client communicates with the server at a specified URL, asking it to provide the file. The server responds with the file data, if available and permissible, along with an HTTP status code (like `200 OK`).

Here's a basic example of downloading a file named welcome.txt from our API at `http://localhost:8000/notes`. This approach downloads the entire file at once, which is manageable for smaller files.

```Go
package main

import (
    "fmt"
    "io"
    "net/http"
    "os"
)

func main() {
    // Base URL for the API
    baseURL := "http://localhost:8000"

    // Specify the note name to download
    noteName := "welcome.txt"

    // Send a GET request to download the file
    resp, err := http.Get(fmt.Sprintf("%s/notes/%s", baseURL, noteName))
    if err != nil {
        fmt.Printf("Error occurred: %v\n", err)
        return
    }
    defer resp.Body.Close()

    // Check for HTTP errors
    if resp.StatusCode != http.StatusOK {
        fmt.Printf("HTTP error occurred: %s\n", resp.Status)
        return
    }

    // Save the file locally
    body, err := io.ReadAll(resp.Body)
    if err != nil {
        fmt.Printf("Error reading response body: %v\n", err)
        return
    }

    // Create (or overwrite) the file and write all data at once
    err = os.WriteFile("downloaded_"+noteName, body, 0644)
    if err != nil {
        fmt.Printf("Error writing file: %v\n", err)
    }
}
```

This code sends a GET request and writes the full response content to a local file. This method works well for small files but can strain memory for larger files.

&nbsp;

**Leveraging GET Requests and Streaming**

When dealing with large files, downloading them all at once can be inefficient and strain memory. To address this, you can use Go's `io` package to download files in chunks, thus optimizing memory usage and maintaining efficiency.

Below is a detailed code example demonstrating how to download the same file using streaming:
```Go

package main

import (
    "fmt"
    "io"
    "net/http"
    "os"
)

func main() {
    // Base URL for the API
    baseURL := "http://localhost:8000"

    // Specify the note name to download
    noteName := "welcome.txt"

    // Send a GET request to the API
    resp, err := http.Get(fmt.Sprintf("%s/notes/%s", baseURL, noteName))
    if err != nil {
        fmt.Printf("Error occurred: %v\n", err)
        return
    }
    defer resp.Body.Close()

    // Check for HTTP errors
    if resp.StatusCode != http.StatusOK {
        fmt.Printf("HTTP error occurred: %s\n", resp.Status)
        return
    }

    // Define the filename for storing the downloaded content
    fileName := "downloaded_" + noteName

    // Open the local file in binary write mode
    file, err := os.Create(fileName)
    if err != nil {
        fmt.Printf("Error creating file: %v\n", err)
        return
    }
    defer file.Close()

    // Copy the response body to the file in chunks
    _, err = io.Copy(file, resp.Body)
    if err != nil {
        fmt.Printf("Error writing to file: %v\n", err)
    }
}
```

In the code above, the `os.Create` function initializes a new file where the downloaded content will be stored, ensuring it's ready for writing. Instead of reading the entire response into memory, `io.Copy` streams data directly from resp.Body to the file, processing it in chunks. This approach optimizes memory usage, making it more efficient for handling large file downloads without overwhelming system resources.

By utilizing streaming, even large files are downloaded efficiently. This technique is especially useful when file sizes increase.

&nbsp;

**Verification of Downloaded File Content**

Once you've downloaded a file, it's imperative to verify its contents to ensure a successful transfer. In our example, after downloading, you can open the file and print its content to confirm data integrity:

```Go
package main

import (
    "fmt"
    "os"
)

func main() {
    // Specify the note name to verify
    noteName := "welcome.txt"

    // Open and read the downloaded file to verify its content
    content, err := os.ReadFile("downloaded_" + noteName)
    if err != nil {
        fmt.Printf("File error occurred: %v\n", err)
        return
    }

    fmt.Println(string(content))
}
```

If everything is functioning correctly, you should see an output similar to:

```text
Welcome to Your Notes! 📝

This is a sample note that comes with the application.
```
This step is essential for data verification. The familiar error-handling techniques come into play once more, using Go's error-handling practices to gracefully address any 
issues during the download and verification process.

&nbsp;

&nbsp;

Example 1 : Downloading Files with GET Requests in Go
```Go
package main

import (
    "fmt"
    "io"
    "net/http"
    "os"
)

func main() {
    // Base URL for the API
    baseURL := "http://localhost:8000"

    // Specify the note name to download
    noteName := "welcome.txt"

    // Send a GET request for the specified note
    response, err := http.Get(fmt.Sprintf("%s/notes/%s", baseURL, noteName))
    if err != nil {
        fmt.Printf("Other error occurred: %v\n", err)
        return
    }
    defer response.Body.Close()

    // Check for HTTP errors
    if response.StatusCode != http.StatusOK {
        fmt.Printf("HTTP error occurred: %s\n", response.Status)
        return
    }

    // Define the filename for storing the downloaded content
    fileName := fmt.Sprintf("downloaded_%s", noteName)

    // Open the file in binary write mode and write the content directly
    file, err := os.Create(fileName)
    if err != nil {
        fmt.Printf("Error creating file: %v\n", err)
        return
    }
    defer file.Close()

    _, err = io.Copy(file, response.Body)
    if err != nil {
        fmt.Printf("Error writing to file: %v\n", err)
        return
    }

    // Print success message if file is downloaded successfully
    fmt.Printf("Note downloaded successfully: %s\n", fileName)

    // Read and print the content of the downloaded file
    content, err := os.ReadFile(fileName)
    if err != nil {
        fmt.Printf("Error reading file: %v\n", err)
        return
    }
    fmt.Println("Content of the downloaded file:\n")
    fmt.Println(string(content))
}
```

&nbsp;

Example : Downloading Files from an API with Go
```Go
package main

import (
    "fmt"
    "io"
    "net/http"
    "os"
)

func main() {
    baseURL := "http://localhost:8000"
    noteName := "welcome.txt"
    fileName := fmt.Sprintf("downloaded_%s", noteName)

    err := downloadFile(baseURL, noteName, fileName)
    if err != nil {
        fmt.Printf("Error: %v\n", err)
        return
    }

    err = printFileContent(fileName)
    if err != nil {
        fmt.Printf("Error: %v\n", err)
    }
}

func downloadFile(baseURL, noteName, fileName string) error {
    response, err := http.Get(fmt.Sprintf("%s/notes/%s", baseURL, noteName))
    if err != nil {
        return fmt.Errorf("request error: %w", err)
    }
    defer response.Body.Close()

    if response.StatusCode != http.StatusOK {
        return fmt.Errorf("HTTP error: %s", response.Status)
    }

    file, err := os.Create(fileName)
    if err != nil {
        return fmt.Errorf("error creating file: %w", err)
    }
    defer file.Close()

    // TODO: Fix this inefficient implementation that loads the entire file into memory
    _, err = io.Copy(file, response.Body)
    if err != nil {
        return fmt.Errorf("error reading response: %w", err)
    }


    fmt.Printf("Note downloaded successfully: %s\n", fileName)
    return nil
}

```

&nbsp;

Example: Download and Verify File from API in Go
```Go
package main

import (
    "fmt"
    "io"
    "net/http"
    "os"
)

func main() {
    // Base URL for the API
    baseURL := "http://localhost:8000"

    // Specify the note name to download
    noteName := "welcome.txt"

    // Send a GET request for the specified note file
    response, err := http.Get(fmt.Sprintf("%s/notes/%s", baseURL, noteName))
    if err != nil {
        fmt.Printf("Error occurred: %v\n", err)
        return
    }
    defer response.Body.Close()

    // Check for HTTP errors
    if response.StatusCode != http.StatusOK {
        fmt.Printf("HTTP error occurred: %s\n", response.Status)
        return
    }

    // TODO: Define the filename for storing the downloaded content
    fileName := "downloaded_" + noteName
    
    // TODO: Open the file in write mode and write the content
    file, err := os.Create(fileName)
    if err != nil {
        fmt.Printf("Error creating file: %v\n", err)
        return
    }
    
    defer response.Body.Close()
    
    _, err = io.Copy(file, response.Body)
    if err != nil {
        fmt.Printf("Error writing to file: %v\n", err)
    }
    
    
    content, err := os.ReadFile(fileName)
    if err != nil {
	    fmt.Printf("Error reading file: %v\n", err)
	    return
    }

    // TODO: Read and print the content of the downloaded file
    fmt.Printf("File saved as: %s\n", fileName)
	fmt.Printf("File content:\n%s\n", content)
    
}
```

&nbsp;


Example: Download a File from an API Endpoint in Go
```go
package main

import (
    "fmt"
    "io"
    "net/http"
    "os"
)

func main() {
    // Base URL for the API
    baseURL := "http://localhost:8000"

    // TODO: Specify the note name to download as "welcome.txt"
    noteName := "welcome.txt"
    // TODO: Define the filename for storing the downloaded content
    fileName := "downlaoded_" + noteName
    
    // TODO: Send a GET request for the specified note
    resp, err := http.Get(fmt.Sprintf("%s/notes/%s", baseURL, noteName))
    if err != nil {
        fmt.Printf("Error occurred: %v\n", err)
        return
    }
   
    // TODO: Check for HTTP errors
    if resp.StatusCode != http.StatusOK {
       fmt.Printf("HTTP error occurred: %s\n", resp.Status)
       return
    }
    // TODO: Open the file in binary write mode and write content
    file, err := os.Create(fileName)
    if err != nil {
        fmt.Printf("Error creating file: %v\n", err)
        return
    }
    defer file.Close()
    
    _, err = io.Copy(file, resp.Body)
    if err != nil {
        fmt.Printf("Error writing to file: %v\n", err)
    }
    // TODO: Read and print the content of the downloaded file
    content, err := os.ReadFile(fileName)
    if err != nil {
        fmt.Printf("Error reading file: %v\n", err)
        return     
    }
    
    defer resp.Body.Close()
    // TODO: Handle request errors
    // TODO: Handle file creation, write, and read errors
    fmt.Println(string(content))
}
```

&nbsp;

&nbsp;

&nbsp;


Uploading Files to an API with Go
Uploading Files to an API
Welcome to the next step in your journey of mastering API interactions with Go! In our previous lesson, you learned how to handle errors in API requests, enhancing your skills in building robust applications. Today, we will take a look at the process of uploading files to an API. This capability is crucial for creating applications that need to store or share files, such as documents, images, or any other type of data with an external server.

Understanding file uploads will further expand your ability to interact with APIs, equipping you to build more robust and feature-complete applications. By the end of this lesson, you will learn how to send a file to a server using Go, ensuring that you can manage uploads confidently and efficiently.

Understanding HTTP File Uploads
To upload files via HTTP, the POST method is commonly used, as it’s designed for submitting data to a server, including files. The key to sending files is using multipart/form-data, a format that allows both text and binary data to be sent together, organized into separate parts. This format ensures the server can properly handle the uploaded file along with any additional data.

In Go, the mime/multipart package is used to handle multipart/form-data. This package provides the necessary tools to create a multipart form, allowing you to include files and other data in your HTTP requests.

Uploading a File Step by Step
Uploading a file to an API involves several key steps to ensure the data is properly prepared and transmitted. This process typically includes opening the file, formatting it for HTTP transmission, and sending it using an HTTP request. In this section, we’ll explore each step in detail, starting with how to open a file in Go.

1. Opening the File
Before sending a file to an API, you first need to open it for reading. This ensures the file exists and can be read correctly:

Go
Copy to clipboard
file, err := os.Open("file.txt")
if err != nil {
    fmt.Println("Error opening file:", err)
    return
}
defer file.Close()
This code opens file.txt and defers its closing to avoid resource leaks.

2. Creating the Multipart Form
To send a file via multipart/form-data, a buffer is created to hold the form data:

Go
Copy to clipboard
var requestBody bytes.Buffer
writer := multipart.NewWriter(&requestBody)
The multipart.NewWriter helps construct a form request with multiple parts.

3. Attaching the File to the Form
A field is created in the form to hold the file’s content:

Go
Copy to clipboard
part, err := writer.CreateFormFile("file", "file.txt")
if err != nil {
    fmt.Println("Error creating form file:", err)
    return
}

_, err = io.Copy(part, file)
if err != nil {
    fmt.Println("Error copying file data:", err)
    return
}
This attaches the file content to the form under the field "file".

4. Finalizing the Form Data
Once all fields are set, the writer needs to be closed:

Go
Copy to clipboard
err = writer.Close()
if err != nil {
    fmt.Println("Error closing writer:", err)
    return
}
Closing finalizes the form data so it can be sent.

5. Sending the HTTP Request
A POST request is created to upload the file:

Go
Copy to clipboard
request, err := http.NewRequest("POST", "http://example.com/upload", &requestBody)
if err != nil {
    fmt.Println("Error creating request:", err)
    return
}
request.Header.Set("Content-Type", writer.FormDataContentType())
The request is configured with the correct content type, ensuring the server knows how to handle the request.

6. Executing the Request
Finally, the request is executed, and the response is handled:

Go
Copy to clipboard
client := &http.Client{}
response, err := client.Do(request)
if err != nil {
    fmt.Println("Error sending request:", err)
    return
}
defer response.Body.Close()

if response.StatusCode != http.StatusOK {
    fmt.Println("Failed to upload file:", response.Status)
    return
}

fmt.Println("File uploaded successfully")
This sends the request and checks if the upload was successful.

Code Example: Uploading a File
Now, let's delve into the process of uploading a file using Go. Consider the following code example, which utilizes the "/notes" endpoint to upload a file named meeting_notes.txt.

Go
Copy to clipboard
package main

import (
    "bytes"
    "fmt"
    "io"
    "mime/multipart"
    "net/http"
    "os"
)

func main() {
    // Define the API base URL and the file to be uploaded
    baseURL := "http://localhost:8000"
    fileName := "meeting_notes.txt"

    // Attempt to upload the file and handle any errors
    if err := uploadFile(baseURL, fileName); err != nil {
        fmt.Println("Error uploading file:", err)
    }
}

// uploadFile manages the overall process of opening a file, creating the request, and sending it
func uploadFile(baseURL, fileName string) error {
    // Open the file for reading
    file, err := os.Open(fileName)
    if err != nil {
        return fmt.Errorf("file not found: %s", fileName)
    }
    defer file.Close()

    // Create the multipart form data containing the file
    requestBody, contentType, err := createMultipartForm(file, fileName)
    if err != nil {
        return fmt.Errorf("error creating form data: %w", err)
    }

    // Create the HTTP request for uploading the file
    request, err := createUploadRequest(baseURL, requestBody, contentType)
    if err != nil {
        return err
    }

    // Send the request and return any errors encountered
    return sendRequest(request)
}

// createUploadRequest constructs an HTTP POST request with the multipart form data
func createUploadRequest(baseURL string, requestBody *bytes.Buffer, contentType string) (*http.Request, error) {
    request, err := http.NewRequest("POST", fmt.Sprintf("%s/notes", baseURL), requestBody)
    if err != nil {
        return nil, fmt.Errorf("error creating request: %w", err)
    }
    // Set the Content-Type header to indicate multipart form data
    request.Header.Set("Content-Type", contentType)
    return request, nil
}

// sendRequest executes the HTTP request and handles the response
func sendRequest(request *http.Request) error {
    client := &http.Client{}
    response, err := client.Do(request)
    if err != nil {
        return fmt.Errorf("error sending request: %w", err)
    }
    defer response.Body.Close()

    // Check if the server responded with a success status
    if response.StatusCode != http.StatusOK {
        return fmt.Errorf("failed to upload file: %s", response.Status)
    }

    fmt.Println("File uploaded successfully")
    return nil
}

// createMultipartForm creates a multipart form containing the file
func createMultipartForm(file *os.File, fileName string) (*bytes.Buffer, string, error) {
    var requestBody bytes.Buffer
    writer := multipart.NewWriter(&requestBody)

    // Create a form field to store the file data
    part, err := writer.CreateFormFile("file", fileName)
    if err != nil {
        return nil, "", err
    }

    // Copy the file content into the multipart form field
    if _, err = io.Copy(part, file); err != nil {
        return nil, "", err
    }

    // Finalize the multipart form by closing the writer
    if err = writer.Close(); err != nil {
        return nil, "", err
    }

    // Return the request body and content type
    return &requestBody, writer.FormDataContentType(), nil
}
This code example demonstrates how to properly upload a file to an API:

The os.Open function opens the file, which is essential for properly handling the file data.
The mime/multipart package is used to create a multipart form, and the file is attached to the form.
The http.NewRequest function sends a POST request to the API's /notes endpoint, attaching the file in the request.
If the upload is successful, a success message is printed to the console.
Verifying the File Upload
Once a file is uploaded, it's important to verify it to ensure that the file is stored correctly on the server. You can achieve this by sending a GET request to the corresponding endpoint and checking the content of the uploaded file.

Go
Copy to clipboard
package main

import (
    "fmt"
    "io"
    "net/http"
)

func main() {
    // Base URL for the API
    baseURL := "http://localhost:8000"

    // Specify the file name to verify
    fileName := "meeting_notes.txt"

    // Create a GET request to retrieve the file content
    response, err := http.Get(fmt.Sprintf("%s/notes/%s", baseURL, fileName))
    if err != nil {
        fmt.Println("Error sending request:", err)
        return
    }
    defer response.Body.Close()

    // Check the response status
    if response.StatusCode != http.StatusOK {
        fmt.Println("Failed to retrieve file:", response.Status)
        return
    }

    // Read and print the content of the file
    body, err := io.ReadAll(response.Body)
    if err != nil {
        fmt.Println("Error reading response body:", err)
        return
    }

    fmt.Println(string(body))
}
In this code, we retrieve the content of the file from the server and print it out. This allows us to confirm that the file has been uploaded and stored successfully.

text
Copy to clipboard
Meeting Notes

Date: 2023-10-18
Time: 3:00 PM
Location: Conference Room A

Attendees:
- Alice Johnson
- Bob Smith
- Charlie Brown
...
This output confirms that the file meeting_notes.txt is present on the server and its contents are intact, with details such as the date, time, location, and attendees of a meeting.











&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

