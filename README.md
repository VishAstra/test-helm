Helm Chart Installation

Create the Chart: Save the above files in the correct directory structure.
Install Helm (if not already installed): Follow the official Helm installation instructions.
Install the Chart:


>>   _helm install clickhouse ./clickhouse-chart_

Use code with caution.
content_copy
4. Testing Data Movement

Connect to ClickHouse: Use the clickhouse-client (you might need to port-forward to the pod if you're running locally) to connect to your ClickHouse pod.

Create a Table:

SQL
CREATE TABLE test_table (id UInt64, data String) ENGINE = MergeTree()
PARTITION BY toYYYYMM(toDate(id))
ORDER BY id
SETTINGS storage_policy = 'default';
Use code with caution.
content_copy
Insert Test Data: Insert enough data to exceed the move factor threshold for one partition.

Monitor Partition Sizes:

SQL
SELECT partition, sum(bytes) AS size FROM system.parts WHERE table = 'test_table' GROUP BY partition;
Use code with caution.
content_copy
You should see data moving from the hot partition to the cold partition over time.

Query Data: Ensure you can still query all data, regardless of the partition.
