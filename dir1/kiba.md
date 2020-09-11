###### kiba ETL
---

```.sh
vi Gemfile
bundle install
bundle exec kiba
```

```kiba_run.rb
job = ETL::SyncJob.setup(some_config)
Kiba.run(job)

job = Kiba.parse do
  require 'dsl_extensions/progress_bar'
end

require 'kiba-pro/destinations/sql_upsert'

module ETL
  module SyncPartners
    module_function
    
    def setup(source_file, sequel_connection, logger)
      Kiba.arse do
        logger.info "Starting processing for file #{source_file}"
      end
      
      source CSVSource,
        filename: source_file,
        csv_options: { headers: true, col_sep: ',' }
        
      destination Kiba::Pro::Destination::SQLUpsert,
        table: :partners,
        unique_key: :crm_partner_id,
        database: sequel_connection
    end
  end
end

job = Kiba.parse do
  pre_process do
  end
  
  source DataCSV
  transform do |row|
    @row_read_form_source_count += 1
    row
  end
  
end


```

```
```

```
```

E
```extract.rb
require 'csv'

class DataCsvSource
  def initialize(input_file)
    @csv = CSV.open(input_file, headers: true, header_converters: :symbol)
  end
  
  def each
    @csv.each { |row| yield(row.to_hash) }
    
    @csv.close
  end
end

```

T
```transform.rb
transform do |row|
  row[:this_field] = row[:that_field] * 10
  
  row
end

class DataTransform
  def process(row)
    row[:this_field] = row[:that_field] * 10
    
    row
  end
end


```

L
```load.rb
require 'csv'

class DataCsvDestination
  def initialize(output_file)
    @csv = CSV.open(output_file, 'w')
  end
  
  def write(row)
    unless @headers_written
      @headers_written = true
      @csv << row.keys
    end
    
    @csv << row.values
  end
  
  def close
    @csv.close
  end
end
```

```pp.rb
count = 0

pre_process do
  Email.send(supervisor_address, 'processing started')
end

source DataCsv, file: Data_file
transform do |row|
  count += 1
  
  row
end

post_process do
  Email.send(supervisor_address, "#{count} rows successfully processed")
end
```

```data=[rpcess=scro[t/et;
source CsvSource, 'incoming/customers_*.csv'

transform GeocodeTransform, [:address1, :ip, :city], [:lat, :lon, :geocoding_status]

lon
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

