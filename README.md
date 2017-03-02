# hbase tutorials

## Exercises:

### 1. Create a table and load some sample data manually:

    create 'employee',{NAME => 'PERSONAL', VERSIONS => 1}, {NAME => 'OFFICIAL' , VERSIONS => 3}

    put 'employee' , '1234', 'PERSONAL:name', 'Rob'
    put 'employee' , '1234', 'PERSONAL:dob' , '01-02-1959'
    put 'employee' , '1235', 'PERSONAL:name', 'Albin'
    put 'employee' , '1235', 'PERSONAL:dob' , '01-02-1970'
    put 'employee' , '1235', 'OFFICIAL:department', 'supportteam'
    put 'employee' , '1235', 'OFFICIAL:manager', 'Alan'
    put 'employee' , '1234', 'OFFICIAL:department', 'sales'


### 2. Find details on Region Assignment for the newly created table:

    scan 'hbase:meta',{FILTER=>"PrefixFilter('employee')"}


### 3. Scan and get records from the table


    scan 'employee'
    ROW                                           COLUMN+CELL
    1234                                         column=OFFICIAL:department, timestamp=1485496362149, value=sales
    1234                                         column=PERSONAL:dob, timestamp=1485496360272, value=01-02-1959
    1234                                         column=PERSONAL:name, timestamp=1485496360237, value=Rob
    1235                                         column=OFFICIAL:department, timestamp=1485496360340, value=supportteam
    1235                                         column=OFFICIAL:manager, timestamp=1485496360359, value=Alan
    1235                                         column=PERSONAL:dob, timestamp=1485496360321, value=01-02-1970
    1235                                         column=PERSONAL:name, timestamp=1485496360299, value=Albin
 

    get 'employee', '1235'
    COLUMN                                        CELL
    OFFICIAL:department                          timestamp=1485496360340, value=supportteam
    OFFICIAL:manager                             timestamp=1485496360359, value=Alan
    PERSONAL:dob                                 timestamp=1485496360321, value=01-02-1970
    PERSONAL:name                                timestamp=1485496360299, value=Albin
    4 row(s) in 0.0140 seconds
 
    describe 'employee'
    Table employee is ENABLED
    employee
    COLUMN FAMILIES DESCRIPTION
    {NAME => 'OFFICIAL', BLOOMFILTER => 'ROW', VERSIONS => '3', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FALSE', DATA_BLOCK_ENCODING =>     'NONE', TTL => 'FOREVER', COMPRESSION => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLICATION_SCOPE => '0'}
    {NAME => 'PERSONAL', BLOOMFILTER => 'ROW', VERSIONS => '1', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FALSE', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', COMPRESSION => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLICATION_SCOPE => '0'}


    put 'employee' , '1235', 'OFFICIAL:department', 'professionalservices'
    put 'employee' , '1235', 'OFFICIAL:manager', 'Amay'

    get 'employee', '1235'
    COLUMN                                        CELL
    OFFICIAL:department                          timestamp=1485496550198, value=professionalservices
    OFFICIAL:manager                             timestamp=1485496562141, value=Amay
    PERSONAL:dob                                 timestamp=1485496360321, value=01-02-1970
    PERSONAL:name                                timestamp=1485496360299, value=Albin
    4 row(s) in 0.0140 seconds


    put 'employee' , '1235', 'OFFICIAL:department', 'finance'
    put 'employee' , '1235', 'OFFICIAL:manager', 'Richard'


    get 't1' , '456', {COLUMN => 'f1:name',VERSIONS => 2,TIMERANGE => [0,1485493181466] }

### 4. Create a new table called ‘drug_stock’ , with the following columns - pre-split as regions : A-G , G-M, M-R , R-W , W-Z

Row Key:  drug_name
pharma_name
Generic_name 
Manufacture_year
Expiry_date
Price
Qty


create 'drug_stock','info',SPLITS=>['G','M','R','W']

scan 'hbase:meta',{COLUMNS=>'info:server',FILTER=>"PrefixFilter('drug_stock')"}
scan 'hbase:meta',{COLUMNS=>'info:regioninfo',FILTER=>"PrefixFilter('drug_stock')"}

put 'drug_stock','PARACITAMOL','info:pharma_name','novartis'
put 'drug_stock','PARACITAMOL','info:price',59
put 'drug_stock','PARACITAMOL','info:price',70

put 'drug_stock','VICKS','info:pharma_name','drreddys'
put 'drug_stock','VICKS','info:price',105

put 'drug_stock','cetrizine','info:pharma_name','glaxo'
put 'drug_stock','cetrizine','info:price',40


