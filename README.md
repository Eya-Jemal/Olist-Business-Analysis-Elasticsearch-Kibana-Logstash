# Olist-Business-Analysis-ELK
This is a project to analyse on the customers, products, sallers, reviews and orders of the Brazilian E-commerce giant Olist with **Elasticsearch**, **Kibana** and **Logstash**.
## Dataset Configuration 
1.  Here's the link to the downloads page : https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce?select=product_category_name_translation.csv
2.  Create two datasets that groups the CSV files by their primary key :
   	+  **Payment_Dataset_Dimension :** Contains
   		+  	olist_customers_dataset.csv
 		+	olist_geolocation_dataset.csv
  		+	olist_order_items_dataset.csv
    	+	olist_order_payments_dataset.csv
     	+	olist_orders_dataset.csv
        +	olist_products_dataset.csv
       +	olist_sellers_dataset.csv
         +	product_category_name_translation
      
	+  **Reviews_Dataset_Dimension :** Contains 
		+  	olist_customers_dataset.csv
 		+	olist_geolocation_dataset.csv
  		+	olist_order_items_dataset.csv
    	+	olist_order_reviews_dataset.csv
     	+	olist_orders_dataset.csv
        +	olist_products_dataset.csv
        +	olist_sellers_dataset.csv
          +	product_category_name_translation



## Downloading and configuring ELK

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
        path => ["D:/D-II/Sem2/bi/Project/Brazilian E-commerce Public Dataset/your_data.csv"]
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
        index => "your_index_name"
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

## Explore and visualize data stored in Elasticsearch

### Explore data :
1.	Open Kibana: Open your web browser and navigate to Kibana.
2.	Go to Management: Click on the "Management" tab located in the left-hand menu of Kibana and click on the "Stack Management"
3.	Access Data Views : Under the "Kibana" section in the Management menu, select "Data Views".
4.	Create Data Views: Click on the "Create Data Views" button.
6.	Enter the name of your index pattern in the "Index pattern" field. You can use wildcard patterns to match multiple indices (e.g., my_index*).
7.	Select Time Filter Field (Optional): If your data has a time-based field (e.g., a timestamp), Kibana will ask you to select it. This allows you to use time-based filters and visualizations. Choose the appropriate time field from the dropdown menu.
8.	Click "Save Data Views to kiban".

Now that your index is created, access "Index Management" under the "Kibana" section in the Management menu to discover your index.

### Visualize data : 
1.	Go to Dashboard: Click on the "Dashboard" tab located in the left-hand menu of Kibana.
2.	Create a New Dashboard
3.	Configure Visualizations: Configure each visualization by selecting the index pattern, fields, and any additional settings or filters. Customize the appearance and settings of each visualization according to your requirements.
4.	Save Dashboard
5.	View Dashboard :
   
   ![image](https://github.com/Eya-Jemal/Olist-Business-Analysis/assets/62604009/eb168a02-c731-4b03-bd70-dbd6a6b24a05)

   ![image](https://github.com/Eya-Jemal/Olist-Business-Analysis/assets/62604009/c76d0283-579d-4fb3-bcc0-e83e18ade889)


   ![image](https://github.com/Eya-Jemal/Olist-Business-Analysis/assets/62604009/4e1fa0ab-693f-4b8f-8a96-105fb3f992b8)


   ![image](https://github.com/Eya-Jemal/Olist-Business-Analysis/assets/62604009/e05bf7c5-45c4-4475-a8d3-1157402a87b1)


   ![image](https://github.com/Eya-Jemal/Olist-Business-Analysis/assets/62604009/f37bf3f2-3b73-424a-930b-1e739783f5ba)







