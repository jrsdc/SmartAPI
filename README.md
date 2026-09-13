# Smart Campus API

## This repository contains my implementation of a Smart Campus API with sensor reading management for Client-Server Architecture.

## The project was built using JAX-RS (Jersey) to manage rooms, sensors and sensor readings through RESTful endpoints. 

## Technologies Used
- Java
- JAX-RS (Jersey)
- Maven
- Tomcat 9
- JSON

Build and Running instructions:
1. Clone this repo
2. Open this repo in Netbeans
3. If not already installed - install Tomcat 9 to run the server
4. Right click on the project and click 'Clean and Build', followed by 'Run'
5. The base server is /Coursework/api/v1

## Curl Commands

1. Get all rooms
   
curl -X GET http://localhost:8080/Coursework/api/v1/rooms

2. Create a new room
   
curl -X POST http://localhost:8080/Coursework/api/v1/rooms  -H "Content-Type: application/json" \
  -d '{"id": "CLASS-001", "name":"Classroom 1","capacity":30}'
   
3. Filter sensors by their type
   
curl -X GET "http://localhost:8080/Coursework/api/v1/sensors?type=Temperature"


4. Adding a sensor reading
   
curl -X POST http://localhost:8080/Coursework/api/v1/sensors/TEMP-01/readings \
   -H "Content-Type: application/json" \
   -d '{"value":26.5}'

5. Delete a room
   
curl -X DELETE http://localhost:8080/Coursework/api/v1/rooms/CLASS-001

Question: In your report, explain the default lifecycle of a JAX-RS Resource class. Is a new instance instantiated for every incoming request, or does the runtime treat it as a singleton? Elaborate on how this architectural decision impacts the way you manage and synchronize your in-memory data structures (maps/lists) to prevent data loss or race conditions.

A: JAX-RS has a default that treats resource classes that ensures variables aren't preserved in between requests. I stored my data in a static HashMap so that it could belong to the class and not any objects, making it belong to the class meant the data persisted accross all requests.

Question: Why is the provision of ”Hypermedia” (links and navigation within responses) considered a hallmark of advanced RESTful design (HATEOAS)? How does this approach benefit client developers compared to static documentation?

A: HATEOAS is the endpoint of REST design. Instead of relying of static documentation the responses contain hyperlinks that assists developers to resources. This also benfits client because they do not need to rely on documentation that may become outdated.

Question: When returning a list of rooms, what are the implications of returning only IDs versus returning the full room objects? Consider network bandwidth and client side processing.

A: Returning only IDs in a list reduces data usage majorly compared to returning full objects. In this case specifically network bandwidth is reduced by a large amount. Compared to returning IDs which forces clients to make additinal requests to get more details.

Question: Is the DELETE operation idempotent in your implementation? Provide a detailed justification by describing what happens if a client mistakenly sends the exact same DELETE request for a room multiple times.

A: DELETE is idempotente , making a DELETE request multiple times reutrns the same result as can be seen in my video presentation for example. The error that is often returned doesn't change and the action has no major effects on the api as a whole.

Question: We explicitly use the @Consumes (MediaType.APPLICATION_JSON) annotation on the POST method. Explain the technical consequences if a client attempts to send data in a different format, such as text/plain or application/xml. How does JAX-RS handle this mismatch?

A: Using/Sending data in a differrent format does not work, JAX-RS returns that the media type is unsupported. JAX-RS check the type of the header in the request and if it doesn't match it's immediately rejected, this also assists the application in ensuring it doesn't recieve data that cannot be read.

Question: You implemented this filtering using @QueryParam. Contrast this with an alternative design where the type is part of the URL path (e.g., /api/vl/sensors/type/CO2). Why is the query parameter approach generally considered superior for filtering and searching collections?

A: The query parameter is superior for filters and searching because you can visually it processes cleaner URLs, also query parameters are optional, -/sensors still works without the filters.

Question: Discuss the architectural benefits of the Sub-Resource Locator pattern. How does delegating logic to separate classes help manage complexity in large APIs compared to defining every nested path (e.g., sensors/{id}/readings/{rid}) in one massive controller class?

A: Firstly there is responsibilites being passed and seperated so that each class can handle their request, maintainance of the data is also a lot easier for external developers to undertsand assist in, testing is done in smaller classes to ensure sections of the project work - being able to fork out the issue allows for easier debugging. In conclusion if an API had all it code in a single section of a project there would be contiuous issues with understanding multiple lines of code and their logic.

Question: Why is HTTP 422 often considered more semantically accurate than a standard 404 when the issue is a missing reference inside a valid JSON payload?

A: HTTP 442 means that the URL is right and the JSON is fine, however the data inside that request has invalid references.

While 404 means that an endpoint does even exist on the server.

Contextually if a client makes a request for a specifc roomId that doesn't exist 

Question: From a cybersecurity standpoint, explain the risks associated with exposing internal Java stack traces to external API consumers. What specific information could an attacker gather from such a trace?

A: Exposing Java stack can have major issue such as:

- Revealing the name of classes/packages and file locations
- Revealing database table names



