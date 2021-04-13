## js-array

```javascript

/* Returns all the elements of array1 which are not present in array2 */
const diff = (array1, array2) => array1.filter((item) => array2.indexOf(item) < 0);

/* Returns all the elements of array1 which are not present in array2 (without duplicates) */
const diffWithoutDuplicates = (array1, array2) => Array.from(new Set(array1.filter((item) => array2.indexOf(item) < 0)));

/* Create an array with given length and fill it with given value */
const fillArray = (lengthOfArray, valueToFill) => Array(lengthOfArray).fill().map((v) => valueToFill);

```

## js-string

```javascript
const trimAllWhitespaces = (word) => word.replace(/\s/g,'');
```


## js-async
```javascript

/* Pause the execution for given duration in miliseconds */
const sleep = timeToSleepInMs => new Promise((resolve) => setTimeout(resolve, timeToSleepInMs));
```

## js-AWS
```javascript
/* Update element with itemData (object) in DynamoDB table (tableName) using keyAttribute (PK) */
const updateDynamoDbItem = async (docClient, { itemData, tableName, keyAttribute }) => {
  const params = {
      TableName: tableName,
      Key: {},
      ExpressionAttributeValues: {},
      ExpressionAttributeNames: {},
      UpdateExpression: "",
      ReturnValues: "UPDATED_NEW"
  };

  params["Key"][keyAttribute] = itemData[keyAttribute];

  let prefix = "set ";
  for (const attribute of Object.keys(itemData)) {
    if (attirbute === keyAttribute) {
       continue;
    }
    params["UpdateExpression"] += prefix + "#" + attribute + " = :" + attribute;
    params["ExpressionAttributeValues"][":" + attribute] = item[attribute];
    params["ExpressionAttributeNames"]["#" + attribute] = attribute;
    prefix = ", ";
  }
  return documentClient.update(params).promise();
}

/* Save data in S3 */
const saveInS3 = async (S3Client, { bucketName, keyName, contentType, data }) => {
  const params = {
    Bucket: bucketName,
    Key: keyName,
    Body: data,
    ContentType: contentType,
  };
  return S3Client.putObject(params).promise();
}

/* Scan for all of items inside DynamoDB and return (it can be expensive) */
const scanAllItemsFromDynamoDB = async (docClient, { tableName }) => {
  const params = { TableName: tableName };
  const scanResults = [];
  
  let items;
  do {
      items = await docClient.scan(params).promise();
      items.Items.forEach((item) => scanResults.push(item));
      params.ExclusiveStartKey = items.LastEvaluatedKey;
  } while (typeof items.LastEvaluatedKey != "undefined");

  return scanResults;
}

```


### commands

```bash

# Start a PostgreSQL server
pg_ctl -D /usr/local/var/postgres start  

# Delete all of the Redis keys matching the pattern
redis-cli -c -h REDIS_URL -p REDIS_PORT keys "pattern" | xargs redis-cli -c -h REDIS_URL -p REDIS_PORT del
```
