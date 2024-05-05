# Olist-Business-Analysis-ELK
This is a project to analyse on the customers, products, sallers, reviews and orders of the Brazilian E-commerce giant Olist with **Elasticsearch** **Kibana** **Logstash**.

## Downloading and configuring ELK:

### Elasticsearch :
1.  Here's the link to the downloads page : [https://www.elastic.co/downloads/elasticsearch](https://www.elastic.co/downloads/elasticsearch)
2.  Extract and launch a command prompt under the *bin* directory.
3.  Install Elasticsearch as a service with this command :
```
elasticsearch-service.bat install
```
4.  Launch Elasticsearch as a service :
 
```
elasticsearch-service.bat start
```


5.   change the password of the Elasticsearch user named 'elastic' :
   ```
   elasticsearch-reset-password.bat -i -u elastic
   ```

6.   Stop and restart the Elasticsearch service to apply changes :
   ```
elasticsearch-service.bat stop
```
   
```
elasticsearch-service.bat start
```

7.   Launch Elasticsearch using :
   ```
elasticsearch.bat
```

8.   Access to Elasticsearch via HTTPS at https://localhost:9200/ and authenticate with the password you've chosen for the *elastic* user :

   ![image](https://github.com/Eya-Jemal/Olist-Business-Analysis/assets/62604009/bf66a33a-f332-47ab-b43e-d92e65f609ee)
 
   
### Kibana :
1.  Here's the link to the downloads page : https://www.elastic.co/downloads/kibana
2.  Extract and launch a command prompt under the *bin* directory.
3.  Launch Kibana using :
   ```
kibana.bat
```
4.  Access to kibana via HTTPS at http://localhost:5601/ and authenticate with the password you've chosen for the *elastic* user :
   ![image](https://github.com/Eya-Jemal/Olist-Business-Analysis/assets/62604009/cdaf31ed-51fb-4582-9d48-3159e64aa58f)

### Logstash :
1.  Here's the link to the downloads page : https://www.elastic.co/downloads/logstash
2.  Extract and launch a command prompt under the *bin* directory.
3.  Configure your Logstash configuration file, you need to create or edit a .conf file:
    Logstash configuration file will vary based on your specific ETL (Extract, Transform, Load) requirements.
   ```
input {
    file {
        path => ["D:/D-II/Sem2/bi/Project/Brazilian E-commerce Public Dataset/olist_dataset_reviewFinale.csv"]
        start_position => beginning
	sincedb_path => "nul"
    }
}

filter {
    csv {
        separator => ","
	skip_header => "true"
	columns => ["review_id","order_id","review_score","review_comment_title","review_comment_message","review_creation_date","review_answer_timestamp","order_item_id","product_id","product_category_name","product_name_lenght","product_description_lenght","product_photos_qty","product_weight_g","product_length_cm","product_height_cm","product_width_cm","product_category_name_english","seller_id","seller_zip_code_prefix","geolocation_lat_saller","geolocation_lng_saller","seller_city","seller_state","shipping_limit_date","price","freight_value","customer_id","customer_unique_id","customer_zip_code_prefix","geolocation_lat_customer","geolocation_lng_customer","customer_city","customer_state","order_status","order_purchase_timestamp","order_approved_at","order_delivered_carrier_date","order_delivered_customer_date"]
	remove_field => ["@timestamp","@version","message","path","host","review_comment_message"]
    }


	mutate { convert => {
	"review_score" => "integer"	
	"order_item_id" => "integer"
	"product_photos_qty" => "integer"
	"product_weight_g" => "integer"
	"product_length_cm" => "integer"
	"product_height_cm" => "integer"
	"product_width_cm" => "integer"
	"seller_zip_code_prefix" => "integer"
	"geolocation_lat_saller" => "float"
	"geolocation_lng_saller" => "float"
	"price" => "integer"
	"freight_value" => "integer"
	"customer_zip_code_prefix" => "integer"
	"geolocation_lat_customer" => "float"
	"geolocation_lng_customer" => "float"
	
	}    
	}

	
	date {
        match => [ "shipping_limit_date", "dd/MM/YYYY" ]
	timezone => "UTC"
	target => "shipping_limit_date"
      	}

	date {
        match => [ "order_purchase_timestamp", "dd/MM/YYYY" ]
	timezone => "UTC"
	target => "order_purchase_timestamp"
      	}

	date {
        match => [ "order_approved_at", "dd/MM/YYYY" ]
	timezone => "UTC"
	target => "order_approved_at"
      	}

	date {
        match => [ "order_delivered_carrier_date", "dd/MM/YYYY" ]
	timezone => "UTC"
	target => "order_delivered_carrier_date"
      	}

	date {
        match => [ "order_delivered_customer_date", "dd/MM/YYYY" ]
	timezone => "UTC"
	target => "order_delivered_customer_date"
      	}
	
	date {
        match => [ "order_estimated_delivery_date", "dd/MM/YYYY" ]
	timezone => "UTC"
	target => "order_estimated_delivery_date"
      	}

	date {
        match => [ "review_creation_date", "dd/MM/YYYY" ]
	timezone => "UTC"
	target => "review_creation_date"
      	}

	date {
        match => [ "review_answer_timestamp", "dd/MM/YYYY" ]
	timezone => "UTC"
	target => "review_answer_timestamp"
      	}




}

output {
    stdout {
        codec => rubydebug
    }

    elasticsearch {
        hosts => ["https://localhost:9200"]
	ssl_certificate_verification => false
	user => "elastic"
      	password => "elastic"
        index => "olist_dataset_review_etl_geo_cs"
    }
}


```

+ To add a map visualization in Elasticsearch and Kibana, one of the solutions you can follow is to access the Dev Tools in Kibana and create a field of type geo_point for your future index, follow these steps:
  
  1- configure mapping in Kibana :

  
  	+ Open Kibana: Open your web browser and navigate to Kibana.
 
  	  
  	+ Access Dev Tools: Click on the "Dev Tools" tab located in the left-hand menu of Kibana. This will open the Kibana Dev Tools interface.
  	  
  	+ Create Mapping: In the Dev Tools interface, switch to the "Console" tab if it's not already selected.
  	  
	+ Use the following command to create a mapping for your index with a field of type geo_point: repeat the process for each field

  ````
  put _template/geo {
  "index_patterns" : [ 
    "your_index_name" 
    ], 
  "settings" : {} , 
    "mappings" : {
      "properties" : { 
        "your_geofield_name" : { "type" :"geo_point"
          
        }
        
      }},
      "aliases" : {}
  }
    ````
  
  ![image](https://github.com/Eya-Jemal/Olist-Business-Analysis/assets/62604009/fb932ee4-8f4b-4236-8af8-59834cd76cda)

  2- configure mapping in Logstash :
  	+ link the fields created in Kibana Dev Tools with a new fields in Logstash configuration file using existing data from your dataset with the format latitude,longitude.

  ```
  mutate{ add_field => {
	"geolocation_saller" => "%{geolocation_lat_saller},%{geolocation_lng_saller}"
	"geolocation_customer" => "%{geolocation_lat_customer},%{geolocation_lng_customer}"

	}		
	}

  ```

4.	Load data into Elasticsearch using Logstash with your specific configuration file:
   ```
logstash -f /path/to/your/file.conf
``` 

