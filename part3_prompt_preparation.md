# Part 3: Prompt Preparation

## Selected PR: https://github.com/aio-libs/aiokafka/pull/196

---

## 3.1.1 Repository Context 

aiokafka is a Python library designed to provide asynchronous interaction with Apache Kafka. It is built using asyncio, which allows applications to handle multiple operations concurrently without blocking execution.

The library is commonly used in backend systems where real-time data processing is required. It enables developers to build efficient streaming applications, such as event-driven systems and analytics pipelines.

The intended users of this repository are backend developers and data engineers who need high-performance Kafka integration in Python-based applications. It abstracts the complexity of Kafka communication while maintaining efficiency and scalability.

---

## 3.1.2 Pull Request Description 

This pull request introduces socket grouping to improve how the client handles network communication.

Previously, the system used a single socket per node. Because Kafka operations are synchronous, long-running requests such as fetch operations could block other requests like commits.

The new implementation separates sockets into groups based on operation type. Coordination operations use a dedicated socket group, while fetch operations use another.

This change ensures that operations do not block each other, improving performance and responsiveness.

---

## 3.1.3 Acceptance Criteria

✓ When a fetch request is long-running, commit requests should not be delayed  
✓ The system should maintain separate sockets for coordination tasks  
✓ Requests must be routed correctly to socket groups  
✓ No regression in existing functionality  
✓ Multiple sockets per node should be managed safely  

---

## 3.1.4 Edge Cases

- Network failure during socket creation  
- Concurrent requests across groups  
- Resource cleanup issues  
- High concurrency scenarios  
- Compatibility with previous implementation  

---

## 3.1.5 Initial Prompt 

Refactor the Kafka client implementation to support multiple socket groups instead of a single socket per node.

Currently, the system uses one socket per node, which causes blocking when long-running requests occupy the connection. This affects performance and delays critical operations such as commits.

Your task is to introduce socket groups to separate different types of operations.

Requirements:
- Create a socket group abstraction  
- Assign coordination operations to a dedicated group  
- Ensure fetch operations use a separate socket  
- Maintain mapping of node to socket groups  
- Update request routing logic  

Acceptance Criteria:
- Commit requests must not be delayed by fetch operations  
- Each socket group must operate independently  
- No regression in functionality  
- Proper connection cleanup  

Edge Cases:
- Handle network failures  
- Ensure thread-safe async operations  
- Avoid duplicate socket creation  

Testing:
- Add unit tests for routing logic  
- Simulate concurrent operations  
- Verify no blocking occurs  
