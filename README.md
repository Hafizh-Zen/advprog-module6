REFLECTION 1

The handle_connection function starts by accepting a TCP stream, which represents an incoming HTTP request from a web browser. It wraps this mutable stream with a buffered reader to minimize system calls by improving reading efficiency.

With buffering in place, the function reads the request line by line using the lines method. Each line is mapped to extract its value, assuming successful reads. This continues until it reaches an empty line, marking the end of the HTTP headers. All the collected lines are stored in a vector of strings and then printed in a formatted manner using println.

REFLECTION 2

![Commit 2](images/commit2.png)

This function operates similarly to handle_connection but introduces a crucial addition. After reading the incoming request, it constructs an HTTP response that includes a status line with the HTTP version, status code, and message. It also specifies the content length, indicating how much data will be sent, followed by the HTML content intended for the browser.

To build this response, the function defines each component as a separate string variable. It then combines them using string formatting to create the complete response, which is sent back through the stream as bytes so the browser can properly display the content.

REFLECTION 3

![Commit 2](images/commit3.png)

The main goal of validation here is to inspect the status line, specifically focusing on the URI—the second element in the request line. This part contains the path of the requested resource and helps identify what the client is asking for.

In this implementation, the function checks if the request is a valid GET request. If so, it responds with the hello.html file. If not, it returns a 404 Not Found response. This targeted validation improves efficiency by simply confirming whether the request is valid, instead of examining every request in detail.

REFLECTION 4

After executing and reviewing the code, I observed that the redirection to the hello.html page was noticeably slow. This slowdown was caused by the presence of a sleep function in the code, which tied up system resources and delayed the handling of my original request. While the program was sleeping, the request was placed in a queue, causing a wait time. As a result, the response was delayed, and the page load felt sluggish because the server couldn't process other requests until the sleep duration ended.








