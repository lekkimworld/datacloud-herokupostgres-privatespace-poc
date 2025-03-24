# Using Heroku Postgres in Private Space from Data Cloud
This repo is mainly a note-to-self kind of thing... When using Heroku Postgres in a Private Space data in Data Cloud you need to use mTLS to connect, enable external connections (by default private space Heroku Postgres plans are only accessible inside the space) and whitelist the IP's connecting to Heroku Postgres. 

Steps are as follows:
1. In Data Cloud Setup find your Data Cloud "Home Org Instance" as this will tell you where the instance is hosted. This will be something like `CDP2-AWS-PROD1-USEAST1`. Once you have it refer to the [documentation](https://help.salesforce.com/s/articleView?id=data.c360_a_data_cloud_ip_address_allowlist.htm&type=5) for the IP addresses to whitelist in CIDR format. As of this writing the IP addresses are: "54.204.177.212/32", "35.153.189.123/32", "54.82.22.132/32", "54.87.94.242/32", "52.200.70.195/32", "3.223.146.214/32".
3. Now refer to [Connecting to a Private or Shield Heroku Postgres Database from an External Resource](https://devcenter.heroku.com/articles/heroku-postgres-via-mtls) to configure external access to your Heroku Postgres plan. Once you've enabled it download the mTLS key and certificates (a bundle of 3 files).
4. Update the IP whitelist for the Heroku Postgres instance using the Heroku CLI or the Dashboard. I find using the CLI the easiest. To do this you need the app name from Heroku (for this example `young-reef-43874`), the Heroku Postgres instance name (for this example `postgresql-deep-41052`) and the IP addresses to whitelist. Ensure you have the mTLS plugin installed as described in the article. Now add the IP addresses and remember you own for testing...
```bash
heroku data:mtls:ip-rules:create postgresql-deep-41052 --app young-reef-43874 --description "Data Cloud, US-EAST, CDP2-1" --cidr "54.204.177.212/32"
heroku data:mtls:ip-rules:create postgresql-deep-41052 --app young-reef-43874 --description "Data Cloud, US-EAST, CDP2-2" --cidr "35.153.189.123/32"
...
heroku data:mtls:ip-rules:create postgresql-deep-41052 --app young-reef-43874 --description "Data Cloud, US-EAST, CDP2-6" --cidr "3.223.146.214/32"
```
5. Test the whitelisting with `psql` CLI - also nicely described in the documentation referenced above.
6. Now go back to Data Cloud and configure the connector. You need both the certificates and the key from the bundle, the schema to connect, and database name and the username and password from the Heroku Postgres instance.
