# Billing

Running jobs and storing data on the parallel filesystem consume the billing 
units allocated to your project:

- for computing, your project is allocated CPU-core-hours 
- for storage, your project is allocated GB-hours.

## Compute billing

For compute, your project is allocated CPU-core-hours that are consumed when 
running jobs. Depending on the partition, the way this billing is carried out 
differs.

### Standard partition

The standard partition is operated in exclusive mode: the entire node will always 
be allocated. In practice, you 128 core-hours are billed for every allocated 
node and per hour even if your job requested less than 128 cores per node.

For example, 16 nodes for 12 hours: 

```
16 nodes x 12 hours x 128 core-hour = 24576 core-hours
```

### Small partition

When using the small partition you are billed per allocated core or if you are 
above a certain threshold per chunk of 2GB of memory. Here is the formula that 
is used for billing:

```
corehours = max(ncore, ceil(mem/2GB)) x time
```

- if you use less than 2GB of memory per core, you are charged per allocated
  cores
- if you use more than 2GB of memory per core, you are charged per 2GB slice
  of memory
- if you are using the large memory nodes you will be billed per 2GB slice
  of memory

For example, 4 cores, 4GB of memory for 1 day:

```
4 cores x 24 hours = 96 core-hours
```

For example, 4 cores, 32GB of memory for 1 day:

```
32GB / 2GB x 24 hours = 384 core-hours
```

## Storage billing

Storage is billed by volume as well as time. The billing units are GB-hours.

### Regular Lustre filesystem

On the regular (spinning disk) Lustre filesystem, 1GB of data consume 1GB-hour
out of your storage allocation for every hour it stays on the filesystem.

For example, 375GB for 4 days:

```
375 GB x 4 days x 24 hours = 36000 GB-hours
```

### Flash Lustre filesystem

The flash based filesytem is billed at a 10x rate: 1GB of data consume 10GB-hour
out of your storage allocation for every hour it stays on the filesystem. As
a consequence, if you don't want to consume your storage allocation too quickly,
it's recommended to remove your data from the flash filesystem as soon as possible.

For example, 150GB for 2 days:

```
150 GB x 2 days x 24 hours x 10 = 72000 GB-hours
```