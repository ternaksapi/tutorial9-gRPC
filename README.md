# Tutorial Modul 8 - Subscriber
---
### Muhammad Yusuf Haikal
### 2206081490
### Pemrograman Lanjut A
---

## 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
In unary RPC, the client sends a single request to the server, and the server responds with a single response. Unary RPCs are suitable when you have a simple operation that requires a single request and a single response, for example authenticating a user, performing a calculation and obtaining the result, etc.

In server streaming RPC, the client sends a single request to the server, and the server responds with a stream of responses. It is suitable for when the server needs to push a large amount of data or a continuous stream of updatesto the client. For example, streaming audio or video content, real time stock market prices update, etc.

In bi-directional, both the client and the server can send a stream of responses to each other independently. It is suitable for when there is a need for ongoing, interactive communication such as a messaging application.

## 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
There are few things to  consider while implementing gRPC service, particularly regarding authentication, authorization, and data encryption. Since gRPC itself does not provide built-in authentication mechanisms, we need to implement our own authentication scheme or integrate with existing authentication services. Other than that, we also have make sure that we validate and sanitize all user input to prevent injection attacks, such as SQL injection, code injection, or other types of attacks.

## 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
One thing to pay attention to is with concurrencry and synchronization. Bidirectional streaming involves simultaneous communication between the client and server, which requires proper handling of concurrency and synchronization to avoid race conditions, deadlocks, and other concurrency-related issues. Rust's ownership model and thread safety can help mitigate some of these issues, but proper synchronization mechanisms like mutexes, channels, or locks may still be required


## 4. What are the advantages and disadvantages of using the `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?
ReceiverStream allows you to separate the logic of producing the stream items (e.g., fetching data from a database, processing data, etc.) from the gRPC service implementation. This separation can lead to more modular and testable code. It also works well with Tokio's asynchronous runtime, allowing you to efficiently handle multiple concurrent streams and leverage non-blocking I/O operations. The disadvantage, meanwhile, is the addedcomplexity. Using ReceiverStream introduces an additional layer of abstraction and complexity to your code, as you need to manage the asynchronous sender (mpsc::Sender) and receiver (mpsc::Receiver) separately. Also, if not used correctly, ReceiverStream can lead to deadlocks, especially if the producer and consumer are not properly synchronized or if the stream is not consumed correctly.


## 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
Like I've said in the previous point, ReceiverStream seperation of concerns with stream items allows it to achieve seperation of concerns, making the code to be in the form of modules, thus making the code more maintainable and extensible. 


## 6. In the **MyPaymentService** implementation, what additional steps might be necessary to handle more complex payment processing logic?
There's a few things we can do. One thing is to implement input validation. We can validate the input parameters received in the PaymentRequest message to ensure they meet the expected criteria. Check for the presence of mandatory fields, validate data formats (e.g., user ID format, amount format), and enforce any business rules or constraints. Also, if our payment processing logic requires interactions with external systems (e.g., payment gateways, databases), we can implement integration mechanisms to communicate with these systems securely and efficiently. Handle communication failures, retries, and timeouts gracefully.


## 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
Overall, the adoption of gRPC as a communication protocol in distributed systems promotes language and platform interoperability, aligns well with service-oriented and microservices architectures, and offers potential performance benefits. However, it also introduces challenges in terms of integrating with legacy systems, ecosystem maturity, and operational considerations. Careful planning and evaluation of the trade-offs are necessary to ensure a successful adoption of gRPC in the context of a specific distributed system architecture and technology stack.


## 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
The choice between HTTP/2 (gRPC) and HTTP/1.1 (or HTTP/1.1 with WebSocket) for REST APIs depends on the specific requirements of your distributed system or application. If you prioritize performance, efficiency, and support for bidirectional streaming, gRPC with HTTP/2 may be a better choice. However, if simplicity, browser compatibility, and a mature ecosystem are more important, REST APIs built on HTTP/1.1 (or with WebSocket) may be a better fit. In some cases, a hybrid approach combining gRPC for internal communication and REST APIs for external-facing services may be employed to leverage the strengths of both approaches.


## 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
EST APIs are synchronous in nature, meaning that clients must wait for a response from the server after sending a request. This can introduce latency, especially for long-running or complex requests, while gRPC facilitates asynchronous communication, meaning that clients and servers can continue processing other tasks while waiting for messages from each other. This reduces latency and improves responsiveness, especially for real-time applications. REST APIs follow a request-response model that can be less conducive to real-time communication and responsiveness, gRPC's bidirectional streaming capabilities offer a more efficient and scalable approach for real-time communication. gRPC enables asynchronous, bidirectional communication, allowing clients and servers to exchange messages in real-time without the need for polling or long-polling techniques.

## 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
There are a few implications of the schema-based approach of gPRC using Protocol Buffers. Protocol Buffers provide strict type definitions, ensuring that data structures and types are consistent across clients and servers. This type safety can help catch errors at compile-time or deserialization-time, reducing runtime errors and improving overall robustness. Protocol Buffers also use a binary serialization format, which is more compact and efficient than JSON, especially for large payloads or high-throughput scenarios. Other than that, Protocol Buffers have built-in mechanisms for handling backwards and forwards compatibility, allowing clients and servers to evolve independently while maintaining compatibility with older versions.
