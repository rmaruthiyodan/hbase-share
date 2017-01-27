# hbase tutorials

## Exercises:

### 1. Create a table and load some sample data manually:


create 'EMPLOYEE',{NAME => 'PERSONAL', VERSIONS => 2}, {NAME => 'OFFICIAL' , VERSIONS => 3}

put 'EMPLOYEE' , 'PERSONAL:1234', 'NAME', 'RATISH'

get 't1' , '456', {COLUMN => 'f1:name',VERSIONS => 2,TIMERANGE => [0,1485493181466] }


### 2. Find details on Region Assignment for the newly created table:

scan 'hbase:meta',{FILTER=>"PrefixFilter('table1')"}
