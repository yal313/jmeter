# JMeter
## 1. JMeter connects to MongoDB for load testing
1) Download the latest jmeter, currently I'm using JMeter 3.2 http://mirrors.koehn.com/apache//jmeter/binaries/apache-jmeter-3.2.zip 
2) JMeter 3.2 /lib contains mongo-java-driver-2.11.3.jar, if your Mongo has been upgraded to Mongo 3.0 which supports SCRAM-SHA-1, this driver needs to be upgraded to 2.13 or higher to support this auth mechanism. (In this example I use mongo-java-driver-2.13.2.jar)
3) MongoDB Source Config and MongoDB Script are deprecated in newer version of JMeter, but using them is NOT RECOMMENDED anyway. http://jmeter.apache.org/usermanual/component_reference.html#MongoDB_Script_(DEPRECATED)
4) JSR223 Sampler should be used to connect to Mongo. (In this example, I use groovy as language.)

		import com.mongodb.DB;
		import com.mongodb.DBCollection;
		import com.mongodb.MongoClient;
		import com.mongodb.MongoClientURI;
				
		MongoClientURI uri = new MongoClientURI("mongodb://${DB_USERNAME}:${DB_PASSWORD}@${DB_SERVERS}/?authSource=${DB_DATABASE}&authMechanism=SCRAM-SHA-1");
		MongoClient client = new MongoClient(uri);
		DB db = client.getDB(${DB_DATABASE});
		DBCollection collection = db.getCollection(${DB_COLLECTION});
		SampleResult.setResponseData(String.valueOf(collection.getCount()));
