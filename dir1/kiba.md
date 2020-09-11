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
end


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

