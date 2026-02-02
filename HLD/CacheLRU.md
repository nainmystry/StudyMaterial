Cache (LRU/LFU) HLD 

High-Level Overview:
An In-Memory Cache System that stores key-value pairs for fast data access, implements eviction policies (LRU/LFU) to manage memory constraints, and provides high-performance read/write operations using strategy-based eviction algorithms.

1. Core Architecture
Cache Client: Applications/services making cache requests

Cache Engine: Core caching logic with storage and eviction management

Eviction Strategy Layer: Pluggable algorithms for cache management

Monitoring & Metrics: Real-time cache performance tracking

2. Key Components & Entities
Core Entities (with SRP):
Cache - Main interface for cache operations (get, put, delete)

CacheStorage - Underlying data storage structure

Options: HashMap + Doubly Linked List (for LRU), Priority Queue (for LFU)

EvictionPolicy Interface - Contract for eviction strategies

LRUStrategy - Evicts Least Recently Used items

LFUStrategy - Evicts Least Frequently Used items

FIFOStrategy - Evicts First-In-First-Out

RandomStrategy - Random eviction

CacheEntry - Individual cache item

Attributes: key, value, timestamp, accessCount, frequency

CacheConfig - Configuration management

maxSize, defaultTTL, evictionPolicy, cleanupInterval

CacheMetrics - Performance tracking

Hit rate, miss rate, eviction count, average load time

Supporting Entities:
CacheLoader - Loads data on cache miss (cache-aside pattern)

Serializer - Handles object serialization/deserialization

ExpirationManager - Manages TTL-based eviction

DistributedCacheCoordinator - For distributed cache scenarios

CacheStatistics - Analytics and reporting

3. Component Diagram (PlantUML)
<img width="881" height="779" alt="image" src="https://github.com/user-attachments/assets/3b59b999-687e-4c5a-9415-5a21a6fb55dd" />


4. Data Flow
Read Operation (Cache Hit):
text
1. Application calls cache.get("key")
2. Cache checks memory storage
3. If found → update access metrics (timestamp for LRU, frequency for LFU)
4. Return cached value (O(1) for LRU, O(log n) for LFU)
Read Operation (Cache Miss):
text
1. Cache doesn't find key
2. Invoke CacheLoader to fetch from data source
3. Check if cache is at capacity
4. If full → invoke EvictionPolicy to remove item(s)
5. Insert new entry with current timestamp/frequency
6. Update CacheMetrics (increment miss count)
Write Operation:
text
1. Application calls cache.put("key", value)
2. If key exists → update value and metadata
3. If key doesn't exist → check capacity, evict if needed
4. Insert new entry
5. If TTL specified → schedule expiration
Eviction Process (LRU Example):
text
1. Cache reaches maxSize
2. LRUStrategy traverses doubly linked list
3. Find least recently used node (tail.prev)
4. Remove from hash map and linked list
5. Update eviction count in metrics
5. Scalability & Extensibility
Distributed Caching: Consistent hashing for sharding

Write-through/Write-back: Different consistency models

Monitoring Integration: Prometheus/Grafana dashboards

Dynamic Configuration: Hot-reload cache settings

Multi-level Caching: L1 (in-process) + L2 (distributed) cache

Compression: Reduce memory footprint for large values

6. Why Strategy Pattern?
Swap Policies at Runtime: Change from LRU to LFU based on access patterns

A/B Testing: Compare performance of different eviction algorithms

Custom Policies: Implement application-specific eviction logic

Maintainability: Each strategy is isolated and independently testable

7. Interview Talking Points
"Strategy Pattern allows us to switch eviction policies without changing cache core logic"

"LRU uses O(1) operations via HashMap + Doubly Linked List"

"LFU trades slightly slower eviction (O(log n)) for better frequency-based optimization"

"Cache metrics help choose the right strategy based on actual workload patterns"

"We can implement hybrid strategies like LRU-K for specialized use cases"

8. Performance Considerations
LRU: Fast (O(1)), good for temporal locality patterns

LFU: Slightly slower (O(log n)), better for frequently accessed items

Memory Overhead: 50-100 bytes per entry for metadata

Concurrency: Read-write locks vs. concurrent data structures

Serialization Cost: Consider for complex objects
