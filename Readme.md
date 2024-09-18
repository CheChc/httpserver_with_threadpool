## C++实现的HTTP服务器
### 2024-9-13 23:46
实现功能；拆分为两部分，一部分为http服务器，一部分为线程池用于服务器处理

线程池起到了优化并发请求处理的作用。它允许服务器在同一时刻处理多个客户端连接，而无需为每个请求启动一个新的线程，从而有效地减少了系统的开销

#### 工作流程：
客户端请求到达：当有客户端连接到服务器时，服务器接受该连接，并生成一个 handleClient 任务来处理客户端的请求。

任务进入线程池：服务器将这个任务提交到线程池中，线程池会将该任务加入队列中。

线程执行任务：线程池中的线程会从队列中取出任务并执行。执行的任务就是处理客户端连接的 handleClient 函数。

并发处理多个客户端请求：多个线程可以同时处理不同的客户端连接，实现并发处理。

#### 优势总结：

提高服务器的吞吐量：线程池使得服务器可以同时处理多个客户端请求，减少了客户端的等待时间，从而提高了服务器的响应速度和并发能力。

减少系统资源消耗：通过复用线程，减少了频繁创建和销毁线程的开销。

避免资源争用：通过限制线程池的大小，服务器可以避免因为创建过多线程而耗尽系统资源。

### 2024-9-18 17:02

实现功能：拆分为4部分，http服务器拆分为两部分：接受请求和处理请求，以及线程池部分和日志部分

这个项目体结合 C++ 与 Boost.Asio 框架的高性能特性，主要功能如下：

### 1. **多线程架构**
- **线程池设计**：项目实现了一个灵活的线程池，用来处理并发的客户端请求，提升了服务器的处理能力。线程池允许多个工作线程并行处理 I/O 操作，避免单线程阻塞，最大化 CPU 使用率和响应速度。
- **异步处理**：利用 Boost.Asio 提供的异步 I/O 操作，减少了等待 I/O 操作完成的时间，提升了程序的整体性能。相比传统的同步 I/O 操作，异步处理可以有效利用系统资源，特别是处理高并发时。

### 2. **可扩展性**
- **模块化设计**：各个模块（`HttpServer`、`HttpHandler`、`ThreadPool`、`Logger`）设计独立，职责分明，便于维护和扩展。未来可以轻松添加新的功能，比如支持更多的 HTTP 方法（如 POST、PUT）或改进日志系统。
- **线程池大小可调**：可以灵活设置线程池的大小，以应对不同的负载需求。对不同的服务器环境（如不同 CPU 核数）可以进行优化调整。

### 3. **高性能和并发处理**
- **Boost.Asio 网络库**：项目使用 Boost.Asio 库进行高效的网络通信处理，Boost.Asio 是 C++ 中处理异步 I/O 的常用库，支持跨平台开发，具有高性能和稳定性。
- **共享 `io_context`**：通过在多个线程中共享 `asio::io_context`，实现了高效的 I/O 调度和并发请求处理。

### 4. **日志记录功能**
- **详细的日志记录**：每个请求和响应都通过 `Logger` 类记录到日志文件，便于调试和监控。可以跟踪客户端请求、服务器响应的状态，这对于生产环境中的问题定位至关重要。
- **持久化日志**：日志记录被保存在文件中，可以作为历史记录保存，便于分析用户行为和系统运行状态。

### 5. **灵活的 HTTP 处理**
- **请求处理分离**：`HttpHandler` 类负责具体的请求处理逻辑，处理逻辑和网络层分离，便于开发者针对不同的请求类型定制化处理（如处理静态文件、动态内容等）。
- **简易的 HTTP 响应**：当前返回简单的 `Hello, World!` 响应，可以在此基础上扩展为更复杂的响应逻辑，比如 RESTful API 或者文件服务。

### 6. **跨平台支持**
- **Boost 库的使用**：Boost 是一个跨平台的库，项目可以在多个操作系统上编译运行，具备良好的可移植性。
