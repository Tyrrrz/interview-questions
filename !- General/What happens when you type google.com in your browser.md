# What happens when you type google.com in your browser

___

**Q: What happens when you type google.com in your browser?**

Upon typing in "google.com" and pressing Enter, the browser takes a number of steps which can be outlined as:

- Resolve IP address of the URL via DNS
- Generate an HTTP request with headers (`accept`, `user-agent`, `cookie`, etc)
- Open an HTTP connection to the resolved IP address
- Send the request to the server
- Receive the response from the server
- Parse response headers
- Depending on the response headers, perform additional operations
- Decompress the response body if it's compressed (e.g. gzipped)
- Parse the HTML code inside the response body
- Resolve any additional resources (images, stylesheets, scripts, etc)
- Start loading those resources via their URLs using the same steps
- Render the HTML as soon as required resources are loaded, continue loading remaining resources in background
- When all the resources are loaded, close the HTTP connection

___

**Q: What are "additional operations" that a browser can perform depending on response headers?**

For example, it can redirect to a different resource on a 301/302/303/307 status code and a `location` header.

Browsers also respect policies regulated by the server, e.g. `cache-control` header which instructs how the resources can be cached.

Response headers may also contain information regarding the response content, including its type, length, and encoding, which may be used by the browser to properly display the content.

___

**Q: What is DNS?**

DNS, which stands for Domain Name System, is essentially a dictionary that maps URLs of Internet resources to their corresponding IP addresses. Most people call it "the phonebook of the Internet".

In most cases, the IP of the first outbound DNS server is provided by the ISP, which can be configured on a per-router or per-machine basis. It's also possible to use any other DNS server by setting the corresponding IP address in system's settings.

Any individual DNS server may route a query to a different DNS server if it doesn't have the requested DNS record.

Browsers and operating systems have their own local DNS cache which helps avoid querying DNS servers every time.

___

**Q: How are DNS records kept up to date?**

When setting up a domain, the owner configures it by specifying name servers given by their hosting provider. These changes reflect slowly because once the records change on the name server, an adjacent DNS server may still use its older cached records for some time.

Since a request from a user's device to a remote server usually takes multiple hops through different DNS servers each with their own local cache, it may take up to 72 hours before the correct IP address is fully propagated.

The process of updating DNS records on changes is called "DNS propagation".

___

**Q: What is a web server?**

Web server is a piece of software that handles HTTP requests, e.g. IIS, nginx, Apache, Kestrel, etc. It receives an incoming request from a client (e.g. browser), routes it to a specific handler based on request parameters (i.e. URL, headers, content), processes it, and then returns a response.
