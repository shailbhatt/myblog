# Apache Thrift : In microservices API world

<img src="./apache_thrift/asf-estd-1999-logo.jpg" alt="apache_logo" width="250" />

The world of APIs is changing. While developing APIs, it is important to take into consideration, the architecture upon which the system is being built. Choices made in the design phase could make or break the entire system. Wrong API design selection can turn useful concepts and smart applications into useless chunks of code. This happens to many start-ups.

Most architectures fall broadly into two categories- SOAP or REST. For a long time, the two were dominant in the API market. Common thought among designers was that you were either RESTful or SOAP in orientation; this mindset changed with one significant development. In 2007, social media juggernaut Facebook released a technical paper detailing an internal architecture system upon which the bulk of their system operated, and that marked the birth of ‘Thrift’.

‘Thrift’ is a software library and set of code-generation tools developed at Facebook to accelerate development and implementation of efficient and scalable backend services.

<code>
It’s primary goal is to enable efficient and reliable communication across programming languages by abstracting the portions of each language that tend to require the most customization into a common library that is implemented in each language.
</code>

####Specifically, Thrift allows developers to define datatypes and service interfaces in a single language-neutral file and generate all the necessary code to build RPC clients and servers.

Thrift takes care of all the challenging cross-language interaction in a networked environment. The key pre requisites for this to funtion properly are:

1. Types : A common type system must exist across programming languages.
2. Transport : Each language must have a common interface for bidirectional raw data transport.
3. Protocol : Datatypes must have some way of using the Transport layer to encode and decode themselves.
4. Versioning : For robust services, the involved datatypes must provide a mechanism for versioning themselves. Specifically, it should be possible to add or remove fields in an object or alter the argument list of a function without any interruption in service (or, worse yet, nasty segmentation faults).
5. Processor : Finally, we generate code, capable of processing data streams to accomplish remote procedure calls.


###Benefits and drawback of thrift:

While both REST and SOAP are wonderful architectures, they are limited in the languages they speak; Thrift functions in binary and has support for common languages such as C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk, OCaml and Delphi and more. Thrift is open source, meaning it is monitored, changed, and tracked by many groups, making it more secure and up to date.

But, the biggest drawback of Thrift is the fact that it is relatively new, and thus is fighting a crowded field to build a solid user base.

Currently I am working on a project implementing a thrift API for communication between PHP, Java, & Node.Js services. We have acheived a massive gain in performance by utilizing binary serialization to handle data and the results are amazing.

If you decide to get started with thrift. Please visit https://thrift.apache.org/.

Happy Learning!

reference: https://thrift.apache.org/static/files/thrift-20070401.pdf
