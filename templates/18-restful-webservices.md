# 18 Restful Webservices

That URI is call resources, you should think of all the stuff in terms of resources. Unlike soap, they don't contains words, all the action are express in the request. getUser.do?id=123 =>GET method XX.com/users/123. The apis contains **nouns** not verbs. Also they usually using pural form. not **user** but **users**. It's like browsing a directory. multiple users under the XXX/users/ folder. And when you specify "XXX/users" this will give you all the users

**create -> post,** creating a reouse is always to the collection resources. Like when you create new message, use "/messages" because you don't know the ID at the time of creation, and usually ID is manage by the system not the client. (repeatedly execute will have side effects. **not Idempotence**)

**read -> get,** no side effects **Idenmpotence**

**update -> put** (/patch for partial updates), no side effects **Idenmpotence**

**delete -> delete,** no side effects **Idenmpotence**

Resource relations: a comment a user made on a post in a social media application.

comments/2 => /messages/1/comments/2 (this design desision depends on what the client knows. if the client only knows the commentId, it would take them extra effort to find out the messageID. If when they access the comment they always know the messageID, then the 2nd uri approach is better because it keeps the relationship between comment & the message) This happenned when you have 1 : many relationship

Service Definition : soap -> wsdl, thrift also has that.

Header's contentType value tells each other what the message format is. (text/xml;applicatio/json)

Use Data transfer object or per-request objects

get/customer/123?expand=invoices

better than

get/customers/123/invoices

versioning:

api/v1/customers --> api/v2/customers

pagination & parameter for filtering results: "/messages?year=2014\&offset=30\&limit=10 " means I want message posted in year 2014 starting from record 30 give me the next 10

HTTP status code : 1XX - information; 2XX success (200-OK, 201 Created, 204 No Content); 3XX Redirection(302 Found, 307 Temporary Redirect, 304 Not Modified - use when nothing changed since last time you request the message) 4XX Client error(400 Bad Request, 401 Unauthorized - need to log in, 403 Forbidden - logged in but not have permission, 404 Not Found, 415 Unsupported Media Type) 5XX Server Error(500 Internal Server Error)

**Upload a file with restful**: (SOAP can use attachement)

You basically have three choices:

1. Base64 encode the file, at the expense of increasing the data size by around 33%.
2.  Send the file first in a&#x20;

    `multipart/form-data`

    &#x20;POST, and return an ID to the client. The client then sends the metadata with the ID, and the server re-associates the file and the metadata.
3. Send the metadata first, and return an ID to the client. The client then sends the file with the ID, and the server re-associates the file and the metadata.
