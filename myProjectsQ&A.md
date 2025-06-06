Tell me about a C++ project where you had to make key architectural decisions.

What trade-offs did you consider?

How did your decision impact performance, scalability, or maintainability?

"In one of our telecom applications, we initially delivered a containerized image. The customer later requested support for a stateless architecture so they could handle instance failures gracefully and scale in/out without affecting ongoing transactions.

However, our system was originally stateful. Each message in a transaction needed to be processed by the same instance because we were storing transaction state in the local cache. Stateless behavior meant that different messages in the same transaction might land on different instances — which would break our design.

To solve this, we moved the state storage from the in-memory cache to Couchbase. All transaction-related data was now stored centrally, making it accessible across instances.

The trade-offs were:

Performance impact due to the added read/write to Couchbase per message.

Increased database storage as more state was persisted.

But the benefits were significant:

The system became horizontally scalable.

We could now tolerate instance failures without losing state.

It enabled true container-native behavior under Kubernetes."**


