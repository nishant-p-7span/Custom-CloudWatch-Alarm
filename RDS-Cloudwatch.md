# RDS instance Memory (RAM) monitoring metrics.
## To know the free memory of the RDS instance we use cloudwatch metrics name: `freeablememory`
- This cloudwatch mertices shows how much RAM is available.

## How to create memory utilization metrics from freeablememory metrics.
- go to cloudwatch metric -> RDS -> freeablememory -> add meth -> empty expression -> expression:`-(1-m1)` -> create.
- `m1` is the name of freablememory metrices.
- Using this method we can create memory utilization graph from 
