# Webserv

This is a project for creating a HTTP server in C++ 98. The program takes a configuration file as an argument, or uses a default path. The server is non-blocking and uses only 1 poll() (or equivalent) for all the I/O operations between the client and the server (listen included). The use of execve to run another web server is not allowed. The server should be compatible with web browsers and must be able to handle GET, POST, and DELETE methods. Stress tests should be run to ensure the server stays available at all times. The configuration file should allow for the setup of port and host for each 'server', as well as server_names or not. MacOS users are allowed to use fcntl() to set file descriptors in non-blocking mode.

## Project specifications

This HTTP server had to be written in C++ 98.

<b>Allowed functions:</b>
``` execve, dup, dup2, pipe, strerror, gai_strerror, errno, dup, dup2, fork, htons, htonl, ntohs, ntohl, select, poll, epoll (epoll_create, epoll_ctl, epoll_wait), kqueue (kqueue, kevent), socket, accept, listen, send, recv, bind, connect, getaddrinfo, freeaddrinfo, setsockopt, getsockname, getprotobyname, fcntl```

## How to use

The current version of Webserv is developed and tested on macOS.

<b>Requirements:</b>
- GCC / CLANG Compiler
- GNU Make

```
git clone https://github.com/srein91/webserv.git cub3d
```
```
cd webserv && make
```
```
./webserv <path to config file>
```

## Configurations

This server uses a configuration file to manage settings such as the listening port, server names, index files, and other options. This file is passes as an argument when starting the server. If no configuration file is provided, the server will use a default configuration.

A server block with a location block can look like this:

```
server 
{
	server_name			  non;
	port				      8080;

	root				      docs/;
	index				      test.html;

	allowed_methods		GET POST DELETE;
	limit_body			  999999;
	error_pages			  other_error_pages/;
	cgi					      php py;

	location /cgi
	{
    allowed_methods		  GET;
    root				        cgi/;
    index				        hello.php;
    directory_listing	  off;
	}
}
```
block identifiers:\
server		        servers are sockets that listen to one or multiple port(s)\
location	        locations are used to define how the server should handle\
                    requests for specific URI paths.\
\
server identifiers:\
server_name:        The name of the server - required and has to be unique.\
port:               The port(s) the server listens to. - required and has to be unique.\
\
root:               The directory the server operates in (including subdirectories). - required.\
index:              The default file the server answers with if the request is the root directory\
                    (defaults to index.html)\
allowed_methods:    The HTTP methods allowed inside the server block.\
                    Available options are: GET, POST, DELETE\
                    (defaults to to GET, POST and DELETE)\
limit_body:         The maximum body size of the response.\
					(defaults to 65536)\
error_pages:        Defines the folder to use for the error pages.\
                    (defaults to error_pages/)\
cgi:                The CGI allowed inside the server block.\
                    Available options are: php, py\

location identifiers:\
prefix:             The string right after the location identifier.\
                    It specifies the directory the location block looks for in the request URI. - required.\
allowed_methods:    The HTTP methods allowed inside the location block.\
                    Available options are: GET, POST, DELETE\
                    (defaults to to GET, POST and DELETE)\
root:               The actual directory of the location inside the server. - required.\
index:              The file that will be sent if the request is the root directory of this location. - required.\
directory_listing:  Enables / Disables directory listing in the location block.
					(defaults to off)\
<br>
![alt text](https://github.com/SRein91/Webserv/blob/main/Webserv.png?raw=true)

<br>
<hr>
<b>This is a 42 project which has to be written in C++ 98.<br></b>
