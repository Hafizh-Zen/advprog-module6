REFLECTION 1

The handle_connection function starts by accepting a TCP stream, which represents an incoming HTTP request from a web browser. It wraps this mutable stream with a buffered reader to minimize system calls by improving reading efficiency.

With buffering in place, the function reads the request line by line using the lines method. Each line is mapped to extract its value, assuming successful reads. This continues until it reaches an empty line, marking the end of the HTTP headers. All the collected lines are stored in a vector of strings and then printed in a formatted manner using println.

REFLECTION 2

![Commit 2 screen capture](image/commit2.png)

This function operates similarly to handle_connection but introduces a crucial addition. After reading the incoming request, it constructs an HTTP response that includes a status line with the HTTP version, status code, and message. It also specifies the content length, indicating how much data will be sent, followed by the HTML content intended for the browser.

To build this response, the function defines each component as a separate string variable. It then combines them using string formatting to create the complete response, which is sent back through the stream as bytes so the browser can properly display the content.

REFLECTION 3

![Commit 3 screen capture](image/commit3.png)

The main goal of validation here is to inspect the status line, specifically focusing on the URI—the second element in the request line. This part contains the path of the requested resource and helps identify what the client is asking for.

In this implementation, the function checks if the request is a valid GET request. If so, it responds with the hello.html file. If not, it returns a 404 Not Found response. This targeted validation improves efficiency by simply confirming whether the request is valid, instead of examining every request in detail.

REFLECTION 4

After executing and reviewing the code, I observed that the redirection to the hello.html page was noticeably slow. This slowdown was caused by the presence of a sleep function in the code, which tied up system resources and delayed the handling of my original request. While the program was sleeping, the request was placed in a queue, causing a wait time. As a result, the response was delayed, and the page load felt sluggish because the server couldn't process other requests until the sleep duration ended.

REFLECTION 5

Integrating multithreading into the application greatly enhanced its performance by enabling the use of worker threads to handle incoming requests in parallel. This means multiple requests can be processed at the same time, removing the bottleneck caused by the sleep function, which previously delayed other requests.

The implementation starts by setting up a communication channel for passing tasks to threads. A thread pool is then created with multiple workers—each one dedicated to handling a single request. When a request is received, it's wrapped in a function, placed in a Box, and sent to a worker. The worker locks onto the task until it's done. In this case, four worker threads are used, and for each incoming stream from the TCPListener, the thread pool runs the handle_connection function to manage the request.






